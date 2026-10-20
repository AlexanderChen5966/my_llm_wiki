---
title: 從 Vibe Coding 到 Agentic Engineering：前端開發者的系統升級路徑
date: 2026-06-17
updated: 2026-08-03
tags: [ai/agent, ai/claude]
source: https://ithelp.ithome.com.tw/articles/10400104
type: article
status: processed
asset_value: 55
---

Vibe Coding（Accept All 快感）在 demo 有效，在長期維護的真實 repo 很快失效，因為它讓 agent 在沒有規則的情況下高速往錯誤方向跑。Agentic Engineering 的本質是補回開發基本功：**先規劃、再執行、最後驗證**（PEV Loop），並把所有默契寫成檔案讓機器看得懂。

## T（核心論點）

**2026 年拉開差距的不是誰比較會下 prompt，而是誰能把 repo 整理成 agent 進來也不會迷路的系統。** 問題不是模型不夠聰明，而是專案沒有把規則寫給機器看——人類同事可以靠經驗補洞，agent 不會。

## B（適用場景）

- 正在從 Vibe Coding 轉型為更系統化 AI 輔助開發的前端工程師
- 管理有歷史包袱的真實 repo（不只是全新 demo 專案）
- 需要決策 Cursor vs Claude Code 使用場景的開發者

## R（完整內容）

### PEV Loop

所有 Agentic Engineering 的底層邏輯：

```
Plan（規劃）→ Execute（執行）→ Verify（驗證）
     ↑__________________________|
```

**許多團隊只做了中間那一格**（丟 prompt 就算 Execute，沒有真正規劃和驗證）。

真正的規劃需要回答：
- 這次修改可以碰哪些目錄，哪些地方不能碰？
- 成功條件是畫面看起來差不多，還是 lint / typecheck / 測試都要過？
- 如果 agent 產生了新的抽象，誰要負責把它跟現有架構對齊？

### AGENTS.md 的意義

Next.js 16.2 的決策：`create-next-app` 直接產生 `AGENTS.md`，框架承認「如果你不主動餵 agent 正確的規則，它就會拿自己的模糊記憶亂補」。

**AGENTS.md 最少要包含：**
- 專案目錄的責任分界
- 常用命令和驗證順序
- 哪些區域不能讓 agent 自作主張重寫
- UI、API、資料層各自的約束

重點不在檔名，而在「有一份給機器讀的操作說明」的態度。

**Next.js 16.2 的務實設計：** 把瀏覽器 log 往終端機送，讓 agent debug 時能接觸到真實執行期訊號，而非只靠靜態檔案和猜測。

### Cursor vs Claude Code：位置不同，不是輸贏

| | Cursor | Claude Code |
|--|--------|-------------|
| 工作位置 | 編輯器內 | Terminal |
| 適合場景 | 局部調整、互動微調、畫面驗證 | 全 repo 修改、重構、批次驗證 |
| 人的角色 | 一直在迴圈裡即時判斷 | 委派任務、設定邊界 |
| 比喻 | 陪打 | 委派 |

**成熟工作流是分工，不是站隊：**
- 編輯器：局部探索、細修、畫面確認
- 終端機：全專案修改、重構、批次驗證

### 三件事從 Vibe Coding 升級

**1. 把默契寫成檔案**  
把目錄結構、命名習慣、資料流方向、禁止事項寫進 repo。不再靠資深工程師的腦袋承擔這些規則。

**2. 把驗證變成預設**  
至少把 lint、typecheck、關鍵測試、基本畫面檢查變成標配。是在防自己被速度沖昏頭，不是在防模型。

**3. 把工作切成適合委派的單位**  
告訴 agent 要改哪裡、要跑哪些命令、交付什麼結果。任務越乾淨，模型差異才不會被放大成災難。

### Vibe Coding 的正確定位

Vibe Coding 沒有消失——它**適合探索階段**（開腦洞、做原型、試 UI）。一旦有真實使用者 / 資料 / 上線壓力，開發方法就不能停在「看起來差不多可以」。

## C（關鍵行動）

- [ ] 建立或更新 AGENTS.md，明確目錄責任分界與驗證順序
- [ ] 定義哪些目錄禁止 agent 自作主張修改
- [ ] 將 lint + typecheck 加入 agent 完成後的驗收條件
- [ ] 明確區分哪些任務用 Cursor（局部微調），哪些用 Claude Code（全 repo 任務）

## 補充資料：一個第一人稱的失敗案例（2026-08-03 併入）

來源：〈Vibe Coding：終局〉（dev.to，konark_13）。⚠️ 本文核心論點（規格先行、PEV Loop）與本條目及 [[spec-driven-ai-workflow]] 概念重疊，只收其**獨有的敘事案例**與一句話金句。

### 具體失敗案例：「壞掉導覽列的多重宇宙」

> 你叫 AI 改一個按鈕，它把導覽列改了。你請它修導覽列，結果驗證功能壞了。你修驗證功能，半數樣式又不見了。到這時候，你的聊天紀錄看起來不像軟體開發，反而像一場情侶諮商：「我只叫你改一件事。」「我知道，但我以為這樣會比較好。」

作者回頭檢查自己的提示詞，發現問題不在 AI：
> 「幫我做一個專案管理應用程式。」——就這樣。**沒有需求。沒有架構。沒有約束。只有感覺。**

### 一句話結論

> **終局不是 Claude，不是 Gemini，不是 Codex，不是更好的提示詞，甚至也不是 AI。終局是：在叫 AI 幫我思考之前，先學會思考。**

作者明確主張**規格驅動不是取代 vibe coding**，而是替它指明方向；vibe coding 仍是探索點子、做原型最快的方法，**改變的是期待**——不再期待 AI 從一句話神奇理解一切。

## 相關連結

- [[agents-md-guide]] — AGENTS.md 完整指南（效果研究 +4% vs -3%）
- [[synthesis/synthesis-agentic-engineering]] — Agentic Engineering 哲學與實踐知識精粹（本主題 5 篇整合）
- [[ai-architecture-survival-guide-non-architect]] — 同主題基層操作視角：本篇談角色升級的系統框架，該篇談不懂架構時的六個生存習慣與五個警訊
- [[agentic-engineering-mindset]] — Agentic Engineering 哲學觀（Karpathy 鬼魂比喻）
- [[agentic-engineering-sop]] — 六階段實戰 SOP（Plan/Tasks 三層落檔）
- [[claude-agents-skill-file-comparison]] — SKILL.md vs CLAUDE.md vs AGENTS.md 三檔分層策略
- [[vibe-vs-agentic-coding-maturity]] — Vibe/Agentic 不是二選一（產品成熟度決定工具）
