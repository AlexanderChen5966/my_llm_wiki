---
title: "OpenSpec v1.0 大改版：從 3 個命令到 10 個命令的 OPSX 工作流程"
source: "https://blog.cashwu.com/blog/2026/openspec-opsx-commands"
author:
  - "[[Cash]]"
published: 2026-02-03
created: 2026-05-21
description: "OpenSpec v1.0 將原本 3 個命令的剛性階段式工作流程，擴展為 10 個彈性動作式的 OPSX 命令。這篇文章帶你了解每個新命令的用途，以及如何組合使用它們來提升 AI 輔助開發的效率"
tags:
  - "clippings"
status: processed
---
## 前言

用 Claude Code、Cursor 這類 AI 輔助開發工具時，你可能聽過 OpenSpec 這個 spec-driven development 框架。最近 OpenSpec 發布了 v1.0 大版本更新，把原本 3 個命令的工作流程，擴展成 10 個更彈性的 OPSX 命令

這篇文章整理了每個新命令的用途和組合技巧

## 為什麼要改版？

舊版 OpenSpec 的工作流程是 **剛性的階段式**

```
PLANNING ────────► IMPLEMENTING ────────► DONE
         （無法回頭）
```

實際開發時常遇到需求要臨時調整的狀況，但舊版流程很難回頭修改

新版 OPSX 改成 **彈性的動作式**

```
proposal ──► specs ──► design ──► tasks ──► implement
    ▲           ▲          ▲                 
    └───────────┴──────────┘
              （隨時可以回頭）
```

## 新舊版命令對照

| 舊版 (Legacy) | 新版 (OPSX) 對應 |
| --- | --- |
| `/openspec:proposal` | `/opsx:new` + `/opsx:ff` |
| `/openspec:apply` | `/opsx:apply` |
| `/openspec:archive` | `/opsx:archive` |

如果你是從舊版升級，只需要執行 `openspec init` ，原有的 changes 和 specs 都會保留

---

## 完整命令介紹

### 1\. /opsx:new <change-name> — 建立新的 change

一切從這裡開始。執行後會在 `openspec/changes/` 下建立一個新的 change 資料夾

```
You: /opsx:new add-dark-mode

AI: Created openspec/changes/add-dark-mode/
    Ready to create: proposal
```

注意這個命令只建立資料夾，不會產生任何文件。後續要搭配 `/opsx:ff` 或 `/opsx:continue` 來產生規劃文件

---

### 2\. /opsx:ff — Fast-Forward，一次產生所有規劃文件

已經很清楚要做什麼、想快速進入實作階段的話，這個命令可以一次產生所有 artifacts

```
You: /opsx:ff

AI: Creating all planning artifacts...
    ✓ proposal.md — 為什麼要做、要改什麼
    ✓ specs/ — 需求和場景
    ✓ design.md — 技術方案
    ✓ tasks.md — 實作清單
    Ready for implementation!
```

產生的文件結構

```
openspec/changes/add-dark-mode/
├── proposal.md      # 變更提案
├── design.md        # 技術設計
├── tasks.md         # 任務清單
└── specs/           # Delta specs（這次變更影響的規格）
    └── ui/
        └── spec.md
```

---

### 3\. /opsx:continue — 逐步建立 artifact

想逐步審查每個階段的話， `/opsx:continue` 一次只建立一個 artifact

```
You: /opsx:continue

AI: Creating proposal...
    ✓ proposal.md
    Ready to create: specs, design
    Which would you like next?
```

AI 會根據 artifact 之間的依賴關係，判斷哪些是「可以建立」的狀態

需求還不太確定、想邊做邊調整，或是團隊協作需要分階段 review 時，這個命令就很實用

---

### 4\. /opsx:apply — 執行任務

規劃文件都準備好後， `/opsx:apply` 會開始實作 `tasks.md` 中的任務

```
You: /opsx:apply

AI: Working through tasks...
    ✓ 1.1 Add theme context provider
    ✓ 1.2 Create toggle component
    ✓ 2.1 Add CSS variables
    ✓ 2.2 Wire up localStorage
    All tasks complete!
```

同時有多個 changes 在進行的話，可以指定 change 名稱

```
You: /opsx:apply add-dark-mode
```

處理緊急 bug fix 時特別好用，暫時切到另一個 change 處理完，再切回來繼續

---

### 5\. /opsx:verify — 驗證實作是否符合 spec

檢查「規劃」和「實際實作」之間有沒有偏差（drift）

```
You: /opsx:verify

AI: Verifying add-dark-mode implementation...
    ✓ ThemeContext matches spec
    ✓ Toggle component implements all scenarios
    ⚠ localStorage key differs from spec (expected: 'theme', found: 'user-theme')

    1 issue found. Update spec or fix implementation?
```

