---
title: 自訂 GPT 與 Gemini Gems 退場：跨廠商遷移到 Skills 對照（2026-10）
date: 2026-10-08
tags: [ai/agent, ai/workflow, type/comparison]
source: https://www.bnext.com.tw/article/92451/custom-gpt-migrate-plugin-skill; https://www.bnext.com.tw/article/91895/gemini-gems-spark-skills; https://www.aiposthub.com/google-gemini-skills-gems-transition/（引 Google 官方 support.google.com/gemini/answer/18560919、17094296、17102773，Workspace Studio 說明 18336943）
type: article
status: processed
asset_value: 64
---

> 📅 **2026-10-08 快照，時程依各家官方公告為準。** Gemini 半邊的時程與限制有 Google 官方 support 頁佐證（三篇兩站一致）；OpenAI 半邊**只有數位時代 2026-10-01 一篇媒體轉述，原文沒附 OpenAI 官方連結**，整列標「媒體轉述待核」。

## T（核心論點）

OpenAI 與 Google 在同一時期把「綁在單一助理角色」的客製化（自訂 GPT、Gemini Gems）收掉，改成**可組合、可攜帶的 SKILL.md**。這正好印證 [[research/skill-platform-comparison-2026]] 的論點：**把工作方法存成可攜格式，比綁在單一平台更保險**。兩家的差別在遷移路徑：OpenAI 多包一層 **Plugin 容器**，而且要**建立者手動遷移**；Google 是**系統自動轉換**，但目前功能落差較大。長期價值不在時程，而在「哪些東西搬得過去、哪些會掉」的清單——每次跨平台移植 Skill 都用得到。

## B（適用場景）

**適用：**
- 手上有自訂 GPT 或 Gems，要在退場前備份與遷移
- 要把本庫的 Skill 搬到 ChatGPT 或 Gemini，先判斷哪些能直接用、哪些要改
- 評估「萬用 GPT／Gem」要不要拆成多個小 Skill

**不適用 / 注意：**
- 🔴 OpenAI 時程（10/26、12/11、2027-02-11）**目前只有媒體轉述**，做決定前先到 OpenAI 官方公告核對
- 🔴 **含網路腳本的 Claude Skill 搬到 Gemini 會失效**：Gemini Skill 內的腳本不能對外部網站發請求（Google 官方上傳限制）
- ⚠️ 來源作者狀態：數位時代韋惟珊〈自訂GPT怎麼遷移〉屬部分實測（作者帳號已看到遷移按鈕）；數位時代蘇柔瑋〈Gems怎麼轉Skills〉有截圖實測，但文末自註「本文初稿為 AI 編撰」；aiposthub〈Google Gemini 推出 Skills〉未實測，但每項事實都附 Google 官方 support 頁
- 作者另附導入建議，見原文

## R（跨廠商對照）

來源等級標記：🟢 **Google 官方**（support 頁）｜🟡 **媒體轉述待核**｜⚪ **既有條目**（本庫其他條目已記錄）｜🔵 **截圖實測**

| 項目 | OpenAI 自訂 GPT | Google Gemini Gems |
|---|---|---|
| 停止新建 | 2026-10-26 🟡 | 原文未載明 |
| 退役／轉換 | 既有 GPT **2026-12-11** 退役；符合延期資格且申請獲准的 Enterprise 工作區可延到 **2027-02-11** 🟡 | 個人帳號 **2026-11** 起；Workspace 商務／企業／非營利 **2027-03**；教育帳號 **2027-06**；現有 Gems 與支援的知識檔**由系統自動轉成 Skills，不是單純停用** 🟢 |
| 遷移方式 | **建立者手動**：「我的 GPT → 遷移到外掛程式」，依建立者陸續開放 🟡（作者帳號已看到按鈕＝部分實測） | **系統自動轉換**；也可提前手動「設定 → Skills → 手動建立」 🟢🔵 |
| 目標格式 | **Plugin**（容器）：指令 → Skill、知識檔 → 參考檔、已連接 Apps 併入 Plugin 🟡 | **Skill**：`SKILL.md`＋同資料夾參考檔；資料夾名＝Skill 名；名稱自動轉成全小寫加連字號（如 `article-editor`） 🔵 |
| 會遺失或須重建 | **不會搬**：原選定的模型、過往對話紀錄、**Custom Actions**（串接公司 API 或自訂服務須另外重建）；遷移後舊 GPT 在退役前仍可用，但**唯讀不可編輯** 🟡 | **暫不支援**：Canvas、Deep Research、影片與音樂生成、引導式學習；分享 Skill、加入 Drive 檔、Gemini Notebook 列為「接下來幾週」（截至 10-01）；Workspace Studio「Ask a Gem」步驟到 2027-03 停止，須改用「Ask Gemini」（Workspace Studio Skills 預計 2026-10-05 起逐步開放） 🟢 |
| 呼叫方式 | 對話中直接叫出 Plugin 或 Skill，Skill 也可能被自動套用 🟡；ChatGPT Skills 慣例為 `@skill名稱` ⚪ | 輸入「/」手動選；或依名稱與描述自動套用已啟用的 Skill；可停用不想自動套用的 Skill；同一對話可組合多個 Skill 🟢 |
| 費用與條件 | 原文未載明；ChatGPT Skills Beta 限工作與教育方案 ⚪（見 [[chatgpt-skills-usage-guide]]） | **聊天中使用 Skills 免費**，只有 Spark 任務與排程需 Google AI Pro／Ultra；條件：年滿 18、個人帳號、開啟 Gemini Apps 活動記錄（Keep Activity） 🟢 |
| SKILL.md 相容性 | ChatGPT Skills 以 `SKILL.md` 為入口、採 Agent Skills 開放格式 ⚪；Plugin 容器與 Agent Plugins 1.0（`plugin.json`）的關係**原文沒寫** | 下載的 Skill zip 內含 `SKILL.md`，資料夾結構與 Claude、Codex 相容 🔵；「Gemini 已支援匯入其他平台的 SKILL.md」🟡（數位時代轉述、未附來源）；⚠️ **Skill 內腳本不能對外部網站發請求** 🟢 |
| 載入機制 | 依描述自動判斷 ⚪ | 先看名稱與描述判斷相關性，再讀完整指令，必要時才開附檔 🟢（官方撰寫指南 17102773，與 Agent Skills 的漸進揭露相同） |

