---
title: 讓 AI 介面不再長同一個樣子：先比多個視覺方向、再用主題或 DESIGN.md 固定方向（Hallmark v2、Huashu Design 3.0、Ling UI、GradientDeck、Jakub Krehel Skills）
date: 2026-11-19
tags: [ai/agent, ai/workflow, type/comparison]
source: https://www.aiposthub.com/hallmark-v2-ai-design-skill-themes-browser-checks/（aiposthub／Philo〈Hallmark v2 設計技能更新：48 種主題之外，AI 網站如何少一點模板感？〉，2026-10-23）
source_2: https://www.aiposthub.com/huashu-design-3-style-ledger-reference-board/（aiposthub／Philo〈Huashu Design 3.0：AI 設計怎麼擺脫同一種外觀？〉，2026-10-25）
source_3: https://www.aiposthub.com/ling-ui-design-skill-ming-image-visual-directions/（aiposthub／Philo〈Ling UI Design Skill 是什麼？先比較四種視覺方向，再讓 AI 寫介面〉，2026-10-05）
source_4: https://tools.wingzero.tw/articles/4148（赤紅翼零〈GradientDeck UI：搜尋設計靈感並複製 DESIGN.md，讓 AI 產出的介面更有方向〉，2026-11-04）
source_5: https://tools.wingzero.tw/articles/4077（赤紅翼零〈Jakub Krehel Skills：開源 AI Skills 助你檢視與優化 UI〉，2026-09-24）
official_refs:
  - https://github.com/Nutlope/hallmark（經 aiposthub 轉述，本庫未核對）
  - https://raw.githubusercontent.com/inclusionAI/ling-cookbook/main/resources/recommended-skills/ling-ui-design/SKILL.md（經 aiposthub 轉述，本庫未核對）
  - https://ui.gradientdeck.com/（經 wingzero 介紹，本庫未核對）
  - https://github.com/jakubkrehel/skills（經 wingzero 介紹，本庫未核對）
type: article
status: processed
asset_value: 162
---

AI 能很快生出一頁網站，但打開幾個成品，常是相似的大標題、漸層背景、三張功能卡片；換個題目做簡報，又是同一種奶油底色。本條目合併五篇（raw-pipeline 路徑 ②，各篇 36／30／30／30／36，合計 162），把五個工具放在同一條流程上看：**先比幾個真的不同的方向 → 選定後把方向寫成主題或 DESIGN.md 固定下來 → 用審查 Skill 和瀏覽器檢查驗收**。

> ⚠️ **來源性質**：三篇 aiposthub／Philo（Hallmark、Huashu Design、Ling UI）＋兩篇 wingzero／赤紅翼零（GradientDeck、Jakub Krehel），**全是二手工具介紹，沒有站方實測**（Hallmark 篇明言「沒有把官方示範當本站實測」）。數字（48 種主題、755 張照片、八種寬度、風格庫 60→84 種、四種方向等）是作者查詢當日轉述官方 README，**本庫未回官方倉庫核對**。Huashu Design 作者公開的多輪比較是他的製作經驗，**不是獨立的美感評分**。

## T（核心論點）

> **AI 介面長成同一個樣子，是因為模型傾向重用熟悉的排列；只改配色、只加一句「做得有質感」都擋不住。** 有效的做法是把選擇往前移：**先讓幾個「資訊安排真的不同」的方向攤開比較，選定後把方向寫成可讀取的規則**（主題、DESIGN.md、外觀帳本），代理後續就有依據，不必每次重猜。

三個支撐點：

