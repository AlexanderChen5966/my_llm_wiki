---
title: 代理操作外部工具的權限與驗收（Cursor／Unreal／FigCraft／Framer，2026-10 快照）
date: 2026-11-05
tags: [ai/agent, ai/mcp]
source: https://www.aiposthub.com/cursor-computer-use-desktop-ios-simulator-testing/；https://www.aiposthub.com/epic-unreal-engine-claude-code-plugin-official/；https://www.aiposthub.com/figcraft-open-source-rust-design-mcp-alpha/；https://www.aiposthub.com/framer-mcp-remote-ai-site-design-branches-publish/（皆 aiposthub／Philo）
type: article
status: processed
asset_value: 27
---

> 註：asset_value 27（頻率 3×耗時 3×複雜度 3），**保守估 18（耗時 2）**，屬邊界；四篇單篇皆 <24（12／12／12／8），合併才成條目，否則會丟掉 Unreal 的安全風險與 FigCraft 的明文儲存風險。
> 🔴 **來源性質**：全部是 aiposthub（作者 Philo）**單一作者的二手整理，站方皆未實測**；一手來源（Cursor 官方 X 與文件、Epic／UE 文件與 GitHub、FigCraft README、Framer 官方 X 與文件）**本批未開啟核對**。互相引用不構成交叉佐證。
> ⏰ **快照式、保鮮期短**：Cursor 公告 2026-10-23（文章 10-24）；Unreal 查詢 2026-10-21（v3.1.1、約 871 次安裝）；FigCraft README 2026-10-24（alpha）；Framer 功能公布 2026-10-22。功能、文件、版本與安裝數都會變，引用前回一手核對。
> 已排除：各篇文末訂閱導流與站內延伸閱讀。

## T（核心論點）

代理沿用的是**你的帳號或你的機器的權限**。「能力存在 ≠ 已啟用 ≠ 已驗證」。這四個工具的權限面完全不同（見對照表），但驗收流程一致：**先唯讀確認對象與連線 → 在副本／分支／草稿／專案副本試做 → 人工驗收（看結果與回讀，不看「完成」訊息）**。

## B（適用場景與對照）

**適用**：要讓代理操作桌面 App、遊戲引擎、設計工具、網站編輯器之前，用同一份清單先問權限面。

**對照表**（資料皆為二手轉述、快照日見欄尾）：

| 工具 | 代理在哪執行 | 沿用誰的權限 | 預設寫入範圍 | 能否對外發布或外洩 | 官方自述限制 | 快照 |
| --- | --- | --- | --- | --- | --- | --- |
| Cursor Computer Use | 自架執行節點（macOS 用輔助程式；Linux 用 X11） | 高權限：輔助使用＋螢幕錄製，須已登入桌面 | 代理可點擊、輸入、截圖；二手轉述未提預設寫入範圍（以桌面上帳號能做的事為準，待查文件） | 螢幕錄製可能錄到無關通知與帳號畫面 | 桌面分享文件列為 Linux 專用；Linux 隔離桌面、限制剪貼簿 | 10-23 |
| Epic Unreal 外掛 | 本機，經 UE 5.8 內建 MCP（127.0.0.1:8000/mcp） | 同機同使用者；MCP 無驗證層 | 外掛工具可改專案；`execute_tool_script` 可跑任意 Python | 遙測 `EnableAnalytics` 預設開，送出內容文件未列 | UE 文件稱功能屬實驗性、介面隨時可能改；僅 HTTP／SSE | 10-21 |
| FigCraft | 本機檔案（.figcraft 封存）；可選線上分享／區網 | 本機使用者；線上分享無帳號系統 | 本機離線模式無帳號、無遙測 | 線上分享：伺服器明文儲存、管理者可讀、**停止分享不等於刪檔**、**連結本身即存取憑證**；區網無加密 | alpha；即時協作標預覽；公開服務不承諾備份與可用 | 10-24 |
| Framer MCP | 遠端 MCP（Streamable HTTP＋OAuth） | 登入帳號有權限的專案 | 預設留在分支或草稿 | **明確要求時可直接發布到正式網站**；共用元件改動會擴散 | 舊版 External Agents 本機 CLI 仍支援，含在地化等額外流程 | 10-22 |

### Cursor Computer Use（快照 2026-10-23）

- **macOS 的權限要授給「Cursor Computer Use 輔助程式」，不是終端機、也不是主 Cursor App**；需已登入的桌面工作階段
- 🔴 **檢查指令只證明輔助程式存在，不證明權限已生效**；要跑一次「截圖＋點擊」驗證；系統更新或輔助程式身分改變後重驗
- **桌面分享是獨立選項，文件目前列為 Linux 專用**，不能套到 macOS；macOS 與 Linux 的電腦操作都支援
- **模擬器通過仍需真機**：iOS Simulator 的驗收不取代實機
- 自架文件不可推定所有受管理雲端代理行為相同；官方展示影片不是驗收紀錄

### Epic Unreal Engine 外掛（快照 2026-10-21）

