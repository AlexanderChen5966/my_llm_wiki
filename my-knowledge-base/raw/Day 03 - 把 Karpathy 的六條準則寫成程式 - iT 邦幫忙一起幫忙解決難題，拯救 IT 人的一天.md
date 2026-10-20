---
title: "Day 03 - 把 Karpathy 的六條準則寫成程式 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天"
source: "https://ithelp.ithome.com.tw/articles/10409772"
author:
  - "[[iThome]]"
published:
created: 2026-09-17
description: "前言 Karpathy 那則 gist 立了六條準則：三層是 raw/、wiki/、schema/，三個操作是 Ingest、Query、Lint。整篇文章都在講這六條該是什麼，但沒有一行程式碼，因為..."
tags:
  - "clippings"
status: processed
---
[2026 iThome 鐵人賽](https://ithelp.ithome.com.tw/2026ironman)DAY 3

0

[佛心分享-SideProject30](https://ithelp.ithome.com.tw/2026ironman/side-project-30)

### 為你自己蓋一座會複利的知識庫——WikiBrain系列 第 3 篇

## Day 03 - 把 Karpathy 的六條準則寫成程式

- 分享至

## 前言

Karpathy 那則 gist 立了六條準則：三層是 `raw/` 、 `wiki/` 、 `schema/` ，三個操作是 Ingest、Query、Lint。整篇文章都在講這六條該是什麼，但沒有一行程式碼，因為可以由 LLM 來實作：

> This document is intentionally abstract. It describes the idea, not a specific implementation.

（這個專案後端是跑在 Node 24 上的 TypeScript，HTTP 用 Express 5，資料庫是 PostgreSQL，考慮閱讀的流暢度，本篇僅介紹實作 Karpathy 六條準則的程式與邏輯。）

## raw/

`raw/` 要不可變。實作上這件事要擋兩次，因為有兩種弄壞它的方式。

第一道擋改寫，在 `updateNote` 的最前面：

```ts
if (layerOf(path) === 'raw') {
  throw new NoteError('FORBIDDEN', { 'zh-TW': \`raw/ 為唯讀來源層，不可更新：${path}\`, en: … });
}
```

*出處： [`src/notes.ts:231-235`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L231-L235)*

第二道擋刪除。我一開始只做了第一道，後來才發現網頁介面還是可以刪，而刪掉一份來源之後，所有引用它的 wiki 頁都會指向一個不存在的頁面：

```ts
// raw/ sources are immutable and cannot be deleted from any client; archive them instead (archiveNote).
if (layerOf(path) === 'raw') {
  throw new NoteError('FORBIDDEN', { 'zh-TW': \`raw/ 為不可變的來源層，不能刪除：${path}。要收起來請用「封存」\`, en: … });
}
```

*出處： [`src/notes.ts:365-366`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L365-L366)*

擋掉刪除不代表來源就永遠佔著位子——要收起來有另一條路，也就是之前我們已經講過的封存。錯誤訊息直接把那條路寫出來，因為擋住一個動作的時候，順手告訴對方該走哪裡，比只說「不行」有用得多。

值得注意的是這兩道門寫在哪裡：它們都在 `src/notes.ts` 裡面，也就是 **所有客戶端都得經過的那一層** 。網頁介面、MCP、REST API 三條路最後都呼叫同一組函式，所以守門只要寫一次。

這件事是被上面那個 bug 教會的。如果守門寫在路由上，那每多一個入口就要記得再擋一次——而我第二道門漏掉，正是因為當時只想到 agent 會呼叫的那條路，忘了網頁介面是另一條。放在這一層，新入口就算是明年才加的，也自動被擋住。

## wiki/

`wiki/` 是 agent 的工作區，它有完整的建立與改寫權限。這一層的程式碼反而沒什麼好講的，值得講的是 **沒有寫出來的那個東西** ：

agent 能用的工具一共六個：

```ts
export const TOOLS: ToolDef[] = [
  { name: 'get_instructions', … },
  { name: 'search_notes', … },
  { name: 'read_note', … },
  { name: 'create_note', … },
  { name: 'update_note', … },
  { name: 'list_folder', … },
];
```

*出處： [`src/ingest.ts:44-51`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/ingest.ts#L44-L51)*

裡面沒有 `delete_note` 。所以可以任意改寫、重組、搬動，但沒有任何一個動作能讓一頁真的消失。

另外，寫入一頁 wiki 不是只寫一列：版本快照、連結、標籤是在同一個交易裡一起寫的（ [`writeDerived`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L145) ）。所以不會出現「內容存進去了但連結沒更新」的中間狀態。圖譜跟反向連結永遠跟內容同步。

## schema/

規則層是使用者可以自己改的那一層，所以它不能寫死在程式裡。 `getInstructions` 的作法就是去讀那個資料夾：

```sql
SELECT path, content_md FROM notes
 WHERE workspace_id = $1 AND deleted_at IS NULL AND path LIKE 'schema/%'
 ORDER BY path
```

*出處： [`src/notes.ts:286-291`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L286-L291)*

讀出來原樣接起來回傳，一個字都不改寫。這一層裡有什麼，就是 agent 動筆前會讀到什麼。

Karpathy 把這一層類比成 CLAUDE.md，我在工具描述裡也照抄了這個類比——那句話是寫給模型看的，它讀得懂 CLAUDE.md 是什麼。

那句 `LIKE` 讀出來的東西長這樣。每個新工作區開出來就有一份基礎規則頁，它在 repo 裡是一個普通的 Markdown 檔，套用時原封不動塞進 `schema/` ：

```markdown
# 編纂規則（基礎）

這一頁等同整座知識庫的 CLAUDE.md，依 Karpathy 的 LLM Wiki 模式運作：**人負責找來源、
提問與判斷，LLM 負責所有編纂與整理**。agent 每次動筆前先呼叫 \`get_instructions\`。

## 三層架構

1. **raw/** 是原始來源，唯讀不可變。每則來源的 front-matter 要有 \`source_type\` 與 \`fetched_at\`。
2. **wiki/** 是編纂後的知識頁，由你維護。一事一頁；頁名用能獨立理解的名詞，不用日期或流水號。
3. **schema/** 是規則層，人類說了算。你可以建議修改規則，但不要自行改寫這一層。

## 三個操作

**Ingest（來源進來時，一定要做完整套）**
1. \`read_note\` 讀完整個來源。
2. \`search_notes\` 找出 wiki 裡已有的相關頁，避免重複建頁。
3. 發現與既有頁矛盾時，在兩頁都標記「⚠ 矛盾」並列出雙方來源，不要默默選邊。

（其餘步驟與 Query、Lint 略）
```

*出處： [`templates/general/zh-TW/schema/instructions.md`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/templates/general/zh-TW/schema/instructions.md)*

分界線其實很好記： **會弄壞資料的寫成程式碼，影響品質的寫成規則。** 使用者把規則改壞了，最糟是頁面變難看；但如果 `raw/` 的唯讀也放在規則裡，模型哪天沒讀就把憑據改掉了，那是救不回來的。

依領域還有另外幾份規則頁，疊在基礎規則上面。研究者那一份要求 `wiki/concepts/` 一個術語一頁、 `wiki/arguments/` 一個可被反駁的主張一頁，兩篇論文結論相反時不要合併成一句話，而是開一個論點頁把兩邊放進去。

*出處： [`templates/researcher/zh-TW/schema/researcher.md`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/templates/researcher/zh-TW/schema/researcher.md)*

重點是這一頁 **沒有任何程式碼會動到它** 。 `wiki/arguments/` 不存在於任何定義裡，路徑檢查只認 `raw/` 、 `wiki/` 、 `schema/` 三個開頭，底下要怎麼分完全是規則叫 agent 做的。所以多一種知識庫類型不需要改程式，只要多寫一份 Markdown——這正是把規則放進知識庫而不是提示詞的回報。

## Ingest

編纂是三個操作裡最重的一個，但程式碼出乎意料地少。真正決定 agent 怎麼做事的只有兩段字串。

系統提示詞就只要這麼短：

```ts
export const ingestSystem = (lang: Lang) =>
  \`你是 WikiBrain 知識庫的編纂 agent，依 Karpathy LLM Wiki 模式工作。工具與規則如下；\` +
  \`先呼叫 get_instructions，之後嚴格照規則的 Ingest 步驟做。${langLine(lang)}做完最後用一段文字回報動到哪些頁。\`;
```

*出處： [`src/ingest.ts:132-134`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/ingest.ts#L132-L134)*

它只交代兩件事：先讀規則，然後照規則做。 **具體怎麼編纂，一個字都沒寫在程式裡** ，因為那些在 `schema/` 。

（後來這段提示詞多了一句，講的是信任邊界—— `raw/` 的內容是資料不是指令，而且自動執行不能寫 `schema/` 。）

六個步驟是在每次工作的指令裡才攤開：

```ts
export function ingestPrompt(paths: string[], lang: Lang = 'zh-TW'): string {
  const list = paths.map(p => \`- ${p}\`).join('\n');
  return \`先呼叫 get_instructions 讀編纂規則。然後依規則的 Ingest 六步處理下列來源：\n${list}\n\` +
    \`每個來源都要：讀完整篇、search_notes 找相關頁、建 wiki/sources/ 摘要頁並連回來源、\` +
    \`更新相關實體與概念頁、更新 wiki/index.md、在 wiki/log.md 追加 ingest 條目。做完回報動到哪些頁。\`;
}
```

*出處： [`src/notes.ts:435-439`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L435-L439)*

跟原文對一下就知道這幾乎是逐項翻譯：

> the LLM reads the source, discusses key takeaways with you, writes a summary page in the wiki, updates the index, updates relevant entity and concept pages across the wiki, and appends an entry to the log.

唯一沒照做的是 `discusses key takeaways with you` 。網頁上按下去是一路跑到底，討論被我拆成另一個操作——也就是下一節的 Query。

還有一個地方在推著 agent 走： `get_instructions` 回傳的不只是規則，它會把待編纂清單掛在最前面。

```ts
const pending = await listPendingSources(ws);
const head = !pending.length ? '' :
  \`## 待編纂的來源（${pending.length}）\n\n以下 raw/ 來源還沒有任何 wiki/ 頁連回它，請依規則的 Ingest 步驟處理…\n${list}\n\n---\n\n\`;
return head + rows.map(r => \`<!-- ${r.path} -->\n${r.content_md.trim()}\`).join('\n\n---\n\n');
```

*出處： [`src/notes.ts:293-311`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L293-L311)*

而「待編纂」這個狀態不是欄位，是一句 `NOT EXISTS` ：

```sql
SELECT n.path, n.title FROM notes n
 WHERE n.workspace_id = $1 AND n.deleted_at IS NULL AND n.path LIKE 'raw/%'
   AND NOT EXISTS (
     SELECT 1 FROM links l JOIN notes f ON f.id = l.from_note_id
      WHERE l.to_note_id = n.id AND f.deleted_at IS NULL AND f.path LIKE 'wiki/%')
```

*出處： [`src/notes.ts:420-431`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/notes.ts#L420-L431)*

## Query

對話的系統提示詞比編纂長，因為它要管的事情比較雜：

```ts
\`你是 WikiBrain 知識庫的助理，依 Karpathy LLM Wiki 模式工作。
- 先呼叫 get_instructions 讀規則（含待編纂來源清單）。
- 回答問題（Query）：先 read_note wiki/index.md 找相關頁，再 search_notes、read_note 讀完內容後回答；
  回答要附引用，格式為頁面 path（例如「見 wiki/concepts/xxx.md」）。知識庫裡沒有的事要明說，不要編。
- 回答形式依問題選：比較用 Markdown 表格；流程、關係、時間軸用 mermaid 圖表；用戶要簡報時寫成 Marp 投影片頁。
- 先結論再理由、簡潔。\`
```

*出處： [`src/chat.ts:27-40`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/chat.ts#L27-L40)*

「先讀 index 再鑽進去」這一條直接來自原文，他認為在中等規模下這樣就夠用，不需要整套向量檢索。「知識庫裡沒有的事要明說，不要編」則是我自己加的——一個會附出處的助理如果開始編造出處，比沒有出處更糟。

Query 還有一件事是 Karpathy 特別強調的，之前我們已經引過：有價值的回答要能存回 wiki 變成新的一頁。這在程式裡是 `fileAnswer` ，把某一則回答連同問題與提問時間寫成 `wiki/queries/` 底下的一頁：

```ts
let path = \`wiki/queries/${base}.md\`;
for (let i = 2; taken.has(path); i++) path = \`wiki/queries/${base}-${i}.md\`;
const content = \`---\nsource_type: query\nasked_at: ${msg.at}\n---\n# ${title}\n\n> 問：${question}\n\n${msg.content}\n\`;
const r = await createNote(ws, path, content, actor);
```

*出處： [`src/chat.ts:126-129`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/chat.ts#L126-L129)*

存進去的東西帶著 `source_type: query` 與提問時間，而且問題本身留在頁面裡——沒有問題的答案，三個月後沒有人知道它在回答什麼。第二行那個迴圈是因為同一個問題可能問很多次，重名就接 `-2` 、 `-3` ，不覆蓋舊的那一頁。

而且它走的就是 agent 在用的那個 `createNote` ，沒有另開一條寫入路徑。所以樂觀鎖、版本快照、連結解析全部照跑——存回去的答案跟 agent 編纂出來的頁面，在資料庫裡沒有任何差別。

存進去的頁面會被連結解析、會進圖譜、會被下一次 Query 讀到。 **問答不再是聊天紀錄，它變成知識庫的一部分。**

## Lint

健檢是六個裡面最容易做錯的一個，因為很容易全部丟給模型。我的切法寫在 `lint.ts` 開頭的註解裡：

```ts
/* Deterministic checks are done by the system: orphan pages, broken links,
   pages missing from the index, log format, pending sources.
   Semantic checks (contradictions, stale claims, missing pages) are left to the agent. */
```

*出處： [`src/lint.ts:5-8`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/lint.ts#L5-L8)*

孤兒頁、斷連結、index 有沒有漏、log 格式對不對、還有哪些來源沒編纂——這些全部是 SQL 查得出來的事實，沒有模糊空間，也不該花一毛 token。矛盾、過時的主張、該有卻沒有的頁面，這些需要讀懂內容，才交給 agent。

順序也是刻意的：先跑確定性檢查，把結果塞進給 agent 的提示詞裡，它才知道該從哪裡看起。

```ts
report = await lintWorkspace(ws);
void runJob(job, ws, cfg, { system: lintSystem(lang), user: lintPrompt(report, lang), … });
```

*出處： [`src/ingest.ts:93-98`](https://github.com/wikibrain-app/wikibrain/blob/acf459899304d6155f0e45652646f610bba76322/src/ingest.ts#L93-L98)*

所以 agent 收到的不是「去檢查這座知識庫」，而是一份已經列好的清單，加上「這些是程式算出來的，請你看看有沒有語意層面的問題」。能用 SQL 回答的先回答掉，模型的注意力就留給只有它做得到的那一半。

## 小結

這篇文章介紹 Karpathy 的六條準則實作之後的樣子。簡而言之，準則裡怕 LLM 會弄壞的部分寫成程式碼加以規範，其他的部分就可以寫成 Markdown 的形式，交給使用者定義，再讓 LLM 來處理。

- [留言](#reply)
- [追蹤](https://ithelp.ithome.com.tw/users/login)
- [檢舉](https://ithelp.ithome.com.tw/users/login)[上一篇](https://ithelp.ithome.com.tw/articles/10409505)

[

Day 02 - 深入 Karpathy 的 LLM wiki 架構

](https://ithelp.ithome.com.tw/articles/10409505)[下一篇](https://ithelp.ithome.com.tw/articles/10409915)

[

Day 04 - 用 LLM 執行 Karpathy 的六條準則

](https://ithelp.ithome.com.tw/articles/10409915)

系列文

[為你自己蓋一座會複利的知識庫——WikiBrain](https://ithelp.ithome.com.tw/users/20184129/ironman/9590) 共 7 篇

目錄

1. 3
	[Day 03 - 把 Karpathy 的六條準則寫成程式](https://ithelp.ithome.com.tw/articles/10409772)
2. 4
	[Day 04 - 用 LLM 執行 Karpathy 的六條準則](https://ithelp.ithome.com.tw/articles/10409915)
3. 5
	[Day 05 - Karpathy 的準則，在 WikiBrain 實現會自我強化的知識庫](https://ithelp.ithome.com.tw/articles/10410555)
4. 6
	[Day 06 - 為什麼選 MCP，以及怎麼用無狀態的 Streamable HTTP 實作](https://ithelp.ithome.com.tw/articles/10411041)
5. 7
	[Day 07 - 把提示詞寫進工具描述與錯誤訊息](https://ithelp.ithome.com.tw/articles/10411905)
[完整目錄](https://ithelp.ithome.com.tw/users/20184129/ironman/9590)

[![.](https://itadstatic.ithome.com.tw/B9/1789119962_6aa3cdda3af6d.png)](https://itadapi.ithome.com.tw/media/click?q=B9%7Eithome_forum%7E2609B9001)

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/95181049-28f6-42d1-87ce-28749850b6dd_small.jpg)

[如何成為炙手可熱的資安人才？](https://itplus.ithome.com.tw/vod-page/170?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

21 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/15/6ea5ea50-cc57-4b24-90bb-24d5596c18c0_small.jpg)

[展望金融資安行動方案 2.0](https://itplus.ithome.com.tw/vod-page/317?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

25 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2023/8/14/7641bfd2-529a-4d9f-9076-ec6be5e67e9f_small.jpg)

[勒索病毒終極防線 自動偵測以鞏固企業內部資料安全](https://itplus.ithome.com.tw/vod-page/381?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

臺灣資安大會 |

30 分

![圖片](https://event.ithome.com.tw/itplus/upload-img/2025/6/26/2679a76d-61e0-4e9b-97bb-210315cfba43_small.png)

[開發者體驗 - 是組織還是技術問題？](https://itplus.ithome.com.tw/vod-page/1179?utm_source=iThelp&utm_medium=articlebottom&utm_campaign=iThelparticlebottom)

DevOpsDays |

39 分