1. 🔑 **方向要比的是資訊安排，不只是配色**：Huashu Design 指出舊做法常把「三個方向」做成同一張版面的三種配色——使用者看似有選擇，實際上沒比到不同的資訊順序。比較時先問每版**把什麼放第一眼、希望讀者下一步做什麼**
2. 🔑 **主題是材料，不是模板**：Hallmark 的 48 種主題提供字體、顏色、表面材質、元件風格；**內容放什麼、依什麼順序，仍由頁面用途決定**（訂位頁要讓日期／人數／時段先出現，不是先放品牌故事）
3. 🔑 **方向固定之後要寫成規則**：顏色、字級、間距、元件狀態寫成文字（Ling UI 選定後整理規則；GradientDeck 直接給 DESIGN.md；Huashu Design 用外觀帳本記近期交付避免跨專案重複），代理才知道同一個按鈕在不同頁面該維持什麼樣子

## B（適用場景）

**適用：**
- 還沒選定視覺語氣的新產品首頁、活動頁、新功能介面
- 用 AI 代理快速做原型、也願意提供真實內容（品牌名、數據、產品截圖）的團隊
- 想替既有專案加上「改版前保護清單」與「交付前瀏覽器檢查」的人

**不適用 / 注意：**
- 已有完整設計系統的產品：應沿用既有規則，避免每次新功能都長出新的視覺語言（Ling UI 篇、Hallmark 篇都提醒）
- 資訊架構還不清楚時：漂亮的方向圖代替不了功能排序與內容設計
- 🔴 **設計稿是圖片**（Ling UI 用圖像模型生成），無法證明按鈕可點、表單可送出、手機可讀；生成畫面裡的文字也要重整
- 只給一句「做得漂亮」：再多主題也無法替你決定內容順序（Hallmark 篇結語）

## R（工具與流程）

### 1. 五個工具在流程上的位置

| 工具 | 流程位置 | 核心做法 | 安裝／取得 |
|---|---|---|---|
| **Ling UI Design Skill**（inclusionAI） | 比方向 | 搭配 Ming-Image-0.1-Design 圖像模型，先生成 Clean／Warm／Dark／Bold 四種方向的設計稿，選定再實作 | 官方 SKILL.md（ling-cookbook） |
| **Huashu Design 3.0**（花叔，MIT） | 比方向＋跨專案去重 | 六個候選 → 挑三個做成真實畫面；參考板先截圖；外觀帳本記近期交付 | 官方技能安裝指令或直接取程式庫（原文未列網址；更新後要確認腳本、參考、資產目錄都在） |
| **Hallmark v2**（Nutlope） | 定方向＋驗收 | 先確認頁面要完成什麼 → 從 48 種主題推薦三個 → 首頁主視覺先做先審 → 八種寬度瀏覽器檢查 | `npx skills add nutlope/hallmark -g`（`-g` 全域；拿掉則裝到目前專案） |
| **GradientDeck UI** | 定方向 | 真實網站設計語言整理成 DESIGN.md，一鍵複製給代理；免費、免帳號 | 網站 ui.gradientdeck.com |
| **Jakub Krehel Skills** | 驗收與修整 | 一組 UI 審查 Skill：`better-ui`／`better-typography`／`better-colors`／`better-accessibility`／`better-layout`／`better-writing`，加 `interface-review`、`explain-interface`、`break`、`variant` | `npx skills add jakubkrehel/skills`（另有 Claude Code 外掛市集） |

（Hallmark 支援 Claude Code、Codex、Cursor、OpenCode；GradientDeck 的 DESIGN.md 可給 Claude Code、Codex、Cursor。這些 Skill 都是**工作指引**，實際寫程式的仍是你的代理與模型，不是新的模型。）

### 2. 比方向：怎麼讓「幾個方向」真的不同

