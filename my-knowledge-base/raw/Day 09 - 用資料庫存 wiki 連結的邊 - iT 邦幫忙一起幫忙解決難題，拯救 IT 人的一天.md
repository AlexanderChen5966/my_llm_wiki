---
title: "Day 09 - 用資料庫存 wiki 連結的邊 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10413279"
author:
  - "[[iThome]]"
published:
created: 2026-09-23
description: "前言 這座 wiki 的價值在連結。頁跟頁之間互相引用，才會從一堆摘要變成一張網。 之前我們已經看過連結長什麼樣：寫在 Markdown 內文裡的 [[llm-wiki-pattern]]。但我要回答..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 9

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 9 篇

## Day 09 - 用資料庫存 wiki 連結的邊

- 分享至

## 前言

這座 wiki 的價值在連結。頁跟頁之間互相引用，才會從一堆摘要變成一張網。

[之前我們已經看過連結長什麼樣](https://ithelp.ithome.com.tw/articles/10409505) ：寫在 Markdown 內文裡的 `[[llm-wiki-pattern]]` 。但我要回答的問題是「有哪些頁連到這一頁」跟「整張圖長什麼樣」，中間需要一次轉換。

## 第一版：讀取時解析

最直覺的做法是需要的時候再算。要畫圖譜就把所有筆記撈出來，每一篇用正規表示式抓出所有 `[[…]]` ，再去比對哪一頁對應到哪一篇。

能動，但成本是 O(N×M)：N 篇筆記、每篇 M 個連結，每個連結還要再查一次對應的頁面。一個幾十頁的知識庫感覺不出來，幾百頁就開始卡，而且每次打開圖譜都要重算一遍。

反向連結（誰連到我）更糟，因為要掃過所有筆記才知道答案。

## 第二版：寫入時解析

改成在 **寫入的時候** 就解析完畢，存進 `links` 表：

```sql
links(from_note_id, target, to_note_id)
```

這是欄位簡記，不是原始碼。 `from_note_id` 與 `target` 是最初就有的，完整定義見 [`migrations/001_phase1_schema.sql:36-41`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/migrations/001_phase1_schema.sql#L36-L41) （實際還有 `workspace_id` ）。 `to_note_id` 是後來一次遷移加上的欄位（ [`migrations/004_links_resolution.sql:9-10`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/migrations/004_links_resolution.sql#L9-L10) ），存解析出來的目標頁。之後圖譜就是一句 `SELECT` 走 `links` 表，反向連結是 `WHERE to_note_id = ?`，兩個都有索引。複雜度從 O(N×M) 降到 O(E)，E 是邊的數量。

## 連結解析的兩個麻煩

**第一，連結可以指向還不存在的頁。** 這是 Obsidian 的慣例，也很合理：你在寫東西的時候提到一個概念，順手連過去，那一頁之後再寫。所以 `to_note_id` 允許是 NULL，代表這條邊還沒有落點。

那一頁真的被建立時，寫入的路徑上要 **反向補洞** ：新頁一建好，就回頭把所有指向它的懸空連結接上，不用等下次重新掃描，圖譜立刻正確。

```ts
const candidates = [...new Set([n.path, noMd, noMd.replace(/^[^/]+\//, ''), n.basename, n.title])];
await c.query(
  \`UPDATE links SET to_note_id = $1
    WHERE workspace_id = $2 AND to_note_id IS NULL AND from_note_id <> $1 AND target = ANY($3::text[])\`,
  [noteId, ws, candidates],
);
```

*出處： [`src/notes.ts:130-142`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L130-L142)*

那串 `candidates` 是同一頁的五種寫法：完整路徑、去掉 `.md` 、再去掉層級前綴、檔名、標題。懸空的邊只要 `target` 命中其中之一就接上。

這件事不只發生在新增。 **改標題也要補洞** ，因為標題本身就是一種合法的連結寫法。你把一頁從「LLM Wiki 模式」改名成「LLM Wiki」，舊標題寫的連結從此對不到頁，用新標題寫的懸空邊則該接上。

**第二，同一個目標有好幾種寫法。** 使用者可能寫 `[[llm-wiki-pattern]]` 、 `[[wiki/concepts/llm-wiki-pattern]]` 、 `[[wiki/concepts/llm-wiki-pattern.md]]` ，甚至直接寫頁面標題 `[[LLM Wiki 模式]]` 。這些應該都指到同一頁。

解析規則按優先序：完整路徑 → 路徑加 `.md` → 三層前綴加 `.md` → 檔名 → 標題。

關鍵是這條規則只寫在一個地方，一支 SQL 裡：

```sql
WHERE n.workspace_id = $1 AND n.deleted_at IS NULL
  AND (n.path = t.target
       OR n.path = t.target || '.md'
       OR n.path IN ('raw/' || t.target || '.md', 'wiki/' || t.target || '.md', 'schema/' || t.target || '.md')
       OR n.basename = t.target
       OR n.title = t.target)
ORDER BY (n.path = t.target) DESC, …
```

*出處： [`src/notes.ts:110-121`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L110-L121)*

前端渲染連結時也要判斷同樣的事（這條連結有沒有對應的頁），所以前端有一份對應的實作（ [`web/src/lib/links.ts`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/web/src/lib/links.ts) ）。這是刻意接受的重複，但註解裡寫明了規則以後端那支 SQL 為準，改的時候兩邊要一起改。

## 正規表示式要防回溯

上線前的安全審查抓到一個我完全沒想到的問題： **解析連結的正規表示式會被二次方成本打爆** 。

原本的樣式大致是 `\[\[(.+?)\]\]` 。看起來沒問題，直到有人送進一份內容是八萬個連續 `[[` 的筆記。回溯讓它跑了十四秒，而且是 CPU 全滿的十四秒。單執行緒的 Node 在那段時間什麼都做不了，等於一個人就能讓全站停擺。

修法是把中間那段從 `.+?` 改成明確排除的字元類別：

```ts
/\[\[([^\[\]|#\n]+)(?:[#|][^\[\]\n]*)?\]\]/g
```

*出處： [`src/notes.ts:75-84`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L75-L84)*

排除掉 `[`、`]` 、 `|` 、 `#` 跟換行之後，任何一個 `[[` 最多只會往前掃到下一個方括號就停。後半段那個可選群組順便處理了 `[[頁面#段落|顯示文字]]` 的寫法，只取真正的目標。另外加上單頁內容一 MB 的上限，建立與更新都擋。

**任何會跑在使用者輸入上的正規表示式，都要假設有人會餵最壞的輸入給它。** 這種問題不會在功能測試裡出現，因為正常內容永遠不會長那樣。

## 順便：引用也是連結

學術引用 `[@mensh2017ten]` 我也塞進同一套機制。解析的時候把它當成指向那個引用鍵的連結，引用鍵就是來源頁的檔名，所以走的是解析規則裡「檔名」那一條。pandoc 的多重引用 `[@a; @b, p. 12]` 也吃得下來，一則裡面有幾個鍵就產生幾條邊。於是：

- 引用過的來源自動不再是「待編纂」
- 來源頁的反向連結會列出所有引用它的 wiki 頁
- 圖譜上看得到引用關係
- 健檢會抓出指向不存在來源的引用

## 小結

連結是在寫入的那一刻就變成資料庫裡的邊，所以圖譜與反向連結都只是查一次 `links` 表。使用者手打的字串怎麼寫都行，五種寫法、懸空連結、改過的標題，都得在那一刻對到具體的頁，之後的查詢就不必再懂任何一種寫法。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10412617)

[

Day 08 - 用樂觀鎖擋下使用者與 agent 同時寫入資料庫的衝突

](https://ithelp.ithome.com.tw/articles/10412617)[下一篇](https://ithelp.ithome.com.tw/articles/10413861)

[

Day 10 - 用 Vite 渲染 SEO

](https://ithelp.ithome.com.tw/articles/10413861)

系列文

[為你自己蓋一座會複利的知識庫——WikiBrain](https://ithelp.ithome.com.tw/users/20184129/ironman/9590) 共 13 篇

目錄

1. 9
	[Day 09 - 用資料庫存 wiki 連結的邊](https://ithelp.ithome.com.tw/articles/10413279)
2. 10
	[Day 10 - 用 Vite 渲染 SEO](https://ithelp.ithome.com.tw/articles/10413861)
3. 11
	[Day 11 - 用 Canvas 視覺化圖譜](https://ithelp.ithome.com.tw/articles/10414163)
4. 12
	[Day 12 - i18n 與深色模式](https://ithelp.ithome.com.tw/articles/10414909)
5. 13
	[Day 13 - 抓取網頁來源，匯入知識庫](https://ithelp.ithome.com.tw/articles/10415534)
[完整目錄](https://ithelp.ithome.com.tw/users/20184129/ironman/9590)

[![.](https://itadstatic.ithome.com.tw/B9/1789123100_6aa3da1cbf61d.gif)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9002)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/11/6/34f1f6ac-e336-4ddf-84e9-c27c62fa8a60_small.png)

[兼具關連分析的智慧資安託管服務](https://itplus.ithome.com.tw/vod-page/714?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

泰富國際網絡 |

23 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/2/5/39cbb5bd-9778-44c8-8125-b55ec9be9a7a_small.png)

[你的 Secret 安全嗎？透過機密管理降低 DevOps 和雲端安全風險](https://itplus.ithome.com.tw/vod-page/1116?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

KubeSummit |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/11/19/5eae41d8-fde6-4290-a0cc-0f6039e7bb7a_small.png)

[從品牌重造到供應鏈整合的 AI 演化：Cympack 的實戰經驗](https://itplus.ithome.com.tw/vod-page/1345?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

Hello World Dev Conference |

31 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2026/7/18/e13dedef-9af1-4f17-9cf1-7e36f6bd3251_small.png)

[Quality Management Systems in the Agile Era: Balancing ISO 9001 Compliance and Agility](https://itplus.ithome.com.tw/vod-page/1498?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

41 分