實作完成後跑一次，可以確認有沒有遺漏需求。團隊 code review 前的自我檢查，或是 archive 前確保品質，都很適合用這個命令

---

### 6\. /opsx:sync — 預覽 spec 合併

archive 前想先看看 delta specs 會怎麼合併到主 specs，可以用這個命令預覽

這步驟是可選的， `/opsx:archive` 需要時也會提示你

---

### 7\. /opsx:archive — 歸檔已完成的 change

所有任務完成後， `/opsx:archive` 會將 change 歸檔

```
You: /opsx:archive

AI: Archiving add-dark-mode...
    ✓ Merged specs into openspec/specs/ui/spec.md
    ✓ Moved to openspec/changes/archive/2026-02-03-add-dark-mode/
    Done! Ready for the next feature.
```

這個命令會把 delta specs 合併到主 specs（ `openspec/specs/` ），然後將整個 change 資料夾移到 `openspec/changes/archive/` ，保留完整的變更歷史

---

### 8\. /opsx:bulk-archive — 批次歸檔多個 changes

累積了多個已完成的 changes？這個命令可以一次歸檔

```
You: /opsx:bulk-archive

AI: Found 3 completed changes:
    - add-dark-mode (8/8 tasks complete)
    - fix-login-bug (3/3 tasks complete)
    - update-footer (2/2 tasks complete)

    Checking for spec conflicts...
    ⚠ add-dark-mode and update-footer both touch specs/ui/

    Inspecting codebase to resolve...
    Both changes are implemented. Will merge in chronological order:
    - add-dark-mode (created Jan 20)
    - update-footer (created Jan 22)

    Archive all 3 changes?
```

AI 會自動檢測 spec 衝突，然後按建立時間順序合併

---

### 9\. /opsx:explore — 探索模式

比較特別的命令。還不確定要做什麼、需要先調查或釐清需求時，探索模式很好用

```
You: /opsx:explore

AI: What would you like to explore?

You: 我想改善頁面載入效能，但不確定瓶頸在哪

AI: Let me help investigate...
    [分析程式碼庫]

    我發現幾個可能的瓶頸：
    - 首頁載入了 12 個 API calls，但只有 3 個是 above-the-fold 需要的
    - bundle size 是 2.3MB，主要來自未 tree-shake 的 lodash
    - 圖片沒有 lazy loading

    你想先處理哪個問題？
```

**重要** ：探索模式會 **阻止實作** ，讓你專心思考和調查。準備好了再用 `/opsx:new` 建立正式的 change

像是效能優化要先找瓶頸、架構決策要評估多個方案、Debug 要先理解問題，或是需求還不明確時，都很適合先進入探索模式

---

### 10\. /opsx:onboard — 新手引導

第一次用 OpenSpec，或是要帶團隊成員入門，這個命令會走過完整的工作流程，會拿你真實的 codebase 來練習

```
You: /opsx:onboard

AI: Welcome to OpenSpec! I'll walk you through the complete workflow
    using your actual codebase.

    We'll:
    - Find something small to improve
    - Create a proper change for it
    - Implement it
    - Archive it

    This takes about 15 minutes. Ready to start?
```

---

## 常見工作流程組合

不同情境可以組合不同的命令

### 快速開發（需求明確、小功能）

```
/opsx:new → /opsx:ff → /opsx:apply → /opsx:archive
```

### 完整流程（需要驗證）

```
/opsx:new → /opsx:ff → /opsx:apply → /opsx:verify → /opsx:archive
```

### 探索式開發（需求不明確）

```
/opsx:explore → /opsx:new → /opsx:continue → ... → /opsx:apply
```

### 逐步審查（團隊協作）

```
/opsx:new → /opsx:continue → (review) → /opsx:continue → (review) → ...
```

---

## 小結

雖然看起來好像變的麻煩和複雜了，不過我覺得 OPSX 工作流程的重點是 `彈性`

| 特性 | 舊版 | 新版 OPSX |
| --- | --- | --- |
| 命令數量 | 3 個 | 10 個 |
| 工作流程 | 剛性階段 | 彈性動作 |
| 能否回頭修改 | 不行 | 可以 |
| 探索模式 | 無 | 有 |
| 批次歸檔 | 無 | 有 |
| 實作驗證 | 無 | 有 |

還沒試過 OpenSpec 新版本的的話，可以試試看

---

## 參考資源

- [OpenSpec GitHub](https://github.com/Fission-AI/OpenSpec)
- [Getting Started](https://github.com/Fission-AI/OpenSpec/blob/main/docs/getting-started.md)
- [Commands Reference](https://github.com/Fission-AI/OpenSpec/blob/main/docs/commands.md)
- [Workflows](https://github.com/Fission-AI/OpenSpec/blob/main/docs/workflows.md)

---

> 圖片來源：AI 產生