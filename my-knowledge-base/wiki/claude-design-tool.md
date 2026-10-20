---
title: Claude Design：文字描述→設計稿的 AI 設計工具（Research Preview）
date: 2026-06-15
tags: [ai/claude, dev/flutter]
source: https://medium.com/ddsakura-blog/claude-design-嘗鮮-我用它幫自己的-app-設計新功能-8b1791e8c325
type: article
status: processed
asset_value: 72
---

Claude Design 是 Anthropic 推出的 AI 設計工具（Research Preview），整合在 claude.ai 網頁版，目標是把文字描述直接轉換為設計稿，定位挑戰 Figma。目前 Pro / Max / Team / Enterprise 方案皆可使用，但**用量消耗快，需謹慎管理**。

> ⚠️ 2026-10-19：Claude Design 獨立網站將於 **2026-12-14 關閉**、作品改存為 Artifacts，搬移與備份見文末〈補充資料：獨立網站 12-14 關閉與搬移〉。另：本條目導言稱「整合在 claude.ai 網頁版」、〈與既有入口的關係〉表稱「獨立產品」，兩者描述不同；依 2026-10-08 官方搬移指南（經 aiposthub 轉述），獨立網站與 Claude 內的設計功能並存至 12-14，此處並列不擇一。

## T（核心論點）

Claude Design 對獨立開發者最大的價值不是「取代設計師」，而是**讓你能「討論設計」**——從需求模糊到設計稿有一個可操作的工具鏈，不再卡在「腦袋裡有畫面但畫不出來」的瓶頸。

## B（適用場景）

**適用：**
- 獨立開發者需要設計 App 功能（沒有設計師）
- 快速原型：把模糊需求轉成可討論的視覺稿
- Claude Code 開發流程的設計端輸入

**不適用（目前）：**
- App 版本（目前僅支援 claude.ai 網頁版）
- 用量敏感場景（Claude Design 用量獨立計算，消耗快）
- 需要 design token 或設計系統整合的場景

## R（工作流詳解）

### 推薦工作流（省用量版）

```
Step 1: 一般 Claude 對話討論需求
        → 釐清功能方向（省 Claude Design 用量）

Step 2: 整理具體 prompt + 截圖
        → 讓 Claude 幫你整理成結構化 prompt
          （頁面結構 / 視覺風格參考 / 具體元素）
        → 附上一張最相關的截圖當風格參考

Step 3: Claude Design 生成 + 微調
        → 初稿生成後，描述修改點（如「頂部改成純色」）
        → 它能記憶你上傳的截圖，不需重複上傳

Step 4: Claude Code 實作
        → 把設計稿截圖貼給 Claude Code
        → 說「根據這張設計圖實作 SwiftUI」
        → 或使用「Handoff to Claude Code」功能（Beta）
```

### 實際效果（Fudo Note App 案例）

- 原需求：Favorite 頁和 Browse 頁 layout 重複，需要改
- 討論結果：改成「足跡 Journal」— 有數字成就感 + 排行榜 + 最愛分類的個人飲食回顧
- 視覺方向：Spotify Wrapped 的成就感 + Apple Health 的數據視覺化
- Claude Design 生成後微調一次即完成（頂部漸層→純色）
- 最終輸出：截圖貼給 Claude Code → 產出 SwiftUI code

### 目前限制

- Research Preview 階段，功能和穩定性持續迭代
- 用量獨立計算（與一般對話分開）且消耗快
- 僅限網頁版，App 尚未整合
- 設計稿精細度和真實 Figma 設計仍有差距

### 四款 Claude 工具比較（簡報製作角度）

| | Claude Chat | Claude Design | Claude Cowork | Claude for PowerPoint |
|--|------------|--------------|--------------|----------------------|
| **定位** | 萬用 AI 對話助手 | 對話式視覺設計工作台 | 桌面 AI 代理人 | 內嵌 PowerPoint 的 AI 助理 |
| **能否產出 PPTX** | ✓（直接下載）| 間接（HTML → 匯出 PPTX）| ✓（存本機）| ✓（直接修改現有檔案）|
| **視覺設計品質** | 中等 | 高（Tweaks/Edit/Draw）| 中等 | 高且最穩定（依母片範本）|
| **品牌範本支援** | 可上傳但不保證 | 可設定 Design System | 需文字描述 | **最強**（讀取 Slide Master）|
| **局部修改** | 有限 | Chat/Draw/Edit 三種模式 | 任務型，無法即時點選 | **最精準**（選取特定頁面）|
| **即時預覽** | ❌ | ✓（右側畫布即時更新）| ❌ | 在 PPT 視窗內 |
| **安裝需求** | 無需 | 無需 | 需 Claude Desktop | 需 Microsoft 365 + 增益集 |
| **使用門檻** | 最低（免費版可用）| 中（Pro/Max/Team/Enterprise）| 中 | 最高 |
| **最適場景** | 快速草稿、免費方案 | 邊對話邊預覽的有設計感簡報 | 多資料整合、定期自動生成 | 已有品牌範本、嚴格規範的商務 |

