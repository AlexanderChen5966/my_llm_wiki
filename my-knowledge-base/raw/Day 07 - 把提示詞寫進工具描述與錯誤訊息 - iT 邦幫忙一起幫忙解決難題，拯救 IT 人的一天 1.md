---
title: "Day 07 - 把提示詞寫進工具描述與錯誤訊息 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10411905"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "前言 六個工具：get_instructions、search_notes、read_note、create_note、update_note、list_folder。 看起來就是一組 CRUD，但每..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 7

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 7 篇

## Day 07 - 把提示詞寫進工具描述與錯誤訊息

- 分享至

## 前言

六個工具： `get_instructions` 、 `search_notes` 、 `read_note` 、 `create_note` 、 `update_note` 、 `list_folder` 。

看起來就是一組 CRUD，但每一個名稱、每一句描述、每一則錯誤訊息，都在教 agent 怎麼做事。今天講的其實不是 API 設計，是提示詞工程，只是它偽裝成 API 設計。

## 工具描述就是提示詞

模型看不到你的原始碼，它只看得到工具的名稱、描述、參數說明。那幾行字就是你唯一的指揮權。

比較一下這兩種寫法：

```ts
// 寫給人看的
description: '取得指令內容'

// 寫給模型看的
description: '回傳 schema/ 層的 AI 編纂指令全文（等同知識庫的 CLAUDE.md）。動筆前先呼叫本工具。'
```

