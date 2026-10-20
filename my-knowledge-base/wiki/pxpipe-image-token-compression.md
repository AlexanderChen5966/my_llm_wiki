---
title: pxpipe：把文字/程式碼擠進圖片，用 Fable 5 圖像辨識省 Token
date: 2026-07-09
tags: [ai/workflow, type/reference]
source: 合併自社群分享貼文與官方 GitHub README https://github.com/teamchong/pxpipe
type: article
status: processed
asset_value: 54
---

`teamchong/pxpipe`：本機 proxy，把 Claude Code 請求裡的大塊文字（system prompt、工具說明文件、歷史對話中已收合的舊 turn）渲染成 PNG 圖片再送出，讓模型走視覺通道讀取而非文字 tokenizer。密集內容（程式碼/JSON/工具輸出）圖片 token 效率約 3.1 字元/token，優於文字的約 1 字元/token；官方量測全端帳單省 59-70%（依 workload 而異），但**逐字精確回憶不可靠**，有實測數據佐證且官方誠實揭露。

## T（核心論點）

圖片的 token 成本由像素尺寸決定，與內容文字量無關；密集文字內容轉成圖片後單位 token 承載的字元數遠高於原生文字 tokenizer。這不是傳統的壓縮/摘要類省 token 手法（如 Ponytail 靠減少輸出量），而是利用「視覺通道 token 定價 vs 文字通道 token 定價」的費率結構差異。**核心誠實揭露**：這是有損壓縮，逐字精確的識別碼/hash/密鑰類內容在圖片重讀時會出現「confabulation」（不是辨識失敗報錯，而是無聲地生成看似合理但錯誤的答案）——因為視覺模型讀圖片是 patch embedding 而非 OCR 逐字元判讀，沒有逐字信心分數可以觸發報錯。

## B（適用場景）

搭配 Claude Fable 5 / GPT-5.6（預設允許清單內模型）做 token 密集型 coding session；**不適合**需要逐字精確回憶識別碼、hash、密鑰、人名等 byte-exact 內容的場景（官方文件記載過一次真實失誤：從圖片化的聊天記錄裡誤讀出一個看似合理但錯誤的人名）；Opus 4.7/4.8 與 GPT-5.5 官方標記讀取密集渲染的錯誤率明顯較高（誤讀率約 7%），預設關閉需手動開啟。

## R（技術重點）

**運作機制**：proxy 攔截 `/v1/messages`，把符合資格的大塊內容（>~6k 字元的 tool_result、非最新 turn 的歷史、system prompt+工具文件）重新編碼成 1928px 寬欄位、每頁約 92,000 字元的 PNG 頁面，拼回請求時保留 prompt caching 相容性（靜態前綴不變）；有一個「獲利閘門」（profitability gate，依 N=391 生產資料校準）只在數學上划算時才轉圖，稀疏文字（約 3.5 字元/token 以上）留文字不轉換。

**基準測試（可重現，用模型不可能記憶過的隨機數字題目）**：Fable 5 novel arithmetic 100/100（省 38% token）；gist recall（決策/數值/路徑/名稱回憶，含干擾項，15k-45k 字元 session）98/98；state tracking（值變異 3 次後追蹤最終/最初/計數）18/18；**逐字 12 字元 hex 回憶**：Fable 5 為 13/15，**Opus 為 0/15**（這是 Opus 預設關閉的直接證據）；SWE-bench Lite 試點 10/10（省 65% 請求大小）；SWE-bench Pro 14/19 vs 15/19（省 60%，裁決一致率 18/19）。

**誠實揭露的限制**：有損（尤其逐字識別碼類內容不可靠）／PNG 編碼對大請求增加延遲／CJK 保守支援（ASCII/Latin-1 測試較完整）／verbatim-risk guard 尚未內建，byte-exact 工作建議路由到未在允許清單內的 subagent 模型（走純文字）。README 本身也坦承「大多數 commit 是 Opus/Fable agent session 邊工作邊讀自己的圖片化歷史寫出來的」。

**與社群分享版本的差異**：社群貼文版本只提「約省一半、把 Fable 5 定位成 PM 角色」的概略描述；官方 README 揭露了完整的失效模式、逐字識別碼的實測失敗率、以及「為什麼是無聲錯誤而非報錯」的技術原因（NOT-OCR.md），這些細節對決定是否採用此工具至關重要。

## C（關鍵行動）

- [ ] 僅在「盤點/規劃」等容忍精度損失的任務啟用；code review/精細修改場景關閉或路由到文字 subagent
- [ ] byte-exact 內容（ID/hash/密鑰）務必透過 `CLAUDE_CODE_SUBAGENT_MODEL` 或 agent frontmatter 的 `model:` 指定不在允許清單內的模型處理
- [ ] 導入前先在自己的 workload 上跑 `~/.pxpipe/events.jsonl` 的實測數據，官方數字（59-70%）依 workload 差異大，不宜直接套用
- [ ] Opus/GPT-5.5 讀圖密集渲染錯誤率較高，除非官方更新支援，否則維持預設關閉

## 相關連結

- [[codebase-memory-mcp]] — 同批分享的知識圖譜省 Token 工具（結構查詢取代全文讀取）
- [[claude-fable-5-usage-guide]] — Fable 5 官方使用指南
- [[fable5-subagent-model-routing-design]] — pxpipe 的 byte-exact 逃生門與此文的 model 分角色路由思路一致
