---
title: 拜拜 RAG！用 Claude Code 建立你的 LLM Wiki 與關聯知識圖
date: 2026-05-18
tags: [ai/claude, pkm/obsidian, pkm/workflow]
source: Facebook 社群貼文
type: article
status: processed
---

前幾天，AI 大神 Andrej Karpathy 發表了一套新概念：單純利用 Prompt 搭配 Claude Code，就能建立專屬的「LLM Wiki」。建立完成後，你不僅可以使用 Obsidian 來查看視覺化的知識關聯圖，還能將這個 Wiki 發佈成網頁，供團隊或企業內部查詢。

使用方法非常簡單，跟著做就好：

1. 建立專案資料夾：例如建立一個名為 AI2027 的資料夾。

2. 建立原始資料區：在該資料夾內，新增一個名為 raw 的子目錄。

3. 匯入所有文件：將你需要的所有參考資料放入 raw 目錄中，支援的格式包含 Word、Excel、PDF、MD、TXT 等。

4. 取得核心 Prompt：進入 Claude Code，並複製此處的 Prompt 範本（ [https://reurl.cc/2aLl99](https://l.facebook.com/l.php?u=https%3A%2F%2Freurl.cc%2F2aLl99&h=AUD6loXGAvOgWaEKYfvo207MtLkWSdm551c4LX4oPqxk6Z9rOKOvZXPJcmaRP9-_RXYEDOTe831s0amrvsnuPWe6llchqQjZy8aUm97FYNUIgpWpf8UypZEE91d0y731Ab95uDeSy3NoS99A5glvy3kYqTAxT18KXM33BQ&__tn__=-UK-R&c[0]=AUD0B7CCELkPthEpsVu4GLrFu2lH4KYgOnRbmcRkpC8Lghl26-Jto1EEPBj-5iEThr3wCp7m8hAKOcSoRMBkkJ23MfoX58OHq_egs7J3Xl4fCAS90uLZZUeliaWJ2kNcRFXlGqyeIOIMOufLisCkReT2N0x6ZXJnaOj6wrX7q7bkfxcIfn8) ）。

5. 輸入客製化指令：將上述 Prompt 貼入 Claude Code 後，加上以下這段專屬提示詞：

「你是我專屬的 LLM Wiki Agent。請完成上面所有文字的內容實作，當作我的第二個大腦，並一步一步指導我。請建立 CLAUDE.md 作為 Schema 檔案，內含完整的運行規則。接著建立 index.md 和 log.md，定義資料夾的命名規範與傳統，並產出你的第一個文摘範例讓我確認。從現在開始，所有的互動都要遵守 Schema 規則，且全部使用繁體中文。」

送出指令後，放手讓 Claude Code 開始運作即可！

接下來，你會看到 Claude Code 開始自動進行知識的關聯性整理，並建立互相對應的文件。你會發現在原來的目錄下，多出了一個 wiki 資料夾，裡面裝滿了自動生成的 Wiki 頁面。如果你想檢視這些知識的關聯圖，只需用 Obsidian 開啟該專案目錄，一切即可一目了然。

未來如果有了新的文章，只要在 raw 目錄中把新文件放入，然後直接跟 Claude Code 說：「幫我整理新文件」，它就會自動將新知識編譯進 Wiki 中。

這件事的顛覆性在哪？

傳統的 RAG（檢索增強生成）非常複雜，需要架設向量資料庫、使用 Embedding 模型，而且查詢時大多只基於資料碎片來拼湊答案，模型本身並沒有「累積」知識。

但利用 Claude Code 將知識整理成 Wiki，就等同於建立了一個標準的 Graph（圖論）資料庫！它的速度極快、成本極低（只需要訂閱 Claude）。然後你隨時可以請 Claude Code「幫我把這個 Graph 做成網頁發佈」，一個輕量且強大的企業知識庫瞬間成型。

有了它，你可以用自然語言詢問系統任何問題，它都會給出精準的答案並附上對應的來源文章（這不正是 Wiki 的核心價值嗎？）。可以說，RAG 過去的複雜性，已經被 Coding Agent 給大幅消滅了。

如果大家的公司或團隊正好有知識管理的需求，快點動手架起來吧！