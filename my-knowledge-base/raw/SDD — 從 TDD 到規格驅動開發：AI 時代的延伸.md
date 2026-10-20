---
title: "SDD — 從 TDD 到規格驅動開發：AI 時代的延伸"
source: "https://blog.cashwu.com/blog/2026/sdd-from-tdd-to-spec-driven-development"
author:
  - "[[Cash]]"
published: 2026-02-15
created: 2026-05-21
description: "TDD 用測試驅動設計，SDD 用規格引導 AI。AI 擅長寫程式碼，但不知道該做到什麼程度。結構化的 Spec 定義了範圍和邊界，搭配 TDD 驗證結果，讓 AI 不只做得快，還做得對"
tags:
  - "clippings"
status: processed
---
> **測試系列文章**
> 
> - [Test Double 測試替身入門：五種類型一次搞懂](https://blog.cashwu.com/blog/2026/test-double-stub-mock-in-practice)
> - [TDD 的兩大學派：底特律 (Detroit) vs 倫敦 (London)，你是哪一派？](https://blog.cashwu.com/blog/2026/tdd-schools-detroit-vs-london)
> - [單元測試和 TDD 之間的關係](https://blog.cashwu.com/blog/2026/tdd-and-unit-testing-relationship)
> - [TDD、ATDD、BDD：三者到底差在哪？](https://blog.cashwu.com/blog/2026/tdd-atdd-bdd-comparison)
> - [AI 時代，TDD 不是過時了，而是更重要了](https://blog.cashwu.com/blog/2026/tdd-in-ai-age)
> - **SDD — 從 TDD 到規格驅動開發：AI 時代的延伸 (本篇)**
> - [前置驗證與後置驗證：你的程式碼有說清楚它的承諾嗎？](https://blog.cashwu.com/blog/2026/precondition-postcondition-unit-test)
> - [單元測試的好壞：你的測試在保護你，還是在拖累你？](https://blog.cashwu.com/blog/2026/good-test-bad-test)

## Prompt 的天花板

你跟 AI Agent 說：「幫我實作一個使用者註冊功能。」

AI 可能會給你一套包含 email 驗證、密碼強度檢查、手機簡訊驗證、OAuth 第三方登入、email 啟動流程的完整實作。很完整，但你其實只是要一個最基本的帳號密碼註冊而已

問題出在哪裡？不是 AI 的能力不夠，而是你給的「規格」不夠明確

自然語言 prompt 有一個根本性的限制：它是模糊的。「使用者註冊功能」這七個字，對你來說意思很明確，但對 AI 來說，它的可能解讀空間極大。AI 沒有你的業務上下文，不知道你團隊的技術決策，不清楚這個功能在整個系統中的定位。所以它只能猜，而猜的策略通常是「做多不做少」

這跟 [前一篇文章](https://blog.cashwu.com/blog/2026/tdd-in-ai-age) 提到的過度實作問題是同一個根源：AI 擅長 How，但不知道 What 的邊界

## 什麼是 SDD

SDD，Specification-Driven Development (規格驅動開發)，核心概念很簡單： **用結構化的規格文件來引導 AI 產生符合需求的程式碼**

如果說 TDD 是用測試來驅動設計，SDD 就是用規格來驅動 AI 的實作。它不是取代 TDD，而是在 AI Agent 時代，提供了一個新的前置步驟

傳統的開發流程是：需求 → 設計 → 實作 → 測試 TDD 的流程是：需求 → 測試 → 實作 → 重構 SDD + TDD 的流程則是：需求 → **規格** → 測試 → AI 實作 → Review

規格在這裡扮演的角色，就像是 AI 的導航系統。你不是告訴 GPS「帶我去一個好吃的餐廳」(太模糊)，而是給它一個精確的地址(結構化的規格)

## 從 Prompt 到 Spec 的差異

用一個實際的例子來看差異

**用 Prompt**

```
幫我實作一個密碼驗證器，要檢查密碼強度
```

AI 可能會給你一套包含長度檢查、大小寫檢查、數字檢查、特殊符號檢查、常見密碼黑名單、密碼熵計算的完整實作。你只想要前三項，但你得花時間從 AI 的輸出裡篩選

**用 Spec**

```markdown
## 密碼驗證器規格

### 功能範圍

- 驗證密碼是否符合基本強度規則
- 回傳驗證結果和錯誤訊息列表

### 驗證規則(僅以下三項，不做其他)

1. 密碼長度至少 8 個字元
2. 必須包含至少一個大寫字母
3. 必須包含至少一個數字

### API 設計

- class: PasswordValidator
- method: validate(password: string)
- return: { isValid: boolean, errors: string[] }

### 不在範圍內

- 特殊符號檢查
- 常見密碼黑名單
- 密碼熵計算
- 密碼歷史比對
```

差別在哪裡？Spec 不只說了「要什麼」，更重要的是明確說了「不要什麼」。這就是 [前一篇](https://blog.cashwu.com/blog/2026/tdd-in-ai-age) 提到的控制 AI 實作範圍——用規格來定義邊界

## SDD 的實踐層次

SDD 在實務上有幾種層次的做法

### 最基本：結構化的 Markdown 規格

最簡單的做法就是寫一份結構化的 Markdown 文件，包含功能範圍、API 設計、輸入輸出規格、邊界條件、不在範圍的項目。不需要特殊工具，任何 AI Agent 都能讀懂

```markdown
## 功能：訂單折扣計算

### 範圍

計算單筆訂單的會員折扣金額

### 規則

- 一般會員：無折扣
- 銀卡會員：5% 折扣
- 金卡會員：10% 折扣
- 折扣後金額取整數(四捨五入)

### API

- function: calculateDiscount(memberLevel: string, amount: number): number
- memberLevel: 'regular' | 'silver' | 'gold'

### 測試案例(預期)

- regular, 1000 → 0
- silver, 1000 → 50
- gold, 1000 → 100
- gold, 599 → 59.9 → 60(四捨五入)

### 不在範圍

- 促銷碼折扣
- 折扣疊加邏輯
- 運費計算
```

### 進階：工具化的 Spec

手寫 Markdown Spec 完全可行，但當專案越來越大、change 越來越多，你會開始想要一套工具來管理這些規格文件

[OpenSpec](https://github.com/Fission-AI/OpenSpec) 就是這樣的工具。它是一個 SDD 框架，主要搭配 Claude Code 等 AI 輔助開發工具使用。原理跟手寫 Markdown 一樣——結構化的規格引導 AI 實作——只是把格式標準化，並且用命令來管理整個流程

OpenSpec 的 OPSX 命令對應到 SDD 的開發流程大致是這樣

```
探索需求     建立 change    產生規劃文件         實作        驗證        歸檔
  │              │              │               │          │          │
  ▼              ▼              ▼               ▼          ▼          ▼
/opsx:explore → /opsx:new → /opsx:ff      → /opsx:apply → /opsx:verify → /opsx:archive
                            /opsx:continue
                            (逐步審查)
```

- `/opsx:explore` ：需求還不明確時，先進入探索模式調查和釐清
- `/opsx:new` ：建立一個新的 change，對應一次功能變更
- `/opsx:ff` 或 `/opsx:continue` ：產生 proposal、specs、design、tasks 等規劃文件。 `ff` 一次全部產生， `continue` 逐步產生方便審查
- `/opsx:apply` ：根據 tasks 清單開始實作
- `/opsx:verify` ：檢查實作是否符合 spec，找出偏差
- `/opsx:archive` ：歸檔完成的 change，把 delta specs 合併回主 specs

工具的好處是減少了 prompt 的輸入量，並且確保規格的格式一致。但核心邏輯不變：你需要先想清楚要做什麼、做到什麼程度。工具不會替你思考，它只是讓你的思考結果更容易被 AI 消化

> 完整的命令介紹可以參考 [OpenSpec OPSX 命令完整介紹](https://blog.cashwu.com/blog/2026/openspec-opsx-commands)

### 搭配 TDD：Spec → Test → AI Implementation

SDD 最有威力的用法是跟 TDD 結合。流程是這樣的

**第一步：寫 Spec** (人類主導)。定義功能範圍、API 設計、邊界條件、不在範圍的項目。這一步是人類的思考過程，不能跳過

**第二步：從 Spec 產生測試** (AI 輔助 + 人類 Review)。基於 Spec，讓 AI 產生測試案例。人類 Review 測試，確認是否涵蓋了 Spec 中定義的範圍，有沒有漏掉的邊界條件，有沒有多做的部分

**第三步：AI 實作** (AI 主導 + 人類 Review)。讓 AI 根據 Spec 和測試來實作。因為有了測試作為驗證，你可以確認 AI 的實作是否符合預期

**第四步：Review 和重構** (人類主導)。檢查 AI 的實作品質，必要時進行重構。測試保護你不會在重構過程中搞壞東西

```
人類思考         AI 輔助          AI 主導         人類把關
   │                │                │                │
   ▼                ▼                ▼                ▼
 寫 Spec    →   產生測試    →    實作程式碼    →   Review
(定義範圍)   (Review 測試)  (跑測試驗證)    (確認品質)
```

## Given-When-Then：Spec 的最佳格式

Spec 可以用很多格式來寫，但有一種格式特別適合：BDD 的 **Given-When-Then**

[這個格式](https://blog.cashwu.com/blog/2026/tdd-atdd-bdd-comparison#bdd-%E6%A1%86%E6%9E%B6--%E9%A9%97%E6%94%B6%E5%B1%A4%E7%B4%9A-cucumber--gherkin) 原本是 Dan North 設計給「人」看的——讓業務人員和開發者能用同一種語言溝通需求。但在 AI 時代，它意外地成為跟 AI 溝通的最佳格式

為什麼？因為 Given-When-Then 具備幾個 AI 最容易處理的特徵

**結構化** 。不像自然語言 prompt 那麼隨意，Given-When-Then 有固定的結構。AI 可以明確知道前置條件是什麼(Given)、觸發動作是什麼(When)、預期結果是什麼(Then)

**具體** 。SBE (Specification by Example，實例化需求)的核心就是用具體的例子取代抽象的描述。「系統應該根據會員等級提供折扣」是抽象的；「金卡會員購買 1000 元商品，應付金額是 900 元」是具體的。AI 處理具體例子的能力遠遠好過處理抽象描述

**可驗證** 。每個 Then 都是一個可以被程式化驗證的斷言。這意味著 AI 產生的程式碼有明確的通過/失敗標準

回想前一篇文章提到的：AI 擅長 How，不擅長 What/Why。Given-When-Then 格式恰好把 What(要做什麼行為)和驗證標準(怎麼算做到了)都講清楚了，讓 AI 可以專注在 How

### 實際比較

用同一個網球計分的需求來看差異

**自然語言 prompt**

```
幫我實作一個網球計分系統，要能追蹤兩個玩家的分數，
用 love、fifteen、thirty、forty 來表示，
還要處理 deuce 和 advantage 的情況。
```

這個 prompt 不算差，但 AI 可能會在很多地方做出你不預期的設計決策

**Given-When-Then 風格的 Spec**

```
Feature: 網球計分系統
  追蹤兩位球員的分數，用標準網球術語回報比分

  Scenario: 比賽開始時
    Given 一場新的比賽
    When 查詢目前比分
    Then 比分應該是 "love all"

  Scenario: 一方得分
    Given 一場新的比賽
    When player1 得 1 分
    Then 比分應該是 "fifteen love"

  Scenario: 雙方各得一分
    Given 一場新的比賽
    When player1 得 1 分
    And player2 得 1 分
    Then 比分應該是 "fifteen all"

  Scenario: 平分 (Deuce)
    Given 一場新的比賽
    When player1 得 3 分
    And player2 得 3 分
    Then 比分應該是 "deuce"

  Scenario: 優勢 (Advantage)
    Given 雙方比分為 deuce
    When player1 得 1 分
    Then 比分應該是 "advantage player1"

  Scenario: 從優勢回到 Deuce
    Given player1 有 advantage
    When player2 得 1 分
    Then 比分應該是 "deuce"

  Scenario: 贏得比賽
    Given player1 有 advantage
    When player1 得 1 分
    Then player1 贏得比賽
```

差異很明顯。每個場景都把前置條件、動作、預期結果寫清楚了，AI 可以精確地知道每個情境下系統應該怎麼表現。不會多做，也不會少做

而且你不一定要用 Cucumber 工具鏈。直接在測試程式碼中用 Given-When-Then 的思維來組織就夠了

```javascript
describe('會員折扣計算', () => {
  it('金卡會員購買 1000 元商品，應付金額為 900 元', () => {
    // Given
    const member = { level: 'gold' }
    const order = { amount: 1000 }

    // When
    const result = calculateDiscount(member, order)

    // Then
    expect(result.finalAmount).toBe(900)
  })
})
```

重點是 Given-When-Then 的 **思維方式** ，不是 Gherkin 的語法格式

## SBE：讓 Spec 活起來

提到 Given-When-Then，就不能不提 [SBE (Specification by Example，實例化需求)](https://blog.cashwu.com/blog/2026/tdd-atdd-bdd-comparison#bdd-%E7%9C%9F%E6%AD%A3%E7%9A%84%E5%83%B9%E5%80%BCspecification-by-example-sbe) 。Gojko Adzic 在 2011 年系統化了這個概念，核心是用具體的例子來描述需求，而不是用抽象的文字

在 AI 時代，SBE 有了一個新的價值

當你用 Given-When-Then 寫的場景對應到可執行的測試時，這些測試就是 Gojko Adzic 說的 **Living Documentation** (活文件)——不是寫完就丟的文件，而是可以持續執行、持續驗證的活文件

這解決了 AI Coding 的另一個問題： **上下文的延續** 。AI 沒有長期記憶(至少目前是)，每次對話都是從零開始。但如果你的專案裡有一組完整的 Given-When-Then 場景和對應的測試，AI 可以快速理解「目前系統做了什麼」「預期行為是什麼」

| 傳統 SBE | AI 時代的 SBE |
| --- | --- |
| 給人看的活文件 | 同時也是給 AI 看的活文件 |
| 確保團隊對需求有共識 | 確保 AI 理解系統現有行為 |
| 當需求變更時更新文件 | 當 AI 修改程式碼時作為護欄 |
| 驗收測試的基礎 | AI 實作的驗證標準 |

所以 SBE 在 AI 時代不只是團隊協作的工具，更是 AI 理解專案上下文的入口。你的 Spec 寫得越完整、場景覆蓋得越好，AI 就越能在現有系統的脈絡下做出正確的實作

## SDD 不是銀彈

這裡要特別強調一件事： **SDD 不是銀彈**

SDD 的價值是減少了跟 AI 溝通的模糊空間，但它只是一個工具，不能取代你的思考。要看你的 Spec 做到多少、規格寫得多完整。它只是減少了 prompt 的輸入，讓溝通更精確，但 **該做的事情還是要做，它沒有不見**

具體來說，以下這些事情 SDD 不能幫你做：拆解需求(你需要理解業務才能寫出好的 Spec)、設計決策(選擇什麼架構、用什麼設計模式，這些判斷還是你的)、品質把關(AI 產生的程式碼可以通過所有測試，但可能塞了一個 God Class 或搞出不必要的耦合——測試驗證行為，但設計品質、可讀性、可維護性需要人來判斷)、以及邊界條件的發現(需要領域知識和經驗)

SDD 減少的是「輸入的摩擦力」，但不減少「思考的必要性」

## Spec 寫多少才夠？

一個常見的問題是：Spec 要寫到多細？

答案取決於你對 AI 的信任度和功能的複雜度

**簡單功能** (工具函式、格式轉換等)：Spec 可以很簡短，幾行就夠。AI 在這類任務上的表現通常很好，不需要過度約束

**中等功能** (業務邏輯、API endpoint 等)：需要明確的範圍、API 設計、關鍵的 Given-When-Then 場景。特別是「不在範圍」的部分要寫清楚

**複雜功能** (跨模組互動、狀態管理等)：除了 Spec 之外，可能還需要拆分成多個小 Spec，每個小 Spec 對應一個 TDD 循環

一個衡量標準是：如果你把 Spec 給另一個(人類)開發者，他能不能理解你要做什麼、做到什麼程度？如果可以，那這份 Spec 給 AI 也夠用了。如果連人都看不懂，AI 更不可能猜對

## SDD + TDD 的整合

SDD 和 TDD 不是競爭關係，而是互補關係

TDD 的核心是「用測試驅動設計」。SDD 的核心是「用規格引導 AI」。兩者結合的效果是：規格定義了「要做什麼」(What)，測試驗證了「做到了嗎」(驗證)，AI 負責「怎麼做」(How)

| SDD 的價值 | TDD 的價值 |
| --- | --- |
| 解決 AI「做太多」的問題 | 解決 AI「做得對不對」的問題 |
| 定義範圍和邊界 | 驗證行為和品質 |
| 引導 AI 的方向 | 確保 AI 的正確性 |

如果你只用 SDD 不用 TDD，你有了方向但沒有驗證。如果你只用 TDD 不用 SDD，你有了驗證但 AI 可能走歪。兩者結合，方向對了、結果也對了

整合起來的完整流程

```
第一步：需求探索(人類主導)                    ← /opsx:explore
├── 理解業務需求，討論邊界條件
├── 產出關鍵場景(Given-When-Then 思維)
└── AI 輔助：提醒可能遺漏的邊界

第二步：撰寫 Spec(人類主導，AI 輔助)          ← /opsx:new + /opsx:ff 或 /opsx:continue
├── 寫結構化的規格(功能範圍、API 設計、限制)
├── 用 Given-When-Then 列出關鍵場景
├── 明確寫出「不在範圍」的項目
└── AI 輔助：檢查場景完整性和一致性

第三步：產生測試(AI 主導，人類 Review)          ← /opsx:apply (tasks 中的測試任務)
├── 從 Spec 和場景產生測試程式碼
├── 人類 Review：確認測試涵蓋範圍是否正確
└── TDD 精神：測試要能表達預期行為

第四步：實作(AI 主導，人類 Review)              ← /opsx:apply (tasks 中的實作任務)
├── AI 根據 Spec 和測試實作
├── 跑測試驗證
└── 人類 Review：確認實作品質和設計合理性

第五步：重構(人類主導，AI 輔助)                ← 重構後 /opsx:verify + /opsx:archive
├── 基於測試保護進行重構
├── 讓設計浮現(Emergent Design)
└── 保持測試綠燈
```

你可能注意到了，上面五個步驟裡有三個都出現了「人類 Review」。這不是多餘的儀式。Spec 定義了範圍，測試驗證了正確性，但程式碼「能動」和「寫得好」是兩回事。AI 可以產出通過所有測試的程式碼，同時把所有邏輯塞進一個方法、建立不必要的抽象層、或是用過度工程的方式解決一個簡單的問題。這些東西測試抓不到，Spec 也管不到，只有人看得出來。Review 不是流程上的簽核，而是確保產出的是真正可維護的軟體的最後一道防線

## 回到核心

工具在變，速度在變，但軟體開發的本質沒變：理解需求、做好設計、確保品質。AI 是最好的加速器，但方向盤還是要握在你手上

**速度是 AI 給的，方向是 Spec 給的，掌控是 TDD 給的。**

該做的事情還是要做，它沒有不見

---

> 圖片來源：AI 產生