**一句話選擇：**
- 快速草稿可下載 → **Chat**
- 邊對話邊看結果、有設計感 → **Design**
- 多資料整合、背景自動排程 → **Cowork**
- 已有公司 PPT 範本、在 PowerPoint 裡操作 → **for PowerPoint**

### Claude Design 使用技巧（6 條）

1. 先上傳素材再下指令（給 Claude 有料可讀）
2. 指定頁數與頁面結構（「8 張，含封面/痛點/架構/定價」）
3. 第一版確認架構順序，再微調細節
4. 修改要具體（「第 3 頁標題 40px 改 60px」而非「感覺不對」）
5. 善用 Tweaks 探索風格方向，不要一直重新生成
6. 對方向不確定時：「給我封面 3 個不同配色版本讓我選」

## C（關鍵行動）

- [ ] 測試 Step 1 先在一般對話討論需求的省用量策略
- [ ] 評估 Claude Design → Claude Code 的完整流程是否能替代 Figma → 工程師流程
- [ ] 關注「Handoff to Claude Code」功能是否正式推出

## 相關連結

- [[flutter-google-io-2026]] — Flutter 3.44：A2UI（AI 生成 Compose UI）類似理念的 Google 版本
- [[claude-code-tools]] — Claude Code 的 Read/Edit 工具（設計→實作的後半段）
- [[uiux-pro-max-skill]] — UIUX Pro Max Skill（更系統化的 AI 前端設計 Skill）
- [[claude-code-design-skills-overview]] — 六款 Design Skill 精選
- [[open-design-claude-design-comparison]] — Open Design vs Claude Design 完整比較
- [[figma-ai-make-mcp-design-agent]] — Figma AI（Make/MCP/Design Agent）另一 AI 設計工具生態對照
- [[claude-opus-5-5-release-notes]] — Opus 5.5 互動教學頁需求範本與四項驗收（手機、純鍵盤、邊界值、數值來源）
- 📎 2026-10-19：[[claude-dashboards-beta-2026-10]] — 同為 Artifacts 範本、同受組織 Artifacts 開關控制；[[claude-cowork-merged-into-chat-2026-09]] — 09-16 Claude Design 整合進對話（本次關站的前一階段）
- 📎 2026-10-19：同日官方公告（`dashboards-and-motion`，Team Lead 核對）說明 **Docs／Slides／Design 已脫離 beta**；另推出 Motion（解說動畫、MP4，Team／Enterprise beta），與設計畫布的原型、Export HTML 分工不同 → [[claude-motion-beta-2026-10]]
- 📎 2026-10-19：rar.design 觀察 Opus 5.5 首週熱門動畫作品裡沒有 Claude Design 的作品，有講明介面的多半在 Claude Code 完成；作者認為 Claude Design 適合 UI 動效、行銷短動態（作者觀點，有倖存者偏差）→ [[claude-code-pure-code-animation-practice]]

## 補充資料（來源：犬哥網站〈Claude Design 是什麼？〉）

補充本條目主題（Claude Design 定位與工作流）的功能細節，來源角度為功能總覽：

- **定位**：Anthropic 2026 年 4 月推出，定位於「設計 → 原型 → 視覺驗證」中游的 AI 視覺設計工作台。
- **Tweaks 可視化調整面板**：不必每次重新生成，用可視化面板探索與微調風格方向（呼應本條目使用技巧第 5 條）。
- **微動畫互動**：生成的原型支援微動畫互動效果，不只是靜態視覺稿。
- **三大產出**：Prototype（原型）、簡報、品牌設計系統，用自然語言生成。
- **Canva 無縫交付**：除了 Handoff to Claude Code，也能無縫交付到 Canva 繼續編輯。
- **Research Preview 限制**：不支援多人即時協作、不支援精細向量繪圖（與本條目「設計稿精細度與真實 Figma 仍有差距」一致）。

## 補充資料：Claude Code 新增 `/design` 指令（2026-08-19 併入）

來源：https://www.bnext.com.tw/article/91917/claude-code-design-command （Anthropic 產品設計師 Nate Parrott 於 2026-08-17 宣布早期預覽；Claude Code v2.1.235）

