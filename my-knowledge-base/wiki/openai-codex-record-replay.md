---
title: OpenAI Codex Record and Replay：示範一次自動生成可重用 Skill
date: 2026-06-23
tags: [ai/agent, ai/claude, dev/workflow]
source: https://www.koc.com.tw/archives/646598 | https://www.bnext.com.tw/article/91292/openai-codex-record-and-replay-skills-automation-youtube-and-workflows | https://www.koc.com.tw/archives/646263
type: note
status: processed
---

## T（主題脈絡）

2026-06-18，OpenAI 在 Codex 應用程式（Mac）正式推出「Record and Replay」功能。傳統 AI 自動化需要用文字寫出每一個步驟的 prompt；Record and Replay 改為「示範」模式：使用者操作一次，Codex 觀察、理解意圖、自動生成可編輯的 Skill。同月，Codex CLI 開放支援所有第三方 AI 模型，不再限定 OpenAI 自家模型。

## B（核心主張）

Record and Replay 的本質是**把隱性的重複流程變成顯性的可重用 Skill**。它不是巨集錄製（像素軌跡），而是 Computer Use 模型理解操作「意圖」後生成的結構化指令文件（SKILL.md），輸出結果使用者可以直接檢視和修改。

## R（關鍵洞察）

### 運作流程

1. 在 Codex 開啟 Plugins → 選「Record a skill」
2. Codex 請求錄製權限後，使用者**正常**操作目標流程（例如 YouTube 影片上傳：抓 metadata → 上傳素材 → 填設定 → 存為非公開）
3. 說「我做完了」或點停止鍵結束錄製
4. Codex 分析錄影，自動生成 SKILL.md，包含：使用時機、所需輸入、執行步驟、驗證方式

**關鍵差異**：Codex 理解的是「從試算表抓標題」，不是「點擊座標 (x, y)」。生成的 Skill 有泛化能力，下次只要換不同的影片素材包丟進去，它就知道對應到試算表的哪一列。

### 技術架構：agentskills.io 開放標準

Record and Replay 底層採用 Agent Skills 開放標準（agentskills.io）。一個 Skill 是一個資料夾：

```
my-skill/
  SKILL.md         # 必備：名稱、描述、指令
  scripts/         # 可選：可執行腳本
  references/      # 可選：參考文件
  assets/          # 可選：模板與資源
```

**漸進式披露（Progressive Disclosure）**：Codex 啟動時只載入每個 Skill 的名稱 + 描述 + 路徑（佔模型 context 視窗約 2%），只有決定使用某個 Skill 時才載入完整 SKILL.md。安裝幾十個 Skill 也不會爆 context。

**觸發方式**：
- 明確調用：prompt 中直接指定 Skill 名稱
- 隱式調用：Codex 根據任務自動匹配 description（Record and Replay 生成的 Skill 走這條路）

### Codex CLI 多模型支援

同期更新：Codex CLI 透過 `config.toml` 可接入任何相容 OpenAI Responses API 的服務：
- DeepSeek、Claude、Gemini、Ollama 本地模型均可
- 意義：Codex 的 Computer Use + Record and Replay 能力，可搭配更便宜或特定優勢的模型執行

### 與 SKILL.md 生態的關係

Record and Replay 讓**不會寫 SKILL.md 的人**也能創建 Skill——直接示範即可。生成的 Skill 符合標準格式，可以在 Claude Code、Kiro 等支援 agentskills.io 的工具間互用。這降低了 Skill 生態的製作門檻。

## C（連結與延伸）

- [[skill-md-9-types-and-gotchas]] — SKILL.md 九大類型與最高信號段落方法論
- [[skill-md-writing-techniques]] — SKILL.md 撰寫技巧（description 模板 / ≤500 行 / context fork 隔離）
- [[ai-agent-skills-popular-2026]] — 2026 年最受歡迎的 Agent Skills 生態現況
- [[codex-plugin-workflow]] — Codex Plugin 可重用 AI 工作流模組架構
- [[synthesis-ai-dev-workflow-frameworks]] — Superpowers / GStack / spec-driven 工作流框架比較
