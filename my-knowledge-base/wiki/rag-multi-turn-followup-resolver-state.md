---
title: 多輪對話 RAG：FollowUpResolver 只解代名詞，歷史不能當證據
date: 2026-09-03
tags: [ai/agent, type/how-to]
source: https://ithelp.ithome.com.tw/articles/10405955（Agentic RAG 系列 Day 24）
type: article
status: processed
asset_value: 64
---

## T（核心論點）

基本多輪 RAG 的三個設計目標：用近期歷史解析省略、把追問改寫成可獨立搜尋的問題、讓**歷史只協助理解，不取代知識庫證據**。第一版只保存最近四輪問答（`ConversationState.max_turns=4`），有限視窗同時控制 Token、隱私與錯誤累積。`FollowUpResolver` 接收近期歷史與新問題，只輸出可獨立搜尋的 `search_query`，Prompt 明確規定**歷史回答只用來解析代名詞與省略，不得視為可信事實、不能直接拿來回答**——每一輪都重新 Retrieval，不沿用上一輪來源，因為知識庫可能更新，追問也可能需要不同文件。多輪最危險的是**錯誤與主題一起累積**：只有通過引用驗證的最終回答才加入歷史，即使加入，下一輪仍不能引用歷史作為證據；Session 隔離（不可猜測的 `session_id`）比「記得更多」更重要。

## B（適用場景）

**適用：** 設計有限視窗的多輪對話狀態管理；建立 FollowUpResolver 解析代名詞與省略指涉；判斷追問是延續主題還是切換新主題；設計多人服務的 Session 隔離與生命週期管理。

**不適用 / 注意：** ⚠️ 與既有 Data Machi 條目 [[agent-memory-three-layers-reuse-vs-requery]]（Day 21）同樣談 Reuse vs Re-query 判準，但角度互補——該篇是**通用 Memory 三層架構**（Recent Conversation/Summary/Tool Result）與 Freshness/TTL 概念，本篇是 **RAG 情境的 FollowUpResolver 代名詞解析機制**，強調「歷史只能解析指涉、不可當證據引用」的具體限制，實作層次不同，建議互相 wikilink。本篇「只留四輪」只是容易理解的起點，未真正控制 Token 預算。

## R（操作步驟）

1. **有限對話狀態**：`ConversationState` 只保存最近 4 輪 `question`/`answer`，超過即從最舊輪移除，控制 Token、隱私與錯誤累積；正式產品需替不同使用者建立獨立 Session。
2. **FollowUpResolver 只輸出可獨立搜尋的問題**：Prompt 明確說明「歷史回答只用來解析代名詞與省略，不得視為可信事實，也不能直接拿來回答」；解析失敗、搜尋式為空或超過 240 字時回退原問題。
3. **用完整問題搜尋，回答仍對準原本追問**：Resolver 產生的完整句只進 Retrieval；Generation 仍看到使用者原本問的省略句與本次重新檢索到的來源。
4. **每一輪都重新 Retrieval**：不沿用上一輪來源，因為知識庫可能更新，追問也可能需要不同文件；上一輪回答只能幫助解析主題，不能成為下一輪的證據捷徑。
5. **本機兩輪實測**：第一輪「什麼是 CSRF？」不需改寫；第二輪「如果是無狀態 API，還要開著它嗎？」成功補回主題，改寫成「Spring Security 對於無狀態 API 是否仍需啟用 CSRF 保護」——但也額外加入了原問題沒有的 `RESTful`，提醒 Follow-up 解析同樣可能漂移。
6. **防止錯誤與主題一起累積**：只有通過引用驗證的最終回答才加入歷史；即使加入，下一輪仍不能把歷史答案當證據引用；第一輪拒答時歷史會記錄拒答文字，但第二輪仍要重新檢索。
7. **主題切換不應被歷史硬接**：Resolver 應能輸出 `follow_up`／`new_topic`／`clarification` 等決策；新問題包含完整主詞與新技術實體時應偏向 `new_topic`，不把舊歷史帶進搜尋。
8. **使用者主動修正時以新訊息為準**：「我剛才說的是 Session Cookie，不是 Bearer Token」應重新檢索，不得把已被否定的舊條件繼續當前提。
9. **Session 隔離是最小要求**：以不可猜測的 `session_id` 區隔狀態並再次檢查歸屬權限；避免跨使用者資料外洩；同一 Session 併發請求需加鎖或序號，拒絕過期寫入。
10. **多輪評測要看整段對話**：三輪各自 90% 若彼此獨立，整段成功率只約 73%；評測應以 Conversation 為單位，涵蓋代名詞、省略、主題切換、使用者修正等情境。

## C（關鍵行動）

- [ ] 對話狀態只保留有限輪數，控制 Token、隱私與錯誤累積，並依角色建立獨立 Session 生命週期
- [ ] FollowUpResolver 的 Prompt 明確規定歷史只能解析指涉，不得當作可信事實或直接回答依據
- [ ] 每一輪都重新 Retrieval，不沿用上一輪來源
- [ ] 只有通過引用驗證的回答才寫入歷史，且下一輪仍不可引用歷史當證據
- [ ] 以不可猜測的 session_id 隔離多使用者狀態，並在多輪評測中以整段對話為單位計算成功率

## 原始摘要

「讓 LLM 不只會回答，還會查證：打造 Agentic RAG 智慧知識助理」系列 Day 24：建立有限狀態多輪 RAG，僅保留最近四輪對話，FollowUpResolver 把追問改寫成可獨立搜尋的完整問題（歷史只用於解析代名詞，不得當作可信事實），每輪都重新檢索。本機兩輪實測成功把「它」解析為 CSRF。下一篇（Day 25）進入評測階段，正式計算 Recall@K 與 MRR。

## 相關連結

- [[rag-query-rewriting-protected-tokens-fallback]] — **系列上一篇（Day 23）**：本篇的 FollowUpResolver 延伸該篇問題改寫的受保護 Token 與回退機制到多輪情境
- [[rag-eval-recall-mrr-hit-rate-dedup]] — **系列下一篇（Day 25）**：多輪 RAG 完成後，系列進入檢索評測階段
- [[agent-memory-three-layers-reuse-vs-requery]] — 既有條目（Data Machi Day 21）：同樣談 Reuse vs Re-query 判準，但本篇聚焦 RAG 情境的代名詞解析與「歷史不可當證據」機制，角度互補
