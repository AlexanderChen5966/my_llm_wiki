---
title: 帶 Claude Code 動一次真重構：邊砍不動，但群集會回應
date: 2026-08-18
tags: [dev/architecture, ai/agent, dev/testing]
source: https://medium.com/@n913239（〈圖說遷移只做一半，那我就把它拆——帶 Claude Code 動一次真重構〉，作者 MikeWang）
type: article
status: processed
asset_value: 72
---

系列前篇（診斷）見 [[graphify-codebase-knowledge-graph]]——那篇把一個真實 iOS 專案掃成知識圖，找出 `RecordDetailViewController` 這顆 god node（99 條邊、1722 行）。**本篇是執行：真的動一刀，然後用同一張圖量它有沒有變好。**

## T（核心論點）

> 🔑 **邊（耦合）是黏的，一刀砍不動；但群集（概念邊界）會回應重構。**

實測結果反直覺到值得完整記下：抽出一個乾淨、有 14 個測試罩著的 validator 之後——

- 檔案從 **1722 行掉到 1660 行**（只少 62 行）
- god node 的度數 **只從 120 掉到 119（一條邊）**
- 🔴 **整張圖的節點還「變多」了（+13）**

> **好的重構未必讓圖變小，而是讓它分層更誠實。**

以及那句最該貼在牆上的話：

> 🔑 **修好一顆 god node 不是一次英雄式重構，是一百次無聊的小刀。**

## B（適用場景）

**適用：**
- 面對 Massive View Controller／god class 的實際下刀順序
- 用 Claude Code 做遺留程式碼重構時的分工與把關重點
- 想量化重構成效（而且不想被數字騙）

**不適用 / 注意：**
- 實作為 iOS／Swift 專案，但方法論與語言無關
- ⚠️ 單一專案的一次實測，非統計結論

## R（四個步驟）

### Part 1：從圖挑病灶（不憑印象）

99 條邊拆開看：**92 條是它自己的方法**、6 條是繼承（父類 ＋ UIKit delegate）。

🔑 **所以它肥不是因為連到很多「別人」，而是因為一個類別扛了 92 個方法——教科書級的 Massive View Controller。**

那 92 個方法光看名字就自己分群了：建 UI（`make*` 43 個 ＋ `setup*` 13 個 ≈ **56 個**）／日期選擇（約 18 個）／鍵盤與手勢、TextField delegate／**驗證（約 7 個）**。

**第一刀選驗證邏輯**——不是因為它最大（它其實最小），而是因為它**最安全、最好測**：

> **驗證是「給一組輸入、回一個合不合法」，天生就是純函式的形狀；而純函式正是 characterization test 最好下手的地方。先從測得動、風險最低的那塊開刀——先求不出事，再求漂亮。**

### Part 2：安全網先行，卻發現「根本測不了」

打開那幾個驗證方法，表面收 String、看起來很純，體內卻糾了三種東西：

```swift
func isInputValid(...) -> Bool {
    Utility.showAlert(alertInfo, on: self)          // ① 副作用：直接彈 alert
    let hasTime = optionButton.isSelected || ...    // ② 讀 UI：8 顆 button 的狀態
    if !(itemQty > 0) { ... }                       // ③ 純規則：混在中間
    return false
}
```

🔴 **「要單元測試這種東西，你得先生出一個真的 view controller、把每顆 button 擺到對的狀態、還得攔截 alert——測試比被測的邏輯還難寫。」**

🔑 **所以第一刀根本不是「搬」，是「剝」**：把**決策**從**讀 UI**和**彈 alert** 裡剝出來。抽出純粹的 `RecordValidator`——吃素值（`RecordInput`）、回結果列舉（`RecordValidationError` 或 `nil`）、**不碰 UIKit、不彈 alert、不讀 `self`**。

剝乾淨後測試就變得很無聊——**而無聊正是重點**。14 個 case 把原本規則一條條釘住（必填、勾了選項沒填數量、至少選一個時段、至少選一種方式、日期先後、以及**規則的優先順序**）。

### Part 3：一刀一刀拆，三處改動都很小

`isInputValid` 瘦身（只做三件事：蒐集 `RecordInput` → 丟給 validator → 決定要不要彈 alert）／`getQuantityErrorMessage` **整個刪掉**／日期檢查只留 alert 包裝。

**與 Claude Code 的分工很明確**：

> **Claude 提 diff、我逐段把關。它最擅長的正是這種機械但繁瑣的搬遷——把散落的判斷收攏、對齊參數、保持 alert 文字一字不差。我要盯的，是它有沒有偷改行為（把某條規則的順序調換、或漏掉一個邊界）。**

