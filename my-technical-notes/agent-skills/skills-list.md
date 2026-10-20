---
title: Claude Skills 完整列表
date: 2026-06-01
tags: [ai/agent, ai/claude]
type: reference
status: processed
source: /Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/SKILL_INDEX.md
---

> 完整 Skill 存檔位於：`/Users/alexander/Desktop/紀錄文件資料夾/實驗用文件/Skill存檔/`
> 最後更新：2026-10-08（新增 `point-reading-diy` v0.3.2：卡米點讀筆 DIY 點讀包製作；**開發中、未安裝**，存檔於 `point-reading-diy/`）

---

## 知識管理類（pkm）

| 名稱 | 描述 | 版本 | 更新時間 |
|------|------|------|----------|
| `knowledge-organizer` | 使用 TBRC 四層框架處理 raw/ 素材，產出結構化 wiki 條目；含資產化價值評分；支援 PDF；v1.6.0 標記制度改分流：`.md` 改回 Edit frontmatter `status`（僅此一行），非 md 維持 `.processed` 旁車標記；v1.6.1 `touch` 檔名改單引號避免 shell 展開 `$` | 1.6.1 | 2026-08-13 |
| `daily-brief-generator` | 掃描 Vault 產出每日簡報；今日建議行動改用 SCQA 框架（S 現況/C 衝突/Q 問題/A 行動）；v1.5.0 raw `.md` 積壓判斷改回只看 frontmatter，並補上非 md 掃描的存在性檢查 | 1.5.0 | 2026-08-13 |
| `wiki-health-check` | 每月健康檢查（七項，含精粹過時偵測）+ 5A+ 階段評估；v1.8.0 三項方法修正：精粹過時改用**來源清單集合差**（廢止 `source_count` vs glob 總數，該法兩邊反向偏差會互相抵銷成假陰性）、連結掃描**先剝除程式碼區塊**並解析路徑寫法（否則圍籬內的 regex 會誤報）、流程新增**先讀前一份報告**（不重複列已判定的誤報，並須覆核前次矛盾是否已修）| 1.8.0 | 2026-09-03 |
| `raw-pipeline` | 三 Agent 軟編排批次處理，支援 12 種格式（含 PDF/圖片），適合 raw/ 積壓 10 篇以上；v1.9.0 標記制度改分流（`.md` frontmatter／非 md 旁車標記）；含「排除項目」滾動日誌區塊；v1.9.1 `touch` 檔名改單引號 | 1.9.1 | 2026-08-13 |
| `knowledge-synthesizer` | 多篇同類型 wiki → 一篇精粹條目（CAVE：共識/獨特視角/知識缺口/精粹論點）| 1.0.0 | 2026-05-13 |
| `alignment-check` | wiki 補充資料段落歸屬對齊檢查；找出放錯位置的段落，建議搬移目標 | 1.0.0 | 2026-05-22 |
| `knowledge-elevation` | 三層昇華：E 層加深 / 跨主題 meta-synthesis → wiki/concepts/ | 1.1.0 | 2026-05-22 |
| `obsidian-second-brain` | Karpathy LLM Wiki 進化版；自動維護 Vault，素材重寫現有頁面，矛盾自動調和 | — | — |
| `scientific-brainstorming` | 開放式研究腦力激盪、跨學科連結探索、研究缺口識別 | — | — |

---

## 開發類（dev）

