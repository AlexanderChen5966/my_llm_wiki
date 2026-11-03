---
title: ArtCraft 開源 Adobe／Office 替代品：時間線與狀態表（2026-10-07 ～ 10-14）
date: 2026-10-30
tags: [ai/claude, dev/architecture]
source: https://tools.wingzero.tw/articles/4091 ; https://www.aiposthub.com/artcraft-seven-open-source-creative-apps-alpha/ ; https://techorange.com/2026/10/08/artcraft-software-ai-developer-adobe/ ; https://www.bnext.com.tw/article/92597/artcraft-open-source-office-clones ; https://www.aiposthub.com/ji-tian-fu-ke-photoshop-gong-neng-de-photocraft-dui-shang-yi-ge-ren-zuo-liao-shi-duo-nian-de-photopea-xian-zai-shui-bi-jiao-neng-yong-2/ ; https://www.aiposthub.com/lightcraft-raw-photo-editor-lightroom-workflow/ ; https://www.aiposthub.com/pdfcraft-open-source-pdf-merge-split-organize-guide/ ; https://www.aiposthub.com/photocraft-open-source-image-editor-ai-agent-psd/ ; https://www.aiposthub.com/vectorcraft-open-source-vector-editor-svg-pdf-ai-compatibility/ ; https://www.bnext.com.tw/article/92727/ai-software-deflation-saas-moats ; https://www.aiposthub.com/photocraft-photoshop-psd-compatibility-text-mask-effects/ ; https://www.aiposthub.com/claude-photocraft-mcp-batch-layer-edit-export-workflow/
type: reference
status: processed
asset_value: 28
---

> **建立說明**：本條合併 12 篇來源（初建 5 篇，2026-10-30 追加單品深度 4 篇＋數位通縮 1 篇＋PhotoCraft PSD 檢查 1 篇；2026-11-03 追加「Claude 搭配 PhotoCraft」操作層 1 篇，分數維持約 28）。asset_value 重估約 28：初建 5 篇時約 21、追加 5 篇後約 26，**低於 skill 門檻（單篇 50／合併 24）**；追加後僅略高於合併門檻，且每篇單獨都遠低於 50，仍是**為使用者指定合併建立**。asset_value 為整理者估算，非精確計分。
> **時效提醒**：ArtCraft 每週都在快速變動，下列狀態、星數、完成度都是當日快照，使用前請回官方頁與各 repo 重新確認。

## T（核心論點）

2026-10 初，開發者 Brandon Thomas（ArtCraft）宣稱用 Claude Opus 5.5 以 Rust 一次公開七款對應 Adobe 的開源應用，隨後又推出三款對應 Office 的應用。**可確認的是：repo 與星數真實存在、各款自標 Alpha 或開發中；不能確認的是「clean-room」「一個月內 100% 功能相容」這類開發者單方主張。** 各來源的共同結論一致：這是「重建成熟軟體的成本被 AI 壓低」的訊號，但距離能取代 Adobe／Office 的日常專業工作仍遠，官方 README 也自陳尚未達到。追加的單品深度文（LightCraft／PdfCraft／PhotoCraft／VectorCraft）都停在「官方文件整理＋建議驗收方法」，**沒有任何一篇有安裝實測**；數位通縮一文則把這件事放進「軟體價值往哪裡去」的創投評論，屬觀點而非事實。

## B（適用場景）

**適用：**
- 想追蹤「AI 讓個人開發者重寫大型商用軟體」這個趨勢的實際案例與數字，並區分哪些是事實、哪些是放話
- 評估是否試用某一款開源替代品（先看狀態表，再用自己的素材驗收）

**不適用 / 注意：**
- 不要把七款（或十款）當成一組已成熟的專業軟體；狀態、完成度各款不同
- 本條多數數據是二手媒體引述，且 2 篇來源（數位時代 10/13、10/20）自註「初稿為 AI 編撰」（見來源說明）
- 單品深度各節的版本號與缺口都是 2026-10-08 查閱的官方頁，之後可能已改版
- 星數、完成度百分比會很快過時，只能當該日期的快照

## R（時間線與狀態表）

### 時間線總覽

