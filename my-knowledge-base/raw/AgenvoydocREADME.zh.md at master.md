---
title: "Agenvoy/doc/README.zh.md at master"
source: "https://github.com/pardnchiu/Agenvoy/blob/master/doc/README.zh.md"
author:
  - "[[pardnchiu]]"
published:
created: 2026-06-22
description: "Make AI actually work for you - A personal agent that writes its own tools and repairs itself. - Agenvoy/doc/README.zh.md at master · pardnchiu/Agenvoy"
tags:
  - "clippings"
status: processed
---
![Agenvoy](https://github.com/pardnchiu/Agenvoy/raw/master/doc/logo.svg)

**跑在你電腦上的個人 AI Agent**

一句話建工具，自動測試，直接呼叫。  
讓你現有的 Agent 也能自己造工具。  
你說一句話，剩下 Agent 幫你完成。

## 一鍵安裝

> MacBook 建議額外執行 `sudo pmset -c sleep 0` ，避免休眠影響排程。

```
curl -fsSL https://cloud.agenvoy.com/install.sh | bash
```

---

## 你可以這樣用它

| ### 查資料  > 台北天氣如何？ >  > Agent 找資料、呼叫工具、整理結果後回答你。 >  > 如果沒有工具，它會自己建立。 | ### 建立自動化  > 每天早上 8 點報台積電股價 >  > Agent 會確認： >  > 確認後自動建立排程。 |
| --- | --- |
| [![](https://camo.githubusercontent.com/84bf97d95b6db41f2d913b88db5bf13cb92da4ae3ad3f8fb5dc61c414f473716/68747470733a2f2f692e7974696d672e636f6d2f76692f666c6f4d427341667a69592f6d617872657364656661756c742e6a7067)](https://youtu.be/floMBsAfziY) | [![](https://camo.githubusercontent.com/81bd03d2b71196710c992d7bbc634e68b2765c4e0a30d8123e17a2ef8abf6aea/68747470733a2f2f692e7974696d672e636f6d2f76692f35546f336a6f4b6c4670552f6d617872657364656661756c742e6a7067)](https://youtu.be/5To3joKlFpU) |
| ### 搜尋你的文件  > 找出去年所有報價單 >  > 哪份文件提到 Prompt 指南？ >  > Agent 直接從你的文件搜尋答案。 | ### 完成長流程工作  > 幫我整理今天 GitHub Commit 並產生進度摘要 >  > Agent 可以拆解任務、呼叫工具、整合結果，再回覆給你。 |
| [![](https://camo.githubusercontent.com/9c91e92bc018da8301fc2cf92fc305baa708b33334c04c33b6ea4ca2a02a3e95/68747470733a2f2f692e7974696d672e636f6d2f76692f76716f513651766c3871552f6d617872657364656661756c742e6a7067)](https://youtu.be/vqoQ6Qvl8qU) | [![](https://camo.githubusercontent.com/423e926804eb8d90eb0eb1e183747740a894a80a5258a8663f7edcb5465c2883/68747470733a2f2f692e7974696d672e636f6d2f76692f6e495631787a5f48494a672f6d617872657364656661756c742e6a7067)](https://youtu.be/nIV1xz_HIJg) |

### 讓你正在用的 AI Agent 擁有自建工具的能力

> Agenvoy 同時是 MCP server。
> 
> Claude Code、Codex、OpenCode 等 AI Agent 直接連上，就能：
> 
> - 使用你所有的沙箱工具
> - 找不到工具時自動建立新的
> - 建好的工具所有 Agent 共用
> 
> 一行設定，工具庫即時共享。 影片中建立的範例： [`fetch_weather`](https://github.com/pardnchiu/Agenvoy/blob/master/doc/demo/fetch_weather) 、 [`fetch_crypto_price`](https://github.com/pardnchiu/Agenvoy/blob/master/doc/demo/fetch_crypto_price)

| #### Claude Code 建立天氣工具 (1) | #### Codex 複用天氣工具，再建加密貨幣工具 (2) | #### Agenvoy 測試兩個工具 (3) |
| --- | --- | --- |
| [![](https://camo.githubusercontent.com/6659827c8c43967a8832627ba63ffce8dcf21fb14f13fff24bbf3527b6a9db89/68747470733a2f2f692e7974696d672e636f6d2f76692f6f6e3549616f78424f31452f6d617872657364656661756c742e6a7067)](https://youtu.be/on5IaoxBO1E) | [![](https://camo.githubusercontent.com/7ddc078c8a64729fa17ccada850e4aa322103d291d14fda78eca75551bd202d8/68747470733a2f2f692e7974696d672e636f6d2f76692f32444446434963626e736f2f6d617872657364656661756c742e6a7067)](https://youtu.be/2DDFCIcbnso) | [![](https://camo.githubusercontent.com/2173e0d6953d30d223029bc41897a6973f2577c911c6ba686a32f2297a613809/68747470733a2f2f692e7974696d672e636f6d2f76692f4b5073346f397844466a4d2f6d617872657364656661756c742e6a7067)](https://youtu.be/KPs4o9xDFjM) |

---

## 核心能力

| 能力 | 說明 |
| --- | --- |
| 自動工具生成 | 缺工具時自行建立並保存 |
| 自我排程 | 一句話建立定時任務 |
| 長期記憶 | 保留重要資訊與上下文 |
| 文件搜尋 | 從本機文件回答問題 |
| Sub-Agent | 多 Agent 協作 |
| MCP client | 連接外部 MCP 服務 |
| MCP server | 讓任何 AI Agent 使用你的沙箱工具 |
| Tool Market | 分享與安裝工具 |
| 語音轉錄 | 音訊與影片轉文字 |
| 自我改進 | 執行失敗後自動修正 |

---

## 跟其他工具比

|  | **Agenvoy** | OpenClaw | Hermes-agent |
| --- | --- | --- | --- |
| 安裝方式 | 一行指令，單一檔案 | pnpm monorepo | pip + docker |
| 多模型同時用 | 自動選 | 手動切 | 手動切 |
| 對話 UI | 按鈕／選單／modal | 純文字 | 純文字 |
| 自己生成工具 | ✅ | ❌ | ⚠️  僅 skill |
| 聊天驗證 | 6 碼驗證碼 | 人工核准 | 人工核准 |
| 跨 session 推送 | ✅ | ❌ | ⚠️  有限 |
| 文件搜尋 | 語意＋關鍵字 | 僅對話記憶 | 僅對話記憶 |

---

## 文件

- [新手入門](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Getting-Started.zh.md)
- [架構](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Architecture.zh.md)
- [核心概念](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Core-Concepts.zh.md)
- [Provider 設定](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Providers.zh.md)
- [工具系統](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Tools.zh.md)
- [記憶系統](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Memory-System.zh.md)
- [Skill 系統](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Skill-System.zh.md)
- [MCP 整合](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/MCP-Integration.zh.md)
- [安全與沙箱](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Security-and-Sandbox.zh.md)
- [命令列參考](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/CLI-Reference.zh.md)
- [設定檔](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Configuration.zh.md)
- [產品對照](https://github.com/pardnchiu/Agenvoy/blob/master/doc/wiki/Comparison.zh.md)

## License

本專案以 [Apache License 2.0](https://github.com/pardnchiu/Agenvoy/blob/master/LICENSE) 授權。

## 社群貢獻者

 [![Azetry](https://github.com/Azetry.png)](https://github.com/pardnchiu/Agenvoy/issues/3)[![oceanasd](https://github.com/oceanasd.png)](https://github.com/pardnchiu/agenvoy/issues/49)

## Contributor

歡迎 [開 issue](https://github.com/pardnchiu/agenvoy/issues/new) 分享想法。

[![Agenvoy contributors](https://camo.githubusercontent.com/356b9f53ef6029e70284f01480a9b43c02d0d40952f36c8bdef90fda1ba90ee0/68747470733a2f2f636f6e747269622e726f636b732f696d6167653f7265706f3d706172646e636869752f6167656e766f792663616368655f627573743d323032362d30352d3132)](https://github.com/pardnchiu/agenvoy/graphs/contributors)

## Star History

[

![Agenvoy star history](https://camo.githubusercontent.com/c506d962afdf0934b83b3781a36df66026e39fff487b5dee056db173a9396969/68747470733a2f2f6170692e737461722d686973746f72792e636f6d2f7376673f7265706f733d706172646e636869752f6167656e766f7926747970653d446174652663616368655f627573743d323032362d30352d3132)

](https://star-history.com/#pardnchiu/agenvoy&Date)

曲線往上走，就是我們想要的訊號。按 ★ 推一把。

---

©️ 2026 [邱敬幃 Pardn Chiu](https://www.linkedin.com/in/pardnchiu)