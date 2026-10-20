---
title: AI 寫程式做 3D 的驗收：結構與外觀分開檢查（3JSBench 為主幹，CD 盒、Blender MCP、桌面賽車、Voyager 為情境例）
date: 2026-10-19
tags: [ai/workflow, dev/testing]
source:
  - https://www.aiposthub.com/3jsbench-ai-threejs-geometry-quality-guide/
  - https://www.aiposthub.com/opus-threejs-cd-case-animation-geometry-interaction/
  - https://www.aiposthub.com/blender-mcp-ai-3d-film-scene-workflow/
  - https://www.aiposthub.com/gemini-desk-photo-threejs-racing-game/
  - https://www.aiposthub.com/voyager-3d-model-product-commercial-guide/
  - https://www.aiposthub.com/manimgx-python-agent-animation-rendering-benchmark/
type: idea
status: processed
asset_value: 21
---

> 🔴 **來源集中**：6 篇全部出自 **aiposthub／Philo**（2026-10-10～10-15），文末互為延伸閱讀（CD 盒 → 3JSBench；Blender MCP → 3JSBench；桌面賽車 → Blender MCP；ManimGX → Blender MCP）。「先結構後外觀、多角度看、保留可編輯來源、一次改一項」是**同一作者把同一套驗收框架套到不同工具**，不能算多個獨立佐證。6 篇**全部沒有實測**（作者在各篇自承：書桌場景是假設案例、「本文未測試作者示範的裝置適配」「未宣稱已替讀者完成產品實測」「本站未完成相同成品」）。
>
> 🔴 **3JSBench 是單一社群來源**（僅 General Context Labs 的 X 討論串，查詢日 2026-10-10；**沒有 repo、論文或官網**），**沒有排名、模型名單或分數**，作者也刻意不推冠軍。
>
> 為何放 ideas：叢集合併後 asset_value 約 21（3JSBench 21、CD 盒 10、Blender MCP 驗收段、桌面賽車 3、Voyager 6），仍是單一作者的方法論；待自己做一個 Three.js 作品實際跑過兩張表，或出現一手評測，再考慮升 wiki。ManimGX 與 Blender MCP 的工具事實補在 [[claude-codex-video-toolchain-selection]]，本條只收驗收觀點。

## T（核心論點）

🔑 **畫面好看 ≠ 物件結構正確。** AI 生成的 3D 物件可能在正面截圖裡完全合理，轉個角度就看到桌腳懸空、燈座穿進桌板、零件分離。結構（接觸、連續、穿模／表面閃爍、文字）適合寫成**確定性檢查**；外觀（色彩、構圖、造型）才交給**偏好判斷**。兩者可以放在同一份報告，但**不要混成一個沒說明權重的總分**（3JSBench 原作者觀點，經 aiposthub 轉述）。驗收要多角度（正、側、背、底）、附實際手機尺寸的畫面；修一處之後，要回頭檢查其他已通過的地方。

## B（背景）

- 適用：Three.js 網頁 3D 物件／動畫、經 MCP 讓 agent 操作 Blender 做的場景、要讓使用者旋轉放大的產品展示、要交接給別人繼續改的 3D 專案
- 不適用：只交一張靜態宣傳圖（作者提醒：真的只需要靜態圖，就**明說交付範圍縮小了**，不要用關閉旋轉來掩蓋背面錯誤）
- 和 [[karpathy-opus5-lotr-3d-case]] 的關係：那條說「AI 看不到自己的作品，產出能力 ≠ 驗收能力」；3JSBench 是把這個缺口**做成結構化評測**的一個社群嘗試

## R（關鍵細節）

### 1. 3JSBench（General Context Labs，經 aiposthub 轉述 X 討論串）