| 日期 | 事件 | 來源 |
|---|---|---|
| 9/30～10/1 | 七款 Adobe 對應應用陸續上線（數位時代說法）。PhotoCraft repo 建立日為 9/30（10/14 篇） | 數位時代 10/13；aiposthub 10/14 |
| 10/3 | Thomas 公開七款對應 Adobe 的開源應用（10/14 篇說法，與上列有出入，見矛盾 1） | aiposthub 10/14 |
| 10/7 | 官方產品頁快照：七款狀態（見下表）；GridCraft／DeckCraft／WordCraft 三個 GitHub 專案上線；同日另建 CADCraft（對應 AutoCAD）、SoundCraft（對應 Pro Tools）專案 | wingzero、aiposthub 10/7；數位時代 10/13 |
| 10/8 | TechOrange 轉載 Ars Technica／Crypto Briefing：Thomas 放話「一個月內 100%」，數日後改口「99% 需要幾個月」 | TechOrange 10/8 |
| 10/11 | 七個 repo 合計 86,265 顆星；PhotoCraft 44,649 顆 | aiposthub 10/14 |
| 10/13 | Office 三款星數：WordCraft 約 9,300、GridCraft 約 3,900、DeckCraft 約 2,600 | 數位時代 10/13 |
| 10/8 | aiposthub 單品深度四篇（LightCraft 0.2.1、PdfCraft 0.2.1、VectorCraft 0.4.0、PhotoCraft）以 10/8 官方頁為準；10/8 凌晨 PhotoCraft 約 15,459 顆星 | aiposthub 單品文 |
| 10/14 | PhotoCraft 約 5.9 萬顆星；自評完成度與 Photopea 比較 | aiposthub 10/14 |
| 10/17 | All-In Podcast 討論 Photoshop 仿製案與「軟體通縮」（二手轉述） | 數位時代 10/20 |
| 10/20 | PhotoCraft 近 6.9 萬顆星；數位時代整理創投觀點 | 數位時代 10/20 |

### 表 1：十月七日狀態快照（七款 Adobe 對應）

> 來源：wingzero、aiposthub 10/7（皆為轉述官方產品頁 getartcraft.com/apps）。**狀態截至 2026-10-07。**

| 應用 | 對應 Adobe | 官方標示狀態（截至 10/7） | 功能描述（官方頁轉述） |
|---|---|---|---|
| PhotoCraft | Photoshop | **早期 Alpha** | 圖層、遮色片、調整圖層、筆刷、文字、向量工具；支援 PSD／PSB |
| PrintCraft | Acrobat | **早期 Alpha** | PDF 閱讀、搜尋、頁面整理、合併、拆分、加密 |
| VectorCraft | Illustrator | 開發中 | 鋼筆、形狀編輯、布林運算、向量效果、點陣轉向量 |
| FilmCraft | Premiere Pro | 開發中 | 多軌時間軸、剪接、色彩校正、特效、轉場、混音 |
| LightCraft | Lightroom | 開發中 | 照片資料庫、非破壞式 RAW 調整與遮罩 |
| EffectCraft | After Effects | 開發中 | 圖層、關鍵影格、效果、3D 攝影機與燈光 |
| DesignCraft | InDesign | 開發中 | 跨頁、母版、文字串接、段落樣式、文字環繞、印刷用 PDF |

> **命名修正**：10/7 兩篇用 PrintCraft 稱呼 Acrobat 對應款；10/8 aiposthub 單品文稱官方產品頁已用 **PdfCraft**，但 0.2.1 的部分安裝檔仍以 `printcraft` 命名。兩者指同一款。

共通特性（官方自述，**未經獨立驗證**）：Rust 原生桌面應用、開源免費、檔案在使用者裝置上處理、部分工具有命令列／JSON 控制通道／MCP 以供 AI 代理操作。

**可編譯 WebAssembly 的數量與範圍差異**
- aiposthub 10/7：官方指出**六款**可編譯成 WebAssembly 在瀏覽器使用（未指出是哪一款）。
- wingzero 10/7：只寫「部分工具也支援瀏覽器版本」，沒有數字。
- TechOrange 10/8：寫**七款**都提供 WebAssembly 版本。
- 數位時代 10/13：Office 三款也可透過 WebAssembly 在網頁使用。
- aiposthub 10/14：PhotoCraft 提供桌面版（macOS／Windows／Linux／FreeBSD）或網頁版。