| 名稱                                | 描述                                                                                                                                                                                                                                                                              | 版本        | 更新時間       |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | ---------- |
| `code-review v1`                  | Flutter + Riverpod 專屬 Code Review，分級報告（Critical/Warning/Suggestion）                                                                                                                                                                                                             | 1.0.0     | 無記錄        |
| `code-review v2`                  | 多技術棧 Code Review，自動偵測 Flutter/Spring Boot/Android                                                                                                                                                                                                                               | 1.1.0     | 2026-01-08 |
| `stack-review` (原 code-review v3) | 多技術棧輕量 review + /doc-update 提示；主線工作流已改為 ai-pair，此 skill 為小改動替代方案。**v1.3.0 Step 1 強制先 `git fetch` 並以 `origin/main` 為 diff 基準，分支落後須先合併再 review；v1.4.0 `common.md` 新增「跨服務契約（enum/DTO）」檢查，並標明 review 的能力邊界（後端新增 enum 值、前端零 diff 的漂移 review 抓不到，須靠契約測試）**                            | 1.4.0     | 2026-07-29 |
| `requirements`                    | 模糊需求轉標準化任務文件，含影響範圍分析、風險清單；v1.2 修正 metadata header 格式統一（頂部 block，移除底部散落欄位）。**v1.3.0 接上 `project-documentation` 交接斷點**：Step 2 拆為 2a（先讀本 repo 與上游 repo 的架構／API／變更歷程文件 ＋ 可信度判斷：標 `⚠️ 推導未驗證`／`source-commit` 過期／無該欄位者皆須翻原始碼；JSON 鍵命名與包裝層數預設不採信文件）／2b（程式碼探索並記錄結論來源）；風險清單加跨服務契約 enum・DTO・權限資源；metadata 加 `後端依賴`（上游變動反查下游任務的唯一缺口）與 `參考文件基準`；範本加「契約依據」章節 。**v1.4.0 加「後端契約同步模式」**（非互動，供 b2b-backend Stop hook 以 `claude -p` 呼叫；硬邊界：只能 append 末尾「⚠️ 後端契約變更（自動同步・待確認）」區塊，不得改 metadata／新建任務文件／跑 commit・build_runner，命中 0 份時不准猜） | 1.4.0     | 2026-07-30 |
| `doc-update`                      | code review 通過後更新任務文件狀態，同步 CHANGELOG.md                                                                                                                                                                                                                                         | 1.1.0     | 2026-03-18 |
| `flutter-b2b-documentation`       | Flutter B2B 專案文件產出（已由 project-documentation 取代）                                                                                                                                                                                                                                 | 1.0.0     | 無記錄        |
| `project-documentation`           | 多技術棧文件產出（Flutter/Spring Boot），**六種**文件類型（v2.1.0 新增「變更歷程文件」）。**v2.1.0 可信度強化**：原則 1「不得產出流程未要求的內容」（起因：範本只要求型別名稱，AI 自行補整份 JSON 範例且鍵命名全錯 396 處，下游照抄後整頁解析失敗）、`⚠️ 推導未驗證` 標記、`source-commit` 必填、wire 契約推導階段 B（序列化命名策略／包裝類實際欄位／enum 值／手寫 toJson）、Step 0 更新不覆蓋、Step 5 產出自檢、命名與路徑改配合專案既有慣例。**v2.2.0 補上專案自有版本／日期慣例的處理**（起因：b2b-backend 三份文件用自己的「文件版本」／「最後更新」footer + 版本總覽表，而非 YAML frontmatter，更新時被漏改，且日期一度誤填成最後一個 commit 的日期而非文件實際撰寫日）：Step 0 新增「無 frontmatter 時比照處理，須找出所有出現位置」；Step 5 自檢明確要求日期用文件撰寫當下、非 commit 日期 。**v2.2.1 消除原則 3 指令衝突**：v2.2.0 的「專案自有版本／日期慣例」與原則 3「frontmatter 必填 source-commit」打架（實測後果：後端文件 source-commit 0 處，模型選了流程層），改為描述原則「文件須帶可判斷版本的標記」＋形式 A/B 二擇一；frontmatter-spec 補形式 B 整節；並指出形式 B 缺 commit hash 的結構性弱點 | 2.2.1     | 2026-08-03 |
| `doc-generator`                   | 通用文件產生器                                                                                                                                                                                                                                                                         | 1.0.0     | 無記錄        |
| `multi-tool-coordination`         | 人工操作多 AI 工具分工指南（執行型見 ai-pair）                                                                                                                                                                                                                                                   | 1.0.0     | 無記錄        |
| `context-sharing`                 | 跨工具上下文共享                                                                                                                                                                                                                                                                        | 1.0.0     | 2025-01-28 |
| `dev-doc-organizer`               | 開發文件整理                                                                                                                                                                                                                                                                          | 1.0.0     | 無記錄        |
| `ai-pair`                         | **v5.0 結構性改版**：Claude 額度只花在協調決策與 review 驗證，探索／實作外包 external CLI；新增 `explorer`（四道護欄）；developer 改雙引擎（**copilot 預設**／native 升級）＋**A/B 准入制**（回報誠實性為淘汰項）；**廢除「reviewer 不得讀 source」禁令**（恢復跨 repo 契約查證）；傳遞改 manifest 路徑；模型層改執行期發現。四項上線阻擋已驗收，**契約面清單經 A/B 證明有效**（嚴格版：無清單 PASS/0 → 有清單 BLOCK/1 Critical） | 5.0.0     | 2026-07-30 |
| `ai-pair-v4-test`                 | v4 測試分支（Phase 1 驗證中）；v4.1 校正誤判 2 項、新增缺口 4 項                                                                                                                                                                                                                                     | 4.1.0     | 2026-05-14 |
| `figma-to-requirements`           | Figma 截圖 → UI 任務需求草稿（讀本專案 DESIGN.md，產 token 對應表+狀態矩陣+待新增 token 清單），交棒 requirements；通用設計可跨 Flutter 專案                                                                                                                                                                            | 1.0.1     | 2026-06-23 |
| `figma-to-flutter`                | ⚠️ **已退役歸檔**（2026-06-23）：知識（Figma→Flutter 對應表）併入各專案 DESIGN.md，工作流改為 figma-to-requirements → requirements → 對著 DESIGN.md 實作。原：Figma 截圖轉 Flutter 代碼（B2B 專屬，含已過時的 screenutil 尺寸規則）                                                                                                 | 1.0.0（退役） | 2026-03-18 |
| `zerospec-skills`                 | ZeroSpec Prompt Pack 改造為 Claude Code Skills；10 個指令（scan/build/spec/adr/sa/impl/drift/update/audit + 智慧入口）；含跨工具橋接（CLAUDE.md/GEMINI.md）、狀態自動偵測、Self-Review Protocol；同步至官方 v0.5.2 含 DRIFT/IMPL/Post-Edit Self-Check                                                                | 2.0.0     | 2026-06-01 |
| `sanlong-pay-onboarding`          | sanlong（山隆 PAY，iOS+Android Flutter APP）**專案專屬**新人上手導覽。與 b2b-manager 那支**同架構、內容全異**：手機 APP／自製 SLRouter + NavStack／單一大型 SLController（3418 行，刻意設計非技術債）／無 screenutil。五份 reference 857 行；十道食譜。收錄專案特有地雷：`isDraggable: true` 會變底部彈出而非側滑、防重複提交擋在 SLController 而非 UI、待支付檢查是 APP 端本地 flag 不是 API 欄位、`FlavorConfig` 設了但沒被讀（實際生效是 `BASE_URL`）、flavor 名稱兩平台不一致（staging→iOS `stage`/Android `staging`；正式→iOS 預設 scheme `Runner`/Android `product`）、`.run/` 的 `additionalArgs` 不可複製到命令列（iOS `--target dev` 會被 `-t` 覆蓋，IDE 內無害因 scheme 來自 `buildFlavor`）。基準 `main@190038a9`。**測試**：事實面逐條驗證通過、10 題答案覆蓋率全中；自動觸發與實跑 build 未驗。**目前未安裝** | 1.0.0     | 2026-08-25 |
| `b2b-manager-onboarding`          | b2b-manager（Flutter Web）**專案專屬**新人上手導覽。仿 ECPay `ecpay-api-skill`：SKILL.md 當決策樹路由，5 份 reference 做 progressive disclosure（day1-setup／codebase-map／first-task-recipes／conventions-gotchas／glossary-domain）。定位「路由器不是教科書」——專案已有 CLAUDE.md／AGENTS.md／DESIGN.md／docs/，新人卡在不知道讀哪份、什麼順序。三種入口（四小時一條龍／十分鐘版／單點查詢）＋10 題自我測試；硬性要求「絕不憑記憶回答，每個答案落地到檔案路徑」。⚠️ 內容價值全在具體行號，**不可跨專案共用**，其他專案複製結構重寫內容、命名沿用 `<repo 名>-onboarding`。**目前未安裝**（僅存檔） | 1.1.0     | 2026-08-25 |

