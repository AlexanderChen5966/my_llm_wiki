---
title: Opus 5.5 影片案例庫：yihui-dev 提示詞收藏 vs athemeroy 研究型快照，數量不能合併、模型歸屬要逐筆核對
date: 2026-10-19
tags: [ai/claude, ai/workflow, type/comparison]
source:
  - https://github.com/athemeroy/awesome-opus-5-5-videos
  - https://github.com/yihui-dev/awesome-opus5-5-videos
  - https://www.kocpc.com.tw/archives/670578
  - https://www.bnext.com.tw/article/92576/claude-opus-5.5
official_refs:
  - https://github.com/athemeroy/awesome-opus-5-5-videos（README 本體即本次剪藏，一手；凍結快照 2026-09-26 21:05 CST；docs/、data/ 子頁本批未讀）
  - https://github.com/yihui-dev/awesome-opus5-5-videos（README 本體即本次剪藏，一手；README 自述 2026-09-29 更新、2026-09-30 剪藏；prompts/ 與 data/videos.json 本批未讀）
type: article
status: processed
asset_value: 51
---

> 🔴 **兩份 README 都是一手，但性質完全不同**：
> - **yihui-dev/awesome-opus5-5-videos**＝**提示詞收藏**，主要入口導向 Skillry（商業導流），⚠️ **README 未見授權聲明**（只有 Credits）。
> - **athemeroy/awesome-opus-5-5-videos**＝**研究型凍結快照**，原創文字與圖表採 **CC BY 4.0**（X 貼文、影片、提示詞、外部專案不在授權範圍內），方法透明（56 組查詢、計數單位逐層交代、圖表可重跑）。
> - 兩個 repo 名稱只差一個連字號，但 README 互不提及，是**同題的平行專案，不是 fork**。
>
> ⚠️ 兩篇中文媒體都指向 yihui-dev 那份，屬**同一個一手來源的二手轉述**：電腦王阿達（kocpc，Rocky，2026-09-30）沒有增量；數位時代（蘇柔瑋，2026-10-12）**文末自註「本文初稿為 AI 編撰」**，本條只取它的「513 組」一個數字。兩篇都沒實測。

## T（核心論點）

「Opus 5.5 做的影片」其實是**很多種製作路徑**的產物：程式繪製 2D、Manim 解說、3D／即時圖形、改寫既有素材、指揮外部影片模型（如 Opus＋Seedance）、錄製 app 或遊戲、混合。🔑 **同樣的外觀可能來自不同工具**（athemeroy README："A frame alone cannot establish the production path."）。所以引用案例庫之前，要先做**歸因驗收**：這支片的動態畫面是誰算出來的？提示詞完整嗎？模型標籤可靠嗎？兩份清單的數量**母體不同、不能合併**——yihui-dev 數的是「提示詞」，athemeroy 數的是「X 上抓到的影片檔」；兩邊都**不代表 Opus 5.5 的實際用量**。

## B（背景）

**適用：**
- 找 Opus 5.5 程式動畫的靈感與提示詞起點（yihui-dev：可複製的 Prompt 區塊＋技術標籤）
- 想知道某種外觀通常怎麼做、該檢查什麼（athemeroy：七條製作路徑＋「What to check」）
- 寫文章或簡報要引用「社群做了多少 Opus 5.5 影片」時，用來校正說法

**不適用／注意：**
- 🔴 **不能寫成「1,000+ 支 Opus 影片」**：athemeroy 標題的「1,000+」實際對應 1,401 個檔案或 1,119 個 yes／likely 判定，README 自己明說這些單位**不能讀成獨立創作者數、已驗證的 Opus 執行數，或 X 上的全部原片**
- 🔴 **分類器不是證據**：athemeroy 用 **Gemini 3.8 Flash**（另一個 LLM）讀貼文文字＋9 格抽樣畫面來判斷「是否有 Opus 參與」，README 原話是這些判斷 "do not establish model calls or authorship"；兩種門檻都 "Neither threshold gives a bound on actual Opus use"
- ⚠️ **yihui-dev 的收錄前提沒經過驗證**：README 寫「Every video here was made by asking Claude Opus 5.5 to write the animation as code」，但 README 精選的 100 項裡就有 3 項標了 **AI image**；而 athemeroy 同期的 X 樣本有 282／1,401 被判為 no，也有外部影片模型路徑。「viral」沒有寫收錄標準
- 不要拿提示詞重跑就期待重現：兩份 README 都承認部分創作者只分享部分提示詞、或只描述成果

