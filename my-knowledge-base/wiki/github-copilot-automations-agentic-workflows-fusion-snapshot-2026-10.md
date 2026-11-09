---
title: GitHub Copilot 2026-10-28 更新：共享 Automations、Agentic Workflows、Fusion 與 Projects（快照）
date: 2026-11-05
tags: [ai/agent, ai/workflow, dev/ci-cd]
source: https://www.aiposthub.com/github-shared-copilot-automations-agentic-workflows-ga/；https://www.aiposthub.com/github-copilot-fusion-projects-routing-cross-repo-cost/（皆 aiposthub／Philo，官方 changelog 2026-10-28 的二手整理）
type: article
status: processed
asset_value: 27
---

> 註：asset_value 27（頻率 3×耗時 3×複雜度 3），**保守估 18（耗時 2）**，屬邊界；兩篇單篇皆 18，合併才成條目。
> 🔴 **來源性質**：兩篇皆 aiposthub（作者 Philo）**單一作者的二手整理，站方皆未實測**；一手來源（GitHub changelog、Artificial Analysis 貼文）**未核對**。文中「建議」多為作者補充，非產品行為。
> ⏰ **快照 2026-10-29**：方案資格與推出狀態逐步推出、時間不一，會變；Fusion 折扣只到 2026-11-30。
> 邊界：raw 另有「Copilot 吃到飽終結」（計價背景）**尚未處理，本條未引用其內容**，之後處理時併入本條。
> 已排除：Copilot App 電腦操作預覽與影像生成等同日周邊、訂閱導流、站內延伸閱讀。

## T（核心論點）

這次更新是兩條互不取代的路：**共享 Copilot Automations**（Copilot 雲端工作，歸程式庫擁有）與 **Agentic Workflows**（把 Markdown 編譯成 Actions，走你既有的 CI 管道）；另有 **Fusion**（多模型協調）與 **Projects**（跨程式庫任務）。選哪個，先看**誰擁有、誰付額度、要不要走 Actions**，而不是看功能名稱。第三方評測數字未驗證，別當選型依據。

## B（適用場景）

使用 GitHub Copilot 做定期或事件觸發的 agent 工作、或考慮用 Markdown 寫 CI agent 流程的團隊。公司已用 Copilot CLI 的場景（參見 [[jenkins-github-copilot-cli-pr-review]]）可對照。

## R（關鍵細節）

### 1. 共享 Copilot Automations vs Agentic Workflows

| 項目 | 共享 Copilot Automations | Agentic Workflows |
| --- | --- | --- |
| 本質 | Copilot 雲端工作 | **Markdown 編譯成 Actions YAML** |
| 歸屬 | **歸程式庫擁有**；讀權限可看、寫權限可編輯 | 在程式庫的 Actions 內 |
| 額度 | **AI 額度記程式庫擁有者**；組織庫需管理者啟用付費政策 | 預設 Copilot CLI 引擎**仍需 Copilot 授權** |
| 限制 | 建立時選 **Shared，建立後類型不可更改** | 已 **GA**，對 Free／Team／Enterprise 開放 |
| 推出 | **逐步推出**（Pro／Pro+／Max／Business／Enterprise，快照 10-29） | 支援自架 ARC、Docker-in-Docker、模型路由 |
| 安全預設 | — | 預設唯讀、隔離容器、防火牆、安全輸出與威脅偵測（對照 [[agent-permission-and-os-sandbox]]） |

- `gh aw forecast` 預算預測轉為正式功能
- 費用文章只有質性描述，**無數字**

### 2. Copilot Fusion（已 GA）

- 自動選模與協調；使用者選 **Standard／High／Max** 投入程度；管理者可限制可選模型
- 執行方式：**Single**、**Cascade**（高效模型先做、品質檢查後升級）、**Critique**（起草→審查→修改）
- 🔴 **Artificial Analysis 評測數字為第三方 X 貼文經 aiposthub 二手轉述，未驗證**（Copilot CLI、Fusion Max、Coding Agent Index v2.0）：綜合 60 分、**比 Claude Code＋Opus 5.5 Max 低 2 分**；平均每題 US$5.59 對 US$11.42；「終端機任務領先、其他測驗落後」同為二手。**成本是測驗口徑，不是帳單**
- 折扣：付費方案 Fusion 模型成本 **30% 折扣只到 2026-11-30**
- 支援 VS Code、Copilot App、CLI、JetBrains

### 3. Projects（公開預覽）

- 可跨多程式庫，並納入設計稿、Slack、會議筆記
- 🔴 **公開預覽**，行為可能變動

## C（關鍵行動）

- [ ] 先決定：要的是「Copilot 雲端自動工作」還是「在 Actions 內跑的 agent 流程」，再看歸屬與額度
- [ ] 試 Agentic Workflows 先用唯讀預設、跑 `gh aw forecast` 看預算
- [ ] 引用 60 分、2 分差、5.59／11.42 美元時保留「第三方經二手轉述、未驗證」標註
- [ ] 2026-11-30 前折扣只當限時資訊；處理「Copilot 吃到飽終結」時回來補計價背景

## 原始摘要

兩篇 aiposthub 對 GitHub 2026-10-28 changelog 的整理：共享 Copilot Automations（歸程式庫、額度記擁有者、逐步推出）與 Agentic Workflows（Markdown 編譯成 Actions、GA）的比較；Copilot Fusion GA（三種投入程度、Single／Cascade／Critique）與引用的 Artificial Analysis 數字；Projects 公開預覽與限時折扣。

## 相關連結

- [[github-copilot-token-reduction]] — 用量與 token 面
- [[github-copilot-context-design-strategy]] — Copilot 的脈絡設計
- [[jenkins-github-copilot-cli-pr-review]] — Copilot CLI 在 CI 的實例
- [[four-vendor-ai-tool-selection]] — 多廠商工具選型
- [[agent-permission-and-os-sandbox]] — 唯讀預設與隔離的通則