### 它把順序倒過來

> **瞄準的老問題：開發者只能用文字描述想要的畫面，生成結果不對就重來一輪。**
> **`/design` 把順序倒過來——先看圖、再施工。**

用法：在終端機或桌面版輸入「`/design` 為這個功能設計幾個選項」→ Claude 產出**多張可編輯的介面設計稿（畫板）**
→ 挑一張直接改 → 讓 Claude 照著實作。執行 `claude update` 後即可試用。

✅ **它會讀取你的程式碼庫、比對現有 UI 風格**，把設計稿做成可分享的模擬畫面放進 Artifact。
編輯器與提示功能是**直接把 Claude Design 整合進 Claude Code**，可在畫布上調整而不必回頭改文字指令。

### 🔴 最重要的一句限制（官方自己說的）

Parrott 坦言**實作是否遵循設計「取決於模型」**，只是模型「在這方面變得越來越好」。所以：

> 🔑 **畫板是給模型看的施工圖，不是硬性規格——成品仍可能與設計稿有落差。**

⚠️ 另一個當前限制：**這版預覽仍要手動按「儲存」設計才會帶進實作**（官方表示很快會取消這個步驟）。
被問到畫板內容會不會延續到建置步驟時，Parrott 回覆「會延續過去」。

### 與既有入口的關係

| 入口 | 做什麼 |
|---|---|
| **Claude Design**（2026-04-17 研究預覽）| 獨立產品，對話生成設計／原型／簡報，會讀團隊 codebase 與設計檔自動建立設計系統；完成後打包「交接包」一道指令交給 Claude Code |
| **`/design-sync`**（Claude Code 既有）| 把程式庫裡的 **React 設計系統上傳到 Claude Design**，讓設計採用團隊真實元件 |
| **`/design`**（本次新增）| 🔑 **新意在入口**：把「產生設計選項 → 直接編輯 → 交給 Claude 實作」整套縮成 Claude Code 裡的一個指令，**不必先從 Claude Design 或 MCP 流程進入** |

📎 2026-10-19：獨立網站將於 2026-12-14 關閉，見文末〈補充資料：獨立網站 12-14 關閉與搬移〉。

### ⚠️ 三個官方還沒回答的問題（決定它能不能進正式流程）

1. **畫板會存成程式庫裡的檔案，還是只存在單次工作階段？**（前者才能讓設計審查進入 PR 流程）
2. **能否載入已在 Claude Design 建好的設計系統？**（決定能不能接上既有設計資產）
3. **當實作結果偏離畫板時，Claude Code 會不會主動修回來？**

> **在這三個答案出爐前，`/design` 比較像寫程式前的快速草圖工具。**

（⚠️ 第 3 點正是 [[ai-era-quality-assurance-test-oracle]] 的核心問題在設計領域的版本：**沒有驗證機制的規格只是建議**；
而「先產生可討論的版本」這個價值見 [[four-vendor-ai-tool-selection]] 收錄的「終於有一個東西可以開始嫌了」）

## 補充資料：2026 新版整理——匯入、`/design-sync`、共用額度池與匯出擴增（2026-09-15 併入）

來源：https://www.aiposthub.com/claude-design-new-features-design-system-code-sync/ （整理 4 月研究預覽、6 月工作流更新、8 月 Help Center 補完的演進）

- **從 GitHub／設計檔／程式庫匯入設計系統的具體流程**：新版可從 GitHub 程式庫、設計檔、原始上傳檔或本機程式碼匯入一個或多個設計系統，Claude 會用這些元件產生作品並檢查輸出是否符合設計系統；來源本身混亂或缺元件時，產出規則也會一起受影響。團隊導入流程可簡化成四步：①切換到正確組織並完成 onboarding；②上傳程式庫／簡報／設計稿／Logo／色票／字體資料；③檢查 Claude 抽出的顏色、字體、元件與版面模式；④建立測試專案驗證，確認後再開啟 Published（此處 Published 指「提供給組織內專案使用」，不等於公開發布網站）。
- **`/design-sync` 指令與 MCP Server 連線指令**：`/design-sync` 是 Claude Code 裡用來同步設計系統的指令（把程式庫裡的設計系統上傳到 Claude Design，讓設計採用團隊真實元件）；MCP 連線方式為：
  ```bash
  claude mcp add --scope user --transport http claude-design https://api.anthropic.com/v1/design/mcp
  ```
  加入後執行 `/design-login` 完成登入，即可從 Claude Code 建立、編輯或同步 Claude Design 專案。兩者可搭配使用，不互相取代。