- **100 個任務、697 項確定性檢查**：依固定條件判斷，不把每件作品的對錯交給另一個 LLM 評分
- 另收集 **9,500+ 票、來自 1,000+ 位創作者**的並排偏好投票——只反映該批作品的審美選擇，**不能代替結構驗收**
- 檢查面向：幾何連接（懸空、分離）、交疊表面閃爍（z-fighting）、文字錯誤
- 100 個任務／697 項檢查描述的是**測試範圍**，不能換算成某個模型在你網站上的成功率（作者觀點）
- ⚠️ 作者明說沒有使用未核對的安裝入口，本文沒有評測程式的安裝方式

### 2. 兩張驗收表（aiposthub 作者觀點；以假設的「書桌＋檯燈＋電線＋STUDY 招牌」場景示範，**不是 3JSBench 實跑**）

| 驗收面向 | 通過條件（書桌例） | 要留下的證據 |
|---|---|---|
| 接觸 | 桌腳接桌板並落地，燈座接桌面 | 正面、側面與底部畫面 |
| 連續 | 支架與電線沒有斷裂 | 接點近距離畫面 |
| 交疊 | 沒有不合理的穿入與表面閃爍 | 旋轉時的問題位置 |
| 文字 | STUDY 字母完整、順序正確 | 原字串與招牌截圖 |
| 構圖 | 放進網頁後主體清楚、文字可讀 | 桌機與手機實際尺寸 |

- 前四項是結構，第五項是外觀；結構表只過了外觀，就**不能說 3D 資產合格**，交付時要列出剩下的結構缺陷
- 模型自己回報「已連接」只能當自述，要用實際畫面與程式資料核對
- 修正時：只修失敗的接點、固定相機／縮放／光線做前後比較、一次只改一種條件（位置、尺寸或形狀）；新版本仍要看原本通過的位置（修好燈罩卻把桌腳移離地面＝新缺陷）
- 比較模型時用**同一份需求、同一套角度、同一張表**，不要替其中一個模型多寫接點描述；修改成本（反覆破壞接點、要修文字）也一起記錄

### 3. Opus 5.5 情境例：CD 盒開蓋動畫（aiposthub 轉述 Ann Nguyen 2026-10-11 X 貼文；單一創作者示範，**不是 Anthropic 官方功能**；未實測、不是復刻原作者提示詞）

- 作者自稱「不需要 3D 模型」＝**不必匯入外部模型檔**；Three.js 裡仍用 geometry＋material 建出幾何（aiposthub 依 Three.js 官方 Mesh／ShaderMaterial／Object3D 文件解釋，剪藏未附連結）
- 規則外形（盒子）適合程式建模；複雜雕刻、人物、大量細部商品不一定（作者觀點）
- 結構驗收：盒蓋要繞**鉸鏈支點**轉、不是繞盒蓋中心；盒蓋與封面放在**同一個父層**一起開合；透明面的繪製順序要在關閉／半開／全開多角度看——這類錯誤是結構問題，**改速度解決不了**
- 互動驗收：中途再點一次的行為要一致（反向、忽略或排隊擇一）；重播不能累積偏移；手機寬度、觸控取代懸停、鍵盤可觸發、尊重減少動態效果的偏好；反覆進出頁面要釋放材質與紋理
- 協作順序：先做盒身／盒蓋／開合關係，再調透明度、邊緣、反光；每次選版保留**關閉、半開、全開三張比較圖**
- 本例內容大多是通用的 3D 網頁驗收常識，和 Opus 5.5 本身的關聯薄

### 4. 可交付的 3D 專案（Blender MCP 篇，aiposthub 作者觀點）

- 🔑 **影片好看 ≠ 專案可交付**：.blend 要能重新開啟、物件可以分別選取且有可辨識的名字、主體／背景／地面的材質可以分開調；換一台電腦開啟不缺材質；交付影片＋.blend＋外部素材（字型、紋理、模型）的來源與授權說明
- 動畫先看開頭、中段、結尾三個時間點；修改後重開場景播放，確認檢查的是目前儲存的版本，不是上一次的截圖
- 工具本身（MCP for Blender 改名、安裝、`BLENDER_MCP_SAFE_MODE=1`）見 [[claude-codex-video-toolchain-selection]] 第 9 節附註

### 5. 其他情境（各一行）

