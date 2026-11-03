---
title: Impeccable Style：AI Coding Agent 程式碼風格規範指南
date: 2026-05-20
tags: [ai/agent, ai/claude]
source:
  - https://tools.wingzero.tw/article/sn/3915
  - https://codelove.tw/@tony/post/qJRgrq （2026-10-29 快照；Qiita 原文 jinxin4869 的中譯）
type: article
status: processed
asset_value: 12
---

Impeccable Style 是一套專為 AI coding agent 設計的程式碼風格規範網站，核心定位是「可直接給 AI 使用的 coding style guide」。目標不是讓 AI 能寫程式，而是讓 AI「寫出團隊真正願意接受的程式」。

## T（核心論點）

AI coding agent 時代，約束 AI 往往比呼叫 AI 更重要。傳統 style guide 是給人類讀的，Impeccable Style 把工程規範轉換成 AI 更容易理解與遵循的形式，本質是 machine-readable engineering culture。

## B（適用場景）

**適用：** AI 深度參與的開發團隊；長期維護的大型 codebase；使用 Claude Code / Cursor 的工程師；希望建立 AI coding 標準的公司；多人與多 agent 同時協作的場景
**不適用 / 注意：** 一次性小型專案（規範收益不明顯）；與 lint 工具（ESLint/Prettier）定位不同，是哲學層級的規範而非格式化工具

## R（操作步驟）

1. 從 Impeccable Style 網站複製適合的 style 規範內容
2. 放入專案的 AI 規則文件中（`CLAUDE.md`、`AGENTS.md`、`.cursor/rules/`、`SYSTEM_PROMPT.md`）
3. 在 Claude Code 或 Cursor 中啟用 always-on rules
4. 讓 AI 在生成與修改程式碼時自動套用規範
5. （選用）搭配 Cursor 時可拆分為 scoped rules，依資料夾套用不同規則

## C（關鍵行動）

- [ ] 把 Impeccable Style 規範整合進現有 CLAUDE.md 或 AGENTS.md
- [ ] 與 Skills（任務）、Rules（邊界）的三層架構搭配：Style 定義 AI 應該怎麼寫

## 原始摘要

Impeccable Style 涵蓋命名規範、架構偏好、工程原則，強調簡潔與可維護性，降低 AI 過度設計問題。與 Agent Rules Books、Andrej Karpathy Skills 形成互補關係：Skills 定義 AI 如何完成任務、Rules 定義 AI 能做什麼、Style 定義 AI 應該怎麼寫。

## 相關連結

- [[claude-md-guide]] — CLAUDE.md 撰寫技巧，放置 Style 規範的主要載體
- [[agents-md-guide]] — AGENTS.md 作為 Style 規範的替代載體
- [[claude-skills-ecosystem]] — Skills 在 AI 開發工作流中的位置
- 📎 2026-10-30：[[taste-skill-anti-slop-frontend]] — 同為去 AI 味的前端設計 Skill；Impeccable 主打「流程」（問產品事實→隱喻候選→抽籤→審查），Taste 主打固定風格規則，兩者互補
- 📎 2026-10-30：[[claude-code-design-skills-overview]] — 六款 Design Skill 精選，Impeccable 的指令數該篇寫 20 個，已過時（現 24 個）

## 補充資料（來源：pbakaus/impeccable GitHub README，2026-05）

### 完整 23 命令清單（⏱ 2026-10-29 已更新：此為 2026-05 README 快照，現行為 24 個指令，見文末「v4.x 更新」；原文保留）

所有命令透過 `/impeccable <command>` 呼叫，或用 `/impeccable pin <command>` 建立獨立捷徑：

| 命令 | 功能 |
|------|------|
| `craft` | 完整 shape-then-build 流程含視覺迭代 |
| `teach` | 一次性設定：收集 design context，產出 PRODUCT.md 和 DESIGN.md（⏱ 2026-10-29 已更新：後續版本的入口為 `/impeccable init`，是否仍保留 `teach` 本庫未核對） |
| `document` | 從現有專案程式碼反向生成 DESIGN.md |
| `extract` | 從程式碼提取可重用元件和 tokens 到設計系統 |
| `shape` | 寫程式碼前先規劃 UX/UI |
| `critique` | UX 設計審查：層次、清晰度、情感共鳴 |
| `audit` | 技術品質檢查（a11y、效能、響應式）|
| `polish` | 最終 pass：設計系統對齊 + 上線前整備 |
| `bolder` | 強化無聊的設計 |
| `quieter` | 緩和過於大膽的設計 |
| `distill` | 提煉到本質 |
| `harden` | 錯誤處理、i18n、文字溢位、邊緣情況 |
| `onboard` | 首次使用流程、空狀態、激活路徑 |
| `animate` | 加入有目的性的動態 |
| `colorize` | 引入策略性色彩 |
| `typeset` | 修正字體選擇、層次、大小 |
| `layout` | 修正版面、間距、視覺節奏 |
| `delight` | 加入令人愉悅的細節 |
| `overdrive` | 加入技術上非凡的效果 |
| `clarify` | 改善不清楚的 UX 文案 |
| `adapt` | 針對不同裝置適配 |
| `optimize` | 效能改善 |
| `live` | 視覺變體模式：在瀏覽器中迭代元件 |