- **使用額度已改為與 Chat/Code/Cowork 共用方案池**：這是與早期版本（本條目原記載「Claude Design 用量獨立計算，消耗快」）的重要差異——新版不再有獨立每週額度，改成與 Claude Chat、Claude Code、Claude Cowork 共用同一個方案使用量池，複雜專案、較大程式庫與更多修改輪次會消耗較多用量，達方案限制後需等額度重置（已啟用 usage credits 的帳號可繼續使用額外額度）。
- **匯出目的地擴增**：除原有的截圖／Handoff to Claude Code／Canva 外，新版可下載 `.zip`、匯出 PDF、PPTX 或獨立 HTML，官方列出的外部目的地擴增到 Adobe、Base44、Canva、Gamma、Lovable、Miro、Replit、Vercel、Wix 等。
- **團隊導入建議**：Team／Enterprise 最重要的導入順序不是先讓所有人生成頁面，而是由熟悉品牌與產品介面的 **2–4 位設計師先建立並驗證設計系統**，再逐步開放給設計、產品與其他部門；一個組織可有多個設計系統，但應清楚命名用途與擁有者避免選錯版本。Enterprise 目前尚未支援 Claude Design audit logs。


## 補充資料：獨立網站 12-14 關閉與搬移（2026-10-19 併入）

來源：https://www.aiposthub.com/claude-design-standalone-migration-december-14/ （Philo，2026-10-09；整理官方搬移指南 https://support.claude.com/en/articles/17440474-migrate-from-standalone-claude-design-to-claude 與 Artifacts 管理指南 https://support.claude.com/en/articles/16994751-artifacts-admin-guide-for-team-and-enterprise-plans ）

> ⚠️ 官方連結未經本次核對（2026-10-19），以下事實經 aiposthub 單一媒體轉述；作者明言**未替任何帳號執行搬移**（未實測）。專案搬移方案官方尚未公布，本段日後可能需要更新。

- **時程**：2026-10-08 宣布；獨立網站在 **2026-12-14 前**仍可使用，關閉後該入口導向 Claude。不是停止提供 Claude Design，而是設計功能集中到 Claude 裡。
- **新作品形式**：新建立的設計、設計系統與簡報都成為 **Artifacts**。

### 會搬／不會搬／會失效

| 類別 | 官方安排 |
|---|---|
| ✅ **設計系統**（可先搬） | 搬移一次涵蓋**整個組織**（含其他人的設計系統），同一輪只需一人啟動；已公開且非私人的設計系統搬移後**分享給組織所有成員**，個人方案不會分享；重新搬移時**不覆寫**已在新環境修改過的設計系統（會被跳過，不一定是失敗）；無法搬移時會列原因（內容太大、檔案問題、組織功能未啟用） |
| ⏳ **專案**（暫不需操作） | 仍在獨立版運作至關閉；出現在新作品清單不代表已成為新環境中的可編輯專案；**專案搬移細節「之後公布」** |
| ❌ **聊天與留言** | **不隨設計系統搬移**，網站關閉後無法再查閱 |
| ❌ **公開連結** | 獨立網站專案的公開連結在關閉後**失效** |

### 限制條件

- 設計功能依賴 Artifacts：組織若**關閉 Artifacts**，成員不能在 Claude 建立設計，也不能搬移；搬設計系統還須啟用 **Design systems 範本**
- 組織擁有者可管理作品功能、範本與分享設定，Enterprise 可搭配自訂角色分配能力
- 🔴 採**零資料留存（ZDR）**的組織不能啟用 Artifacts，**無法走同一路徑搬移**，須依自身設定另行安排保存

### 逐一下載專案（官方步驟）

在舊網站開啟作品 → **Share** → **Project HTML** → 保留 **Project archive** → 匯出 **ZIP**。

📎 可互證：本條 2026-09-15 補充段的「可下載 `.zip`／獨立 HTML」與這條官方 ZIP 步驟一致；同段「Published＝提供給組織內使用」與「公開且非私人的設計系統分享給全組織」一致。

### 作者建議（aiposthub／Philo，非官方程序）

- 依資料類型分四份清單盤點：設計系統／既有專案／聊天與留言／公開連結；不能把「換入口」當成「全部搬好」
- 約定一個切換時間，避免有人繼續改舊環境、有人已在新環境製作，兩邊視覺規則分岔
- 搬移後讓一般成員實際建一份小作品，確認能選到預期的設計系統
- 下載後確認檔案能打開、素材齊全、負責人知道放在哪裡；重要決定（如核准依據）在期限前另存

**待追蹤**：專案搬移方案官方「之後公布」——公布後回來補本段，並確認 12-14 後舊專案的取用方式。
