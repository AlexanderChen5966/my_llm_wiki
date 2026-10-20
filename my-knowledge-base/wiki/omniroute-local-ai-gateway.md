---
title: OmniRoute：本機優先 AI Gateway 的四層 quota fallback 與壓縮管線
date: 2026-08-25
tags: [ai/workflow, ai/claude, type/how-to]
source: https://github.com/diegosouzapw/OmniRoute
type: reference
status: processed
asset_value: 64
---

## T（核心論點）

與其在額度用完時手動換工具，不如把「換 provider」這件事下沉到本機的一個 OpenAI 相容端點——OmniRoute 用四層 quota-aware fallback（Subscription → API Key → Cheap → Free）加上可組合的壓縮管線，讓 Claude Code 這類代理型工具在 rate limit 之下不中斷，且改動只有一個 base URL。

## B（適用場景）

**適用：**
- Claude Code / Codex / Cursor / Cline / Copilot 等 OpenAI 相容工具，經常在長流程中途撞到 quota 或 rate limit
- 手上散著多家免費額度（Cerebras、Cloudflare AI、Z.AI GLM、OpenCode Zen…）卻無法統一調度與稽核
- 想在不改 client 的前提下壓低工具輸出（shell / test / build / git log）吃掉的 token
- 需要本機自持：prompt 不經第三方雲端、金鑰以 AES-256-GCM 加密存本機、telemetry 預設關閉

**不適用 / 注意：**
- 它是 gateway 而非 provider——prompt 仍會送到當次選中的上游供應商，local-first 指的是「控制面在本機、不多一跳託管處理」，不是不外送
- 「~15.1 億免費 token/月」是專案自家目錄以 pool 去重後算出的數字，官方明講每兩週重稽核且會雙向變動；不要當成穩定容量規劃
- 壓縮是有損的（Caveman 為規則式散文壓縮），雖然 code block / URL / JSON 宣稱位元組級保留，但正式產出前仍應自行驗證
- 免費 provider 的模型品質與延遲差異大，`auto` 自動挑選可能在不知情下換掉模型——需要輸出穩定性的場合請指定明確 model ID
- 版本迭代極快（本篇對應 v3.8.51），provider 數與功能表隨版本變動

## R（操作步驟）

1. 安裝並啟動：`npm install -g omniroute` 後直接執行 `omniroute`；dashboard 在 `http://localhost:20128`，API 在 `http://localhost:20128/v1`。零設定即可用 `{"model":"auto"}` 打（預接 OpenCode Free、Felo 等免金鑰 provider）。
2. 接第一個免費 provider：Dashboard → Providers → 連 Kiro AI（免費 Claude，約 50 credits/月/帳號）或 OpenCode Free（免驗證）。
3. 把工具指過來：Base URL 填 `http://localhost:20128/v1`，API Key 從 Dashboard → Endpoints 複製。或用一行啟動既有 CLI：
   - `omniroute run claude --model openai/gpt-5.4`
   - `omniroute run codex --model glm/glm-5.2`
   - `omniroute configure codex`（互動挑 provider+model，直接寫入該工具自己的設定檔；另支援 claude / opencode / qwen / aider / goose / cline / continue / kilo）
4. 選 routing 行為：直接把 `auto` 變體當 model ID 用——`auto/coding`（品質優先）、`auto/fast`（低延遲）、`auto/cheap`（低成本）、`auto/offline`（額度餘裕最多）、`auto/smart`（品質優先 + 10% 探索）。要自建就從 19 種 strategy 挑（`priority`、`cost-optimized`、`headroom`、`reset-aware`、`cache-optimized`、`lkgp`、`fusion`、`pipeline`…），Auto-Combo 引擎以 15 個因子即時計分。
5. 開壓縮：預設 stacked 管線是 `RTK → Caveman`。合成公式 `combined = 1 − (1 − RTK) × (1 − Caveman_input)`，平均 89.2%、區間 78.4–94.6%。壓縮決策優先序（高→低）：per-request `x-omniroute-compression` header › routing-combo override › 具名 profile › adaptive/auto-trigger › panel 預設 › off；實際套用的計畫會回寫在 `X-OmniRoute-Compression` response header。
6.（選用）讓 agent 自己操作 gateway：`claude mcp add-server omniroute --type http --url http://localhost:20128/api/mcp/stream`，remote MCP 提供 110 個 tool / 33 個 scope 與完整 audit trail；另有 A2A（`/.well-known/agent.json`）。
7.（選用）遠端模式：`omniroute connect <host>` 取得 scoped token，之後同一套 CLI 全部打遠端；token 分 `read` / `write` / `admin`，會生程序的路由僅限 loopback。

