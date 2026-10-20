---
title: Apple 官方 CLAUDE.md 91 行拆解：「不寫會怎樣」是唯一的入場測試
date: 2026-09-01
tags: [ai/claude, ai/prompt-engineering]
source: https://medium.com/@n913239（MikeWang，2026-08-28）／被拆解的檔案：https://github.com/apple/containerization/blob/main/CLAUDE.md（Apache-2.0，掃描版本 commit 74ace148）
type: article
status: processed
asset_value: 85
---

拿 Apple 官方寫給 Claude Code 的 `CLAUDE.md`（`apple/containerization`，Apache-2.0）當標準，量化拆解後對照官方文件，再回頭檢討作者自己那份三週從 53 行長到 100 行的檔案。

🔴 **本條目修正了本庫既有的一條判準**——見下方「推導成本」一節，並已在 [[claude-md-optimization-techniques]] 標注。

## T（核心論點）

一份 `CLAUDE.md` 值不值錢，不看它多長，看**每一行的「被違反成本」有多高**。唯一的入場測試：

> **把每一條都翻譯成「如果沒寫，AI 會怎麼做錯」——翻得出來的，才有資格留在檔案裡。**

「請寫出乾淨的程式碼」翻譯出來是「不會怎樣」（它本來就在盡量寫乾淨的程式碼），所以那句話沒有資格佔一行。

🔑 更進一步：**`CLAUDE.md` 不是一份寫給 AI 的專案簡介，是一份「我們一起踩過的雷」的清單。它應該是長出來的，不是設計出來的——而它長出來的唯一方式，就是你真的做錯過。**

## B（適用場景）

**適用：** 體檢自己的 `CLAUDE.md`／`AGENTS.md`；判斷一條規則該進 `CLAUDE.md`、還是該寫成 hook／lint／CI；決定架構說明要不要寫進去。

**不適用 / 注意：**
- ⚠️ Apple 那份是**系統專案**（跨 macOS/Linux、27 個模組、虛擬化與 gRPC）。它能只用 91 行，很大程度是因為有一整套工具在替它記規則（見下方「反直覺比較」）——工具鏈薄的專案，行數自然會多，不該直接拿 91 行當目標。
- 🔴 **刪除法（每半年清空）不適用於回饋慢的場景**——作者明確指出 Boris Cherny 測的場景回饋迴路很緊（刪掉、跑一次、壞了當場就知道）。

## R（操作步驟）

### 一、Apple 那份的形狀（量化）

| 指標 | 數值 |
|---|---|
| 總行數 / 非空行 | 91 行 / 63 行 |
| 段落 | 4 段：`Build / Test / Format`、`Architecture`（含 4 子節）、`Conventions`、`Requirements` |
| 點名的 `make` 指令 | 二十多個（`make all`／`make check`／`make protos`／`make linux-integration`…）|
| 行內 code span | **230 個，分布在 53 行裡**——每 4 個非空行就有超過 3 行帶著具體指令、檔名或設定值 |
| 空泛用語掃描 | `clean code`／`best practice`／`please`／`be helpful`／`high quality`／`readable code` — **命中 0 筆** |

> 官方建議單一 `CLAUDE.md` 控制在 **200 行以內**（每次 session 整份載入，太長會吃 context）。Apple 用了不到一半。
>
> 🔴 **2026-09-03 更正**：原文（及本條目初稿）把「降低遵循率」一起寫進來，**但那部分不成立**——1,650 session 實測顯示檔案長度 25／100／250／500 行對遵守率**無可測量差異**，官方措辭也只說 shorter files produce better adherence 而未給門檻。**200 行是 context 成本的目標值**；另注意「只讀前 200 行」的硬截斷屬 `MEMORY.md` 而非 `CLAUDE.md`。完整查證見 [[claude-md-guide]]。

### 二、它挑什麼來寫：「猜不到，而且猜錯要付代價」

第一段第一句就是示範：

> *The project is built via `make`, not directly with `swift build`.*

一句話擋掉最可能犯的第一個錯——這個 repo 有**兩個** Swift package（根目錄一個、`vminitd/` 一個），後者還得在 Linux 容器裡用 musl 靜態編譯。不講，AI 一定直接 `swift build` 然後卡死。

