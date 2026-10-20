---
title: "Day 08 - 用樂觀鎖擋下使用者與 agent 同時寫入資料庫的衝突 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10412617"
author:
  - "[[iThome]]"
published:
created: 2026-09-18
description: "前言 假設一個情境，你在網頁上改 wiki/concepts/llm-wiki-pattern.md，改到一半離開座位。同一時間，Claude 正在背景編纂一份剛匯入的論文，讀完之後判斷這份論文的結論..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 8

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 8 篇

## Day 08 - 用樂觀鎖擋下使用者與 agent 同時寫入資料庫的衝突

- 分享至

## 前言

假設一個情境，你在網頁上改 `wiki/concepts/llm-wiki-pattern.md` ，改到一半離開座位。同一時間，Claude 正在背景編纂一份剛匯入的論文，讀完之後判斷這份論文的結論該補進同一頁，於是它也動手改。你回來按下儲存，它三十秒前已經存過一次了。

你按下儲存的那一秒，系統要決定這次寫入放行還是擋下來，而它判斷的依據全部在資料庫裡：那一頁現在是誰的、是第幾版、你讀到的又是第幾版。

## 知識庫的五個資料表

之前我們已經走過的那個流程裡，agent 的工作是讀規則、看目錄、搜尋有沒有相關的既有頁、然後寫一頁新的或改一頁舊的。它需要的是 **結構** ：這一頁連到哪些頁、這個資料夾底下有什麼、哪些來源還沒有人引用過。這些全都是關聯式查詢，所以我用 PostgreSQL，五張表：

```sql
workspaces      -- 每個使用者一個工作區，所有資料的邊界
notes           -- 筆記本體，path 唯一，含軟刪除
note_versions   -- 每次修改的快照
links           -- [[連結]] 解析後的邊，圖譜與反向連結都靠它
tags            -- front matter 的標籤
```

*出處： [`migrations/001_phase1_schema.sql`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/migrations/001_phase1_schema.sql)*

外加 `mcp_tokens` 放存取憑證。

*五張表的關係。 `links.to_note_id` 那條虛線指回 `notes` ，可以是 null，那就是還沒有落點的懸空連結。*

## 工作區隔離：一條紀律

使用者之間的資料如何隔離？我的做法是： **路由層一行筆記 SQL 都不准寫** 。所有以路徑為入口的查詢集中在 `src/notes.ts` ，而那些函式每一個的第一個參數都是工作區 ID。

```ts
export async function readNote(ws: string, path: string) { … }
export async function searchNotes(ws: string, opts: { query: string; folder?: string; tag?: string; limit: number }) { … }
export async function createNote(ws: string, rawPath: string, content: string, actor: Actor) { … }
export async function updateNote(ws: string, rawPath: string, content: string, ifVersion: number, actor: Actor) { … }
```

*出處： [`src/notes.ts:178-261`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L178-L261)*

每一條進得來的查詢都帶 `WHERE workspace_id = $1` 。

要講精確一點： `notes` 這張表不是只有 `notes.ts` 碰得到，健檢、統計、書目匯出、模版套用這些模組也會查它，加起來十幾個檔案。分界線不在「哪個檔案」，在 **哪一種查詢** ：以使用者給的路徑為入口的，一律走 `notes.ts` ；已經拿到 note id 之後的衍生查詢（版本、連結、標籤），靠 id 本身已經被工作區綁定過了。

所以要犯錯只有一種方式：某個模組自己用使用者給的路徑去查表，而沒有帶工作區。這條紀律寫進了專案的貢獻指南，也是安全審查第一個看的地方。

## 版本快照

每次寫入都存一份快照到 `note_versions` 。這讓版本歷史、比較差異、復原都變成查詢問題。

代價是儲存。一頁改一百次就有一百份完整內容。所以有個每日清理：保留最近九十天的快照，最新版永遠留著。免費方案縮到七天。

存完整內容而不是 diff，是刻意的取捨。diff 省空間但每次讀取都要重算，而且鏈中間壞掉一環後面全毀。文字很便宜，資料庫壓縮之後更便宜，我選簡單。

## 搜尋先用最笨的方法

搜尋我一開始用 `ILIKE '%關鍵字%'` ，就是全表掃描。

聽起來很糟，但我先量了再決定：一千頁的工作區大約 35 毫秒，一萬頁、每頁五 KB 大約 350 毫秒；如果加上 `LIMIT 10` 而且關鍵字常見，通常 0.6 毫秒就回來了，因為找到十筆就停。

對一個個人知識庫來說，一千頁已經是很認真的使用者了。所以決定是： **先用最笨的方法，等真的有工作區超過五千頁再上 pg\_trgm 索引** 。這個門檻寫進了規劃文件，也做進了營運面板的監控項目，超過就會提醒我。

不做的東西要記錄下來，還要記錄「什麼時候該做」，否則它只會變成一筆被遺忘的技術債。

## 悲觀鎖會卡死，所以選樂觀鎖

以上是讀取這一側。寫入這一側的麻煩不在效能，在 **同時** 。

處理同時有兩種鎖，差別在什麼時候擋。

**悲觀鎖（pessimistic locking）** 先擋再說。你一打開那一頁開始編輯，它就被鎖住，別人連開都開不了，等你存完才放行。它假設衝突很常發生，寧可讓人排隊。資料庫的 `SELECT ... FOR UPDATE` 就是這一類。

**樂觀鎖（optimistic locking）** 誰都不擋，任何人隨時都能開、能改。只有在按下儲存的那一瞬間才檢查一次：這一頁還是你當初讀到的那一版嗎？是就寫進去，不是就拒絕，請你拿最新的內容重做。它假設衝突很少，而事實上也的確很少。