## R（關鍵細節）

### 1. 數量並列（不可合併）

| 說法 | 出處 | 日期 | 計數單位 | 可信度 |
|---|---|---|---|---|
| 475 prompts／README 精選 100 組 | yihui-dev README | 2026-09-29 更新 | **提示詞**（prompts/ 與 data/videos.json） | 一手 |
| 475 組（同文另寫「超過 470 組」） | kocpc | 2026-09-30 | 同上 | 轉述，與 README 相符 |
| **513 組** | 數位時代（初稿為 AI 編撰） | 2026-10-12 | 同上 | **轉述，未經一手核對**；與 README「持續更新」相容，並列不取代 475 |
| 1,511 → 1,419 → 1,449 → **1,401** | athemeroy README | 2026-09-26 凍結 | 去重候選貼文 → 含 MP4 的貼文 → MP4 附件 → **SHA-256 不重複檔** | 一手 |
| **1,119**（980 yes＋139 likely；另 282 no） | athemeroy README | 同上 | Gemini 3.8 Flash 判定的**檔案** | 一手，但是 LLM 判定 |
| **168** | athemeroy README | 同上 | **刻意挑選**審閱的原貼文案例（非隨機抽樣） | 一手 |

- 母體不同：前三列是**提示詞數**，後三列是**X 影片檔數**（包含非程式路徑）
- athemeroy 補充：1,449 − 1,401 ＝ 48 個逐位元相同的附件；同一作品的不同編碼仍會算成不同檔案
- 查核日 2026-09-27 之後補的 7 則新案例與 4 個開源系統**不計入**凍結數字

### 2. 兩份清單對照

| | yihui-dev | athemeroy |
|---|---|---|
| 定位 | 提示詞收藏（"Pick one, copy its prompt, and try it"） | 案例研究：Opus 在影片裡扮演什麼角色、證據是什麼 |
| 收錄前提 | 自述「全部是 Opus 5.5 寫程式做的」（未驗證） | 明確區分程式繪製、外部影片模型、改寫素材、錄 app；分清「創作者揭露／公開專案相符／自己觀察」 |
| 每項附 | 預覽縮圖、X 原貼文、prompts/ 檔、技術標籤 | 抽樣單格、X 原貼文、製作路徑、證據等級 |
| README 分類（Agent A 計數，已核對） | Motion graphics 58／Explainers 16／3D scenes 14／Games & interactive 12 | 領域 11 類 × 風格 13 類交叉表 |
| 技術標籤（100 項） | Canvas 73、SVG 42、GSAP 21、Three.js 20、GLSL 19 最多；AI image 3 | — |
| 授權 | **未見授權聲明**；Credits：影片與提示詞屬原創作者，預覽是「為本清單託管的短片」，要改要刪請開 issue | 原創註解、文字、圖表 CC BY 4.0；**不散布原始 MP4**；縮圖仍屬創作者 |
| 導流 | 首屏大圖與主要 CTA 都導向 Skillry，所有連結帶 UTM、預覽圖託管在 media.skillry.dev；「live remake」由誰、用什麼模型做、是否收費都沒交代 | 未見 |

### 3. athemeroy：七條製作路徑與該檢查什麼（一手）

| 製作路徑 | 該檢查什麼 |
|---|---|
| 程式繪製 2D | 公開的程式碼和畫面、時間、素材對得上嗎？ |
| 教學解說（如 Manim） | 事實、公式、旁白、提供的素材有被核對嗎？ |
| 3D／即時圖形 | 這是程式錄影、渲染場景，還是影片模型輸出？ |
| 改寫既有素材 | 提供的表演、音訊、產品檔貢獻了多少？ |
| 外部影片模型管線（如 Opus＋Seedance） | 動態畫面是哪個工具產生的？Opus 指揮了什麼？ |
| App／遊戲錄製 | 交付物是可玩的程式，影片只是錄影嗎？ |
| 混合／無法判定 | 輸入、預算、評分方式可比嗎？ |