**平台與儲存庫範圍差異**：aiposthub 10/7 提醒，七款 Crafting Apps 與更廣泛的 **ArtCraft 平台**（GitHub 上 storytold/artcraft，含互動式 AI 圖片影片製作）有關聯，但**不能把平台的每項功能都視為免費或離線**；且各 repo 可能授權不同，應回到具體專案確認。

### 表 2：十月八日開發者放話（單方主張，未驗證）

> 來源：TechOrange 10/8（轉載 Ars Technica、Crypto Briefing）

| 主張 | 出處／時間 | 查證狀態 |
|---|---|---|
| 用 Claude Opus 5.5 以 **clean-room** 方式打造 | Thomas 於 Reddit | ⚠️ **開發者自述，未驗證**（TechOrange 10/8 僅轉述；數位時代 10/13 也只寫「他聲稱」） |
| 「一個月內達到 100% 功能相容」 | Reddit | ⚠️ **放話**；Crypto Briefing 稱承諾「仍有待證實」 |
| 「99% 功能相容需要一段時間，但會是幾個月，不是幾年」 | 數日後改口於 Hacker News | ⚠️ **放話**，已與上一句不一致 |
| 約 10 年大型 Rust 專案經驗 | Thomas 自述 | 未驗證 |
| 採 MIT 或 Apache 授權、免費使用 | 轉述 | 與其他篇一致（見矛盾 5） |
| 營收來自 ArtCraft 原有視覺 AI 模型與 IDE 的 Token 制服務；該模型與第三方模型並列 | Thomas 自述 | 單方說明 |
| 「Software is over」「一次就生成了整個 Photoshop」 | Hacker News 留言 | ⚠️ 放話 |
| 現階段為非常早期 Alpha，社群指出不少缺陷 | Ars Technica、Crypto Briefing 轉述 | 第三方觀察（二手） |
| 若商業外觀（trade dress）過度模仿 Adobe 外觀與操作，可能有法律風險；逆向工程本身普遍被認為可接受 | Ars Technica 轉述 | 媒體觀點，非法律結論 |

### 表 3：十月十三日 Office 三款

> 來源：數位時代 10/13（**自註初稿為 AI 編撰**，且多為轉述 Tom's Hardware、TechRadar 等）。**數字截至 2026-10-13。**

| 應用 | 對應 Office | GitHub 星數（10/13） | 備註 |
|---|---|---|---|
| WordCraft | Word | 約 9,300 | 可讀寫 .docx |
| GridCraft | Excel | 約 3,900 | 說明頁附接入 Claude Code 的指令 |
| DeckCraft | PowerPoint | 約 2,600 | 可開啟與儲存 PowerPoint 檔；專案頁標示 **alpha，朝 beta 推進** |

- 授權：MIT 與 Apache-2.0 雙授權；支援 macOS、Windows、Linux、BSD；可透過 WebAssembly 在網頁使用。
- 設計重點：「每個按鈕同時也是一道指令」，可用命令列或 MCP 操作，讓 AI 代理不開介面就產出文件／試算表／簡報。
- **批評與限制**：TechRadar 評估三款進度不如先前 Adobe 複製品；外界批評為 vibe coding 堆出的「AI 垃圾」（Thomas 回應自己本來就會寫程式）；Reddit 網友指出 LibreOffice／Google Docs 免費多年也沒明顯衝擊 Office 銷售，並質疑單人能否跟上除錯與改版；Tom's Hardware 讀者留言認為企業採購重技術支援，較適合個人工作室。
- Thomas 對 LibreOffice／GIMP 等既有開源專案的評論（如「GIMP 很爛」）屬個人立場，不作為事實主張。

### 表 4：十月十四日新增資訊

> 來源：aiposthub 10/14（Hank 週報首篇，其餘 4 個本週議題與 ArtCraft 無關，未收）。**數字截至 2026-10-11 至 10-14。**