### 7 個領域參考文件

每個命令執行時自動載入所有 7 個 domain reference：

| 參考文件 | 涵蓋範圍 |
|---------|---------|
| typography | 字型系統、字體配對、模組比例、OpenType |
| color-and-contrast | OKLCH、tinted neutrals、dark mode、無障礙 |
| spatial-design | 間距系統、網格、視覺層次 |
| motion-design | 緩動曲線、staggering、reduced motion |
| interaction-design | 表單、focus states、loading patterns |
| responsive-design | Mobile-first、流體設計、container queries |
| ux-writing | 按鈕標籤、錯誤訊息、空狀態 |

### 反模式規則（部分列舉）

Impeccable 內建 27 條確定性反模式規則（無需 LLM，CLI 可獨立執行）＋ 12 條 LLM critique 規則（⏱ 2026-10-29 已更新：此為 2026-05 數字；10 月版 README 記載檢測規則 59～62 條，見文末「v4.x 更新」）：

- 避免過度使用字體（Arial、Inter、系統預設）
- 避免彩色背景上使用灰色文字
- 避免使用純黑或純灰（一律加入色調）
- 避免把所有東西包在 card 裡或 card 套 card
- 避免使用 bounce / elastic easing（感覺過時）

### CLI 獨立掃描（無需 AI key）

```bash
npx impeccable detect src/               # 掃描目錄
npx impeccable detect index.html         # 掃描 HTML 檔
npx impeccable detect https://example.com  # 掃描 URL（Puppeteer）
npx impeccable detect --fast --json .    # regex-only，JSON 輸出
```

偵測 24 種問題：AI slop（side-tab borders、purple gradients、bounce easing、dark glows）＋一般設計品質（行長、cramped padding、小觸控目標等）。

### 支援 11 種工具的安裝（範例：Claude Code）

```bash
# 專案級
cp -r dist/claude-code/.claude your-project/

# 全域（所有專案）
cp -r dist/claude-code/.claude/* ~/.claude/
```

其他支援工具：Cursor、OpenCode、Pi、Gemini CLI、Codex CLI、VS Code Copilot、Kiro、Trae（中國版 + 國際版）、Rovo Dev、Qoder。授權：Apache 2.0。

## v4.x 更新（2026-10-29 快照）

> 來源：codelove.tw 小編精選轉載（2026-10-29），為 Qiita 作者 jinxin4869 文章的中譯；作者看了兩支解說影片（Gary Chen、Nuno Tavares）、讀 GitHub 儲存庫，並做了 4 版本實驗。**⏱ 版本號、指令數、規則數都是快照，Impeccable 更新很頻繁**。實驗為單人單次、單一需求、環境為 Windows＋WSL 2、Claude Code v2.1.287、Sonnet 5.5；**感受類判斷（如「改名後是否成立」）是作者主觀**。頁面末的 AdSense 與站內熱門文章、排行榜不納入。

### 版本演進（以下三個時點勿混用）

| 項目 | 說明 |
|---|---|
| 實驗版本 | v4.4.0（2026-10，實驗日 10/3；判定引擎 v0.1.11） |
| README 基準 | v4.5.2（2026-10-14） |
| 發文時最新 | v4.8.0（作者發文日 10/28） |
| 指令數 | 1 個 skill、**24 個指令**（上方 23 命令表為 2026-05 舊快照） |
| 不用 LLM 的檢測規則 | 61（v4.1.0）→ 59（v4.5.2）→ 62（v4.8.0）；作者認為數字在變，反映「AI 味」的定義本身也在變 |
| 安裝 | `npx impeccable install` 後在 Claude Code 執行 `/impeccable init` |

### 新機制（作者依儲存庫內容與影片整理）

