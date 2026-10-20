---
title: Claude Code／Codex 做影片的工具選型：渲染引擎、風格 Skill、剪輯工具五類地圖，與 Remotion 授權等安裝前四檢查
date: 2026-10-19
tags: [ai/agent, ai/workflow, type/comparison]
source:
  - https://www.aiposthub.com/claude-code-video-skills-2026/
  - https://rain.tips/2026/08/25/claude-code-remotion-codex-animation-workflow/
  - https://www.aiposthub.com/finch-remotion-editing-styles-raw-footage-guide/
  - https://www.aiposthub.com/cinetic-claude-code-product-animation-skill-guide/
official_refs:
  - https://github.com/remotion-dev/remotion/blob/main/LICENSE.md（remotion.dev/license 轉址至此；2026-10-19 Team Lead 核對）
  - https://www.remotion.dev/docs/ai/skills（經 rain.tips 轉述，未核對）
  - https://github.com/Leonxlnx/cinetic（經 aiposthub 轉述，未核對）
  - https://www.finchtech.ai/market/chips/base/remotion-editing-styles-finchip-new_finchip（經 aiposthub 轉述，未核對）
type: article
status: processed
asset_value: 51
---

> 🔴 **來源集中**：4 篇中 aiposthub／Philo 佔 3 篇（Skills 推薦、Remotion Editing Styles、Cinetic），同站互為延伸閱讀，只算**一個轉述來源**；另一篇是 rain.tips（Rain Chu，整理型教學，沒附任何成品）。
> 🔴 **實測很少**：aiposthub 總表標「本站實測」的只有 4 項（Codex＋Remotion、GSAP Skills、OpenMontage、hook-lab），**實測細節都在本批沒收的其他文章**，這裡只有摘要結論；HyperFrames、Cinetic、Hairline、ChatCut、Palmier Pro、Higgsfield MCP、Remotion Editing Styles 全部**未實測**。
> ✅ Remotion 授權已由 Team Lead 2026-10-19 核對官方 LICENSE.md，與 aiposthub 的轉述一致。

## T（核心論點）

Claude Code 與 Codex 本身不輸出 MP4，它們負責寫程式或操作別的軟體。所以選工具前，先分清楚是哪一種需求：**從零做動畫**，需要一個把程式算成影片的渲染層（HyperFrames、Remotion，或自己寫逐格截圖＋FFmpeg），再視需要加動畫風格 Skill；**手上已有拍好的素材**，要找的是剪輯與字幕工具。選型時，比功能更要緊的是四項檢查：**授權、權限、系統需求、成熟度**——Remotion 4 人以上公司要付費、OpenMontage 是 AGPL、Skill 載入時就能執行指令，這些都會直接影響能不能用。

## B（背景）

**依需求找第一類工具（aiposthub 作者觀點）：**

| 想做的影片 | 先看哪一類 | 代表工具 |
|---|---|---|
| 產品宣傳短片 | 動畫風格 Skill＋渲染引擎 | Cinetic 搭配 Remotion 或 HyperFrames |
| 知識型動畫、資訊圖卡 | 渲染引擎 | Remotion、HyperFrames |
| 口播初剪、加字幕 | 剪輯與字幕工具 | HyperFrames 剪輯流程、ChatCut、Palmier Pro |
| 網站或產品頁的互動小動畫 | 動畫風格 Skill | GSAP Skills、Hairline、SVG 動畫 |
| 從腳本到成片一次跑完 | 整套影片代理 | OpenMontage、Higgsfield |
| 拆解爆紅短影音結構 | 影片分析 Skill | hook-lab |

**不適用／注意：**
- 只想做 Logo 描邊、Loading、動態圖表：直接請 Claude 產生 SVG 動畫就夠了，不必裝 Skill（作者觀點；寫法見 [[ideas/claude-code-animation-prompt-qa-delivery]]）
- 一次只裝一兩個工具，出問題比較好排查（作者觀點）

## R（關鍵細節）