### Gemini：手動加知識檔的步驟（🔵 截圖實測）

1. 「設定 → Skills → 手動建立」，先建出 Skill
2. 下載該 Skill 的 .zip（內含 `SKILL.md`）
3. 把 `SKILL.md` 與知識檔放進**與 Skill 同名的資料夾**
4. 在該 Skill 選「更多 → 取代 Skill」，上傳整個資料夾（建議用網頁版或 Mac 版）

## C（關鍵行動）

- [ ] **遷移前先備份**：名稱、說明、完整指令、知識檔，外加 2–3 組測試提示與當下的基準回答
- [ ] Gemini 備份用 Google Takeout：勾「Gemini」（Gems 資料）＋「我的活動 → Gemini Apps」（對話、生成內容、上傳檔）；Email 下載連結 **7 天有效**，匯出不會刪伺服器資料（🔵 截圖實測）
- [ ] 遷移後用同一組測試提示比對輸出，確認行為沒跑掉
- [ ] OpenAI 端的 **Custom Actions 另外重建**（遷移不會帶過去）；原本指定的模型也要重新確認
- [ ] 把萬用 GPT／Gem 拆成 **2–4 個 Skill**，每個在 description 寫清楚「何時該用」（原則見 [[skill-md-writing-techniques]]、[[agent-skill-design-patterns]]）
- [ ] 要搬到 Gemini 的 Skill，先檢查腳本有沒有對外連線；有的話會失效，要改寫或留在 Claude 端
- [ ] 「讓 AI 直接打包成 SKILL.md＋zip」（蘇柔瑋文中方法三）屬**作者建議、可行性未驗證**，要用先小量試
- [ ] OpenAI 時程待官方公告核對後，回來更新本條

## 原始摘要

- **數位時代〈自訂GPT怎麼遷移？GPT轉Skill、Plugin步驟與停用時間一次看〉**（韋惟珊，2026-10-01）：轉述 OpenAI 公告的自訂 GPT 退役時程與「遷移到外掛程式」流程，列出會搬與不會搬的內容，建議把萬用 GPT 拆成多個 Skill；文中 Gemini 時程與另兩篇一致。原文沒附 OpenAI 官方連結。
- **數位時代〈Gems怎麼轉Skills？3招備份技巧打包AI提示詞〉**（蘇柔瑋整理，2026-10-01，文末自註初稿為 AI 編撰）：引 Google 官方 support 18560919 的分批時程，提供手動複製、Google Takeout、AI 打包三種備份法，並以截圖示範手動重建 Skill 與資料夾結構。
- **aiposthub〈Google Gemini 推出 Skills：常用指令用「/」呼叫，Gems 將分階段退場〉**（Philo，2026-10-01）：Skills 於 2026-09-30 進入 Gemini 聊天；整理呼叫方式、載入機制、使用條件、功能落差清單、腳本限制與 Workspace Studio 遷移，每項附 Google 官方 support 頁。

## 相關連結

- [[chatgpt-skills-usage-guide]] — ChatGPT Skills 本體；其 T 層「GPTs 與 Skills 互補」已加 2026-10-08 時效附註
- [[research/skill-platform-comparison-2026]] — 「存成可攜格式」的原始論點；「Gems 十月底停用」舊說已並列官方時程
- [[gemini-spark-personal-agent]] — Gemini Spark 的「技能」元件就是 Gemini Skills；Spark 任務與排程才需付費
- [[skill-md-writing-techniques]] — description 決定觸發；Gemini 同樣先看名稱與描述
- [[agent-skill-design-patterns]] — 拆 Skill 時的設計模式
- [[sop-to-skill-4-steps]] — 把既有指令改寫成 Skill 的步驟
- [[agent-plugins-open-standard]] — Agent Plugins 1.0（`plugin.json`）；與 ChatGPT Plugin 容器的關係**未明**
- [[claude-code-plugins-guide]] — Claude 端的 Plugin 機制
- [[ai-skills-mcp-plugins-agents-comparison]] — Skills／MCP／Plugins／Agents 的層次區分
- [[google-workspace-notebooklm-gemini-admin-workflow]] — Workspace 端既有的 Gemini 自動化；留意「Ask a Gem」2027-03 停止