- **桌面賽車**（Genevieve，**Gemini 4 Argon，不是 Claude**＋Blender MCP＋Three.js；aiposthub 未實測，原作者未公開成本、修改輪數、成功率）：照片缺背面、厚度與尺寸，**模型補的是推測**，先決定做創作原型還是精確重建；可玩原型另外要驗**碰撞形狀（≠ 外觀）**、操控方向與鏡頭一致、「完成一圈」的判定（防倒車刷圈）、手機能同時按加速與轉向
- **Voyager**（創作代理；aiposthub 依官方案例 voyager.so/blog/3d-model-to-product-commercial 整理，作者自承本站未完成相同成品；產品細節與價格交代很少）：🔑 **手上有可信的產品模型時要保留幾何，不要讓文字生影片重新猜一次產品**；第一輪只檢查模型、不做動畫

## C（連結與應用）

- [ ] 下一個 Three.js 作品試做兩張表（結構／外觀），至少留正面、背面、側面、底部與手機畫面
- [ ] 給 AI 的需求先寫接點關係（誰接誰、落在哪個面），再寫風格；要求它回報檢查結果，但自己再用畫面核對
- [ ] 修正提示詞限定範圍（「只修燈罩接點，保留桌面與配色」），每輪回歸檢查已通過項目
- [ ] 3D 動畫先修結構（支點、父子層級）再修節奏與材質；保留關閉／半開／全開比較圖
- [ ] Blender 類交付：影片＋可重開的 .blend＋素材來源清單，換機器重開一次
- [ ] 待驗證：3JSBench 若出現 repo、論文或排行榜再回來補；自己實作一輪後再決定是否升 wiki

## 原始摘要

- **aiposthub〈3JSBench 揭露 AI 3D 物件的連接問題…〉（Philo，2026-10-10，主幹）**：3JSBench 數字與面向（轉述 X 討論串 3 則）、書桌假設案例、兩張驗收表、修正與比較方法。訂閱 AI 郵報、延伸閱讀（ThreeUI Pro 方案、Gemini 3 three.js 教學）導流未收。
- **aiposthub〈Opus 5.5 做出 CD 盒三維動畫…〉（Philo，2026-10-13）**：「免匯入模型」的意思、轉軸與父層、透明繪製順序、互動與網站交付驗收。訂閱與延伸閱讀（3JSBench、Rive CLI）未收。
- **aiposthub〈Blender MCP 教學…〉（Philo，2026-10-12）**：本條只取「可交付專案」的驗收；安裝與安全模式見 [[claude-codex-video-toolchain-selection]]。小阪淳 X 短片未說明用哪個模型；延伸閱讀未收。
- **aiposthub〈Gemini 桌面賽車示範…〉（Philo，2026-10-15）**：只取一行情境；大量「設計建議，不代表原作者採用」的內容未展開。延伸閱讀（含 Gemini Pixel CAD）未收。
- **aiposthub〈Voyager 教學…〉（Philo，2026-10-10）**：只取「保留幾何」一行；15 秒三鏡頭流程、提示詞範例未展開。
- **aiposthub〈ManimGX 動畫教學…〉（Philo，2026-10-13）**：工具事實補在 [[claude-codex-video-toolchain-selection]] 第 2 節附註；本條只列為同叢集來源。

## 相關連結

- [[karpathy-opus5-lotr-3d-case]] — AI 看不到自己的作品；3JSBench 是「產出 ≠ 驗收」的結構化評測版
- [[chatgpt-blender-mcp-3d-dashboard-case-study]] — 把可執行的驗收標準寫進提示詞；Blender＋MCP 建模的實測案例
- [[claude-code-pure-code-animation-practice]] — BitsMinds 測試：「評估動畫要看動起來對不對，只比截圖會看走眼」；看樣片迴圈
- [[opus-5-5-video-case-libraries]] — 歸因驗收（這支片是誰、用什麼做的），和本條的結構驗收互補
- [[claude-codex-video-toolchain-selection]] — ManimGX、MCP for Blender 的工具事實、授權與安裝前檢查