| 項目 | 內容 | 性質 |
|---|---|---|
| 星數 | 10/11 七個 repo 合計 86,265；PhotoCraft 44,649；10/14 PhotoCraft 約 5.9 萬 | 轉引 Linux Compatible 與 GitHub，時點明確 |
| 建置方式 | 七款皆 Rust，主要工具 Claude Opus 5.5；建置時間有「幾天」與「數週」兩種說法 | 說法分歧，本篇未判定 |
| PhotoCraft 協作紀錄 | 最近 100 筆 commit 中 76 筆帶 Co-authored-by: Claude Opus 5.5；199 位貢獻者、合併 1,004 個 PR | 可從 repo 查核 |
| 專案自評完成度 | 功能廣度約 76%、「可用於真實工作」約 45%、AI 功能約 10%、外掛與腳本約 15%、效能約 25%；README 明寫尚非日常專業工作可用的 Photoshop 替代品 | ⚠️ **專案自評**，非第三方測量 |
| PSD 往返 | 專案自稱 135 檔中 134 檔可往返，仍有 Photoshop 不一定能開的檔案 | ⚠️ 專案自稱 |
| 與 Photopea 比較 | 作者依兩邊公開描述整理，**未做同一批檔案並排測試**。結論：現階段要交件用 Photopea；想看開源 Photoshop 長到哪或想自己改，追 PhotoCraft | 作者整理＋意見 |
| Photopea 背景數字 | 一人維護十多年、廣告占收入近九成；2021 年初年收入逼近 100 萬美元、日活約 30 萬 | 為 2021 年舊數據，非現況 |
| 作者觀點 | 價值可能從「提供完整功能」轉向「能否隨使用者需求持續改變」；「市面約 70% 軟體面臨過時風險」**作者自稱個人直覺** | 意見，非事實 |

### 單品深度（aiposthub 10/8 系列，Philo）

> 共同性質：四篇都是**依官方產品頁與 README 整理的評估方法**，文中明說「沒有對應的本站安裝實測」，截圖皆為官方展示。版本與缺口**截至 2026-10-08**。

**LightCraft（對應 Lightroom）— 官方頁版本 0.2.1**
- 能做什麼：本機照片庫（相簿、資料夾、評分、旗標、搜尋）、非破壞 RAW 編輯（曝光、白平衡、曲線、遮罩、同步設定）、批次輸出（尺寸、命名、中繼資料政策）。桌面版 macOS／Windows／Linux，另有可自行架設的 Web 建置檔。官方稱不需帳號、雲端或訂閱。
- 與 Lightroom 的差距（官方文件自陳）：RAW 完整解碼缺口為 **CR3、壓縮 RAF、壓縮 ORF**，NEF 有特定壓縮變體限制，缺口檔可能只顯示內嵌預覽；DNG 可用檔內色彩矩陣，其他 RAW 的相機校準仍在補；**AI 降噪列為缺口**；主體／天空選取是傳統演算法啟發式，非 AI 分割。Lightroom Classic 有逐機型支援表與 Denoise。
- 相容性：文章提醒檔案能搬不代表 Lightroom 目錄、歷史、預設能搬，建議用新專案評估。
- 限制：「不需雲端」是產品定位，不代表接入 AI 服務後資料也留在本機。官方示範圖多為公有領域黑白歷史照片，不能代表現代相機 RAW 的色彩驗證。

**PdfCraft（對應 Acrobat）— 官方頁版本 0.2.1，早期 Alpha**
- 能做什麼：閱讀、搜尋、頁面整理（旋轉、刪除、插入、重排）、合併、擷取、拆分；官方稱合併時保留並調整連結、表單欄位、圖層與附件；有撤銷、增量儲存、原子寫入。另有命令列與可自行部署的網頁建置檔（不等於公開線上編輯器）。
- 與 Acrobat 的差距：可靠的既有文字編輯（尤其中日韓文字）、非拉丁文字 OCR 仍有缺口；Office 轉換、印前標準、長期數位簽章驗證應先核對官方缺漏；**README 自述尚未與 Acrobat 做完整並排品質評估**。
- 測試證據：官方稱在 **983 份測試文件中沒有崩潰**（專案自稱，僅指該測試集），並仍在修新發現的崩潰與停滯。
- 官方定位：無帳號、無遙測、無雲端處理（專案自述）。