## C（關鍵行動）

- [ ] 先確認「本機」的邊界：控制面與金鑰在本機，但 prompt 仍送往當次選中的上游 provider——敏感專案要先篩掉不可接受的 provider 再開 `auto`
- [ ] 壓縮先用 `Lite`（~15%）或 `Standard`（~30%）跑一段時間，確認輸出品質再往 `Stacked`（78–95%）推；有 `npm run eval:compression` 離線評分管線可先驗證再上線
- [ ] 免費額度數字（455 筆 free-tier 條目 / 40 個 recurring pool / ~1.51B tokens/月）當作參考而非承諾，provider 隨時可改額度、資格與地區

## 原始摘要

OmniRoute（diegosouzapw/OmniRoute，MIT）是本機優先的 AI gateway，把 **353 個註冊 provider**（其中 154 個帶 `hasFree: true`）、**268 provider / 2,566 provider-model 配對 / 1,312 個 raw model ID** 收斂到單一 OpenAI 相容端點 `http://localhost:20128/v1`。

版本線：v3.8.49 → v3.8.50 provider 從 290 增至 350、chat model ID 從 1185 增至 1312，並新增 Modality Bridge（vision/audio/video）、Radar 免費目錄（opt-in）、Quota-Share 排程與即時 quota telemetry；本篇對應 v3.8.51，目標 v3.9.0 LTS。

**四層 fallback**：Tier 1 Subscription → Tier 2 API Key → Tier 3 Cheap → Tier 4 Free，只要還有 eligible healthy target 就往下遞補；另有三層獨立韌性機制（circuit breaker、key cooldown、model lockout）。

**壓縮**：12 個可組合引擎依序執行——Session-Dedup、CCR、Lite、RTK、Responses Tool Output、Headroom（GCF codec，JSON array 無損表格化約 30%）、Relevance、Caveman（規則式散文壓縮，輸出端約 65–75%）、Aggressive、LLMLingua-2（MobileBERT ONNX）、Ultra、OmniGlyph（實驗性 context-as-image）。預設組合 `RTK → Caveman`。分級：Lite ~15%、Standard(Caveman) ~30%、Aggressive ~50%、Ultra ~75%、RTK 60–90%（shell/test/build/git 輸出）、Stacked 78–95%。另有 Output Styles（Terse prose / Less code / Terse CJK 文言）與 adaptive context-budget 調節（policy：`reserve-output` 預設 / `percentage` / `absolute`）。

**免費額度目錄**：455 筆 per-model 條目、40 個 recurring pool、56 個 recurring/keyless free-forever provider；頭條數字 ~1.51B tokens/月 由其中 20 個有正值月預算的 pool 去重後算出（首月加上簽約 credit 可達 ~2.13B），每兩週重稽核。

**部署面**：npm 全域安裝、Docker（AMD64+ARM64）、Electron 桌面（Win/macOS/Linux）、ARM 原生、Android Termux（免 root）、PWA、OpenCode plugin（`@omniroute/opencode-provider`）、VS Code 的 OmniCopilot 擴充（VS Code 1.122 起免 GitHub 登入即可在原生 Copilot Chat model picker 選用）。相容 35 個工具（26 CLI Code + 9 CLI Agent）。

**安全與隱私**：AES-256-GCM 金鑰靜態加密、API-key scoping、IP filtering、rate limit、每條 LLM 路由的 prompt-injection guard（附 red-team suite）、opt-in 憑證遮罩與 PII redaction、upstream header 清洗、sanitized error、本機 SQLite audit trail、telemetry 預設關閉。技術棧為 better-sqlite3（WAL）+ LowDB、SQLite FTS5 + int8 量化向量記憶、OAuth 2.0 (PKCE) + JWT + API Keys。

## 相關連結

- [[opencodex-codex-provider-proxy]] — 同為本機代理的另一種切法：專攻 Codex Responses API 翻譯與 **ChatGPT 帳號池 affinity**（既有執行緒不換帳號、只有新會話才路由），沒有 OmniRoute 的壓縮管線；兩者可對照選型
- [[opensquilla-intelligent-model-router]]
- [[openrouter-fusion]]
- [[llm-cost-optimization-strategies]]
- [[headroom-context-compression]]