### 1. 五類工具地圖（aiposthub 查核日 2026-10-11；授權與費用皆為轉述，Remotion 除外）

| 工具 | 類型 | 授權與費用 | aiposthub 實測 |
|---|---|---|---|
| HyperFrames | 渲染引擎 | Apache-2.0，沒有按次渲染費 ✅ README | 未實測 → 專篇 [[hyperframes-heygen-html-to-video]] |
| Remotion | 渲染引擎 | 個人、≤3 名員工的營利組織、非營利、評估者免費；其餘需 Company License ✅ LICENSE.md（見第 3 節） | 有：Codex＋Remotion |
| Cinetic | 動畫風格 Skill | MIT；用 Remotion 輸出仍受 Remotion 授權 | 未實測 |
| GSAP Skills | 動畫風格 Skill | GSAP 免費含商用；Skills MIT | 有：網站動畫，**非影片** |
| Hairline | 動畫風格 Skill | MIT | 未實測 |
| ChatCut | 剪輯與字幕 | AI 功能點數計費 | 未實測 |
| Palmier Pro | 剪輯與字幕 | GPL-3.0；AI 生成另計點數 | 未實測 |
| OpenMontage | 整套影片代理 | AGPL-3.0；模型與素材服務另計 | 有：5 分給 2.5～3 分 |
| Higgsfield MCP | 整套影片代理 | 需訂閱多個服務 | 未實測 |
| hook-lab | 影片分析 Skill | MIT | 有：6 款中唯一能直接用 |

### 2. 渲染層：「一定要有渲染引擎」vs「自寫截圖＋FFmpeg 也行」（並列，不擇一）

| 說法 | 出處 | 根據 |
|---|---|---|
| 要從零做動畫，得先有渲染引擎，例如 HyperFrames 或 Remotion | aiposthub〈Skills 推薦〉 | 作者觀點 |
| 自己寫 puppeteer 逐格截圖＋ffmpeg-static，沒用 HyperFrames／Remotion | rar.design〈12 種效果〉 | **作者實測** |
| HTML＋CSS＋JS 做成可指定任一秒，Playwright 截圖、ffmpeg 合成 | 數位時代韋惟珊 | **作者實測** |
| 一次性短片用 GSAP／D3 網頁＋Playwright 逐幀截圖＋FFmpeg 就好；多分鏡、字幕、音訊、多尺寸、批次資料的**長期內容管線**才更適合 Remotion | rain.tips | 作者觀點 |
| 用 HyperFrames（`npx hyperframes skills update`） | 益盛科技 | **作者實測** |

- 一種整合讀法（本條整理，供參考）：「把程式算成影片」的**渲染層**是必要的，但**渲染框架**不一定必要；兩條路的實測細節見 [[claude-code-pure-code-animation-practice]]

**兩個框架怎麼選（aiposthub 作者觀點）：**
- 會一點 HTML／CSS，或希望 AI 像改網頁一樣改畫面 → 先試 HyperFrames
- 團隊本來就用 React，或已有 Remotion 專案 → Remotion；公司超過 3 人又要商用，授權成本要列入比較
- 兩邊都不熟的新手 → 作者建議照站內的 Codex＋Remotion 實測文做（本批未收該文）
- Remotion 與 HyperFrames 都把畫面轉成 agent 可改的程式碼；Remotion 較接近 React 生態與逐幀影片管線，HyperFrames 適合直接用 HTML 組合動態內容（rain.tips 作者觀點）

