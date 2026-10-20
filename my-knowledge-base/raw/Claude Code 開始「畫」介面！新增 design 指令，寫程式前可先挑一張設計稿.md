---
title: "Claude Code 開始「畫」介面！新增 /design 指令，寫程式前可先挑一張設計稿"
source: "https://www.bnext.com.tw/article/91917/claude-code-design-command-early-preview"
author:
  - "[[李先泰]]"
published: 2026-08-19
created: 2026-08-19
description: "寫程式免開設計軟體！Anthropic在Claude Code新增/design指令，一句話產出多張可編輯的介面設計稿。"
tags:
  - "clippings"
status: "processed"
---
寫程式之前，先讓 AI 給你幾張設計稿挑。Anthropic 產品設計師 Nate Parrott 美國時間 8 月 17 日在 X 平台宣布，開放 Claude Code「/design」指令的早期預覽：在終端機或桌面版輸入「/design 為這個功能設計幾個選項」這類指令，Claude 會先產出多張可編輯的介面設計稿（畫板），開發者挑出喜歡的一張、直接修改，再讓 Claude 照著實作成程式碼。

編輯功能沿用自 Anthropic 今年 4 月推出的設計工具 Claude Design，執行「claude update」更新後即可試用。

## 讀你的程式碼庫，配你的 UI 風格

這項功能瞄準的是 AI 寫介面的老問題： **開發者只能用文字描述想要的畫面，生成結果不對就重來一輪。** /design 把順序倒過來，先看圖、再施工。Parrott 說明，Claude 會讀取使用者的程式碼庫、比對現有 UI 風格，把設計稿做成可分享的模擬畫面，放進一個 Artifact（Claude 生成、可獨立預覽與分享的互動內容）裡。

掌握最新AI、半導體、數位趨勢！訂閱《數位時代》日報及社群活動訊息

產出的畫板可以直接動手改。Parrott 表示，團隊把 Claude Design 的編輯器與提示功能「直接整合進 Claude Code」，開發者能在畫布上調整設計，不必回頭改文字指令。有開發者在貼文下追問：畫板內容會延續到建置步驟嗎，還是得在提示裡重新描述一次版面？Parrott 回覆「會延續過去」，但提醒這版預覽仍要手動按下「儲存」，設計才會帶進實作，並表示很快就會取消這個步驟。

值得注意的是，Parrott 也坦言實作是否遵循設計「取決於模型」，只是模型「在這方面變得越來越好」。換句話說， **畫板是給模型看的施工圖，不是硬性規格** ，成品仍可能與設計稿有落差。

## 與 Claude Design 是什麼關係？

Claude Design 是 Anthropic 今年 4 月 17 日以研究預覽形式推出的獨立產品，透過對話生成設計、原型與簡報，開放 Pro、Max、Team 與 Enterprise 訂戶使用。官方公告寫明，它會讀取團隊的程式碼庫與設計檔，自動建立專屬的設計系統。

兩個產品原本就有通道。官方公告寫明，設計完成後 Claude 會打包成「交接包」，一道指令就能交給 Claude Code 接手開發；Claude Code 這端也早有「/design-sync」指令，能把程式庫裡的 React 設計系統上傳到 Claude Design，讓設計採用團隊的真實元件，也能透過 MCP（外部工具串接協定）在終端機直連設計功能。

/design 的新意在於入口：把「先產生設計選項、直接編輯，再交給 Claude 實作」整套流程，縮成 Claude Code 裡的一個指令。對開發者最直接的差別，是不必先從 Claude Design 或 MCP 流程進入，直接在寫程式的現場就能叫出設計畫板。

## 三個關鍵問題，官方還沒回答

目前該功能仍在早期預覽定位，目前開發社群提出的疑問包括：第一，畫板會存成程式庫裡的檔案，還是只存在於單次工作階段？前者才能讓設計審查進入團隊的程式碼審查（PR）流程；第二，能否載入使用者已在 Claude Design 建好的設計系統？這決定新入口能不能接上既有的設計資產；第三，當實作結果偏離畫板時，Claude Code 會不會主動修回來？

在這三個答案出爐之前，/design 比較像寫程式前的快速草圖工具；要成為正式開發流程的一環，還得看 Anthropic 接下來怎麼補。

> 延伸閱讀： [AI封裝關鍵零件！矽電容概念股有哪些？力積電打進英特爾，這檔是台股「最純」](https://www.bnext.com.tw/article/91904/what-is-silicon-capacitor-stocks)  
> 資料來源： [Nate Parrott X 貼文](https://x.com/nateparrott/status/2089470636796059754) 、 [Anthropic 官方公告](https://www.anthropic.com/news/claude-design-anthropic-labs) 、 [Claude Code 官方文件](https://code.claude.com/docs/en/commands) 、 [Claude Code v2.1.235 更新紀錄](https://github.com/anthropics/claude-code/releases/tag/v2.1.235) 、 [Anthropic 說明中心](https://support.claude.com/en/articles/14604416-get-started-with-claude-design) 、 [Anthropic 官方部落格](https://claude.com/blog/how-the-product-designer-who-built-claude-design-uses-it-to-explore-ideas-before-building-them)

本文初稿為AI編撰，整理．編輯/ 李先泰