**PhotoCraft（對應 Photoshop）— 10/8 單品文**
- 能做什麼：圖層、遮罩、調整圖層、文字、向量；同一命令登錄系統供 UI、命令列與 AI 代理共用；提供 MCP 伺服器與命令列，可把動作清單套到整個資料夾。桌面編輯可離線，智慧選取在本機執行（官方自述）。
- 與 Photoshop 的差距：官方明說**尚不能作日常專業替代**；缺口含**生成式功能（路線圖列為重要缺口，機器學習後端尚未啟動）**、部分工具、文字排版深度、專業流程與外掛相容性。MCP 只是讓外部代理呼叫編輯命令，不等於內建 AI 生圖。
- 相容性（PSD）：README 稱 psd-tools 的 **309 份測試文件中 307 份開啟重存保留渲染結果**；另一混合測試集 **170 份中 169 份**（專案自述，未重跑）。重存會改寫影像資源、圖層記錄與合成內容，**並非逐位元組一致**；獨立 PSD 程式庫的位元組往返測試是另一條路徑，不能套用到介面內開檔修改再存。
- 其他自述：超過 1,700 項測試、CPU／GPU 合成互相比較；官方路線圖承認早期使用者發現過測試沒涵蓋的基本操作問題。程式碼 MIT 或 Apache 2.0，附帶素材與品牌名稱另有規範。
- **PSD 檢查項目（10/8 另一篇，依官方 0.3.0 文件整理，明說非本站實測）**：把相容性拆成三層——能讀到內容、畫面接近原稿、修改後能交回原流程；三層分開記錄，分類為「可編輯交接通過／僅成品輸出通過／仍有差異」。
  - 文字層：核對字型、換行、中文標點，回存後要能再改字；官方評估文件仍列**路徑文字**缺漏。
  - 遮罩：看頭髮與半透明邊緣（換深色背景檢查）、遮罩是否隨圖層移動、回存後能否再調；剪裁關係與群組須對照來源階層。
  - 圖層效果：陰影／描邊／混合模式／調整圖層合併作用，需逐項比較；印刷文件仍走原印前流程。
  - 智慧物件：0.3.0 評估文件列智慧物件輸出為已完成，**連結檔案會被嵌入**（可能改變素材更新流程）；部分 PhotoCraft 專用濾鏡不會寫入 Photoshop 濾鏡結構（會警告）；智慧物件的某些透視變形與濾鏡遮罩操作、**第三方編輯器回讀驗收**仍有缺漏。
  - 官方數字的讀法：README 稱應用回存會重寫影像資源、圖層紀錄與合成圖，**不會逐位元相同**；獨立 PSD 解析元件的位元組往返是另一條路徑。0.3.0 評估文件的 psd-tools 回存門檻為 **309 份中至少 307 份**，畫面比對門檻**至少 229 份**。
  - 時效與限制：以 2026-10-08 的 0.3.0 文件為準；全文沒有自己的測試結果，結論都是驗收方法。
- 星數：10/8 凌晨約 15,459 顆；repo 9/30 建立（文章特別提醒不要把討論日期當首次發布日期）。
- **Claude 搭配 PhotoCraft 操作層（10/8 官方 README／CLI／MCP 文件整理，aiposthub／Philo，2026-11-03 追加；⚠️ 無實測——原文明說命令範例沒有對應的成功執行紀錄；版本以安裝版 `--help` 為準，官方下載頁版本與主線文件可能不同）**：
  - **接入 MCP**：`claude mcp add --transport stdio photocraft -- /path/to/photocraft-cli mcp --automation-read-root <來源資料夾> --automation-write-root <輸出資料夾>`，以讀、寫根目錄分開限制範圍；註冊後用 `claude mcp get photocraft` 與 Claude Code 內 `/mcp` 檢查連線（註冊成功只代表設定成立，不證明文件可讀或結果可存）。前提是已安裝 Claude Code，網頁版對話不能代替。
  - **CLI**：`run <檔> --cmd <命令識別碼> --params '{…}' --out <成品>`，官方單檔範例為 smartSharpen（amount 80）＋新增曲線調整圖層（curves）後輸出 PNG；`batch --actions grade.json --in <資料夾> --out <資料夾> --format png`，動作清單為含 `actions` 陣列的 JSON（每項 `command`＋`params`）。命令識別碼要用 `commands` 查，不能按中文自行拼；範例數值不是通用推薦參數。
  - **批次預設限制（依目前主線）**：輸出**預設拒絕與輸入同資料夾**；**不遞迴子資料夾**；**省略 `--format` 時沿用各輸入檔副檔名**（單檔範例輸出 PNG 不代表批次也自動輸出 PNG）。JSON 能被解析不代表動作適用，需少量副本試跑。
  - **盤點與分資料夾**：`info` 可輸出文件尺寸、模式、位元深度與圖層資訊，修改前先盤點（找候選文字圖層、確認內容與位置，圖層名稱非唯一依據）；來源、工作副本、輸出分開放，先建檔名清單與命名規則。文字替換的命令與參數要查安裝版命令表（原文主案例只示範銳化與曲線）。
  - **驗收表四項**：工作範圍、編輯結果、交付檔案、工作連續性（可編輯工作檔需重新開啟、改一段文字或關一個效果驗證）。**輸出 PNG ≠ 保留可編輯 PSD**；MCP 不保證所有圖層／效果可處理；工具本機執行 ≠ AI 使用離線（是否傳圖需自行確認）。