📎 2026-10-19 另一種渲染層：**ManimGX**（Academa Labs，X @sinaatalay；aiposthub〈ManimGX 動畫教學〉Philo 2026-10-13 轉述官方文件與 X 貼文，**作者未實測**，剪藏只附 X 連結、沒有官方 repo／文件網址）——GPU 加速、型別化的 **Python 程式動畫引擎**，為 agent 設計了 `inspect` 檢查工具：
- 🔴 **「90 倍」是開發方自測，三個倍數要並列**：同一組基準（Apple M4 Pro、1080p、60fps、5 個場景合計 36.2 秒影片，從啟動到完成 MP4 3.08 秒，各引擎用最快編碼預設）下，相對 **Manim CE 90.6×**、**ManimGL 11.5×**、**Blender Workbench 18.7×**；換基準倍數差很多，不能只寫 90 倍
- **授權**：專案程式碼 MIT，但**原生套件因為含 x264，整體標 GPL-3.0-or-later**；FFmpeg 等內附元件另有條件（轉述官方文件）→ 正好對應第 9 節「① 授權」檢查
- 需求：Python **3.13 或 3.14**（2026-10-13 查核）；套件名是 `manimgx`，不是 `manim`；排版用 Typst（可輸入 LaTeX，不必裝整套 LaTeX）；Linux 有 CPU 回退；瀏覽器版用 Pyodide＋WebGPU
- `inspect` 執行場景、輸出時間線與分鏡並檢查 2D 版面（取播放／等待結束時刻或指定時刻）；⚠️ **3D 場景會略過 2D 版面檢查**（標題寫「3D 影片」，內文主要談 2D 教學動畫）
- 宣稱與 Manim CE「相容方向」，不保證行為完全一致，遷移前先拿代表場景測（作者觀點）
- 動畫按時間求值、時間相關模擬用固定時鐘，換幀率動作一致——和 [[claude-code-pure-code-animation-practice]] 的 `draw(t)` 原則相同
- 🚫 延伸閱讀導流未收

### 3. Remotion 授權（✅ 官方 LICENSE.md，Team Lead 2026-10-19 核對）

- **Free License** 適用：個人、**最多 3 名員工**的營利組織、非營利組織、尚未商用的評估者
- 其餘須購買 **Company License**，價格見 remotion.pro/license
- 員工怎麼計算，LICENSE 沒說明；邊界案例見 remotion.pro/faq
- aiposthub 兩篇（Skills 推薦：「3 人以下公司免費、4 人以上營利公司需購買」；Cinetic：「最多三名員工的營利組織等」）的轉述與官方一致
- ⚠️ **授權的連鎖**：Cinetic 是 MIT、Remotion Editing Styles 技能本身免費，但**只要輸出走 Remotion，就要另外符合 Remotion 授權**。rain.tips 與 Editing Styles 兩篇都沒提 Remotion 授權，屬遺漏

### 4. Remotion 實作要點（rain.tips；整理型教學，未附成品）

- 概念：用 React 寫影片；畫面拆成 Composition（尺寸、幀率、總幀數），元件依目前幀數計算位置、透明度、縮放；Studio 預覽、CLI 輸出
- Remotion 官方 Agent Skills 文件列出 Claude Code 與 Codex 等（轉述 remotion.dev/docs/ai/skills）；作者認為兩者差別在讀到的 Skill、專案上下文、提示詞完整度與驗收流程，不在品牌
- 建專案（轉述官方）：

```bash
npx create-video@latest --yes --blank my-video
cd my-video
npm i
npx remotion skills add
npm run dev
```

- 全域安裝官方 Skill：`npx -y skills@latest add remotion-dev/skills -g -y`；aiposthub 寫的是 `npx skills add remotion-dev/skills`（與 [[frontend-agent-skills-30-picks]] #21 一致）
- 官方 Skill 分工含建立作品、字幕、地圖、SaaS 產品動畫、互動播放器、媒體處理、版本升級、輸出（轉述）
- 🔑 **可重現規則**（作者範本）：所有動畫只能由 `useCurrentFrame`、`interpolate` 或 `spring` 驅動；不要用無法由幀數重現的 CSS transition；不要用 `Math.random`、目前日期或即時網路狀態；整合 GSAP 時要由目前幀數控制進度，不能依賴瀏覽器實際經過的時間
- 🔑 **先做短樣本**（作者範本）：先做 5 秒第一幕當品質樣本，輸出第 0、30、60、90、120、149 幀檢查，確認沒有溢出、遮擋、跳動、空白畫面後再做完整分鏡
- 模仿參考動畫：每 0.5～1 秒擷取代表幀、標鏡頭起訖、列圖層、描述物件移動與緩動，先重建最有代表性的 3～5 秒，再輸出相同時間點的幀並排比較（作者觀點）
- 透明影片（轉述 Remotion 官方建議）：剪輯軟體用 ProRes 4444／4444 XQ＋PNG 影格＋`yuva444p10le`；網頁用 VP8／VP9 WebM＋`yuva420p`，瀏覽器支援不一，正式網站要備不透明版本

