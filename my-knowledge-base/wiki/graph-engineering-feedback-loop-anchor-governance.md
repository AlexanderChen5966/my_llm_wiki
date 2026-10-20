---
title: Graph Engineering 治理視角：反饋迴路網路與 Anchor 錨定
date: 2026-07-29
tags: [ai/agent, dev/architecture, type/concept]
source: https://www.eigent.ai/blog/graph-engineering-ai-agents
type: article
status: processed
asset_value: 80
---

## T（核心論點）

Graph Engineering 是把多個反饋迴路（metrics、evals、audits、policies、workflows）織成一張**互相監督、制衡的網路**，而非各自獨立優化——這與既有條目 [[graph-engineering-node-edge-state]] 的「執行圖」定義**同詞不同義**：該篇的節點是**執行單元**（模型調用/工具/人工審批），本篇的節點是**反饋迴路**（指標、評測、稽核、政策）。單一迴路規模化後必然出現四種結構性失敗：**Goodhart 定律**（指標被打穿後脫離真實目標）、**向上盲區**（迴路無法質疑自己的目標值，就像溫度計不能自問 68°F 是否正確）、**迴路間衝突**（各自健康但系統整體震盪，如回應速度迴路與周全度迴路互相拖累）、**量測衰退**（感測器與日誌漂移，儀表板只互相印證不再對照現實）。全文最核心的主張是：**Anchor（外部固定、機制不可改寫的參照，如 held-out eval set、實際營收、人類判斷）才是防止整張圖變成自洽但脫離現實迴音室的關鍵**——拓撲本身無法決定「值得追求的目標」，這個判斷必須留在圖之外。

## B（適用場景）

**適用：** 設計 Agent 系統的長期自我改善機制（Auto-tuning、RL Fine-tuning、Prompt/Strategy 自動搜尋）；判斷多個 KPI/Eval 迴路之間該如何分層、誰能否決誰；避免 Agent 系統的指標優化脫離真實業務目標；為快速迴路與慢速迴路（日/週/季/年）設計不同的權限與 Cadence。

**不適用 / 注意：** 本篇聚焦「Agent 如何隨時間自我調整」的 Improvement Graph（反饋迴路網路），不是「Agent 做什麼」的 Work Graph（工具/技能/檔案/子任務節點）；不涉及單一任務內的執行圖節點/邊/狀態設計（該部分見既有條目 [[graph-engineering-node-edge-state]]，兩者互補而非重疊，建議搭配閱讀）。

## R（四項設計原則）

1. **指標永不單獨出現**：每個被優化的指標都要配對與錨定——Optimization Metrics（解決率、延遲、吞吐量）、Counter-metrics（續約率、錯誤率、安全事件）、無法被操弄的 Anchor Metrics（銀行存款、已驗證的留存率、實體盤點）。優化迴路驅動主指標，監看迴路盯著 Counter-metric 抓廉價取巧，稽核迴路定期把兩者拉回錨點比對。
2. **目標值要有 Owner，不只是設定檔裡的數字**：每個目標由更慢、更高層的迴路擁有，快迴路不能私自改自己的門檻；目標設定本身成為一個受治理的循環，並有自己的指標——調整這個配額的頻率是否與更健康的營收和留存相關。
3. **速度分層並協調**：日常調校迴路、週營運迴路、季策略迴路、年度稽核迴路，各自節奏不同；快迴路不能否決屬於慢迴路的決策，只能把訊號往上呈報，稀疏的層間連結才能保留時間尺度的分離，避免即時優化器把審慎考慮過的安全或合規政策震盪掉。
4. **部分節點刻意凍結不可調**：訓練迴路永遠看不到的 held-out 測試集、優化器不能放寬的硬性安全與法律限制、系統只能讀取不能編輯的真實查核（實體庫存、實際銀行存款）——這些節點存在正是因為優化器會想弱化它們。

## C（關鍵行動）

- [ ] 檢視現有系統的每個優化指標是否都配對了 Counter-metric 與 Anchor Metric，而非單一指標裸奔
- [ ] 明確標示每個目標值的 Owner（哪一層迴路能修改這個門檻），避免快迴路私自調整自己的目標
- [ ] 為系統中的迴路標註 Cadence（日/週/季/年），確認快迴路只能上報訊號、不能覆蓋慢迴路的決策
- [ ] 找出系統中至少一個必須凍結不可調的 Anchor（held-out 測試集、真實營收、人類判斷），並確認優化機制真的無法改寫它
- [ ] 定期自問：這套反饋迴路網路是否仍對照現實校準，還是已經變成只互相印證的自洽儀表板

## 原始摘要

Eigent AI（Douglas Lai）部落格文章，主張 Graph Engineering 是 Loop Engineering 之後的下一層：把多個反饋迴路織成互相監督、制衡的網路。提出單迴路規模化後必然出現的四種結構性失敗（Goodhart、向上盲區、迴路衝突、量測衰退），並強調 Anchor 是防止整張圖變成自洽但脫離現實迴音室的關鍵。文中區分 Work Graph（Agent 做什麼）與 Improvement Graph（Agent 如何自我調整），並以 LangGraph 等執行框架與 Eigent 的 workforce mode 作為承載分層協調迴路的基礎設施案例。

## 相關連結

- [[graph-engineering-node-edge-state]] — **同詞不同義、互補視角**：該篇是執行圖／DAG／Scheduler 的軟體工程實作視角（節點＝執行單元：模型調用/工具/人工審批），本篇是反饋迴路網路的控制論／組織治理視角（節點＝反饋迴路：指標/評測/稽核/政策）；兩者共享「Graph Engineering」一詞但定義的圖節點內涵完全不同，建議互相參照理解兩種不同尺度的圖工程
- [[synthesis/synthesis-multi-agent]] — Multi-Agent 精粹：本篇的迴路衝突與 Goodhart 失敗模式，補強該精粹在長期自我改善機制上的討論
- [[agent-loop-to-graph-workflow-signals]] — Data Machi 系列既有條目：Loop 何時該走向 Graph Workflow 的五個訊號，屬單一任務執行層級，可與本篇的迴路治理層級對照理解「Loop→Graph」在不同尺度下的意涵