---

## 通用類（general）

| 名稱 | 描述 | 版本 | 更新時間 |
|------|------|------|----------|
| `media-processor` | 媒體檔案處理（影片、音訊、圖片） | 2.0.0 | 2025-12-31 |
| `sign-off-generator` | 專業簽核文件產生 | 1.0.0 | 2026-01-06 |
| `document-translator` | 文件翻譯 | 1.0.0 | 無記錄 |
| `translategemma-translator` | Gemma 模型輔助翻譯 | 1.1.0 | 無記錄 |
| `discussion-organizer` | 學習討論筆記整理，四層判斷邏輯，5 分鐘完成 3-4 小時整理 | 1.0.0 | 2026-03-12 |
| `cross-platform-archive-agent` | 跨平台壓縮打包，自動排除 OS 垃圾檔 | 1.0.0 | 2026-03-03 |
| `point-reading-diy` | 🚧 **開發中、未安裝**。卡米點讀筆 DIY：整段錄音 → 切成一句一檔 → 審核網頁 → 對應「內容貼」碼號 → 打包 DAB 點讀包。有背景音樂時改用 Whisper 時間點切（切點取兩句間音量最低處，Whisper 會把拖長音與停頓算進同一個字）；審核結果綁定切檔版本，重切後不會誤用舊審核。實測《Dear Zoo》切成 18 段；暫停於打包前，等實體貼紙報碼確認書名貼命名與內容貼碼號 | 0.3.2 | 2026-10-08 |

---

## 統計

| 分類 | 數量 |
|------|------|
| pkm（知識管理） | 9 |
| dev（開發） | 18（含 figma-to-flutter 退役；兩支 onboarding 未安裝） |
| general（通用） | 7（point-reading-diy 開發中、未安裝） |
| **總計** | **34** |
