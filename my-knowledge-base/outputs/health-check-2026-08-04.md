---
title: Wiki 健康檢查報告
date: 2026-08-04
tags: [pkm/workflow]
type: reference
status: processed
---

# Wiki 健康檢查報告 — 2026-08-04

## 整體評分

- wiki 條目數：350（+ synthesis 12 + concepts 5）
- research/：43 篇　ideas/：83 篇
- raw 素材數：.md 700 篇（0 未處理）／非 .md 24 篇（0 未處理，抽樣 15 篇反查無幽靈標記）
- 問題總數：8（嚴重 0 / 警告 2 / 建議 6）
- 健康指數：**良好**

---

## 🔴 嚴重問題（需立即處理）

無。矛盾偵測、缺乏來源、懸空連結、孤立筆記四項均未發現真正的嚴重問題（詳見下方各節）。

---

## 🟡 警告（建議本週處理）

### W1. raw-pipeline / wiki-health-check 的「非 .md 未處理」掃描指令本身有 bug
skill 文件與 raw-pipeline 內建的掃描指令：
```
find raw/ -type f ! -name "*.md" ! -name "*.processed"
```
這條指令只排除**檔名本身是 `.processed`** 的檔案，**沒有排除「已有對應 `.processed` 旁車標記」的檔案**——導致每次執行都會把全部 24 個非 .md 素材（含已處理的 `agent_handoff_memory_*.jpg`、`gstack_*` 系列等）誤報為「未處理」。正確判斷需改成逐檔檢查配對：
```bash
find raw/ -type f ! -name "*.md" ! -name "*.processed" | while read f; do
  [ -f "${f}.processed" ] || echo "$f"
done
```
本次已用正確邏輯重新核實：**24 個非 .md 素材全部已有 `.processed` 標記，實際 0 篇未處理**。建議修正 `raw-pipeline` SKILL.md 與 `wiki-health-check` SKILL.md 第 6 項的掃描指令範例。

### W2. 三篇精粹條目快照偏舊，建議確認是否需重跑
| 精粹 | 快照日期 | 距今 | 備註 |
|---|---|---|---|
| [[synthesis/synthesis-practitioner-skills-design]] | 2026-05-22 | 74 天 | 期間新增多篇 Skill 設計類條目（如 emil-design-engineer-skills、grill-me-agent-deep-questioning-skill） |
| [[synthesis/synthesis-spec-driven-workflow]] | 2026-06-05 | 60 天 | 期間新增 ai-sdd-framework-comparison、spec-first-ai-coding-workflow 等 |
| [[synthesis/synthesis-prompt-engineering-evolution]] | 2026-06-17 | 48 天 | 期間新增 claude-opus-5-prompting-guide、gpt-5-6-prompting-4-elements-framework、codex-prompt-4-elements-agents-md 等提示詞框架條目 |

⚠️ 主題比對用 glob/關鍵字掃描噪訊很高（例如以「skill」「prompt」關鍵字掃出的候選中真正貼題的可能只有 3-5 篇），**寧可過報，需人工確認題目相關性後再決定是否重跑** `knowledge-synthesizer`，不要直接依此清單全數覆蓋。

其餘 9 篇精粹（`agent-engineering-architecture`／`agent-framework-philosophy`／`agentic-engineering`／`ai-dev-workflow-frameworks`／`android-flutter-ai-toolchain`／`claude-md`／`karpathy-llm-wiki-method`／`llm-cost-routing`／`multi-agent`）快照日期集中在 2026-07-24～07-27，**距今 8-11 天，優先度低**，且每篇「來源條目」段落列出的 `[[...]]` 數量與 frontmatter `source_count` 逐一核對後**完全一致**（無內部矛盾）。

---

## 🟢 建議（下次處理）

### G1. 高集中度節點（非缺陷，僅供參考）
以下條目入連結數偏高，屬於合理的核心樞紐（多篇條目共同引用的基礎概念），不建議拆分，僅記錄供未來擴充時參考：

| 條目 | 入連結數 |
|---|---|
| [[claude-code-context-management]] | 48 |
| [[harness-engineering]] | 40 |
| [[loop-engineering-6-components]] | 34 |
| [[karpathy-knowledge-base-method]] | 33 |
| [[claude-md-optimization-techniques]] | 30 |

