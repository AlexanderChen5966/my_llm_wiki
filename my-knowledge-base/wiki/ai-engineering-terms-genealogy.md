---
title: AI 工程術語考古：從 Prompt 到 Graph Engineering，哪些是真演進、哪些是造詞
date: 2026-07-27
tags: [ai/agent, ai/workflow, method/critical-thinking]
source: https://ithelp.ithome.com.tw/articles/10397462
type: article
status: processed
asset_value: 85
---

## T（核心論點）

社群流傳的「Prompt → Context → Harness → Loop → Graph Engineering 五階段演進」**這條鏈本身查無任何原始文章一次提出過**——它是內容站事後把五個獨立事件包裝成的敘事。作者查核 50 個來源、正文引用 29 個並附 L1/L2/L3 分級，結論是**這條鏈只有一半是真的**：Context Engineering 是教科書級的真演進，Graph Engineering 到查證基準日只有三天、全部家當是一則 12 字反問推文。

## B（適用場景）

**適用：**
- 看到新的「XX Engineering」時判斷該投入學習還是先觀望
- 為知識庫既有的 loop / harness 條目補上**懷疑視角**（那些條目多半採「這個術語很棒」的立場）
- 需要引用術語起源、命名者、時間點時的可驗證依據

**不適用 / 注意：**
- 資訊基準日 **2026-07-21**，之後的發展需自行追查（作者也明說時間軸停在此）
- 這篇是**術語社會學/考古**，不是實作教學——要學怎麼做仍需看各術語的實作條目
- 部分來源標「未直接取得」（X 貼文、OpenAI 403），逐字引用前應重新取得原文

## R（查證結果與判準）

### 五詞的可驗證時間軸

| 詞彙 | 最早可驗證出現 | 命名者 | 判定 |
|---|---|---|---|
| **Prompt Engineering** | 2018–2021 自然成形 | 無單一命名者 | 真實實踐；**被宣告死亡後轉形，沒有消失**（IEEE Spectrum 2024-03 已發過訃聞）|
| **Context Engineering** | **2025-06-19** | Tobi Lütke（Shopify 創辦人）發推 → Karpathy 6 天後放大 | ✅ **真演進**：8 天內從首發到被 Simon Willison 完整記錄，**3 個月內獲 LangChain + Anthropic 兩家官方定義** |
| **Harness Engineering** | 2026-02-11 | ⚠️ **三個版本互相打架**（OpenAI 最早發文／Osmani 稱 LangChain 的 Vivek Trivedy 是 coiner／中文圈稱 Mitchell Hashimoto 更早但四路查證無一手網址）| 實務真實存在，**無公認發明者**；訊號：**Anthropic 至今只用 `harness design` 普通名詞寫法，沒抬成學科** |
| **Loop Engineering** | 2026-06-06/07 | Steinberger 挑起（650 萬瀏覽）＋ Addy Osmani 命名 | 熱詞：**三種互不收斂的定義並存**（Osmani 五元件／LangChain 四層／Anthropic 四類），踩坑報告已出現 |
| **Graph Engineering** | **2026-07-18** | Steinberger 在 X 的一句反問，**尚無人給出正式定義** | ❌ **還不構成一門學科**：全部家當是一則 12 字反問（280 萬瀏覽），**零官方採用**；LangGraph 官方文件全文查無此詞 |

### 🔴 三個判準（本篇最可複用的產出）

下次再有新的「XX Engineering」出現時套用：

1. **官方或多方採用了嗎？** Context Engineering 用 3 個月拿到兩家官方定義；Graph Engineering 目前是零
2. **有新的工程動作嗎？** 如果描述它時只能重新排列既有工作（寫 prompt、管 context、接工具），它可能只是新標籤
3. **有失敗案例沉澱嗎？** 一個實踐要成熟到有人公開踩坑，才代表真的有人在用

### 🔴 「轉載幾輪就變定論」的具體例證（2026-09-11 補）

