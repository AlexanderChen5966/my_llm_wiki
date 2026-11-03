---
title: 對話太長的 7 種 Context Rot 徵兆與對策：什麼時候該開新對話、怎麼交接不丟進度
date: 2026-10-30
tags: [ai/prompt-engineering, ai/workflow]
source: https://www.bnext.com.tw/article/92708/ai-context-rot-seven-signs-start-new-chat
type: article
status: processed
asset_value: 36
---

> 來源與可信度：數位時代（李先泰，2026-10-20）轉述 Every 專欄（Mike Taylor，2026-10-17）。**文末自註「本文初稿為 AI 編撰」**，由編輯整理；原專欄為 Every〈Why I start a new thread in Claude every time〉。文中數字（21%→100%、39%、759→3,855 字）皆為**二手轉述的研究或個人案例**，未經本庫核對原文。Claude／Codex 自動壓縮行為屬版本相依。站內延伸閱讀與日報訂閱導流不納入。

## T（核心論點）

**對話愈長，舊指令、早期錯誤與無關細節會一起留在上下文裡干擾後續回答，這就是 context rot。** Taylor 的做法是「每換一個任務就開新對話」，甚至關閉 ChatGPT 與 Claude 的記憶功能，以確認 AI 手上只有自己給的資料。要延續進度時不是拖著長對話走，而是**先請 AI 寫交接文件（重要事實、已做決定、下一步），確認內容無誤再帶進新對話**。

## B（適用場景）

**適用：**
- 換了任務、或發現 AI 反覆犯同一個錯、忽略你先前的規則
- 一般使用者（ChatGPT、Claude）判斷「該不該重開對話」的檢查表；也可對照 Claude Code 的長 session

**不適用 / 注意：**
- 7 種分類是 Taylor 把書《Context Engineering with DSPy》的分類轉成使用者徵兆，屬**一位實務者的整理**，不是公認標準
- 「每次都開新對話」是作者個人偏好（他同時關閉記憶功能）；是否關記憶要看自己是否需要跨對話的個人化
- 與 [[claude-code-context-management]] 的「世代反轉注記」（長對話要重開不再是鐵律）並存：本篇是「徵兆出現才重開」，不是無條件重開

## R（7 種問題：徵兆與對策）

| # | 名稱 | 徵兆 | 對策 | 文中證據（二手轉述） |
|---|---|---|---|---|
| 1 | 上下文干擾（distraction） | 要求換方向，AI 仍照舊範例的套路 | 語氣或任務類型一變就開新對話 | 請 ChatGPT 用賈伯斯風格取名：只給文字指示時 21% 的名字以「i」開頭；附幾個 i 開頭範例後變 100% |
| 2 | 上下文衝突（clash） | 你補充了，AI 仍沿著先前假設做 | 需求一次講完放第一則；漏了就帶完整需求開新對話 | 微軟與 Salesforce 2025 實驗：指令拆成多則訊息，6 類任務平均表現下滑 39% |
| 3 | 上下文中毒（poisoning） | 更正過的錯誤又出現在下一版 | 不在同對話爭論；回頭編輯出錯前那則訊息，或重新生成 | Gemini 2.5 技術報告：Pro 玩《寶可夢 藍》誤以為需拿只有重製版才有的「TEA」，耗費許多小時 |
| 4 | 上下文混淆（confusion） | AI 一直扯無關的事實、偏好、框架 | 關記憶；自訂指令愈精簡愈好 | 同事把寫作風格指南從 759 字加到 3,855 字，文章糟到認不出來、改 127 版；刪到 513 字才解 |
| 5 | 上下文忽略（neglect） | 其他文件都引用對，偏偏漏掉夾在中間的關鍵細節 | 只上傳需要的文件、問題分開問、重要資訊放最前面 | Lost in the Middle（Stanford 等，2023）：開頭結尾記得住、中間差；9 份文件的第 6 份被漏 |
| 6 | 上下文漂移（drift） | AI 慢慢忽略先前交代的規則 | 固定偏好存成文件，每次新對話貼最前面 | 50 則訊息後 AI 不再遵守 PEP-8；「200 字內」一小時後回到原長度 |
| 7 | 上下文不足（underspecification） | 回答套在誰身上都說得通 | 見下方交接流程 | 缺資訊時 AI 回「訓練資料的平均值」，例如對代理商一再推薦價值定價 |

前 6 種是資訊太多或太舊，第 7 種是太少，兩端都會出問題。

### 交接進度的做法（出現徵兆但對話裡有值得保留的資訊時）

1. 請 AI 寫一份交接文件：重要事實、已做的決定、下一步
2. 離開前問：「這個任務一開始，有哪些資訊如果先知道，就能省掉好幾個步驟或避開錯誤？」
3. 刪掉回答中錯誤或過時的部分
4. 貼進新對話
5. 長期：把常用指令存成 Markdown 檔放 GitHub，讓 AI 需要時再讀，新對話直接用

## C（關鍵行動）

- [ ] 把「固定偏好」（拼字、語氣、長度）存成一份文件，新對話貼最前面（對應第 6 種）
- [ ] 需求一次寫完放第一則訊息，不要邊聊邊補（對應第 2 種）
- [ ] 看到 AI 重複犯已更正的錯，改為編輯出錯前那則訊息而不是繼續爭論（對應第 3 種）
- [ ] 檢查自訂指令是否過長，優先刪減而不是加規則（對應第 4 種）
- [ ] 長 session 要壓縮時，先確認重要指令是否會被自動壓縮掉（Claude／Codex 會自動壓縮，細節可能消失）

## 原始摘要

Every AI 評測主管 Mike Taylor 把《Context Engineering with DSPy》的 7 種上下文問題轉成一般使用者能辨識的徵兆：干擾、衝突、中毒、混淆、忽略、漂移、不足。對策核心是任務一換就開新對話、需求一次講完、固定偏好存文件、需要延續時先寫交接文件再貼入新對話。他的理由是 AI 不會因為你重開 15 次而不耐煩，也不會提醒對話已經一團亂，回答只會默默變差。資料來源包含 Every 專欄、Lost in the Middle 論文（arXiv 2307.03172）、LLMs Get Lost In Multi-Turn Conversation 論文（arXiv 2505.06120）與 Gemini 2.5 技術報告。

## 相關連結

- [[context-isolation-and-compaction]] — Context Pollution／Rot 的名詞與 Isolation／Compaction 判準；本篇的 7 種徵兆是使用者端的症狀分類
- [[get-shit-done-gsd-framework]] — 以「Context 保鮮」對抗 context rot 的 Claude Code 框架；本篇「新對話＋交接文件」是同一思路的一般使用者版
- [[claude-code-context-management]] — Claude Code 的 context 管控與「長對話要重開不再是鐵律」注記，與本篇「徵兆出現才重開」互補
- [[context-engineering-fundamentals]] — Context Engineering 基礎，7 種問題所屬的上位框架
- [[claude-code-session-handoff]] — Claude Code 的交接檔做法，對應本篇的「交接文件」