### G2. 內容過時檢查目前沒有樣本可判斷
knowledge base 最早的 wiki 條目建立於 2026-05-08，距今僅 88 天，尚未有條目跨過「6 個月無更新」門檻，此項檢查**本次無結果並非掃描失效，而是知識庫年輕、尚不適用此判準**。建議 2026-11 月起才會開始出現真正候選。

### G3. 矛盾偵測抽樣結果：目前抽查的主題群組皆已自我修正
抽查 CLAUDE.md 行數規範（200 行 vs 舊有「14 條」說法）、「IMPORTANT/YOU MUST 前綴」建議（Claude 5 世代已反轉但舊條目已用刪除線標註修正）、「限制 session 長度」建議（已透過雙向 wikilink 交叉提醒）三組最可能有矛盾的主題，**均已在文中自我標註修正或互相連結說明差異**，沒有發現未處理的矛盾。建議下次抽查換其他高密度主題（如 Skills vs MCP 選擇判斷、多 Agent 編排框架選型）。

### G4. 缺乏來源檢查：wiki/ 完全通過，synthesis/concepts 依設計無需 source
`wiki/*.md`（350 篇）全數有非空 `source:` 欄位。`wiki/synthesis/`（4 篇）與 `wiki/concepts/`（5 篇）缺 `source:` 欄位是**設計如此**——這兩類條目改用「來源條目」段落的 `[[wikilink]]` 引用既有 wiki 條目，非外部來源，不需 `source:` 欄位。

### G5. 連結品質：wiki/synthesis/concepts 三目錄 0 孤立筆記
完整比對 350 篇 wiki + 12 篇 synthesis + 5 篇 concepts 的入連結，**沒有任何一篇是孤立筆記**（入連結數 = 0）。懸空連結掃描找到的 18 個候選經逐一核實後，全部是命名空間子資料夾連結（`sanlong/dev-workflow`、`b2b-manager/dev-workflow`、`archive/figma-to-requirements-ui-dev-workflow` 等，掃描腳本未涵蓋這些子目錄所致）或程式碼範例中的示意文字（`Previous Analysis`、`Redis Paper`、`wikilink` 等教學用假連結），**無真正懸空連結**。

### G6. 幽靈標記抽樣：2 個模稜兩可項目，非真正異常
抽樣 15 個 `.processed` 標記反查對應條目，13 篇有明確對應 wiki 條目。2 篇（Mac 快捷鍵大全、音樂格式不要存 MP3）找不到專屬條目，但主題本身偏離知識庫關注範圍（通用 macOS 操作技巧／音訊格式知識），**較可能是當初判定「排除、非知識庫關注領域」後僅標記未建條目**（與本次 Batch 39 排除 itch.io/Tad AI/PublicAPI.dev 的處理方式一致），不建議視為異常，但建議未來排除項目養成在 INDEX.md 滾動日誌中列出的習慣，方便日後稽核區分「真遺漏」與「刻意排除」。

---

## 優先處理清單

1. **修正 raw-pipeline 與 wiki-health-check 兩份 SKILL.md 的非 .md 掃描指令**（W1）——目前的指令邏輯性錯誤，每次執行都會誤報全部非 .md 素材為積壓，過去幾次報告中的「未處理素材」數字可能都被污染
2. **人工確認 3 篇偏舊精粹是否需重跑**（W2）：`synthesis-practitioner-skills-design` / `synthesis-spec-driven-workflow` / `synthesis-prompt-engineering-evolution`，尤其是 skill 設計與提示詞框架這兩個主題近期新增條目較多
3. 建立「排除項目」的滾動日誌習慣（G6），未來 raw-pipeline 批次處理時明確記錄哪些素材是刻意排除而非遺漏
4. 其餘為長期觀察項目（G1-G5），無需立即行動

---

## 🔄 5A+ 階段評估

**當前階段：ADJUST（同時具備 APPLY 的健康指標）**

理由：知識庫規模（350 篇 wiki + 12 篇精粹）已進入「同主題多篇但精粹未必跟上」的階段——3 篇精粹快照偏舊、對應主題持續有新條目累積，是典型 ADJUST 訊號；但同時矛盾偵測、連結品質、來源完整度全部健康，代表知識庫本身運作順暢，也具備 APPLY 階段的特徵。

下一步：先處理 W2（人工確認 3 篇精粹是否重跑），不需要暫停素材收集或做大規模品質修正——目前的積壓消化速度（raw/ 已完全清空）與知識庫品質都在健康水準，可以維持現有 raw-pipeline 節奏，同時把 knowledge-synthesizer 排進下次任務。
