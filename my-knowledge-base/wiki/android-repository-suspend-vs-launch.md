---
title: Android Repository：suspend fun vs 內部 launch 的架構語義
date: 2026-06-17
tags: [dev/flutter]
source: https://codelove.tw/@tony/post/3QjMYa
type: article
status: processed
asset_value: 60
---

現代 Android 官方推薦 Repository 暴露 `suspend fun` 而非在內部 `launch`，核心原因不是語法偏好，而是**任務生命週期歸屬**的架構設計：`suspend` 表達能力，`launch` 表達任務歸屬。Repository 應該定義資料語義，不應該決定執行時機和生命週期。

## T（核心論點）

**`suspend` 表達的是能力，而 `launch` 表達的是任務歸屬。** Repository 內部偷偷 `launch` 並沒有減少複雜度，只是把時序複雜度藏起來了——把「什麼時候做」和「如何做」混在同一層，讓生命週期、取消語義、例外傳播全部變得不透明。

## B（適用場景）

- Android 開發團隊制定 Repository 層 Coroutine 使用規範
- 程式碼審查時判斷 Repository 方法該用 suspend 還是 launch
- 理解結構化並發（Structured Concurrency）的設計意圖

## R（完整內容）

### 兩種寫法的本質差異

**寫法 A：suspend fun（推薦）**
```kotlin
// Repository
suspend fun loadUser(): User = withContext(Dispatchers.IO) {
    api.getUser()
}

// ViewModel
fun refresh() {
    viewModelScope.launch {
        val user = repository.loadUser()  // 任務歸 viewModelScope 管
        _state.value = user
    }
}
```

- Repository 只描述「這件事怎麼做」
- ViewModel 決定「什麼時候做」
- 頁面銷毀時，任務可以自動取消
- 例外沿呼叫鏈自然傳播

**寫法 B：內部 launch（不推薦）**
```kotlin
// Repository
fun load() {
    repositoryScope.launch {  // Repository 自己偷開一個協程
        // do something
    }
}
```

- ViewModel 只是「發了個命令」
- 真正的生命週期不再歸 ViewModel 管
- 頁面銷毀後，任務可能還繼續跑
- 呼叫方不知道完成時機，例外處理不透明

### 四個實際問題（內部 launch 的後果）

**1. 生命週期失控**  
Repository 假設「這份任務的生命週期由我來定」。但 Repository 可能被 Fragment（短生命週期）、ViewModel（中等）、WorkManager（跨程序）同時呼叫——它無法知道該綁定哪個。

**2. 完成時機模糊**  
呼叫方不知道任務何時結束，需要補 callback、事件通知、額外狀態流、手寫完成監聽……而 `suspend fun` 天然就解決了這些。

**3. 例外傳播被打斷**  
```
suspend fun：Repository 丟錯 → ViewModel 捕獲 → UI 決定如何顯示  ✓
launch 版本：例外到哪裡？自己吃掉？印 log？靠 CoroutineExceptionHandler？  ✗
```

**4. 取消能力被削弱**  
`viewModelScope` 取消時，如果 Repository 用自己的 `repositoryScope`，內部任務可能繼續跑 → 頁面沒了網路還在跑 → 多次點擊累積多個背景任務難以收斂。

### withContext vs launch 的混淆（常見錯誤）

很多開發者把這兩個混為一談：

| | withContext | launch |
|--|-------------|--------|
| 改變什麼 | 執行執行緒（切到 IO） | 任務歸屬（建立新 Job） |
| 生命週期 | 不變（仍在同一協程）| 改變（獨立 Job）|
| 取消關係 | 不變 | 改變 |
| 例外傳播 | 不變 | 改變 |

**正確用法：**
- 需要切換執行緒 → `withContext(Dispatchers.IO)`
- 需要建立獨立生命週期的新任務 → `launch`（且考慮清楚 scope）

### Repository 何時可以 launch？（特例）

Repository 內部 launch 有一個合理場景：**任務必須比呼叫方活得更久**（如：離開頁面也要完成的寫入操作）。

正確做法：注入外部的長生命週期 scope，而非 Repository 自己 new 一個：

```kotlin
class MyRepository(
    private val externalScope: CoroutineScope  // 從外部注入
) {
    fun saveToServer(data: Data) {
        externalScope.launch {  // 使用外部注入的 scope，不是自己建立
            api.save(data)
        }
    }
}
```

### 四個判斷問題（Code Review 用）

1. **一次性操作還是持續資料源？** → 一次性：`suspend fun`；持續：`Flow`
2. **誰最適合決定生命週期？** → 如果不是 Repository，它就不該 `launch`
3. **呼叫方需要知道何時完成、是否失敗？** → 需要 → 用 `suspend fun`
4. **任務需要活得比呼叫方更久？** → 不需要 → `suspend fun`；需要 → 注入外部長生命週期 scope

### 團隊約定（五條）

1. Repository 對一次性操作預設暴露 `suspend fun`
2. Repository 對持續資料預設暴露 `Flow`
3. 協程啟動通常由 ViewModel / UseCase / WorkManager 決定
4. Repository 內部可用 `coroutineScope` / `withContext` 組織實作，但不要偷開頂層 `launch`
5. 只有明確需要脫離呼叫方生命週期時，才考慮注入外部長生命週期 Scope

## C（關鍵行動）

- [ ] 在 Code Review checklist 加入「Repository 方法是否使用 suspend fun？」
- [ ] 將 withContext 和 launch 的區別納入新成員 onboarding 文件
- [ ] 檢視現有 Repository 中是否有 `repositoryScope.launch` 反模式

## 相關連結

- [[android-adk-agent-tool-calling]] — Android ADK：Agent 開發中的 Session 狀態管理（相關的非同步設計問題）
- [[flutter-patrol-e2e-testing]] — Flutter Patrol：協程與測試的交互（Flutter 側的類似概念）
