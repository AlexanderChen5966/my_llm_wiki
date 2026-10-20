---
title: Spec-First AI 編程工作流：先讓 AI 出方案，再讓 AI 實作
date: 2026-06-22
tags: [ai/agent, ai/claude]
source: https://codelove.tw/@tony/post/xMVeX3
type: article
status: processed
asset_value: 68
---

直接讓 AI 寫程式碼，它在「資訊不全」時只能靠猜——順手改不該動的檔案、腦補邊界條件、重複造輪子。Spec-First 工作流在實作前插入一個可 review 的方案環節，讓意圖表達先到位，再讓 AI 把意圖翻譯成程式碼。

## T（核心論點）

**「讓 AI 先寫方案（spec）、人工確認後再讓 AI 按方案寫程式碼」**，能大幅降低 review 時才發現偏差的成本。核心三點：
1. Spec 是中文說明（可被 review），不是直接程式碼
2. 人工確認後才能進入實作——spec 階段發現偏差成本最低
3. 第二輪 prompt 把 spec 餵回去，約束 AI 自由發揮的空間

## B（適用場景）

**最適合：**
- 新功能開發（最佳場景）
- 舊模組重構（spec 階段先對齊邊界）
- 技術方案調研（spec 就是調研報告本身）

**效果有限：**
- Bug 修復（主要在排查問題，改動往往是小範圍的）
- 純 Code Review（是為了提前發現問題，不需要走 spec 流程）

## R（完整流程）

### 五步概覽

```
1. 餵上下文：提示詞、需求說明、設計稿、AGENTS.md、相關程式碼
2. 出方案：讓 AI 輸出 spec（中文說明，非程式碼）
3. Review 改方案：人工主導，必須經確認才能進入下一步
4. 按方案實作：讓 AI 嚴格依據 spec 編碼
5. 驗收 review：AI review + 人工 review
```

### Step 2 出方案的 prompt 模板

```
我要在選單下，新增一個模組：
- 需求原型見上傳的截圖
- figma 設計稿如下：[URL]

請根據需求原型、figma 設計稿和當前程式碼上下文輸出詳細實作的方案。

注意：
- 原型截圖中的紅色文案是業務邏輯描述，要重點關注
- 不清楚的地方需要和我確認，禁止隨意推測
```

指定 Claude Code、Codex、Cursor 為 plan 模式，只輸出方案不寫程式碼。

### Step 3 Review 時常見的五類問題

1. **需求理解偏差**：AI 複述的需求和想的不一樣，及時糾正（成本最低）
2. **越界改動**：AI 順手要改無關的檔案，砍掉，保持最小範圍
3. **重複造輪子**：AI 想寫一個工具函式，但專案裡已經有了
4. **假設沒說明**：AI 默默假設了邊界條件，要讓它顯式寫出來
5. **遺漏邊緣情況**：載入狀態、空狀態、權限、異常分支

直到 spec 雙方都認可，才進入 Step 4。

### Step 4 按方案實作的 prompt

```
請嚴格按照上面確認的方案實作。
- 不引入方案之外的改動
- 實作完成後，請按方案的「實作步驟」逐條說明你做了什麼
- 如果實作過程中發現方案有問題，先停下來告訴我，不要自行調整
```

### 基礎設施：AGENTS.md + MCP

**AGENTS.md**（每次對話自動載入，定義安全邊界和工程約束）：

```markdown
### 安全邊界
- 不自動提交程式碼、不自動推送分支、不改寫 git 歷史
- 不讀取 .env*、金鑰/憑證類檔案
- 非必要不新增依賴

### 工程約束
- 技術棧：React / TypeScript
- 重複造輪子順序：公共包 → antd → 新增
- 交付前必須跑 lint + type-check
```

**MCP（降低上下文餵入成本）**：
- Figma MCP：AI 直接讀設計稿，不需人工截圖轉文字
- GitLab MCP：取得 MR 資訊供 Code Review
- 本質：把「人肉中轉上下文」的成本消除

### 提效量化指標（待建立）

`ai-coding-stats` skill 基於 git-ai 的 commit metadata 統計：
- AI 程式碼有效採納率
- AI 程式碼提交留存率
- AI 程式碼占比

## C（關鍵行動）

- [ ] 建立 AGENTS.md，把安全邊界和工程約束寫進去，讓 Claude Code 每次自動載入
- [ ] 下次新功能開發前，先讓 AI 輸出 spec，review 確認後才進入實作
- [ ] 評估安裝 Figma MCP，讓 AI 直接讀設計稿，省去截圖轉文字的時間
- [ ] 在 spec 的 review 清單加上五類問題：需求偏差、越界改動、重複輪子、隱性假設、邊緣情況

## 相關連結

- [[claude-code-team-dev-kit]] — team-dev-kit：用 Skill + Hook 把團隊規則變成自動遵守的東西
- [[agentic-coding-expertise-research]] — Anthropic 研究：領域知識比 coding 技能更重要
- [[loop-engineering-6-components]] — Loop Engineering：下一層自動化（spec-first 是 loop 的前提）