```bash
npx remotion render ProductLaunch out/overlay.mov --image-format=png --pixel-format=yuva444p10le --codec=prores --prores-profile=4444
```

- 輸出：`npx remotion compositions` → `npx remotion render ProductLaunch out/video.mp4`
- 搭配工具：Lottie 用 `npm i @remotion/lottie lottie-web`，**部分 Lottie expression 不保證逐幀一致**，正式輸出前要檢查閃爍（轉述官方）；Three.js 用 `npm i three @react-three/fiber @remotion/three @types/three`（轉述官方）；D3 的資料要先存進專案，避免輸出時受外部 API 影響（作者觀點）
- Blender＋MCP 分工（作者觀點）：Blender 做複雜 3D 資產與鏡頭，輸出透明影片或圖片序列，再交給 Remotion 疊字、圖表、字幕、音訊；第三方 MCP 連接器要先查原始碼、權限與維護狀態

### 5. 動畫風格 Skill

**Cinetic（Leonxlnx；aiposthub 轉述作者 repo 與文件，查閱日 2026-10-08，作者未實測）**
- 第三方 Skill，**不是 Anthropic 官方**；與 [[taste-skill-anti-slop-frontend]] 是同一位開發者
- 流程：企劃 → 視覺 → 節奏 → 聲音 → 檢查；代理先提**三個不同概念方向**，選定後才製作
- 預設用 Remotion 輸出，也可改用 HyperFrames
- 安裝：`npx skills add Leonxlnx/cinetic`（選代理與專案／全域範圍）；另有 Claude Code 外掛方式，**兩者擇一**，避免載入兩份相同工具
- 環境：Node.js 22+、Python 3.11+、FFmpeg 6+、Chromium；Python 套件建議放虛擬環境，而且**代理執行的 `python3` 要讀得到那個環境**——電腦裡有套件和代理讀得到是兩回事
- 測試平台以 Linux 為主，macOS 較少，Windows WSL2 未測
- 動態模糊會增加多次影格計算與渲染量
- 作者的四支示範（Kiln 25 秒橫式有聲／Quire Streaks 12 秒 1080×1920 有聲／Ledgerly Auto-Split 10 秒 1920×1080 無聲循環／Halden 6 秒有聲標誌動畫＋透明版）來自開發版多輪評估，品牌多是虛構，**不保證任意提示詞都能得到同樣品質**
- 🔴 repo 很新（〈Skills 推薦〉寫 2026-09-28 建立），功能與安裝方式可能變動；兩篇都是 aiposthub，沒有獨立佐證

**GSAP Skills（aiposthub 轉述＋站內實測摘要）**
- GSAP 被 Webflow 收購後免費、含商用；限制是不能拿來做與 Webflow 競爭的無程式碼動畫編輯器，完整條款以官方授權頁為準
- 安裝：`npx skills add https://github.com/greensock/gsap-skills`；Skills 為 MIT
- 站內實測：用來做**網站動畫（不是影片）**；只裝 Skills 做出的網站仍有模板感，要搭配設計參考並寫明不要的風格才有差異

**Hairline（aiposthub 轉述）**：等角線條插圖、會跟著滑鼠反應；`npx skills add lucasmarkes/hairline`，裝好後用 `/hairline-create`；輸出單一 HTML；MIT；2026-10-01 建立。

### 6. 剪輯與字幕（已有素材）