- **更正「剛開源」**：倉庫 **2026-06-08 建立**，**2026-06-26 加入 Anthropic 官方外掛清單**，授權 MIT，不是近期才開源
- 結構：三項技能（unreal-mcp、create-toolset、unreal-skill）加 SessionStart 掛鉤；工具來自 UE 5.8 內建 MCP，預設僅 list_toolsets／describe_toolset／call_tool 三個入口；需啟用 ModelContextProtocol 與 AllToolsets 引擎外掛
- 🔴 **本機 MCP 無驗證層**：同機同使用者的任何程式都能連，不適合暴露到本機外；`execute_tool_script` 可跑任意 Python（每次視為高權限）；**遙測預設開啟**
- 🔴 **並行呼叫兩份官方說法不一致，並陳不擇一**：UE 5.8 文件說工具呼叫在遊戲執行緒依序執行、客戶端不該送重疊呼叫；外掛 2026-09-17 提交的說法是伺服器接受並行請求並自行處理同步（有相依者仍須依序）。以實際引擎版本為準
- 「入口工具上下文開銷約 250 詞元」是**貼文自估，無出處、未驗證**
- 建議：先跑唯讀指令（例如列出 Actor）；在專案副本試用；大批量修改前先存檔並提交版控

### FigCraft（快照 2026-10-24）

- Rust 寫的開源設計／原型工具；UI、CLI、JSON 控制與 MCP 共用 400+ 命令（專案自列，未核對）
- 🔴 **alpha**；README 的「約 49%」是**作者自評進度，不是 .fig 相容率**；Figma .fig 匯入為盡力相容（讀得進 ≠ 關係完整保留）
- 授權：**MIT 或 Apache-2.0 雙授權**（第三方 X 介紹只寫 Apache-2.0，以 README 為準）；**素材與商標另有例外**（ATTRIBUTION.md），程式授權不等於品牌使用許可
- 三種分享路徑資料面不同（見表）；本機離線為最保守
- 搬離 Figma 前先測：副本→一個共用元件改文案（文字／自動排列／實例是否全更新）→對比匯出→品牌字型；既有 Figma 專案保留為權威版本

### Framer MCP（功能公布 2026-10-22）

- 遠端 MCP（OAuth）：客戶端須支援 Streamable HTTP 與 OAuth；管理員可能要求組織核准
- 🔴 **變更預設留在分支或草稿，但明確要求時代理可直接發布到正式網站**；任務要寫明「僅草稿」
- **共用元件改動會擴散**到所有使用處；價格、服務條件須提供定稿文案
- 舊版 External Agents 本機 CLI 仍支援（含在地化等額外流程），不必立刻遷移；AI 用量依代理方案計費，不可假設免費

## R（共通驗收流程）

1. **確認代理連到哪裡**：給完整專案連結或明確路徑（避免同名的正式／測試／客戶副本）；多客戶端各自確認連到哪個帳號
2. **先唯讀**：第一個任務只讀取（列 Actor、讀專案、打開指定 App 到已知畫面並回傳截圖），確認連線與權限真的生效
3. **在可丟棄的範圍試做**：副本、分支、草稿、專案副本；錯誤時保留「最後成功位置＋錯誤畫面」
4. **把「不要做」寫進任務**：僅草稿、不發布、不改共用元件、不用 `--dangerously-skip-permissions`（Unreal README 建議避免）
5. **人工驗收看結果，不看完成訊息**：重載確認持久化、預覽含手機、發布後重開公開網址核對、對比匯出
6. **留證據**：環境與版本、帳號角色與資料條件、截圖錄影；測試用專用帳號與專用環境

## C（關鍵行動）

- [ ] 導入任何「代理操作型」工具前，先填一次上方對照表的五個欄位
- [ ] 交辦時一律附「先唯讀、僅副本／草稿、不對外發布」三句
- [ ] 引用本條數字（871 次安裝、約 49%、約 250 詞元）時保留「快照／自評／自估」標註，並在一個月後回一手核對

## 原始摘要

四篇皆為 aiposthub 的整理：Cursor 代理可在執行節點操作桌面 App（含 iOS Simulator）；Epic 的 Unreal Engine Claude Code 外掛於 6 月底即上架官方清單，倉庫並非剛開源；FigCraft 是 alpha 階段的開源 Rust 設計工具；Framer 推出遠端 MCP，讓代理直接操作專案。共通風險是代理沿用高權限，可寫入、可發布或可能外洩資料。

## 相關連結

- [[devin-macos-vm-ios-simulator-foldable-testing]] — 模擬器證據的邊界（Devin 雲端 macOS VM）；Cursor Computer Use 是同類操作模擬器的路線，驗收原則相同
- [[agent-device-cross-platform-testing]] — 專用裝置＝隔離，Mobile MCP 的安全面；與「專用帳號、專用環境」同一護欄
- [[claude-code-figma-mcp-write-setup]] — Figma 版的同一護欄：只在指定頁面建立、AI Sandbox 頁；本篇 FigCraft／Framer 是其他設計／網站工具
- [[agent-permission-and-os-sandbox]] — 代理權限與作業系統沙箱的通則；macOS 權限授予對象、本機 MCP 無驗證層是其具體案例
- [[ai-native-interface-patterns]] — 介面模式（Approval Card）：發布前的人工核准點
- [[claude-code-plugins-guide]] — Claude Code 外掛機制；Unreal 外掛是「說明書＋引擎端 MCP」的案例，啟用外掛不等於連得到編輯器
- [[ideas/cursor-self-hosted-machines-cloud-agents]] — Cursor 自架機器的資料邊界（資料仍會傳 Cursor）；本篇 Cursor 部分的上游背景（批外相關，已處理）