- README 另有 14 組「外觀相似、工具不同」的配對案例（docs/evidence-examples.md，本批未讀）
- 9 格抽樣不能判斷完整動態、音訊、知識正確性、隱藏的模型呼叫或最高畫質母片（README 自承）

### 4. athemeroy：分布與其他觀察（一手，1,119 檔為分母）

- 領域：遊戲／互動 230、廣告／發表 215、AI 談 AI 201，合計 646（57.7%）；**只算 yes 時第一名互換**（廣告 191 > 遊戲 187）——README 說這是門檻造成，不是時間趨勢
- 風格：Motion graphics／UI 350、3D render 324，合計 674（60.2%）；最大交叉格：遊戲 × 3D 161（14.4%）、廣告 × MG／UI 154（13.8%）
- 顏色：鮮豔像素比例中位數 MV 45.5%（77 檔）、廣告 17.6%（215 檔）；共 53 種色彩模式；**不檢驗顏色對按讚數的因果**
- 預覽長度中位數：全部 1,401 檔 39.20 秒；168 案例 52.37 秒；教學解說路徑最長 102.28 秒——不能推論製作時間、成本或成功率
- 按讚數只取 2026-09-27 單日（168 則中 166 則取得），只反映曝光，不能驗證作者或成品品質
- 🔑 **提示詞會被抄寫流傳**：公開的 5 份委託提示詞長度 172～17,664 字元（只是 5 個例子，不是分布）；一份較晚的 MV 提示詞和較早的一份有 **87.0%** 的五詞序列重疊——相似的提示詞**不代表獨立執行**
- 圖表可重跑：`python3 -m pip install -r requirements-charts.txt` 後執行三支 generate 腳本，皆支援 `--check`

### 5. 可重用的開源製作系統（athemeroy 列出，查核日 2026-09-27，不計入凍結數字）

| 專案 | 內容 | 注意 |
|---|---|---|
| Lemo-Opuscar（lemomo-ai） | 39 種風格畫廊、導演指南、技法指南、範例場景程式 | 授權：**程式碼 MIT；指南、風格檔、影片 CC BY 4.0**（另受第三方素材條款限制）；創作者稱 Canvas／WebGL 畫出、未用影片生成模型，但 repo 無法稽核每次模型呼叫 |
| Papermotion（francozanardi） | 剪紙動畫引擎：確定性物理、角色骨架、離線渲染、縮圖總表、聲音檢查 | ⚠️ **同一個 repo 混有兩種模型標籤**：snow／demo／rooftops／sea 標 Opus 5.5，light／embers 標 **GPT 6 Astra** → 逐支看標籤 |
| ClaudeAnimationBase（JohnHeibel） | p5.js＋p5.brush 動畫起手專案（表情、分鏡、縮圖總表、渲染流程） | 起手專案是可重用的輸入，不是每支成品的完整原始碼 |
| Motion Graphics Music Video skill（makevoid） | Claude Code plugin＋Ruby 工具組，依歌曲與 brief 規劃組裝 MV | 會呼叫 **Fal** 的生圖／生影片／生音訊模型 → 不能當成「動態畫面全由 Opus 產生」的證據 |

### 6. yihui-dev：用法三步（一手）

1. 打開 prompt 檔，複製 **Prompt** 區塊
2. 貼到 Claude Opus 5.5（Claude Code、Claude app 或任何跑 Opus 5.5 的 agent）
3. 要求輸出成單一 HTML 檔；需要影片就錄下頁面

- README 自承：部分創作者只分享部分提示詞，或只在貼文描述成果，這些檔案會註明
- kocpc 稱它「免費開源」不精確——README 只寫了 Credits，未見授權聲明；kocpc 說 prompts/ 只有提示詞、沒有參考影片（媒體轉述）

## C（連結與應用）