這座知識庫裡一個人類跟好幾個 agent 混在一起，悲觀鎖會卡死：agent 在背景編纂一頁，使用者連點開來看都不行；反過來使用者開著編輯器去吃午餐，agent 就全部停在那裡。所以我選樂觀鎖，規則只有一句： **修改的時候要說出你以為的版本號** 。

```ts
update_note({
  path: 'wiki/concepts/llm-wiki-pattern.md',
  content: '（新內容）',
  if_version: 7,
})
```

伺服器比對：如果那一頁現在真的是第 7 版，就寫入並變成第 8 版；如果已經是第 9 版了，代表在你讀取之後有人改過，拒絕寫入。

比對跟寫入是同一句 SQL 做完的：

```sql
UPDATE notes SET title = $3, content_md = $4, version = version + 1, updated_at = now()
 WHERE workspace_id = $1 AND path = $2 AND deleted_at IS NULL AND version = $5
RETURNING id, version
```

*出處： [`src/notes.ts:239-241`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L239-L241)*

版本不合就更新到零列， `RETURNING` 回空的，上層據此丟衝突。這裡沒有先 `SELECT` 再判斷再 `UPDATE` ，那種寫法中間有空窗，兩個 agent 可以同時通過檢查。 **條件直接寫在 `WHERE` 裡，資料庫替你把比對跟寫入綁成一個動作。**

## 衝突時要回什麼

拒絕寫入很簡單，難的是 **拒絕之後要給對方什麼** ，那決定了對方能不能自己解決。

錯誤訊息長這樣：

> 版本衝突：wiki/concepts/llm-wiki-pattern.md 目前為 v9，你帶的 if\_version=7。請以目前內容為基礎重新編輯後再送。

而且回應裡 **附上目前的完整內容** 。更新失敗之後我會再查一次那一頁，把版本號、內容、更新時間一起包進錯誤：

```ts
throw new ConflictError(
  { 'zh-TW': \`版本衝突：${path} 目前為 v${n.version}，你帶的 if_version=${ifVersion}。請以目前內容為基礎重新編輯後再送。\` },
  { version: n.version, content: n.content_md, updated_at: n.updated_at },
);
```

*出處： [`src/notes.ts:252-256`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L252-L256)*

這一點對 agent 特別重要。它拿到的不只是「你失敗了」，而是「你失敗了，這是現在的樣子，重做一次」。agent 可以直接把新版內容跟自己原本想寫的東西合併，再送一次，整個過程不需要問人。

如果只回一個 409 狀態碼，agent 唯一能做的就是重讀那一頁再重試，多花一次工具呼叫，還可能在中間又被別人改掉。

## 網頁端：不要丟掉使用者打的字

同一個機制在網頁上要換一種表現方式。

我的第一版做法是：偵測到衝突就跳出提示，按「載入最新版」就把編輯器內容換成伺服器上的版本。

這是錯的。使用者剛打了二十分鐘的字，按下去全部消失。

改成現在的樣子：按鈕文字改為「保留我的修改並載入最新版本號」。按下去之後，編輯器裡的內容原封不動，只把版本號更新成最新的，接著使用者可以自己決定要怎麼合併，然後再存一次。要放棄自己的修改，按取消再重新載入頁面就好。

差別在於 **預設行為要保護使用者的勞動** 。系統可以隨時重新讀取，人打的字重打不回來。

## 為什麼建立也會衝突

`create_note` 碰到已存在的路徑會回 CONFLICT，這也算在同一套機制裡。

agent 常常在不確定某一頁存不存在的時候直接嘗試建立。與其要求它每次先查一次（多一次呼叫、而且查完到建立之間還是有空窗），不如讓建立失敗時把正確做法寫在錯誤訊息裡：

> 筆記已存在：wiki/concepts/xxx.md（請改用 update\_note 並帶 if\_version）

實測 agent 讀到就會自己換工具，一步就接上去。

## 小結

樂觀鎖的意思是誰都不擋。人跟 agent 隨時可以打開同一頁改，只有在按下儲存的那一瞬間比一次版本號，不合就拒絕。使用者或 agent 都不必排隊等對方存完，代價是拒絕真的會發生，而且通常發生在對方已經寫好一整頁之後。透過版本的設計，同時編輯的頁面，可以讓使用者自己決定要如何合併。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10411905)

[

Day 07 - 把提示詞寫進工具描述與錯誤訊息

](https://ithelp.ithome.com.tw/articles/10411905)

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

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/12/20/e32b71d0-e17d-4e0b-a8e3-6127a53a1c44_small.png)

[國泰人壽的可觀測性實踐](https://itplus.ithome.com.tw/vod-page/771?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

38 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/8/8/a3324e03-d6b4-46bd-bec1-1d776537bcd8.jpg)

[深入淺出 RAG：使用 Postgres 打造 RAG 系統](https://itplus.ithome.com.tw/webinar-page/273?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

iThome鐵人賽 |

27 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/709f477b-3e59-49d0-9c99-80cdb72c0d67_small.png)

[運用多維策略，打造企業級設計系統](https://itplus.ithome.com.tw/vod-page/1339?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

46 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/9/15/d3a17d75-d926-4ae7-aa8d-837e7dd463c7_small.png)

[大會主題演講 III【打造韌性數位政府：從資安威脅中突圍的零信任策略】](https://itplus.ithome.com.tw/vod-page/1235?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

數位政府高峰會 |

28 分