🔑 **真正讓作者敢按 commit 的不是行數，是「既有測試不用改就全綠」**——專案本來就有 `RecordDetailViewControllerTests` 透過 mock 呼叫 `super.isInputValid(...)`，正好踩在被改的方法上，**它們一行沒動、全部通過，就是「行為沒變」最硬的證明**。

> **重構的定義本來就是：改結構，不改行為。測試綠，才算數。**

### Part 4：⚠️ 量測前先處理一個會讓數字騙人的前提

重跑 graphify 時工具版本已比前篇新，**同一份程式新版建出來的圖大了一圈**（邊數幾乎翻倍、節點 id 命名規則也變了）——同一顆 VC 前篇讀到 99、現版讀到 120，**變胖的是工具的數法，不是 VC 本身**。

🔑 **所以作者做了一件「笨但必要」的事：把重構前、重構後兩個版本都用「現在這一版」graphify 各掃一次，只比這兩者。**

同一把尺的結果：god node 120 → 119。**那條消失的邊正好是唯一完全刪掉的方法** `getQuantityErrorMessage`（92 個 method 邊剩 91）。其餘方法都還掛在 VC 上——

> 🔑 **因為我把「決策」搬走了，但沒把「讀 UI」搬走。`isInputValid` 現在還是得把那一整排 button 讀成一包資料再丟給 validator。決策變得可測了，VC 的觸手卻一根沒少。**

**真正動的是群集**：Leiden 分群多切出一個新群集，把整條驗證呼叫鏈認了出來——

```
submitButtonClick() → isInputValid() → validate()
                                     → quantityMissingTypes()
                                     → isEndOnOrAfterStart()
+ RecordValidator / RecordInput / RecordValidationError
```

該群集 **18 個成員、內部 25 條邊、對外 17 條**（內比外多，算內聚的一塊）；god node 本體仍留在原本那個大群集裡。

> **重構沒讓 VC 變不連通（度數只 −1），但它讓這個 codebase 多出一塊有名字、指得出來的「驗證」區域。「submit 按鈕 → 驗證 → validator」這條路原本埋在 god node 的大泥球裡，現在被圖認出來、框成了一塊。**

## C（關鍵行動）

- [ ] 下刀順序按「最好測」而非「最大」——先求不出事，再求漂亮
- [ ] 發現「測試比被測邏輯還難寫」時，先**剝**（決策／讀 UI／副作用）再搬
- [ ] 量測重構成效時，**前後都要用同一版工具掃**；並且看群集變化而不只看邊數

## 補充資料

### 🔑 兩句值得單獨記的區分

> **「有測試的重構」和「重構過的專案有測試」是兩回事。**

作者誠實收束：那 120 條邊還在、VC 還是 god node，只讓「驗證」這一角測得動，**其他幾十個方法還在裸奔**，離「整顆 VC 安全」還很遠。

> **結構愈清楚，AI 能替你接手的就愈多。**

三件湊齊才是完整的重構循環：**graphify 是診斷、Claude Code 是執行、14 個 characterization test 是安全網**——看得到病、動得了刀、還有網接著。

### 為什麼「群集」比「邊」更能反映重構

群集是分群演算法（本例用 **Leiden**）把「彼此連得特別密、跟外面連得鬆」的節點自動圈成的一組，等於**圖裡自然浮現的一塊模組**。所以：

- **邊 = 實際的呼叫／依賴關係**，重構若只搬決策不搬讀取，邊幾乎不動
- **群集 = 概念邊界**，抽出具名抽象後會立刻反映

⚠️ 這正好補上 [[graphify-codebase-knowledge-graph]] 收錄的那句批評——「cohesion 指標在最需要它的地方最沒鑑別力」：**本篇顯示 cohesion／degree 對「一次乾淨的抽離」幾乎無感，真正動的是分群結果**。

### 參考出處

characterization test 與「先立測試再重構」的經典出處是 Michael Feathers《Working Effectively with Legacy Code》；graphify 的底層是 **tree-sitter**（掃程式碼、零 token）。

## 相關連結

- [[graphify-codebase-knowledge-graph]] — **系列前篇（診斷）**：God Nodes 量化、Leiden 群集看出「遷移做到一半」、以及 cohesion 指標的侷限
- [[ai-code-review-safeguards]] — AI 生成程式碼的把關（本篇「盯它有沒有偷改行為」的具體實踐）
- [[agent-bug-fix-eval-green-is-not-correct]] — 🔑 **對照**：那篇是 agent 為了讓測試綠而改測試；本篇是**既有測試一行沒動就全綠**才算行為沒變
- [[ai-era-quality-assurance-test-oracle]] — 測試要能表達正確性
- [[wayfinder-decision-map-workflow]] — 垂直切片與「一場 session 最多解一張票」
- [[claude-code-large-codebase]] — 大型專案的配置策略
- [[rust-hot-path-optimization-ai-review]] — 另一個「拒絕 AI 未要求的相依變更」實例