本篇警告過「中文圈第一篇 Graph Engineering 文章 2026-07-20 上線，早於英文定義文，等互相抄的版本轉載幾輪變成定論，想追出處就難了」——[[graph-engineering-node-edge-state]] 收錄的其中一個來源（老周聊架构，2026-07-24）正是這個現象的**具體樣本**：發布日期落在本篇查證基準日（2026-07-21）**之後 3 天**，卻用「五代工程演進」的完整敘事框架、真實任務案例與程式碼把 Graph Engineering 包裝得跟既成事實一樣扎實——**內容品質與說服力，跟這個詞是否真的構成一門學科，是兩件事**。技術操作細節（FSM 框架、Reducer 範例、框架對照）本身有實用價值已保留於該條目，但其「五階段演進」框架敘述不構成對本篇判定的獨立反證，僅供未來遇到類似「內容站事後包裝」案例時的參照樣本。

### 一盆早了一年半的冷水

殺傷力最大的一篇寫在炒作開始之前——Anthropic〈Building Effective Agents〉（**2024-12-19**）：
> 最成功的實作沒有用複雜框架，用的是簡單、可組合的模式；**加複雜度之前，先證明它確實改善結果**。

比 Loop 和 Graph Engineering 早了一年半，讀起來卻像專門為它們寫的。

**實證與產業面同樣冷**：
- **MAST 論文**（Cemri et al., arXiv:2503.13657）分析 7 個框架、200+ 任務失敗軌跡，定性結論：**多數失敗出在系統設計，與模型能力關係不大**（⚠️ 網路流傳的具體失敗率百分比無法在論文正文對應，原作者明言不引用）
- **Gartner**：預測超過 **40% agentic AI 專案會在 2027 年底前取消**
- **Uber**：四個月燒完全年 AI 工具預算後開始設每人每月上限

## C（關鍵行動）

- [ ] 遇到新「XX Engineering」先套三判準（官方採用／新工程動作／失敗案例），不急著建條目或導入
- [ ] 引用術語起源時，注意 Harness 的命名者**無共識**、Loop 的定義**有三種**——別當成單一定論轉述
- [ ] 對 Graph Engineering 保持觀望：盯「有沒有官方發出定義性文章」，一直沒有就多半會退場
- [ ] 導入任何多 agent／複雜編排前，先過 Anthropic 那一關：**證明複雜度確實改善結果，再加複雜度**

## 原始摘要

iThome 文章（資訊基準日 2026-07-21），作者逐節查核 50 個來源、正文引用 29 個，並依 L1（官方一手）／L2（具名權威一手）／L3（優質二手）分級標注，對無法直接取得者（X 貼文、OpenAI 403）明確標「未直接取得」。這種**來源分級 + 取得限制標注**的寫法本身值得參考——與知識庫「量化數據要追溯一手來源」的原則一致。作者另指出中文圈第一篇 Graph Engineering 文章 2026-07-20 上線，早於英文定義文，警告「等互相抄的版本轉載幾輪變成定論，想追出處就難了」。（iThome 頁尾研討會廣告已略）

## 相關連結
- [[synthesis/synthesis-agentic-engineering]] — **本篇為其來源之一**（2026-09-03 重跑，5→8 篇）：🔴 本篇提供該精粹唯一的懷疑視角，校準「XX Engineering 演進鏈」的可信度
- [[loop-engineering-6-components]]、[[loop-engineering-implementation-patterns]]、[[loop-engineering-playbook-cli-tools]]、[[loop-engineering-human-roles]] — 知識庫既有的 Loop Engineering 系列；**本篇補上它們缺的懷疑視角**：三種定義互不收斂、Pragmatic Engineer 訪談指出可能只是模型不夠強時期的暫時性 hack
- [[harness-engineering]] — Harness Engineering 框架；**本篇補上「誰命名無共識、Anthropic 沒把它抬成學科」的來源考證**
- [[context-engineering-fundamentals]] — Context Engineering；本篇證實它是五詞中**唯一的教科書級真演進**
- [[claude-5-context-engineering-rules]] — Anthropic 官方 Claude 5 新規；與本篇「先證明複雜度有用」精神一致（都在講刪減而非疊加）
- [[synthesis/synthesis-multi-agent]] — Multi-Agent 精粹；本篇引用的 MAST 論文「多數失敗出在系統設計」正好呼應該精粹 V 層「評測與失敗模式全面缺席」的缺口
- [[ai-agent-architecture-patterns]] — Agent 架構模式