*出處： [`src/mcp.ts:32`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/mcp.ts#L32)*

第二種多了三件事：回傳的是什麼層的什麼東西、 **什麼時候該呼叫** ，以及一個類比。括號裡那句「等同知識庫的 CLAUDE.md」不是寫給使用者看的，是寫給模型看的。它讀得懂 CLAUDE.md 是什麼，一句話就知道該用什麼態度對待這一層。

## 碰壁之後怎麼辦，寫在錯誤訊息裡

`create_note` 的描述反而寫得很保守：

```ts
description: '在指定 path 建立新筆記。path 須以 raw/、wiki/ 或 schema/ 開頭並以 .md 結尾；已存在則回錯誤（不覆蓋）。'
```

*出處： [`src/mcp.ts:77`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/mcp.ts#L77)*

它只講規則，沒講碰到規則之後該怎麼辦。那句話我放在別的地方，放在錯誤訊息裡：

```ts
throw new NoteError('CONFLICT', {
  'zh-TW': \`筆記已存在：${path}（請改用 update_note 並帶 if_version）\`,
  en: \`Note already exists: ${path} (use update_note with if_version instead)\`,
});
```

*出處： [`src/notes.ts:225`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L225)*

括號裡那半句才是真正在指揮模型的東西。它預先寫好了「碰到這個錯誤時該怎麼辦」，所以真的碰到的時候，agent 不需要猜。我在實測看到的行為正是如此： `create_note` 碰到 CONFLICT 之後，agent 讀完錯誤訊息就自己改用 `update_note` ，沒有卡住，也沒有重試三次。

它不只是給人看的診斷，是給模型看的下一步指示。

而且錯誤訊息比工具描述 **便宜** 。工具描述每一步都要重送一次給模型，寫得越細、每一步越貴；錯誤訊息只在真的碰到的那一次才出現。所以「平常該怎麼做」寫在描述裡，「碰壁之後怎麼辦」寫在錯誤訊息裡，把話講在最省的地方。

## 為什麼 get\_instructions 排第一

規則放在知識庫裡而不是系統提示詞裡，這個取捨之前我們已經算過帳了。但它留下一個壞處沒解決： **模型不會自己知道要去讀那一層** 。

工具設計就是用來補這個洞的，補在三個地方：

1. 把讀規則做成 **第一個** 工具。工具清單的順序會原樣送給模型，排在最前面本身就是一種暗示。
2. 描述裡明寫「動筆前先呼叫本工具」，把時機講死。
3. 模版產生的規則頁裡再重申一次流程，所以它讀完規則會再被提醒一次。

三層都不是強制的，沒有任何程式碼擋住「不讀規則就 create\_note」這條路。實測下來 agent 幾乎每次都會先讀，但那是被引導出來的行為，不是被保證的行為。這是工具設計跟權限檢查最根本的差別： **描述只能影響意圖，擋不住行為** 。真的不能發生的事，還是得寫成程式碼。

## 順便回答「現在該做什麼」

之前我們已經看過產生待編纂清單的那幾行程式。從 agent 的角度，它呼叫 `get_instructions` 拿到的東西是這樣開頭的：

```markdown
## 待編纂的來源（3）

以下 raw/ 來源還沒有任何 wiki/ 頁連回它，請依規則的 Ingest 步驟處理，做完每一則都要在 wiki/log.md 追加紀錄：
- raw/sources/mensh2017ten.md（Ten simple rules for structuring papers）
…
```

規則被推到這份清單後面。這個順序是刻意的：agent 一進來先看到「有三件事待辦」，再看到「該怎麼做」。使用者只要說「幫我整理一下」，它就找得到該做的事，不需要指定檔案路徑。

那句「做完每一則都要在 wiki/log.md 追加紀錄」也是刻意重複的。它跟著待辦清單一起出現，比寫在規則頁的第七行更難被忽略。

## 小結

六個工具的名稱、描述與錯誤訊息，是我唯一能指揮模型的地方。描述講平常怎麼做，錯誤訊息講碰壁之後怎麼辦，兩邊加起來就是這個產品的提示詞。工具只有六個也是同一套算計：每一次呼叫都要把所有描述重送一次，工具越多、每一步越貴。

這份東西歸誰改也分得很清楚。介面與錯誤訊息是我的，改了所有人跟著改；規則頁是使用者的，他改完不必等我發版，我改版也不會動到他寫的那幾行。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10411041)

[

Day 06 - 為什麼選 MCP，以及怎麼用無狀態的 Streamable HTTP 實作

](https://ithelp.ithome.com.tw/articles/10411041)[下一篇](https://ithelp.ithome.com.tw/articles/10412617)

[

Day 08 - 用樂觀鎖擋下使用者與 agent 同時寫入資料庫的衝突

](https://ithelp.ithome.com.tw/articles/10412617)

系列文

[為你自己蓋一座會複利的知識庫——WikiBrain](https://ithelp.ithome.com.tw/users/20184129/ironman/9590) 共 8 篇

目錄

1. 4
	[Day 04 - 用 LLM 執行 Karpathy 的六條準則](https://ithelp.ithome.com.tw/articles/10409915)
2. 5
	[Day 05 - Karpathy 的準則，在 WikiBrain 實現會自我強化的知識庫](https://ithelp.ithome.com.tw/articles/10410555)
3. 6
	[Day 06 - 為什麼選 MCP，以及怎麼用無狀態的 Streamable HTTP 實作](https://ithelp.ithome.com.tw/articles/10411041)
4. 7
	[Day 07 - 把提示詞寫進工具描述與錯誤訊息](https://ithelp.ithome.com.tw/articles/10411905)
5. 8
	[Day 08 - 用樂觀鎖擋下使用者與 agent 同時寫入資料庫的衝突](https://ithelp.ithome.com.tw/articles/10412617)
[完整目錄](https://ithelp.ithome.com.tw/users/20184129/ironman/9590)

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/14/ff4ada01-1df1-40bc-906f-88450c045ae6_small.jpg)

[NC3 Observatory Platform](https://itplus.ithome.com.tw/vod-page/156?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

20 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/21/05871686-40f0-4b12-a187-41075373b1a3.png)

[Organizing Hybrid Conferences: Empowering Community Events from a Technical Perspective During the Pandemic](https://itplus.ithome.com.tw/vod-page/782?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

24 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/21/b72ea45c-9047-4bb8-a57b-6eaf5644897e_small.png)

[生成式 AI CodeGPT 開發經驗談](https://itplus.ithome.com.tw/vod-page/753?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

MWC |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2024/8/2/fa9aa9aa-5e48-4342-a26a-1671e43fe08c_small.png)

[一個關於 API Gateway 的導入評估指引](https://itplus.ithome.com.tw/vod-page/860?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Cloud Summit 臺灣雲端大會 |

30 分