其餘幾條的共同點：
- `.pb.swift` 是**生成檔**——不講，AI 看到它就是普通 Swift 檔，改它天經地義
- `WARNINGS_AS_ERRORS` **預設開啟**——不講，AI 遇到 warning 擋路時關掉它，是完全合理的判斷

🔑 這些都不是「AI 不夠聰明」，是**它沒有你腦袋裡那份專案脈絡**。

### 三、對照組：同一個專案的兩種寫法

通不過測試的版本（大多數人的開頭）：
```
## 專案簡介
這是一個 iOS App，使用 Swift 開發，架構採用 MVVM。
## 開發規範
- 請寫出乾淨、可讀的程式碼
- 請遵循 Swift 最佳實踐
```
⚠️ 更糟的是「這是一個 iOS App、使用 Swift」——**Claude 打開資料夾就知道了**，等於花 context 去講它已經看得到的事。

通得過測試的版本：
```
## 建置
- 用 `xcodebuild -workspace App.xcworkspace`，不要用 `-project`（SPM 相依會解不到）
- `Package.resolved` 有兩份（`.xcodeproj` 與 `.xcworkspace` 各一），
  改私有 repo 網址時兩份都要改，不然還是抓舊網址
## 慣例
- `Generated/` 底下是 SwiftGen 產的，不要手改；改完 `.strings` 跑 `make gen`
- UI 測試約 12 分鐘，本機只跑 unit test：`-only-testing:AppTests`
```
行數差不多，但後者每一條都是「你不講，它就會做錯」。

### 四、🔴 修正判準：「能推導」不等於「推導成本可以接受」

官方 `/doctor` 的精簡標準是——**砍掉「Claude 可以自己從 codebase 推導出來」的東西**（目錄結構、相依清單、**架構概覽**），留下踩雷點、理由、以及「跟工具預設不同」的慣例。

**但 Apple 那份最長的一段，正好就是 `Architecture`（還帶 4 個子節）。** 不管照官方那條還是作者自己先前寫過的「超過 10 行就問能不能從 codebase 推斷」，它都該被砍掉。

作者用自己前一篇的經驗當反證：他拿知識圖去讀這個 repo，**花了大半天**才把「主機和客體隔著 vsock 上的 gRPC」這條架構事實挖出來。而 Apple 的 `CLAUDE.md` 裡，這件事用大約 15 行就講完了，還附上 `VirtualMachineAgent` 這層抽象、兩個 VMM backend 怎麼分、哪些檔是 `#if os(macOS)`。

> 🔑 **判準不是「推不推得出來」，是「推導一次要花多久 × 會推導幾次」。**
> 一個 6,182 節點、15,855 條邊的專案，架構確實推導得出來——代價是大半天。而寫成 15 行，是一次寫、每個 session 都省。

### 五、`CLAUDE.md` 是脈絡，不是強制執行

官方原文：*Claude treats them as context, not enforced configuration. To block an action regardless of what Claude decides, use a PreToolUse hook instead.*

那為什麼 Apple 明明有 CI 開著 `WARNINGS_AS_ERRORS`、pre-commit hook 擋 license header，還要在 `CLAUDE.md` 裡寫一次？**差別在時機：**

> 🔑 **工具是事後擋，`CLAUDE.md` 是讓它一開始就別走那條路。**
> 不講，AI 遇到 warning 擋路就順手關掉 flag、一路寫下去，等 CI 紅了才知道整趟白跑。
> **兩者不是二選一，是一個省來回、一個保底。**

### 六、Boris Cherny 的刪除法，以及它真正在做的事

2026-01 Threads：*Try deleting your CLAUDE.md every 3 months. Then add back an instruction at a time when you see the model struggling with it*
2026-08 YC Startup School（Opus 5 上線隔天）：週期放寬到**每六個月**，範圍擴大到 `CLAUDE.md`、skills、hooks 全部刪掉。同場提到 Opus 5 出來時他們把 Claude Code 自己的 system prompt **砍掉超過 80%**，因為「the model is actually a little bit more intelligent without these prompts」。