**VectorCraft（對應 Illustrator）— 官方頁版本 0.4.0**
- 能做什麼：鋼筆、直接選取、形狀運算、對齊吸附、外觀面板、漸層／混合／重複／封套變形／光暈陰影、圖片描摹、多尺寸輸出。
- 與 Illustrator 的差距：官方主線文件仍列進階效果、排版、操作細節的缺口，**進階 CJK 排版（含直排）有缺口**。
- 檔案相容性：原生格式 `.vectorcraft`（JSON）；SVG、PDF 匯入匯出；PNG／JPEG／WebP 輸出。**`.ai` 僅支援含 PDF 相容內容者**，不保證所有 Illustrator 文件能完整開啟與回存；效果輸出到其他格式可能被展開、簡化或轉成圖片。
- 限制：文章建議轉輪廓前保留可改字的工作檔；描摹只適合輪廓清楚、色少的圖。

### 創投觀點：數位通縮（數位時代 10/20）

> **性質：評論觀點，非事實查證。** 內容為作者轉述 2026-10-17 All-In Podcast 的發言，且文末自註初稿為 AI 編撰。以下只收與 ArtCraft／軟體複製潮相關的論點；其餘（旅展廣告、訂閱日報導流）不收。

- 背景數字：PhotoCraft 截至 10/20 近 6.9 萬顆星；ArtCraft 同期還有對標 Word／Excel／PowerPoint 與 AutoCAD 的專案（與 10/13 篇一致）。
- **Calacanis（悲觀）**：軟體被 AI 抽走「九成價值」，稱「通縮」前所未見；擔心軟體毛利與創投回收（發言者個人看法）。
- **Sacks（客製軟體）**：過去只有大市場值得開發（如 Photoshop），現在企業可為自己寫軟體；程式碼產量已比以前高好幾個數量級；他稱約 95% 程式碼由 AI 撰寫、工程師需求仍上升（發言者自述，未查證）。
- **Horowitz（傑文斯悖論）**：單價下跌、總量暴增；價格崩跌與產業萎縮是兩回事。
- **三種被認為較難被重寫程式碼取代的東西**（節目歸納）：①對手沒有的資料與使用回饋（例：Harvey）；②程式碼以外的授權與內容（例：ElevenLabs 與已故明星遺產方的聲音授權，2024-07）；③電力、GPU、機房（Palihapitiya：「電力才是終極護城河」；Horowitz 稱 token 價格下滑而 GPU 價格未降）。
- **作者自己指出的缺口**：傑文斯悖論只說總量變大，沒說誰分得到；餅可能流向企業自建與新玩家，不一定回到既有 SaaS 公司。
- 前情（作者補述）：2026-01-30 Anthropic 為 Claude Cowork 推出外掛並開源 11 款，2 月初企業軟體股賣壓，被稱為「SaaS 末日」。
- **與本條其他來源的關係**：這是評論層，不能反過來證明 ArtCraft 的完成度；10/14 篇的「軟體價值轉向能否持續改變」是類似方向的個人意見，兩者同為觀點。

### 跨篇矛盾與不一致（只列出，不替來源裁決）