- [ ] 引用任何「Opus 5.5 做的影片」之前，先對照第 3 節判斷製作路徑，回原貼文看創作者自己怎麼說（歸因驗收）
- [ ] 寫數字時照第 1 節的單位寫：「yihui-dev 9/29 收 475 組提示詞（10/12 媒體稱 513，未核對）」「athemeroy 凍結快照 1,401 個不重複檔、1,119 個 yes／likely」——**不要寫「1,000+ 支 Opus 影片」**
- [ ] 拿 yihui-dev 的提示詞重跑前，先看檔案有沒有標「只有部分提示詞」；重跑結果要照 [[claude-code-pure-code-animation-practice]] 的看樣片迴圈驗收，不要只比截圖
- [ ] 用 Papermotion 這類同 repo 多模型的範例時，逐支核對模型標籤
- [ ] 重用 Lemo-Opuscar：程式碼 MIT 可用，但影片與指南是 CC BY 4.0，要標註
- [ ] 待追蹤：athemeroy 的 docs/（14 組配對證據、88 案例提示詞矩陣、可重用模板）與 yihui-dev 的 prompts/ 本批都沒讀；513 組要等一手核對

## 原始摘要

- **athemeroy/awesome-opus-5-5-videos README（GitHub，2026-09-30 剪藏；凍結快照 2026-09-26 21:05 CST，主幹）**：計數單位逐層交代、Gemini 3.8 Flash 分類與限制、領域／風格／顏色／長度統計、七條製作路徑、公開提示詞長度與 87% 重疊、四個可重用開源系統、CC BY 4.0 範圍。工具 Hypit（0.2.3）是作者的分析工具，README 未推銷。
- **yihui-dev/awesome-opus5-5-videos README（GitHub，2026-09-30 剪藏）**：475 prompts／精選 100、四個分類、技術標籤、用法三步、Credits。⚠️ Skillry（skillry.dev/ai-videos/opus-5-5）是這份 README 的**導流目的地，不推薦**；「live remake」來源不明，未收。第六批曾以「無增量、沒有頁面網址」排除的 raw〈Skillry Opus 5.5 Videos…〉（wingzero），現可確認就是這個導流目的地（該 raw 不動）。
- **電腦王阿達〈「Awesome Opus 5.5 Videos」收集 475 組…〉（Rocky，2026-09-30）**：yihui-dev 清單的中文導覽，475／100 與 README 相符，無增量、無實測。把 Skillry 當主要瀏覽入口推薦（替導流背書），不採用。
- **數位時代〈Claude做影片教學！Opus 5.5做出10種AI動畫風格…〉（蘇柔瑋，2026-10-12，初稿為 AI 編撰）**：本條只取「截至目前 513 組提示詞」一列；其餘案例規格補進 [[claude-code-pure-code-animation-practice]] 第 11 節附註。自家產品「一天一 AI PRO」案例、訂閱與延伸閱讀導流未收。

## 相關連結

- [[claude-code-pure-code-animation-practice]] — 實作工作法（`draw(t)`、看樣片迴圈、子代理分工）與首週案例精選；七條製作路徑可補它第 2 節「四條技術路線」的讀法
- [[claude-opus-5-5-release-notes]] — 該條已有「單一挑選的展示作品不能證明重現率」的警語，與 athemeroy「數量不代表 Opus 用量」方向一致
- [[ideas/claude-code-animation-prompt-qa-delivery]] — 提示詞狀態清單＋時間表、交付格式；拿案例庫提示詞改寫時的方法論
- [[ideas/tripo-gpt6-astra-3d-prompts-gallery]]、[[ideas/tripo-3d-prompts-300-multi-model-gallery]] — 同為提示詞畫廊；Tripo 標註「逐字 Prompt／依來源整理」的證據標記，可對照 yihui-dev「部分提示詞會註明」的做法
- [[hyperframes-heygen-html-to-video]] — 渲染層另一個選項（HTML → deterministic MP4）
- [[claude-codex-video-toolchain-selection]] — 渲染引擎、風格 Skill 的選型與授權檢查
- [[ideas/ai-3d-code-structure-vs-appearance-acceptance]] — 3D 作品的結構 vs 外觀驗收；和本條的「歸因驗收」互補（一個問「做對了沒」，一個問「是誰做的」）