- **Claude Code＋HyperFrames 剪口播**：讓 Claude Code 指揮 HyperFrames 與 FFmpeg 做初剪、字幕、圖卡、輸出；🔴 **自動刪停頓與 NG 不是內建功能**，官方示範用的是創作者自己的剪輯工具組（aiposthub 轉述）
- **ChatCut**：可裝成 Codex 外掛，用對話操作多軌時間軸；目前主要依逐字稿判斷，畫面分析開發中（轉述）
- **Palmier Pro**：Mac 剪輯軟體，Claude Code／Codex 透過 MCP 讀寫它的時間軸；**限 Apple 晶片＋macOS 26 以上**（轉述）
- **Podcast 粗剪**：先產生帶時間碼的逐字稿，請 Claude Code 標出贅字、重錄、離題段落；清單仍要人工確認，降噪與混音要用其他工具（作者觀點）

**Remotion Editing Styles（Finch／FinchTechAI；aiposthub 轉述 Finch 官方文章與下載頁，查核日 2026-10-10，作者明言未實跑）**
- 讓 Codex 或 Claude Code 用使用者提供的素材剪片；交付 **H.264 MP4＋Remotion 專案＋素材目錄＋剪輯計畫＋開頭／中段／結尾預覽畫面**
- 技能本身免費、使用不限制；代理與模型費另計。⚠️ 技能免費 ≠ Remotion 授權免費（見第 3 節）
- 安裝：從 Finch 技能市場登入下載，**要保留資料夾結構**（技能用相對路徑讀自己的檔案）；請代理解壓、完整讀 SKILL.md、用技能安裝工具安裝，並確認回覆裡有技能名稱與安裝位置
- 官方示範兩次代理執行 22 分與 27 分（原作者案例，非實測）
- 🔑 流程重點（作者觀點）：**先審剪輯計畫再輸出**；字幕、畫面、聲音**分開驗收**（字幕要對照原聲，不只看順不順）；交付要讓別人接得下去（成片、專案、預覽、素材表分開存放、列出路徑）——與 [[ideas/claude-code-animation-prompt-qa-delivery]]「MP4 不能取代可編輯來源」一致
- 代理回報的計畫驗證、程式檢查、幀取樣只是技術紀錄，不能替你確認有沒有誤剪、課程資訊有沒有寫錯（作者觀點）

### 7. 整套影片代理

**OpenMontage**（aiposthub 轉述＋站內實測摘要）
- 開源影片製作系統，可搭 Claude Code、Codex，用 Remotion 或 HyperFrames 輸出；**AGPL-3.0**：修改後提供他人使用（含架成網路服務）要公開修改後的原始碼
- 站內實測：做出 15 秒與 79 秒直式影片，**滿分 5 分給 2.5～3 分**；最耗時的是環境設定，不同製作流程結果不穩，字幕條不能細調；「適合研究影片代理怎麼運作，還不適合當穩定量產工具」
- ⚠️ **評價並列，不擇一**：既有條目 [[openmontage-ai-video-agent-system]]（wingzero，2026-07）的描述偏正面（品質把關機制、8 種 Pipeline）；aiposthub 實測給 2.5～3／5。兩者都保留

**Higgsfield MCP**：讓 Claude 透過 MCP 串影像生成、Blender、After Effects，交付可再編輯的專案檔；需要同時訂閱多個服務，總成本要先算（轉述，未實測）。

### 8. 影片分析

- **hook-lab**：aiposthub 實測 6 款影片拆解 Skill，只有 hook-lab 能直接用，其他卡在 YouTube 與 TikTok 防爬蟲；⚠️ 有些替代做法會讀取 Chrome 的登入資料，用前要確認能接受這個風險

### 9. 安裝前四檢查（aiposthub，含轉述 Claude Code 官方文件＋作者觀點）