1. **公開日期**：數位時代 10/13 稱七款「9/30 至 10/1 陸續上線」；aiposthub 10/14 稱「10/3 一口氣公開」，同篇又寫 PhotoCraft repo 於 9/30 建立。可能是「repo 建立」與「公開宣布」不同日，但來源未說明。
2. **WebAssembly 數量與意義**：aiposthub 10/7 為六款；TechOrange 10/8 為七款。10/8 單品文又只說 LightCraft、PdfCraft、VectorCraft 提供「可自行架設／部署的 Web 建置檔」，PdfCraft 文章特別說明這**不等於可直接使用的公開線上編輯器**，與 TechOrange「可直接在瀏覽器中使用」的口氣有落差。
3. **一個月 100%**：TechOrange 轉述 Reddit 為「一個月內 100% 功能相容」；數位時代寫「一個月內讓功能追平原版」；Thomas 隨後自改為 99%、數月。三者彼此不同，且都是放話。
4. **經驗年資**：TechOrange 寫「約 10 年大型 Rust 專案經驗」；數位時代寫「自稱 15 年程式開發經驗」。可能是 Rust 與整體程式經歷的口徑不同，來源未說明。
5. **授權表述**：TechOrange「MIT 或 Apache」、數位時代「MIT 與 Apache-2.0 雙授權」、aiposthub 10/14「MIT 或 Apache-2.0」大致相容，但 aiposthub 10/7 提醒各 repo 授權可能不同，需回具體專案確認。
6. **建置時間**：aiposthub 10/14 自述有「幾天」與「數週」兩種說法並存，來源本身未解決。
7. **PSD 往返測試數字與性質**：10/8 PhotoCraft 單品文引 README「psd-tools 309 份中 307 份、混合集 170 份中 169 份」並當作測試成果；10/14 週報寫「135 檔中 134 檔可往返」；10/8 PSD 檢查文則說 307／309 是 0.3.0 評估文件的**最低通過門檻**（畫面比對門檻 229 份），「實際結果需依測試紀錄判讀」。同一組 307／309 在兩篇裡一個被寫成結果、一個被寫成門檻，測試集也各不相同，無法換算，**本條不裁決**。（PSD 檢查文與其他三者的差異僅此，其餘如 134/135 無直接衝突。）
8. **星數成長曲線**（非矛盾，僅時點對照）：PhotoCraft 10/8 凌晨約 15,459 → 10/11 44,649 → 10/14 約 5.9 萬 → 10/20 近 6.9 萬。七款合計僅有 10/11 的 86,265 一個時點。
9. **PdfCraft／PrintCraft 命名**：10/7 兩篇稱 PrintCraft，10/8 單品文稱官方已用 PdfCraft、部分安裝檔仍叫 printcraft（已在表 1 註記，視為同一款）。
10. **「七款皆有自標狀態」與版本號**：10/7 官方頁把 LightCraft、VectorCraft 列為「開發中」，10/8 單品文給出 0.2.1／0.4.0 版本號並提供下載；兩者不衝突，但「開發中」仍有可下載版本，不能用狀態字面判斷可用性。
11. **完成度口徑**：10/7 官方頁只有「Alpha／開發中」；10/14 專案自評提供百分比；10/8 單品文的缺口清單又是另一種量法。三者非同一量尺，不可直接換算。

## C（關鍵行動）

- [ ] 要試用時：先回官方頁 getartcraft.com/apps 與各 repo 確認當下狀態，不沿用本條 10/7 快照
- [ ] 用自己備份過的素材，固定一項任務（輸入、編輯、儲存、重開、輸出）驗收，一次只測一款
- [ ] 引用本條數字時一併帶日期（「截至 10/7」「10/13 星數」），並區分「專案自評」與「第三方測量」
- [ ] 試用單品前，對照上方「單品深度」的缺口清單（CR3 解碼、非拉丁 OCR、`.ai` 僅限 PDF 相容、CJK 排版、生成式功能）確認自己的工作是否踩到
- [ ] 引用「數位通縮」一節時標明是 podcast 發言的二手轉述與評論觀點

## 原始摘要

