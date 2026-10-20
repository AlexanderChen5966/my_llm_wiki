---
title: opencodex：Codex 通用供應商代理與 ChatGPT 帳號池 affinity 設計
date: 2026-08-25
tags: [ai/workflow, ai/claude, type/how-to]
source: https://github.com/lidge-jun/opencodex/blob/main/readme/README.zh-TW.md
type: reference
status: processed
asset_value: 60
---

## T（核心論點）

opencodex 解的不是「換 provider」而是「換大腦」——它把 Codex 的 Responses API 翻譯成各家 provider 的協議，讓 Codex CLI/App/SDK 與 Claude Code 都能跑任意 LLM；而它真正少見的設計是 **ChatGPT 帳號池的 affinity 規則：既有執行緒釘在原帳號、只有新會話才路由到使用量最低的健康帳號**，因為長時間的 SSH／tmux／行動連線一旦在對話中途換帳號，等於把工作階段斷在半路。

## B（適用場景）

**適用：**
- 手上有多個 ChatGPT / Codex 帳號，想按配額自動分流又不希望長會話中途斷裂
- 想在 Codex 裡用 Claude / Gemini / Grok / GLM / DeepSeek / Kimi / Qwen / Ollama，不必等官方支援
- 反向需求：想讓 Claude Code 跑非 Anthropic 模型（`ocx claude` 透過代理啟動）
- 想把不同難度的任務委派給不同模型（subagent 選擇器最多放 5 個路由/原生模型）

**不適用 / 注意：**
- **與 [[omniroute-local-ai-gateway]] 定位不同**：OmniRoute 是通用 OpenAI 相容 gateway，賣點是四層 quota fallback 與壓縮管線；opencodex 專攻 **Codex 的 Responses API 翻譯 + ChatGPT 帳號池**。兩者重疊在「本機代理、多 provider」，但帳號池 affinity 與 Codex 原生注入是 opencodex 獨有
- 🔴 **已知限制**：v2 多智慧體表面（GPT-5.6 Sol/Terra）下，原生父代理 spawn 路由子代理時，任務本文可能以後端加密形式抵達而丟失（issue #92）——需要可靠的跨 provider 委派請改用 v1 表面
- 帳號池涉及多個 ChatGPT 帳號共用，使用前需自行確認是否符合服務條款
- `gpt-5.6-*` 系列可用性仍受上游 preview gate 限制，opencodex 只是先備好路由與目錄後設資料
- 安裝依賴打包的 Bun 執行時；若用 `--ignore-scripts` / `--omit=optional` 會出現 "bundled Bun runtime is missing"

## R（操作步驟）

1. 安裝（需 Node 18+，Bun 自動打包）：`npm install -g @bitkyc08/opencodex`
   - 建議用自己的 Node（nvm/fnm），避免 `sudo npm install -g`
   - 若被 npm 攔截腳本：`npm install -g --allow-scripts=bun @bitkyc08/opencodex`（**務必寫全套件名**，npm 警告給的縮寫指令會把當前目錄重裝進去）
2. 初始化並啟動：`ocx init`（寫入設定並注入 Codex）→ `ocx start`（代理 + 儀表板在 `localhost:10100`）
3. 加 provider：`ocx gui` 開儀表板 → Add Provider → 從 40+ 內建選或填自訂 OpenAI 相容端點 → 貼 API key（Anthropic / xAI / Kimi 可 OAuth 登入）→ 模型自 `/v1/models` 自動發現，免重啟
4. 指定模型用 `provider/model` 格式：`codex -m "anthropic/claude-opus-5" "解釋這個 stack trace"`、`codex -m "ollama/llama3" "重構這個函式"`；省略前綴時依預設 provider 或名稱模式匹配（`claude-*` → Anthropic）
5. 讓 Claude Code 也走代理：`ocx claude`；Claude 儀表板可管理 Opus / Fable / Sonnet / Haiku 四系列的 Desktop 配置
6. 設帳號池：儀表板 → Codex Auth 新增帳號 → 開啟自動切換。系統比較 5 小時 / 每週 / 30 天中最熱的配額視窗，越過閾值時為**新會話**挑使用量更低的合格帳號
7. 收工：`ocx stop` 關代理、停背景服務、並把 Codex 還原成原始設定（無殘留設定、無殭屍程序）

## C（關鍵行動）

- [ ] 確認 affinity 行為符合預期：**現有執行緒不會換帳號**，只有新會話才路由——若你期待長流程中途自動換帳號續命，這個工具刻意不做
- [ ] 記住失敗語意是 **fail-closed**：token 失敗會標記「需重新認證」而不是悄悄回退到另一組憑證；429 讓該帳號進冷卻並可 failover。這與「靜默降級」相反，是刻意的設計
- [ ] 跨 provider 委派前先確認用的是 v1 還是 v2 表面（issue #92 的任務本文丟失只發生在 v2）

## 原始摘要

lidge-jun/opencodex 是輕量本機代理，把 Codex 的 Responses API 翻譯成目標 provider 的協議，streaming、tool 呼叫、reasoning token 與圖片皆雙向工作。

**協議 adapter（5 種）**：Anthropic Messages、Google Gemini、Azure、OpenAI Responses 直通，以及所有 OpenAI 相容 Chat Completions 端點 → 開箱即用 40+ provider。

**帳號池行為**：既有執行緒 id 綁定所選帳號並在後續輪次複用（affinity）；新會話比較 5h／每週／30d 最熱配額視窗自動挑低使用量帳號；儀表板可一鍵重新整理所有帳號配額，請求日誌以非 PII 序號標記池流量；失敗 fail-closed。`openai`（Codex 登入，預設 Pool，可切 Direct）與 `openai-apikey`（API key，不做帳號路由）兩種模式**不互相 fallback**。

**其他能力**：subagent 選擇器可放最多 5 個模型並注入委派指引（`injectionModel` / `injectionEffort` / `fork_turns`，文案可用 `{{model}}`/`{{effort}}`/`{{roster}}` 佔位）；非 OpenAI 模型可透過 `gpt-5.4-mini` sidecar 取得真正的網頁搜尋與圖片理解；`image_gen` 走 `POST /v1/images/generations`；reasoning effort 保留 `low`/`medium`/`high`/`xhigh`/`max`/`ultra` 階梯，其中 `ultra` 實際以 `max` 送出且僅在 provider config 顯式開啟時宣告。

**平台**：macOS（launchd）／Linux（systemd 使用者層級）／Windows（Task Scheduler，免 WSL）皆完整支援，可安裝為開機自啟的系統服務。

**版本注記**：OpenAI provider 目前 marker 為 `openaiProviderTierVersion: 2`，原設定備份於 `~/.opencodex/config.json.pre-openai-tiers-v2.bak`；舊 v1 三供應商設定會自動遷移為單一 `openai` 行。API 層 GPT-5.6 後設資料為 1,050,000 context / 922,000 max input。

## 相關連結

- [[omniroute-local-ai-gateway]] — 同為本機代理，但走通用 gateway + quota fallback + 壓縮管線路線；opencodex 專攻 Codex 協議翻譯與帳號池 affinity
- [[opensquilla-intelligent-model-router]]
- [[openrouter-fusion]]
- [[research/codex-vs-claude-code]]
