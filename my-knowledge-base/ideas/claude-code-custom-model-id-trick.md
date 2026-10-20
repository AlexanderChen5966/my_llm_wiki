---
title: Claude Code /model 選單找不到模型？直接輸入完整模型 ID 切換
date: 2026-07-27
tags: [ai/claude]
source: https://israynotarray.com/ai/20260727/3871780834/
type: idea
status: processed
asset_value: 20
---

`/model` 選單只列出官方推薦的模型，若想用其他版本（例如舊版 Opus 4.6~4.7，因為有人反應新版 Opus 4.8 不如舊版好用），可以直接在對話框輸入完整模型 ID 切換，例如：

```
/model claude-opus-4-8
```

正確的模型名稱要到官方文件的 Model Status 頁面查（也會列出停用時間）。若不想每次 Claude Code 更新就被偷偷改回預設模型，可在 `.claude/settings.json` 中固定：

```json
{
  "model": "claude-opus-4-8"
}
```

注意 settings.json 的權重：系統設定 > 使用者設定。

## 為何歸入 ideas 而非 wiki

分類階段 folder 欄位標記為 wiki，但 asset_value 僅 20，未達 wiki 條目門檻（50）。內容是單一操作技巧，篇幅與泛用性都偏向精簡筆記，故依規則改歸入 ideas/。

## 相關連結

- [[claude-code-model-boomerang]] — 追蹤 Claude Code 意外滯留 fallback 模型並提醒切回主模型的進階工具，若常手動 `/model` 切換可搭配使用
- [[claude-model-selection-guide]] — Fable/Opus/Sonnet/Haiku 選型完整指南
