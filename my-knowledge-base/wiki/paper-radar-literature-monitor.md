---
title: paper-radar：會越用越懂你的論文雷達（開源自架）
date: 2026-07-07
tags: [ai/workflow, pkm/automation, type/concept]
source: https://drpwchen.com/posts/paper-radar/；技術細節補充自官方 GitHub README（github.com/drpwchen/paper-radar）
type: article
status: processed
asset_value: 74
---

復健科醫師陳柏威開源的「論文讀書小站」（github.com/drpwchen/paper-radar）：把卡卡的論文 RSS 改造成每天自動抓取、依興趣評分排序、越用越懂口味的私人雷達，勾選後自動抓全文、整理、評讀、收進筆記系統。任何有 RSS 的領域都能 fork。

## T（核心論點）

面對「幾十本期刊 + 追蹤作者 + 關注主題」的資訊洪流，要的不是「看更多」而是**先幫我篩、幫我排序**的雷達——把「被上百篇淹沒」變成「每天滑一滑、勾幾篇、讀它整理好的重點」。關鍵差異化是**興趣回饋迴圈**：按讚/略過會回頭微調排序模型權重，用越久越準。

## B（適用場景）

- 需長期追蹤多期刊/作者/主題的研究者（原場景：醫學，可 fork 到任何 RSS 領域）
- 與「主動搜尋」（如 paper-discovery）互補：radar 是 **push 式持續監測**，搜尋是 **pull 式單次探索**

## R（五層機制）

1. **自動抓取**：每天清晨抓期刊 RSS + PubMed 作者/主題追蹤查詢（爛 RSS 用 PubMed 查詢繞過，附 DOI）→ 自動去重成一條乾淨清單
2. **興趣評分 + 回饋學習**：每篇用興趣模型算分排序；手機滑動時按讚/略過 → 回頭微調權重（「你教它、它回饋你」）
3. **全文三層徽章**（滑的當下就知道能不能讀）：🟢 開放取用（自動抓 PDF）／🏥 機構訂閱（自動查所屬圖書館有無訂閱）／📎 自己補（附連結或手動丟 PDF）
4. **私密小站**：只有自己進得去，跨裝置同一份狀態（手機捷運上勾的，醫院電腦看得到）
5. **勾選閉環**：勾「想整理內容」或「想評讀品質」→ 自動抓全文 + 整理重點 + **評讀** → 收進筆記系統；外部看到的好文章也能直接丟進網頁處理

## R（補充：PRPM v2 個人研究偏好模型技術細節）

v1 排序只是手寫關鍵字權重的簡單排序器；v2 重寫成會自我學習的個人偏好模型，五層架構：事件層（D1 actions 現況 + append-only 歷史）→ 特徵層（papers.{tags, facets, embedding}）→ 模型層（每特徵 decayed Beta 偽計數 + profile 向量）→ 服務層（Thompson 抽樣分數 + MMR 多樣性 + 探索槽 + why 分解）→ 自省層（偏好儀表板）。

- **每晚無狀態自我訓練**：`train_model.py` 從 D1 全量事件重算，idempotent、無漂移、可完整重建；訊號權重：投入行為（🔬/📚/📎，+2）＞👍（+1）＞😐（−0.3）＞👎（−1.5）＞僅看過（−0.1），套 90 天半衰期讓偏好隨時間漂移
- **Thompson 抽樣排序**：每特徵每天抽一次 Beta θ 當排序分，不確定的特徵自然輪到高位＝內建探索機制；展示分數用 posterior mean 保持穩定
- **MMR 多樣性重排**：對 top 60 貪婪重排，打散同主題連發，避免資訊繭房
- **固定探索槽**（第 6/12/18/24/30/36 位）：推冷門特徵/語意鄰接/隨機驚喜，可量測「跳出同溫層」命中率
- **LLM facet 抽取 + 語意 embedding**：把每篇分類到方法學/場域/族群/樣本數（enum 白名單防幻覺）；用 bge-small（384 維）算語意偏好向量，抓 tag 編不出來的長尾
- **每段都有降級保護**：任一 PRPM 階段失敗即退回前一天的關鍵字排序，不擋主管線

**技術棧**：Python 3.11+（抓取/評分）+ Unpaywall API（開放取用判定）+ 靜態 HTML/CSS/JS 無框架前端 + Cloudflare Pages/Workers/D1/R2（動作層與私密站）+ ntfy（推播）。主機用 Oracle Cloud Free Tier 跑 cron，全部落在免費額度內，可替換成任何 VPS/Raspberry Pi/GitHub Actions。

**自架安全提醒**：Cloudflare Access 要在站台接上公開網域的**同一時刻**設好，不能「先上線再鎖」；主機 cron 用的 API token 只給最小權限（`Pages:Edit` + `D1:Read`，不給寫）；`.env`/`wrangler.toml`/`*.db` 等敏感檔案務必列入 `.gitignore`；ntfy 用有 token 的私有 topic。

**配套下游工具**：作者另開源了 [claude-paper-tools](https://github.com/drpwchen/claude-paper-tools)（`/paper-review` + `/paper-digest` 兩個 Claude Code Skill，含確定性 GRADE 計算與 CrossRef 引用查核）——paper-radar 負責「發現/挑選」，claude-paper-tools 負責「評讀/吸收」，兩者合起來是完整的讀論文 pipeline。

## C（對本知識庫/學術 kit 的啟發）

- [ ] **push 式雷達是 notebooklm-kit 的缺口**：kit 的 paper-discovery 是 pull 式，radar 的「每日自動追蹤→分流」可作為 daily-brief-generator 的上游擴充（v2.3 方向）
- [ ] **興趣回饋迴圈**：paper-discovery v2.2 的 search log（挑選/略過）正是興趣訊號，可再往前一步餵成 interests 權重
- [ ] **機構訂閱徽章**已借鑑進 v2.2（`refs/subscriptions.yaml` → 🏥 標示）
- [ ] **「評讀」（critical appraisal）**與「整理」分開是重要區分——kit 的 TBRC 偏整理，缺方法學評讀層

## 相關連結

- [[agentic-ai-researcher-roadmap]] — 研究者工具地圖（radar 補上持續監測環節）
- [[daily-reading-assistant-ai]] — 每日推送閱讀（同為「化整為零」的消化機制）
- [[academic-research-skills-ars]] — 下游撰稿端（radar 是最上游）
- [[wenhao-yu-ai-workflow]] — 每日 brief 工作流
