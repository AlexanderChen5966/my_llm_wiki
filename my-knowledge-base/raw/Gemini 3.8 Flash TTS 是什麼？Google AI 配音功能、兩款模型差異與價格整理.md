---
title: "Gemini 3.8 Flash TTS 是什麼？Google AI 配音功能、兩款模型差異與價格整理"
source: "https://www.aiposthub.com/gemini-38-flash-tts-ai-studio/"
author:
  - "[[Philo]]"
published: 2026-09-24
created: 2026-09-24
description: "Google 推出 Gemini 3.8 Flash TTS 與 Flash-Lite TTS，新增自訂聲線、逐句語氣控制與雙人對話。本文整理兩款模型差異、繁體中文支援、AI Studio 試用方式、程式串接費用與聲音複製限制。"
tags:
  - "clippings"
status: "inbox"
---
把文章轉成語音，過去通常是選一個預設聲線，再調整語速和音高。Google 在 2026 年 9 月 23 日推出 Gemini 3.8 Flash TTS 與 Gemini 3.8 Flash-Lite TTS，進一步讓使用者設計聲線、逐句安排表演方式，也能讓兩個角色依照同一份劇本對話。Google AI Studio 官方帳號在 [X 公告](https://x.com/GoogleAIStudio/status/2102781516107370894?ref=aiposthub.com) 中，將它們稱為目前最具表現力的語音生成模型。

這次升級的重點，是把 AI 配音從「選一把聲音」推進到「先設計角色，再指揮每句台詞」。它對有聲書、Podcast、影片旁白和多語內容製作有實際用途，但不代表生成結果不必校對，也不能單憑 Google 公布的評測排名就判定它適合所有中文配音。

## Gemini 3.8 Flash TTS 是什麼？它能把文字變成有表演的語音

TTS 是文字轉語音（Text-to-Speech）的縮寫，意思是把寫好的文字轉成語音檔。Gemini 3.8 Flash TTS 和 Flash-Lite TTS 都是這類模型：你提供台詞或旁白，模型輸出音訊，並可調整聲音呈現方式。

這和即時語音對話不同。TTS 的任務是照著腳本生成語音，適合有聲書、廣告旁白或預先錄製的客服內容。Gemini Live API（讓開發者在 App 中呼叫即時語音互動的程式介面）則是處理即時、開放式的語音互動。若你要做能隨使用者提問、即時回話的語音助理，應先看 Live 模型，而不是把 TTS 當成完整的對話系統。 [Google 的 TTS 說明文件](https://ai.google.dev/gemini-api/docs/speech-generation?hl=zh-tw&ref=aiposthub.com) 也把兩種用途分開介紹。

## 兩款 TTS 模型差在哪裡？

簡單來說，Flash 著重聲音細節與表演控制，Flash-Lite 著重大量生成時的速度和成本。Google 文件列出的語言數也不同：Flash 支援 130 種語言，Flash-Lite 支援 101 種，兩款都列有繁體中文文字支援。

| 比較項目 | Gemini 3.8 Flash TTS | Gemini 3.8 Flash-Lite TTS |
| --- | --- | --- |
| 主要定位 | 聲音細節、情緒表演、口音控制 | 高產量、低延遲、成本效率 |
| 官方列出的適用情境 | 有聲書、錄音室旁白、多角色對話、較難的發音與口音 | 大量配音、語音代理流程、朗讀、單人語音生成 |
| 支援語言 | 130 種 | 101 種 |
| 繁體中文文字 | 支援 | 支援 |

對需要細緻控制角色和語氣的創作者，先試 Flash。若每天要產生大量朗讀或配音內容，Flash-Lite 的成本較低。語言清單代表模型接受該語言，不等於每種方言、專有名詞或台灣口音都能一次念對。正式使用前，仍應拿實際稿件試聽，尤其要檢查人名、地名、數字和中英夾雜的句子。 [Gemini 3.8 Flash TTS 文件](https://ai.google.dev/gemini-api/docs/models/gemini-3.8-flash-tts?ref=aiposthub.com) 與 [Flash-Lite 文件](https://ai.google.dev/gemini-api/docs/models/gemini-3.8-flash-lite-tts?ref=aiposthub.com) 提供兩款模型的用途與語言清單。

## 新功能不只換聲音，還能設計角色和逐句安排語氣

第一個改變是「聲線設計」。你可以用文字描述想要的聲音，例如沉穩的自然科學旁白、帶有台灣腔的活潑主持人，或動畫角色的低沉聲線，再試聽並調整。定好的聲線可保存後重複使用，讓同一個角色在不同內容中維持較一致的音色。

第二個改變是逐句控制表演。創作者可以安排語速、情緒、口音、停頓，或加入笑聲、嘆氣等非語言聲音。Google 也展示了雙人劇本功能，能依照不同角色安排輪流說話。這讓配音稿更接近排練好的對話，而不是把整篇文章用同一種語氣唸完。

使用 API 的人要留意一個細節：Gemini 3.8 TTS 會把輸入文字當成要照讀的逐字稿。若把「請用興奮的語氣說」直接寫進台詞，模型可能連這句指示也唸出來。Google 建議把角色和持續的語氣要求放在結構化的語音設定中，台詞本文則只保留真正要聽到的內容。

## AI Studio 怎麼試用 Gemini 3.8 TTS？

不寫程式也能先從 Google AI Studio 試聽。官方公告提供新的語音生成工作區，可從文字描述建立聲線、比較生成結果，再把聲音放進雙人劇本中安排台詞。官方貼文也附有直接開啟語音生成功能的連結。

![](https://www.youtube.com/watch?v=FL6mI_Br-mc)

Google DeepMind 官方示範 Gemini 3.8 文字轉語音與自訂聲線。影片來源： [Google DeepMind 官方 YouTube 頻道](https://www.youtube.com/watch?v=FL6mI_Br-mc&ref=aiposthub.com) 。

初次試用可以先準備一小段自己熟悉的文字，再依序完成三件事：

1. 先選 Flash 或 Flash-Lite。想看聲音表演細節，從 Flash 開始。想測大量朗讀或簡單旁白，可先試 Flash-Lite。
2. 用短描述設定聲線，再為不同台詞安排角色與語氣。
3. 試聽後檢查發音、停頓和情緒是否符合用途，必要時修改文案或聲線描述再生成。

如果要把生成接進網站、App 或大量製作流程，則可透過 Gemini API（供開發者將 Gemini 模型接進自家產品的程式介面）使用模型。AI Studio 適合先試想法，API 則適合接入自己的產品。這類整合要另外考慮用量、隱私和服務條款。

## Gemini 3.8 Flash TTS 價格怎麼算？

截至 2026 年 9 月 24 日，Google 的 Gemini API 定價頁列出兩款新模型的免費層級，並提供付費標準用量價格。2026 年底前是限時價格。從 2027 年 1 月 1 日起，輸入和輸出單價都會提高。

| 標準 API 費率 | 2026 年 12 月 31 日前 | 2027 年 1 月 1 日起 |
| --- | --- | --- |
| Flash TTS 輸入文字 | 每 100 萬 token 0.50 美元 | 每 100 萬 token 1 美元 |
| Flash TTS 輸出音訊 | 每 100 萬 token 9 美元 | 每 100 萬 token 18 美元 |
| Flash-Lite TTS 輸入文字 | 每 100 萬 token 0.50 美元 | 每 100 萬 token 1 美元 |
| Flash-Lite TTS 輸出音訊 | 每 100 萬 token 6 美元 | 每 100 萬 token 12 美元 |

價格按 token（模型處理文字或音訊時使用的計量單位）計算，token 數不等於中文字數或音訊分鐘數。Google 以每秒音訊 25 個 token 換算輸出量。按照這個基準，生成 1 分鐘語音的音訊輸出費，Flash 約 0.0135 美元、Flash-Lite 約 0.009 美元。這只是標準付費層級的輸出費估算，還沒加上輸入文字費、其他服務用量或稅金。實際帳單仍以 Google 當期定價和專案用量為準。

兩款模型目前在定價頁都有免費層級，但免費層級的資料可能用於改善 Google 產品。付費層級則標示不會用於此用途。若要處理客戶資料、未公開腳本或含個人資訊的文字，先確認資料使用條款，再選擇合適的 API 層級。 [Gemini API 官方定價頁](https://ai.google.dev/gemini-api/docs/pricing?ref=aiposthub.com) 列有最新價格和資料用途說明。

## 聲音複製有同意錄音，生成音訊也有 SynthID 浮水印

兩款模型除了從文字設計新聲線，也支援聲音複製。Google 的 API 文件要求提供一段 10 至 30 秒、來自成年說話者的參考錄音，並同時提供同一位說話者清楚表達同意的錄音。這項設計讓使用者能複製自己的聲音，也為聲音本人授權建立一道必要程序。

這不代表取得一段音檔就能任意模仿任何人。製作或散布聲音前，仍要取得本人同意並確認肖像、著作、合約及平台規則等相關權利。同意錄音也不會自動替使用者處理所有法律問題。

Google 表示，Gemini Audio 生成的音訊都會加入 SynthID，這是一種人耳聽不出的浮水印，可協助辨認 AI 生成內容。它能增加透明度，但不應取代清楚標示、本人授權和發布前的人工作業。 [Google 的聲音複製文件](https://ai.google.dev/gemini-api/docs/voice-replication?ref=aiposthub.com) 列出錄音與同意要求， [Google DeepMind 的 SynthID 說明](https://deepmind.google/models/synthid/?ref=aiposthub.com) 則介紹浮水印用途。

## Google 公布的排名代表什麼？仍要用自己的內容試聽

Google 表示，Gemini 3.8 Flash TTS 在 Hume AI 的 Voice Design Benchmark 獲得整體第一名，聲音口音建模也排名第一。Flash 與 Flash-Lite 在 Hume 的 Overall Quality Index 分居第一、第二。這些結果支持 Google 對模型能力的宣傳，但仍是特定測試項目下的成績。

Hume 對自己的 Real World VoiceEQ Benchmark 說明，不同語音能力要分開衡量：自然度、情緒表現、聲線一致性和穩定性並不是同一件事。因此，單一總分不能保證你的中文旁白、台灣口音或專業術語都同樣出色。選模型時，應用實際稿件試聽，也可請目標聽眾盲聽，再決定品質是否足以進入正式流程。 [Google 發布的評測說明](https://blog.google/innovation-and-ai/models-and-research/gemini-models/gemini-3-8-text-to-speech/?ref=aiposthub.com) 與 [Hume 評測框架介紹](https://www.hume.ai/rw-voice-eq?ref=aiposthub.com) 可供查閱。

## Gemini 3.8 TTS 值得試嗎？先依工作量選 Flash 或 Flash-Lite

若你的工作需要設計角色、製作有情緒起伏的旁白，或讓多人依照劇本對話，Gemini 3.8 Flash TTS 值得先試。若重點是大量、較低成本地生成日常朗讀或語音內容，Flash-Lite 是更直接的起點。兩者都提供聲線設計，也都支援繁體中文文字。

這次更新讓創作者能更細緻地控制 AI 配音，也把聲音複製和生成後辨識納入同一套工具。但實際產出仍受稿件、語言口音和使用情境影響。先用自己的文字比較，再把發音校對、聲音授權和資料處理納入流程，才能判斷它是否真的省下錄音與後製時間。

## FAQ

### Gemini 3.8 Flash TTS 和 Flash-Lite TTS 有免費額度嗎？

Google 的 Gemini API 定價頁目前列出兩款模型的免費層級，實際免費用量仍受 API 額度與速率限制約束。免費層級資料可能用於改善 Google 產品。付費層級則標示不會用於此用途。

### Gemini 3.8 Flash TTS 支援繁體中文嗎？

兩款模型的語言清單都列有 Chinese (Hant script)，也就是繁體中文文字。這不代表模型一定能自然說出台灣口音，建議拿常見的人名、地名和中英混合句子實際試聽。

### Gemini 3.8 TTS 能做即時語音聊天嗎？

TTS 適合把已寫好的文字轉成語音。需要模型聽使用者說話、即時回答的互動流程，應改看 Gemini Live API。

### Gemini 3.8 Flash TTS 可以複製別人的聲音嗎？

技術上支援聲音複製，但 Google 要求參考錄音和同一位成年說話者的同意錄音。使用他人的聲音時，應先取得本人明確授權，並確認使用情境和發布方式符合相關權利與條款。