- **PRODUCT.md 與 DESIGN.md 分離**：PRODUCT.md 寫產品事實（誰用、做什麼、什麼技術），DESIGN.md 描述視覺系統。`/impeccable init` 第一步不問顏色與字體，只問產品資訊
- **隱喻候選與強制抽籤**：從使用者熟悉的世界找 **7 個**隱喻候選（至少往三個不同系統挖掘；業界慣例與「照預期反著來」的選項不列入）→ **抽籤**決定主提案（儲存庫稱「抽選前先寫程式碼」為違反契約）→ 瀏覽器開選擇頁，每張卡寫第一個畫面、選它的理由與風險，由人選
  - 代理人自己的首選（IMPECCABLE'S PICK）會誠實寫風險，儲存庫承認自己的第一名通常是「很多次執行都會走到的地方」
  - 從落選候補借的是「規律」不是外觀（例：借舞譜的左右對稱、時間由上往下，不借舞譜外觀）
  - 業界定番方案一定會備一張，但代理人**不會推薦它**（「使用者的門，不是你的門」）；選平均值的權利只留給人
- **craft-floor（定番非禁令）**：寫程式前會讀一份 AI 味型態清單，檔案實際寫的是「這些是該分類的定番，不是禁令，需求文句有依據就可以用」。**唯一絕對禁止的是標題上方的 eyebrow 小標籤**；影片把它說成「禁止模式清單」是簡化
- **兩層審查**：另一個代理人先查是否遵守對使用者的承諾，再用檢測工具做機械檢查；`/impeccable critique` 同時跑設計批評與檢測兩個代理人（作者實測給 32 分滿分中的 20 分）
- **hook 自動檢查**：啟用外掛後會在產出時提示（例：「奶油色背景是 AI 的定番」）
- 設計判準：好的設計＝降低認知成本，四測試（符合直覺／主次清楚／有節制／為對象量身打造）＋作者加的「**把產品名稱換掉，頁面還成立嗎？**」（成立＝平均值）

### 4 版本實驗（虛構河輪 Molenvaart 登陸頁，同一份需求文）

| 版本 | 作法 | 耗時 | 檢出件數 | 改名後仍成立？（作者主觀） |
|---|---|---|---|---|
| a-pure | 原生 Claude Code | 5 分 32 秒 | 8 | 成立 |
| a | 原生但 Impeccable hook 生效 | 5 分 1 秒 | 7 | 幾乎成立 |
| c | 對 a 跑 `/impeccable critique` 再修 | 14 分 57 秒 | 7（與 a 完全相同） | 幾乎成立 |
| b | `/impeccable init` → `craft` 從零做 | 20 分 2 秒 | 5 | **不成立**（結構脫離平均值） |

- 🔑 **機器能計數的問題與「這頁屬不屬於這個產品」是兩條軸**：檢出件數 8／7／7／5 幾乎沒差，真正拉開差距的是「改名後是否成立」，只有 b 是
- 失誤：原以為 a 是純原生，其實 Windows 端已裝外掛、hook 生效（把奶油色改成藍灰），因此另做 a-pure 當對照
- a-pure 為奶油色底（#f6f3ec）＋赤陶色（#d9622b）、eyebrow、三張大數字卡，深色模式又成幾乎全黑＋單橘色，「兩種定番在同頁共存」
- c 的結構沒變（eyebrow、大數字保留）；作者推測是改既有頁面時 Impeccable 會延續現有外觀，且 craft-floor 與評估既有頁的標準不是同一套（批評反而稱讚大數字）
- b 的優點：票價與出發時間不編造，標「待確認」，地圖註「示意」；缺點：捲動時船前進但說明面板不跟著切換、粗黑框接近 2026 年定番。作者多次回答「照你決定」，若明確要求互動結果可能不同
- 兩位影片創作者在同一分岔做了相反選擇（Gary 放棄抽到的燈塔改選開發者主控台；Nuno 直接採用「製圖」，自述看不懂斜線框），借隱喻會連同該隱喻的規則一起帶進來

### 風險與注意事項

- **耗時約 4 倍**：b（20 分）對比原生（5 分）。作者建議公開頁面值得、公司內部工具原生就夠
- **最擅長從零開始**：改既有頁面會延續現有外觀，想換掉要明說「重新設計」
- **AI 會自行註冊 ignore 規則**：hook 允許 AI 對自己有把握的誤判登記忽略。b 中未先徵求同意就註冊整組忽略規則；c 有先問但被回「照你決定」也加了整組。建議檢查 `.impeccable/config.json`
- **檢測工具誤報與漏抓**：c 被警告「文字只有 4px」實測為 32px；被說間距太小者實測足夠；大數字配小標籤版型在 a-pure／a／c 都出現卻一次都沒抓到
- **外部通訊與 telemetry**：候選世界觀從外部 API 取得，另有 telemetry，可設環境變數 `IMPECCABLE_NO_TELEMETRY` 關閉；放進工作儲存庫前先確認
- 年份畫廊（2022 紫色漸層／2025 奶油色＋斜體／2026 粗黑框＋硬陰影）是 Impeccable 方面的整理：避開 AI 味的努力本身會成為下一波 AI 味，所以它提供的是流程而非特定風格