- **Huashu Design 3.0 的六選三**：三套思路各出兩個候選——**適合題目的設計師／同媒介的真實參照／從風格庫取的不同路徑**；縮圖攤開六個，代理挑出彼此有差異、也較少與近期專案重複的三版做成真實畫面，沒選中的保留當備選。風格庫 60→84 種，補了 App 與中文、東亞設計樣張（⚠️ 數量不保證每個題目都有合適答案）
- **參考板要先截圖**：只在需求裡寫設計師名字，模型會回到記得的表面印象，做出說不出來源的拼貼；截下同類媒介的實際畫面，才能拆解格線、標題比例、留白。🔴 互動網站截圖可能只剩一片顏色——官方流程把這種結果視為**沒看到**，不能拿空白截圖證明研究過
- **Ling UI 的四種方向**：Clean（留白克制）／Warm（柔和暖色）／Dark（深色對比）／Bold（大標題色塊）。分三層：圖像模型出設計稿 → 技能安排探索流程 → 程式代理實作。比方向時**放入接近成品的真實標題與段落**（兩個英文單字的稿子換成一長串繁中可能撐不住）；已知需要低干擾介面就不必湊滿四種
- **Jakub Krehel 的 `variant`**：建立多種可比較的設計版本，屬於同一類做法
- 比較三版時：先看主圖、欄位、閱讀節奏是否真的改變；只換背景的多版本只增加審查時間；差異大到漏掉必要資料的那版也不能為了多樣而留

### 3. 定方向：把選擇寫成代理讀得到的規則

- **Hallmark 主題**：每種主題附完成頁面示範圖；看示範時說清楚喜歡的是什麼（較大字級、較安靜留白、適合長文的行距），比「照這張做」更容易轉成可維護的要求。也可以要求自訂方向，此時放下既有主題但**保留共同底線與檢查**
- **GradientDeck DESIGN.md**：依名稱、風格、產業、明暗、色彩組合、字型搜尋真實網站案例 → 開設計頁 → 複製 DESIGN.md 放進專案 → 要求代理依文件的色彩、字型、排版、元件規則做指定頁面。⚠️ 它是參考庫，不是網站編輯器；**複製一份文件不會自動得到相同網站**，仍要檢視與校對。DESIGN.md 格式本身見 [[design-md-for-ai-ui]]
- **Huashu Design 外觀帳本**：記錄已交付作品的樣子，下次選方向時對照，處理的是**跨專案**重複（和這一次的三版是否不同是兩件事）；另有記錄品味原話與教訓的工具。紀錄寫在本機專用目錄、不隨技能散布。⚠️ 記錄要保留適用情境，否則一次針對照片的意見會被錯套到所有產品介面；團隊要分清哪些是品牌規範、哪些只是某次提案的偏好
- **Hallmark 讀既有專案**：先讀字體、配色、框架與已鎖定的設計文件，避免新頁面局部漂亮卻和其他頁面完全不同

### 4. 驗收：從「看起來沒問題」變成可定位的證據

- **Hallmark 三層檢查**：
  - 底線（不能帶著上線）：對比度、鍵盤焦點、減少動態偏好、**捏造事實**、手機版面損壞
  - 模型慣性（可例外但要留理由）：每個標題上方都放短標籤、整頁只剩空框、不適合的頁面結尾硬放訂閱表單
  - 工藝修整（不阻擋交付）
- **Hallmark 瀏覽器檢查**：八種寬度，完成前安排**一位沒有先前對話背景的代理**重新審視。🔴 瀏覽器渲染與截圖需要 Chrome 與相關套件，缺少時只跑靜態檢查——看到「通過」要分清它看過瀏覽器畫面還是只讀了程式檔
- **Hallmark 首頁主視覺先做先審**：問訪客看完能否一句話描述服務、知不知道下一步；🔴 規則也會留意**表單只顯示感謝、其實沒把資料送出**的情況，要把互動結果一起驗收
- **Hallmark 改版保護清單**：先記錄連結、欄位、事件處理、標題，完成後檢查是否保留；可指定只改風格或只修細節；audit 模式只報告不修改。另有 755 張 Wikimedia Commons 開放授權照片，**各張保留自己的授權條件**，換用途要重新核對
- **Huashu Design 設計體檢**：圖片失效、字級、對比、主控台錯誤，並比對候選是不是只換一層外皮。🔴 部分腳本需要 Python 的 Playwright、Pillow 與瀏覽器，缺依賴時部分檢查會跳過並提示——**成品產生了不代表全部檢查都跑過**。可編輯簡報驗收：直接改一個標題看換行與版面；交付只有整頁圖片就不是可編輯
- **Jakub Krehel Skills**：`better-interface` 整合各領域檢查並要求以實際證據支持；`interface-review` 檢視分支、PR 或未提交變更對介面的影響；`break` 把元件放到不同狀態做壓力測試。特色是要求依專案**現有**的框架、樣式系統、元件庫提出建議，而不是套另一套技術。⚠️ 仍要確認代理有讀相關專案文件、有看實際畫面
- 共通原則：字夠大 ≠ 資訊容易理解（報名時間放最下面，每個字都清楚也可能被錯過）——美感與資訊順序的判斷仍要回到頁面用途