| 檢查 | 要看什麼 |
|---|---|
| ① 授權 | Remotion 對超過免費條件的組織收費；OpenMontage 的 AGPL 影響能不能包成服務；GSAP 不能做競爭的動畫編輯器 |
| ② 權限與隱私 | Skill 可以預先允許 AI 使用某些工具，也能在載入時執行指令 → 先打開 SKILL.md 看一遍；外掛可能附帶 hooks 與 MCP 伺服器；`npx skills` 預設傳匿名使用統計，可設 `DISABLE_TELEMETRY=1` 關閉 |
| ③ 系統需求 | HyperFrames：Node.js 22＋FFmpeg；Cinetic：再加 Python 3.11＋Chromium；Palmier Pro：Apple 晶片 Mac；HyperFrames Studio：目前沒有 Windows 版 |
| ④ 成熟度 | Cinetic（2026-09-28 建立）、Hairline（2026-10-01 建立）都很新，功能與安裝方式可能改動 |

- Claude Code 與 Codex 能不能用同一個 Skill：多數可以，`npx skills add` 會依使用的 AI 放到對應資料夾；少數只提供 Claude Code 外掛版，安裝前看專案說明（作者觀點）

📎 2026-10-19 **MCP for Blender**（原 blender-mcp，github.com/ahujasid/blender-mcp）——⚠️ 以下全部是 **aiposthub〈Blender MCP 教學〉（Philo，2026-10-12，作者稱當日查核 README）轉述，未核對 repo**；文中沒給版本號，指令可能隨改名再變：
- 已改名 **MCP for Blender**；是**第三方專案，不是 Blender 官方**附加元件
- 安裝（需先有 `uv`，自動與手動擇一）：`uvx mcp-for-blender setup`（引導選客戶端並安裝啟用附加元件）；手動：`uvx mcp-for-blender install-addon`
- 註冊客戶端（擇一）：`claude mcp add blender uvx mcp-for-blender`；Codex：`codex mcp add blender -- uvx mcp-for-blender`
- Blender 端：偏好設定啟用附加元件，3D 視窗按 N → MCP for Blender 頁籤；不要同時跑多個服務實例
- 對應「② 權限與隱私」：工具能**執行 Blender Python**；`BLENDER_MCP_SAFE_MODE=1` 會限制部分檔案、網路與啟動其他程式的操作（但不等於風險全消，先在副本練習）；另有**遙測**與資料分享設定，要依自己的版本確認怎麼關
- ⚠️ [[claude-code-pure-code-animation-practice]] 第 11 節轉述的「Blender 官方 MCP」可能和這個第三方專案不是同一個，沒有核對，引用時請區分
- 驗收觀點（.blend 可重開、物件命名與材質可分）見 [[ideas/ai-3d-code-structure-vs-appearance-acceptance]]
- 📎 2026-10-19 Team Lead 核對 repo README：已確認改名（README：「formerly `blender-mcp` — the PyPI package is now `mcp-for-blender`」，repo 現為 **ahujasid/mcp-for-blender**，舊 `blender-mcp` 設定仍可用）；`claude mcp add blender uvx mcp-for-blender`、`uvx mcp-for-blender install-addon`、`uvx mcp-for-blender setup` 三條指令一致；`BLENDER_MCP_SAFE_MODE=1` **預設關閉**，開啟後會擋直接檔案存取、執行其他程式、網路存取與殘留執行的程式碼；遙測：內容收集需主動開啟（opt-in），但**預設仍收最小匿名使用紀錄**（工具名、成功與否、耗時、版本、OS），完全關閉用 `DISABLE_TELEMETRY=true`；README 明寫「third-party integration and not made by Blender」、與 Blender Foundation 無關。Codex 註冊指令未核對。

## C（連結與應用）

