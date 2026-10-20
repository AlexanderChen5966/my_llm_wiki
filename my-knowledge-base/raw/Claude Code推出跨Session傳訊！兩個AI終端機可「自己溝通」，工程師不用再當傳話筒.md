---
title: "Claude Code推出跨Session傳訊！兩個AI終端機可「自己溝通」，工程師不用再當傳話筒"
source: "https://www.bnext.com.tw/article/91794/claude-code-cross-session-messaging"
author:
  - "[[李先泰]]"
published: 2026-08-10
created: 2026-08-10
description: "Anthropic於8月7日釋出Claude Code v2.1.224，新增跨session 訊功能，平行執行的多個session可互相傳話。"
tags:
  - "clippings"
status: processed
---
同時開兩個終端機跑 Claude Code 的人，大概都遇過同一種麻煩：左邊那個 session 剛改掉一段程式的介面，右邊那個 session 正在動同一個專案，你得自己切過去，把剛剛發生的事重講一遍。

Anthropic 在美國時間 8 月 7 日釋出的 Claude Code v2.1.224，把這件差事交給 AI 自己做。官方帳號 ClaudeDevs 在 X 發文說明這項名為 cross-session messaging（跨 session 傳訊）的功能： **「你的 session 現在可以互相傳訊息。不必再到另一個 session 重新解釋一遍，你可以叫 Claude 去講。」**

目前該功能僅支援 macOS 與 Linux，原生 Windows 不在範圍內。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

## 傳過去的是一段話，不是「整段對話」

官方文件把這件事定義得很窄，其實訊息就是一個 Claude 寫給另一個 Claude 的一段純文字（可理解成上一個session的摘要）。接收端只看得到這段文字和寄件端的名字，看不到對方的對話歷史，也拿不到對方的檔案。因此如果是要把「整個脈絡搬過去」，官方建議改用接續舊對話（resume session）的做法，而不是用這個新功能。

送出去的訊息長什麼樣？官方文件給的實例是這樣一句話：「Schema 遷移完成，新欄位是 tenant\_id，現在 rebase 到 main 是安全的。」 **把工程術語拿掉，就是「我這邊改完了，你可以動了」。**

接收端不會被打斷。文件寫明，收訊的 Claude 在兩次工具呼叫的空檔讀訊息，執行到一半的指令不會中斷；對方若正好閒置，才直接開一輪新對話把訊息交出去。訊息送達後計入用量，跟你自己打字下指令一樣。

## 三種典型用法

先講清楚操作層級：兩個新工具（ListAgents 列出聯絡得到哪些 session，SendMessage 把訊息送出去）都不用你自己呼叫，訊息內容也不必你擬。你只要用日常語言講出想讓誰知道什麼，Claude 會自己找對象、自己把話寫出來。

官方文件列出三種場景。第一種是跟長時間跑的任務要進度，官方範例的指令直譯過來是 **「問一下我另一個終端機的 session，那個 migration 跑完了沒」。**

第二種是把剛做完的決定交接出去，官方範例是 **「跟那個在處理 payments API 的 session 說明我們剛剛做了什麼」。** 這句正好示範前面那個重點：指令沒有規定訊息要怎麼寫，內容全由 Claude 決定。

第三種是多個 worktree（同一個專案的平行工作副本，讓你不必切換分支就能同時改兩條線）各自開工時互相同步。這一種官方沒有附範例指令，實際上你會說的大概是 **「我剛把登入驗證那段的介面改掉了，通知其他正在動這個專案的 session」。**

上述案例的共通點就是，你可以直接要求 AI Agent 直接跨 session 作業，不用自己切來切去。

## 跨session傳訊不是零限制

要留意的是，跨 session 傳訊目前僅支援 macOS 與 Linux（含 WSL 2 環境），原生 Windows 沒有這項功能，透過 Amazon Bedrock、AWS 上的 Claude Platform、Google Cloud 的 Agent Platform 與 Microsoft Foundry 使用 Claude Code 時同樣沒有。

在傳訊權限上，Anthropic 把三件事直接鎖死：別的 session 傳來的訊息不算你本人的同意，無法代你按下權限確認；訊息不能要求接收端修改權限設定或 `CLAUDE.md` 這類設定檔；訊息裡若寫了 `/compact` 這種指令，抵達時只會被當成純文字，不會執行。

收訊端另有 accept（照收）、hold（先扣住等你批准）、refuse（直接丟掉）三段控制。沒有特別設定時，Claude Code 依兩邊 session 的權限模式自動判斷，企業則可以在管理設定裡一次關掉整個組織的收發。

兩個 session 會不會互相喊話喊到沒完？文件寫明有節流：同一寄件端的重複訊息會限速，短時間內完全相同的直接丟棄，等待讀取的訊息上限是每個 session 50 則。換句話說，訊息迴圈會自己停下來。

Anthropic 手上原本就有 subagents、agent teams、agent view 與 Remote Control，分別處理一個 session 內部的分工、被監督的 AI 團隊、多 session 集中監看與手機遠端操控。這次補上的是最後那條線： **各自獨立、由你分別駕駛的 session 之間要怎麼講話，而這條線的邊界收得很緊，純文字、不傳權限、收訊端保有否決權。**

一般的跨 session 傳訊需要 Claude Code v2.1.224 以上；要主動聯絡其他機器上的 Remote Control session，則需要 v2.1.225 以上。想確認自己這台機器拿到了沒，在 session 內輸入 `/list-agents` 就會知道。

> 延伸閱讀： [泰國4兆市值王也是台達的！從泰達電看它全球布局：「英雄模型」是什麼？38年前為何逆勢南向？](https://www.bnext.com.tw/article/91745/delta-bk) 本文初稿為AI編撰，整理．編輯/ 李先泰