| # | 來源 | 日期 | 角色 |
|---|---|---|---|
| 1 | [wingzero 網路資源](https://tools.wingzero.tw/articles/4091) | 10/7 | 七款對照與狀態 |
| 2 | [aiposthub 七款解析](https://www.aiposthub.com/artcraft-seven-open-source-creative-apps-alpha/) | 10/7 | 狀態快照、WebAssembly、驗收建議 |
| 3 | [TechOrange](https://techorange.com/2026/10/08/artcraft-software-ai-developer-adobe/) | 10/8 | 轉載 Ars Technica／Crypto Briefing，放話與法律風險 |
| 4 | [數位時代](https://www.bnext.com.tw/article/92597/artcraft-open-source-office-clones) | 10/13 | Office 三款；**文末自註「本文初稿為 AI 編撰」** |
| 5 | [aiposthub 週報（Hank）](https://www.aiposthub.com/ji-tian-fu-ke-photoshop-gong-neng-de-photocraft-dui-shang-yi-ge-ren-zuo-liao-shi-duo-nian-de-photopea-xian-zai-shui-bi-jiao-neng-yong-2/) | 10/14 | PhotoCraft 完成度、Photopea 比較、作者觀點 |
| 6 | [aiposthub LightCraft（Philo）](https://www.aiposthub.com/lightcraft-raw-photo-editor-lightroom-workflow/) | 10/8 | 官方文件整理，無安裝實測；RAW 支援缺口、Lightroom 比較 |
| 7 | [aiposthub PdfCraft（Philo）](https://www.aiposthub.com/pdfcraft-open-source-pdf-merge-split-organize-guide/) | 10/8 | 官方文件整理，無實測；合併／拆分驗收、命名說明、983 檔測試自稱 |
| 8 | [aiposthub PhotoCraft（Philo）](https://www.aiposthub.com/photocraft-open-source-image-editor-ai-agent-psd/) | 10/8 | 官方文件整理，無實測；代理操作、PSD 測試口徑、資料流 |
| 9 | [aiposthub VectorCraft（Philo）](https://www.aiposthub.com/vectorcraft-open-source-vector-editor-svg-pdf-ai-compatibility/) | 10/8 | 官方文件整理，無實測；格式相容與 `.ai` 限制 |
| 11 | [aiposthub PhotoCraft PSD（Philo）](https://www.aiposthub.com/photocraft-photoshop-psd-compatibility-text-mask-effects/) | 10/8 | 官方 0.3.0 文件整理，無實測；三層相容性與文字／遮罩／效果／智慧物件檢查法；文末僅站內延伸閱讀，不收 |
| 10 | [數位時代 數位通縮](https://www.bnext.com.tw/article/92727/ai-software-deflation-saas-moats) | 10/20 | All-In Podcast 二手轉述的創投評論；**文末自註初稿為 AI 編撰** |
| 12 | [aiposthub Claude 搭配 PhotoCraft（Philo）](https://www.aiposthub.com/claude-photocraft-mcp-batch-layer-edit-export-workflow/) | 10/8 | 官方 README／CLI／MCP 文件整理，無實測；MCP 讀寫根目錄、CLI run／batch 與批次預設限制、`info` 盤點、驗收表；文末僅站內延伸閱讀 1 則，不收 |

**來源說明**
- 第 1、2 篇為轉述官方產品頁；第 3、4 篇為二手轉載其他媒體；第 5 篇含作者個人意見。本條沒有任何一手實測。
- 第 4、10 篇自註初稿為 AI 編撰，其數字（星數、授權、批評、發言轉述）請當作待核對資料。
- 第 6～9、11、12 篇為同一作者的同系列文，皆為官方文件整理、明言無本站實測；文中的「延伸閱讀」是站內連結，非廣告，但也不具額外證據力。
- 未收錄：各篇的 AdSense 廣告、課程與活動報名（AI 郵報 OPEN LAB、AI 郵報學院課程）、日報／訂閱導流（含第 10 篇的日報訂閱與旅展延伸閱讀）、站內延伸閱讀；第 5 篇中與 ArtCraft 無關的四個週報議題（Karpathy 漏斗、Reflection AI、Grok Bot 路由、.agent 網域）亦未收。

## 相關連結

- [[claude-opus-5-5-release-notes]] — 這批應用所用的模型（Opus 5.5）發布資訊與價格
- [[implementation-cheap-ownership-valuable]] — 「實作變便宜」的論點，ArtCraft 是其具體案例
- [[ai-era-developer-value]] — 「AI 寫程式後開發者價值何在」，對照單人開發者重寫大型軟體
- [[design-speedup-not-evenly-distributed]] — 設計工具與設計職務的 AI 提速，對照開源設計工具的出現
- [[vibe-vs-agentic-coding-maturity]] — 批評者所說的 vibe coding 與成熟度分級
- [[claude-design-tool]] — 另一條 AI 設計工具路線（Claude Design）
- [[ai-era-five-irreplaceable-human-jobs]] — 軟體實作變便宜之後，人類保留價值的另一種盤點（對照「數位通縮」一節的護城河論點）
- [[open-design-claude-design-comparison]] — 開源設計工具與 Claude Design 的比較