## C（關鍵行動）

- [ ] 下一個新頁面：先要三個方向（用 Huashu Design 或 Ling UI），比較時只問「第一眼放什麼、下一步是什麼」，不先比顏色
- [ ] 選定方向後，把顏色、字級、間距、元件狀態寫成 DESIGN.md（可從 GradientDeck 找起點），放進專案
- [ ] 改既有頁面前先跑 Hallmark audit 或 Jakub Krehel `interface-review`（只報告不修改），再決定要不要重做
- [ ] 驗收時確認「檢查通過」是看過瀏覽器畫面還是只讀了程式；實際完成一次主要任務（表單有送出、連結到對的位置）
- [ ] 待驗證：五篇都是工具介紹、無實測；自己用同一頁需求比「有比方向／沒比方向」一次再評估

## 原始摘要

- **Hallmark v2 篇（36）**：從頭重建的設計技能；先問頁面用途 → 48 種主題當材料（推薦三個）→ 首頁主視覺先做先審 → 真實產品截圖與 755 張開放授權照片 → 改版保護清單 → 三層檢查與八種寬度、無背景代理重審；試用建議從單一活動頁開始並保留原版
- **Jakub Krehel Skills 篇（36）**：UI 審查與迭代 Skill 組（better-* 六個＋interface-review／explain-interface／break／variant），依專案既有慣例提建議、以證據支持
- **Huashu Design 3.0 篇（30）**：六候選選三、參考板先截圖、外觀帳本、設計體檢、簡報／原型／動畫分別驗收、依賴與素材權利；10-23 公開介紹 vs GitHub 標籤 10-17 是兩個不同記錄
- **Ling UI 篇（30）**：Ming-Image-0.1-Design 生成四種方向設計稿，圖像模型／技能／程式代理三層分工；設計稿不是可用介面
- **GradientDeck UI 篇（30）**：真實網站設計參考庫，一鍵複製 DESIGN.md，免費免帳號

## 相關連結

- [[claude-code-design-skills-overview]] — 設計 Skill 精選清單；該條把 Hallmark 標為「無專篇的新線索」，本條補上 v2 的流程細節
- [[taste-skill-anti-slop-frontend]] — 另一條反模板路線：用固定風格規則與三旋鈕（VARIANCE／MOTION／DENSITY）壓住模板感；本條主打「先比方向再固定」
- [[ai-ui-acceptance-unslop-and-design-reference]] — 給參考與驗收拆成可檢查項目（unslop 九項、換名測試、狀態表）；本條第 4 節的驗收工具可與它搭配
- [[design-md-for-ai-ui]] — DESIGN.md 概念與 awesome-design-md 範例集；GradientDeck 是另一個 DESIGN.md 來源
- [[impeccable-style-ai-coding-guide]] — Impeccable v4.x 的「隱喻候選 → 抽籤 → 人選」也是先攤開方向再選
- [[claude-video-animation-prompts]] — 同一套「先比三個方向的關鍵影格再做」用在動畫（Huashu-motion，與 Huashu Design 同一位作者）