重點在後半句 `add back an instruction at a time`——他不是叫你別寫，是把 `CLAUDE.md` **從「我覺得該立的規矩」降級成「有實證的補丁」**：每一條都得有一次可指認的失敗當背書。刪光只是逼你重新舉證的手段。

🔑 **三個月改口成六個月，本身就是論點的一部分**：這個週期不是最佳實踐，是**模型能力的函數**——模型愈強，需要外掛的指示愈少、也腐爛得愈快。所以他不給「該寫什麼」的清單，只給「多久清一次」：清單註定過期，流程不會。

🔑 **刪除法其實是在替你分類**：
- **能力型規則**（「不要用 force unwrap」）→ 會隨模型變強而失效
- **事實型規則**（「`Package.resolved` 有兩份」）→ **模型永遠不會自己學會，因為它不是通則，是你們專案的歷史事實**

Apple 那 91 行幾乎全是後者，所以它撐得住六個月一刪。

### 七、一個反直覺的比較

**Apple 的容器專案 91 行，作者寫文章的個人 repo 100 行。** 後者的規則檔居然比較長。

因為 Apple 那個專案**有一整套工具在替它記規則**：`Package.swift` 宣告模組邊界、`Makefile` 定義每條流程、`.swift-format` 管格式、CI 開 `WARNINGS_AS_ERRORS`、pre-commit hook 擋 license header。

> 🔑 **規則只要工具能強制，就不必寫進 `CLAUDE.md`。**

而寫文章**沒有編譯器**：「中英文的 H2 章節數要一樣」「圖片數要等於提示行數」「中文版不能有全形標點」——沒有任何工具會擋你，所以只能寫成散文，每個 session 載入一次，祈禱它被遵守。

作者的結論是：那 100 行裡有一大半**其實可以被腳本檢查**，那它們就不該只活在 `CLAUDE.md` 裡——下一步不是繼續加行數，是把發布前檢查清單寫成腳本。

## C（關鍵行動）

- [ ] 打開自己那份，拿第一條規則問：**這條不寫的話，會怎樣？** 答不出來就刪掉
- [ ] 對每條問第二題：**這條需要「一定擋得住」嗎？** 需要的話寫成 hook／lint／CI
- [ ] 架構段落別急著砍——先算「推導一次要花多久 × 會推導幾次」，成本高就留著
- [ ] 別坐下來想規則，**等它撞出來**（官方判準：同樣的錯犯第二次）
- [ ] 盤點哪些規則其實可被腳本檢查，把它們搬去讓機器擋
- [ ] 定期刪（半年）並觀察：活回來的是事實型規則，沒活回來的是能力型規則——但**回饋慢的專案別用刪除法**

## 相關連結

- [[synthesis/synthesis-claude-md]] — **本篇為其來源之一**（2026-09-03 重跑，5→6 篇）：CLAUDE.md 撰寫方法論精粹；本篇貢獻「不寫會怎樣」測試與推導成本判準

- [[claude-md-optimization-techniques]] — ⚠️ **本篇修正該條目的技巧 3**：「凡 AI 自己翻專案就能發現的，一律刪」需加但書——判準是推導成本，不是可推導性
- [[claude-md-design-patterns]] — 依規模的 7 種範本；本篇提供官方級實例當標尺
- [[claude-md-guide]] — Boris Cherny 六大核心引擎 + Karpathy 12 條
- [[claude-md-behavioral-control-principles]] — 行為控制規則該放哪一層
- [[claude-code-hooks-memory]] — 官方定性「CLAUDE.md 是上下文參考非強制」的機制細節；本篇補上「為什麼有 hook 還要寫」的時機論
- [[ai-native-sdlc-playbook-anthropic]] — Anthropic 官方 `CLAUDE.md` 範例的段落結構（含「Claude 容易犯的錯」欄位），與本篇結論一致
- [[steering-claude-seven-carriers]] — 七種載體與「放錯地方」的症狀
- [[agents-md-guide]] — `AGENTS.md` 的對應寫法
- [[claude-code-auto-memory]] — `MEMORY.md` 的 200 行硬截斷（⚠️ 別與 `CLAUDE.md` 的成本目標值混用——這是本庫曾出現過的實際錯誤）
