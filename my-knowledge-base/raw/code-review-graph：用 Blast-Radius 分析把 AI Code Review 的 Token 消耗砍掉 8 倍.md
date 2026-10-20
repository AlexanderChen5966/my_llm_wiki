---
title: "code-review-graph：用 Blast-Radius 分析把 AI Code Review 的 Token 消耗砍掉 8 倍"
source: "https://www.itnotetk.com/2026/04/20/code-review-graph%EF%BC%9A%E7%94%A8-blast-radius-%E5%88%86%E6%9E%90%E6%8A%8A-ai-code-review-%E7%9A%84-token-%E6%B6%88%E8%80%97%E7%A0%8D%E6%8E%89-8-%E5%80%8D/"
author:
  - "[[Tony.Wu's Blog]]"
published: 2026-04-20
created: 2026-07-20
description: "AI code review 最大的問題不是品質，是 token 浪費。當你讓 Claude 或 GPT-4 review 一個 PR，它為了理解上下文往往需要讀大量相關檔案，而這些檔案裡大部分內容跟..."
tags:
  - "clippings"
status: processed
---
AI code review 最大的問題不是品質，是 token 浪費。當你讓 Claude 或 GPT-4 review 一個 PR，它為了理解上下文往往需要讀大量相關檔案，而這些檔案裡大部分內容跟這次改動根本無關。 [code-review-graph](https://github.com/tirth8205/code-review-graph) 的解法是先建一張 codebase 的結構知識圖，review 時只給 AI 看「這個改動真正會影響到的部分」，平均省下 8.2 倍 token，大型 monorepo 最高能省 49 倍。

---

## 問題背景：AI Code Review 為什麼浪費 Token？

讓 AI review 一個改動 `auth.py` 的 PR，AI 為了回答「這個改動安不安全」，需要知道：  
\- `auth.py` 本身的改動  
\- 哪些 module 會呼叫 `auth.py` 的函數  
\- `auth.py` 依賴了哪些其他 module  
\- 測試覆蓋率是否足夠

傳統的做法是把整個相關目錄塞給 AI，或者用 `git diff` 加上大量的手動 context。問題是你很難精確判斷「哪些檔案是真正需要的」，通常是塞太多（浪費 token）或塞太少（review 不完整）。

---

## Blast-Radius Analysis 是什麼？

Blast-radius 是核爆術語，表示「一個爆炸能影響的範圍」。在軟體工程裡，一個代碼改動的 blast radius 是「這個改動可能影響到的所有模組和路徑」。

code-review-graph 的做法：  
1\. 預先分析整個 codebase，建立一張模組依賴圖  
2\. 當你改了某個檔案，系統計算這個改動的 blast radius  
3\. 只把 blast radius 內的節點（相關模組）傳給 AI

這樣 AI 拿到的是「精準的相關 context」，而不是「大量的可能相關 context」。

---

## 技術實現：AST → Graph → MCP

整個系統的技術棧選得很紮實：

### Tree-sitter：解析 23 種語言的 AST

Tree-sitter 是 GitHub 開發的增量式 parser，能在毫秒級別解析程式碼並生成 AST（Abstract Syntax Tree）。code-review-graph 用它來分析每個檔案的：  
\- 函數和類別定義  
\- import / require 依賴關係  
\- 函數呼叫圖（call graph）

支援的語言包括：Python、TypeScript、JavaScript、Go、Rust、Java、C/C++、Ruby、PHP、Swift、Kotlin、Scala、Haskell、Jupyter Notebook 等 23 種。

### SQLite：本地儲存知識圖

分析結果存進本地的 SQLite 資料庫，schema 大致是：

```sql
-- 節點：每個函數/類別/模組
nodes (id, name, file_path, type, line_start, line_end)
​
-- 邊：依賴關係
edges (from_node, to_node, edge_type)  -- edge_type: imports, calls, extends...
```

用 SQLite 的優點是本地查詢很快，不需要外部服務，graph traversal 用 recursive CTE 就能搞定。

### MCP：28 個工具讓 AI 查詢圖

系統提供 28 個 MCP tool，AI 可以用這些工具主動查詢 graph：

```
get_blast_radius(file_path)      → 返回這個檔案改動的影響範圍
get_callers(function_name)       → 誰呼叫了這個函數
get_dependencies(module)         → 這個模組依賴什麼
find_circular_deps()             → 找循環依賴
get_test_coverage(file_path)     → 對應的測試覆蓋情況
```

---

## 安裝和初始化

### 安裝

```bash
pip install code-review-graph
```

### 初始化 MCP 到 AI 工具

```bash
# 安裝 MCP server 到 Claude Code、Cursor、Windsurf（會自動偵測）
code-review-graph install
```

執行後會自動修改對應的設定檔，例如 Claude Code 的 `~/.claude.json` 。

### 建立 codebase 的知識圖

```bash
# 在你的 project 根目錄執行
code-review-graph build
​
# 輸出範例（大型 monorepo 可能需要幾分鐘）
# Analyzing Python files... 1,247 files
# Analyzing TypeScript files... 893 files
# Building dependency graph... 24,851 nodes, 89,234 edges
# Graph saved to .code-review-graph/graph.db
# Done in 87s
```

建完後的 graph 會存在 `.code-review-graph/` 目錄，加進 `.gitignore` 就好，不需要 commit。

### 自動更新

graph 不需要手動維護，有兩個自動觸發點：  
\- **File edit** ：你儲存檔案時，對應的節點自動更新（增量，通常毫秒級）  
\- **Git commit** ：commit 時重新計算 diff 影響的節點

---

## 與 AI 的整合工作流

有了 MCP server，在 Claude Code 裡的工作流是：

```bash
你：幫我 review 這個 PR，我改了 src/auth/token_validator.py
```

Claude 會自動：  
1\. 呼叫 `get_blast_radius("src/auth/token_validator.py")` 找出影響範圍  
2\. 用 `get_callers("validate_token")` 找哪些地方呼叫了這個函數  
3\. 讀取 blast radius 內的相關檔案  
4\. 給出精準的 review，包含「你的改動會影響到哪些呼叫端」

或者更直接：

```perl
你：Build the code review graph for my last commit
```

Claude 會自動 build graph 並開始 review。

---

## 支援語言清單

23 種語言，涵蓋主流技術棧：

| 類別 | 語言 |
| --- | --- |
| Web/Frontend | TypeScript, JavaScript, CSS |
| Backend | Python, Go, Rust, Java, Ruby, PHP, Elixir |
| Mobile | Swift, Kotlin, Dart |
| Systems | C, C++, Zig |
| JVM | Scala, Groovy, Clojure |
| ML/Data | Python (Jupyter.ipynb) |
| 其他 | Haskell, OCaml |

Jupyter Notebook 的支援尤其有趣，它能解析 `.ipynb` 裡的 cell code 並建立依賴圖，這在 data science 專案做 review 時很有用。

---

## 效能數據

官方的 benchmark 數據：

| 場景 | Token 節省 |
| --- | --- |
| 一般 Python 服務 | 8.2x |
| 中型 TypeScript monorepo | 15-20x |
| 大型 Java monorepo | 最高 49x |

節省這麼多的原因是大型 monorepo 的大部分 code 跟某個具體改動完全不相干，但傳統的 context stuffing 做法還是會把它們包進去。

在 HomeLab 測試一個約 50 個 Python 檔案的服務，改動一個 utility function，blast radius 分析只拉出 8 個相關檔案，而不是整個 `src/` 目錄的 50 個。

---

## 個人觀點

這個工具的最大價值，是把 AI code review 從「整體掃描」改成「精準定位」。

以前的做法是你要手動判斷哪些 context 是必要的，往往要在「給太少（AI 不夠 context）」和「給太多（浪費 token）」之間拿捏。code-review-graph 把這個判斷自動化了：它用 static analysis 先做一次完整的依賴分析，review 時只拿需要的部分。

對大型 monorepo 的團隊來說，這不只是省 token 的問題，更是讓 AI review 變得「可用」的問題。如果每次 review 要塞幾十萬 token 才夠 context，成本和延遲都會讓人卻步。有了 blast radius 分析，review 一個普通的 PR 可以控制在幾千 token，速度和成本都在合理範圍。

唯一要注意的是初始 build 的時間，大型 monorepo 可能要幾分鐘。但這是一次性成本，後續的增量更新很快。