- [ ] 先判斷是「從零做動畫」還是「剪既有素材」，再看對應那一類，不要兩類一起裝
- [ ] 公司要商用 Remotion：先數員工數，超過 3 人就要買 Company License（員工如何計算見 remotion.pro/faq）；用 Cinetic、Editing Styles 時也一樣
- [ ] 一次性短片：先試自寫截圖＋FFmpeg 或 HyperFrames；確定要做長期內容管線（多尺寸、批次資料、字幕）再評估 Remotion
- [ ] 裝任何 Skill 前先讀 SKILL.md、確認外掛帶了哪些 hooks／MCP；需要時設 `DISABLE_TELEMETRY=1`
- [ ] 剪輯類任務沿用 Editing Styles 的順序：先審剪輯計畫 → 輸出 → 字幕／畫面／聲音分開驗收 → 交付專案＋成片＋素材表
- [ ] 待追蹤：aiposthub 站內 Codex＋Remotion、OpenMontage、GSAP、hook-lab 實測文的完整細節（本批未收）；Cinetic 的成熟度

## 原始摘要

- **aiposthub〈2026 Claude Code 做影片 Skills 推薦：HyperFrames、Remotion 到剪輯工具怎麼選？〉（Philo，2026-10-11，主幹）**：五類工具、授權與實測總表、HyperFrames vs Remotion、安裝前四檢查、FAQ。兩段 Skill Shot 14／15 課程推廣（academy.aiposthub.com）、站內延伸閱讀未收。
- **rain.tips〈Claude Code Remotion 教學：用 Codex 做動畫影片〉（Rain Chu，2026-08-25）**：Remotion 概念、官方 Skill 安裝、可重現規則、5 秒樣本與代表幀、參考動畫拆解、GSAP／D3／Lottie／Three.js／Blender 分工、透明輸出、「一定要用 Remotion 嗎」。整理型教學，未附成品；未提授權；剪藏帶進來的廣告分類字樣未收。
- **aiposthub〈Remotion Editing Styles 教學…〉（Philo，2026-10-10）**：Finch 技能的交付物、安裝、審剪輯計畫、分開驗收、交接。作者明言未實跑；作者自擬的作文課提示詞與驗收表未收；Skill Shot 14 課程推廣、訂閱 AI 郵報未收。
- **aiposthub〈Cinetic 是什麼？讓 Claude Code 製作產品宣傳動畫的 Skill〉（Philo，2026-10-08）**：Cinetic 流程、安裝、環境、測試平台、四支示範、授權。作者未實測；本文自擬的中文企劃只取方法不收全文；站內延伸閱讀未收。

## 相關連結

- [[hyperframes-heygen-html-to-video]] — HyperFrames 專篇：README 一手、三條安裝路徑、Studio、益盛四支片實測
- [[claude-code-pure-code-animation-practice]] — 實作工作法與「自寫截圖＋FFmpeg」路線的實測細節
- [[openmontage-ai-video-agent-system]] — OpenMontage 既有條目（評價與本條 aiposthub 實測並列）
- [[codex-plugins-12-guide]] — Codex 插件中的 HyperFrames by HeyGen、Remotion
- [[frontend-agent-skills-30-picks]] — #21 `remotion-best-practices`（`npx skills add remotion-dev/skills`）
- [[claude-code-design-skills-overview]] — GSAP Skills 在設計 Skill 清單中原本「無專篇」
- [[taste-skill-anti-slop-frontend]] — 同一開發者 Leonxlnx 的前端品味 Skill
- [[ai-flavor-removal-writing-technique-guide]] — 也提到 Leonxlnx 的 taste-skill
- [[ideas/lottie-animation-skill]] — Lottie 動畫格式；Remotion 接 Lottie 時 expression 不保證逐幀一致
- [[ideas/claude-code-animation-prompt-qa-delivery]] — 程式動畫的需求寫法、驗收與交付格式
- 📎 2026-10-19：[[opus-5-5-video-case-libraries]] — Opus 5.5 影片案例庫與七條製作路徑（含 Manim 解說路徑）；[[ideas/ai-3d-code-structure-vs-appearance-acceptance]] — 3D 結構 vs 外觀驗收；[[chatgpt-blender-mcp-3d-dashboard-case-study]] — blender-mcp 舊名下的 Codex 實測案例
