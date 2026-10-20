---
title: raw 待處理清單
date: 2026-08-13
tags: [pkm/workflow]
source: 內部工具頁
type: reference
status: processed
---

# raw/ 待處理清單

`raw/` 中尚未整理的素材。判斷規則見 [[inbox-detection-protocol]]。

> [!info] 這頁怎麼用
> 下方清單即時反映 `raw/` 的 frontmatter `status`。處理完一篇後把該檔的 `status` 改為 `processed`，它就會從清單消失。
> 1–9 篇用 `knowledge-organizer` 逐篇處理，積壓 ≥ 10 篇改用 `raw-pipeline` 批次處理。

> [!warning] 需要啟用 Dataview 外掛
> 若下方看到的是程式碼原文而非表格，表示 Dataview 尚未啟用：
> `設定 → 第三方外掛 → 已安裝外掛` 找到 **Dataview** 打開開關即可（外掛已安裝在 vault 內，不需重新下載）。

## 待處理總數

```dataview
TABLE WITHOUT ID length(rows) AS "待處理篇數"
FROM "my-knowledge-base/raw"
WHERE lower(string(default(status, "inbox"))) != "processed"
GROUP BY true
```

> 待處理為 0 時，上方表格會顯示「無結果」，這是正常的。

## 待處理清單

```dataview
TABLE WITHOUT ID
  file.link AS "素材",
  dateformat(default(created, file.ctime), "yyyy-MM-dd") AS "剪存日",
  default(join(tags, ", "), "—") AS "標籤"
FROM "my-knowledge-base/raw"
WHERE lower(string(default(status, "inbox"))) != "processed"
SORT default(created, file.ctime) ASC
```

## 最近處理完的 20 篇

```dataview
TABLE WITHOUT ID
  file.link AS "素材",
  dateformat(file.mtime, "yyyy-MM-dd") AS "處理日"
FROM "my-knowledge-base/raw"
WHERE lower(string(default(status, "inbox"))) = "processed"
SORT file.mtime DESC
LIMIT 20
```

## 非 md 素材（Dataview 看不到）

PDF、圖片、字幕等非 `.md` 素材無法承載 frontmatter，改用 `.processed` 旁車標記，Dataview 無法查詢。要盤點請在終端機執行：

```bash
cd /Users/alexander/notes/my-knowledge-base
find raw/ -type f ! -name "*.md" ! -name "*.processed" \
  | while read f; do [ ! -f "${f}.processed" ] && echo "$f"; done
```

截至 2026-08-13，24 個非 md 素材（12 jpg、5 png、4 webp、2 pdf、1 jpeg）全部已處理。

## 相關

- [[inbox-detection-protocol]] — Inbox 判斷協議（權威來源）
- [[decision-framework]] — 知識庫決策框架
