---
title: GBrain 語意搜尋層整合計畫
date: 2026-11-03
tags: [ai/mcp, pkm/obsidian, pkm/automation]
source: https://github.com/garrytan/gbrain
type: project
status: processed
---

# GBrain 語意搜尋層整合計畫

分支：`feature/gbrain-semantic-search`

## 目標與邊界

**目標**：在不改變 LLM Wiki 整理流程的前提下，替知識庫加一層**本機中文語意搜尋**，讓 Claude Code 能用一句模糊的話撈到相關條目與素材。

**原則**

1. **整理仍由 LLM Wiki 負責**：GBrain 只讀不寫，不參與 wiki 條目的產生與維護
2. **全程本機**：PGLite＋Ollama 跑 embedding，筆記不上雲、不花 API 費
3. **可完整撤除**：所有安裝都有對應的移除步驟
4. **先試再決定**：Phase 2 效果不過門檻就停，不進入整合

**明確不做**：dream cycle／autopilot、`put_page`／`capture` 寫入、typed edges 圖譜、gbrain 的 brain-first hooks。

相關：[[llm-wiki-over-rag-claude-code]]、[[gstack-framework]]、[[tpki-tri-pattern-knowledge-index]]

---

## 現況盤點（2026-11-03 實測）

| 項目 | 現況 | 要求 | 處理 |
|---|---|---|---|
| Bun | 1.3.11 | ≥ 1.4.4 | ⚠️ 需升級（`bun upgrade`）|
| Ollama | 已安裝 | — | ✅ |
| gbrain | 未安裝 | — | Phase 1 安裝 |
| 硬體 | M2 Pro／32 GB RAM | bge-m3 約 1.2 GB | ✅ |
| 磁碟 | 剩 20 GB（96% 已用）| 模型＋索引約 2–3 GB | ⚠️ 夠用但吃緊 |
| 素材量 | wiki 619 篇、raw `.md` 1,478 篇 | PGLite 約 5 萬頁以內 | ✅ |

## 文件查證後的關鍵事實（影響設計）

1. **bge-m3 不在 gbrain 的 Ollama 官方清單**（官方列 nomic-embed-text／mxbai-embed-large／snowflake-arctic-embed2 等），但本機 recipe 會「信任你宣告的維度」，可用 `--embedding-model ollama:bge-m3 --embedding-dimensions 1024`。**要在 Phase 1 實測確認。**
   - 備案：`snowflake-arctic-embed2`（官方清單內、多語、1024 維）
   - ❌ 不用預設的 `nomic-embed-text`：以英文為主，中文效果差
2. **embedding 模型 init 後不能直接改**，換模型要跑 `gbrain migrate embeddings`。所以模型選擇要在 Phase 2 先比完再定。
3. **沒有原生的唯讀模式**。starter surface 含 `put_page`、`capture`。→ 用 Claude Code 的 `permissions.deny` 擋掉寫入類工具。
4. **`gbrain setup claude-code` 會寫入全域設定**（`~/.claude.json`、`~/.claude/settings.json` 的 hooks、`~/.claude/CLAUDE.md` 區塊）。→ 不用它，改成手動以 project scope 註冊 MCP。
5. **import 是否寫入來源資料夾，文件沒寫**（官方提到新 brain 會在 content root 放 skill 檔）。→ Phase 1 一律從**副本**匯入，確認行為後才碰正式路徑。

---

## Phase 0：前置準備（約 10 分鐘）

```bash
bun upgrade                     # 1.3.11 → ≥ 1.4.4
ollama pull bge-m3              # 約 1.2 GB
ollama pull snowflake-arctic-embed2   # 備案，Phase 2 比較用
```

✅ 完成條件：`bun --version` ≥ 1.4.4；`ollama list` 看得到兩個模型

## Phase 1：隔離安裝與匯入（約 30 分鐘）

1. 安裝（只能從 GitHub 裝，**不要** `npm i gbrain`，那是無關套件）：
   ```bash
   bun install -g github:garrytan/gbrain#latest-stable
   gbrain --help && gbrain import --help && gbrain sync --help   # 補齊文件沒寫的旗標
   ```
2. 建立 wiki 副本（pilot 期間不碰正式資料夾）：
   ```bash
   rsync -a --delete ~/notes/my-knowledge-base/wiki/ ~/gbrain-pilot/wiki/
   ```
3. 初始化與匯入：
   ```bash
   gbrain init --pglite --embedding-model ollama:bge-m3 --embedding-dimensions 1024
   gbrain providers test --model ollama:bge-m3
   gbrain import ~/gbrain-pilot/wiki/
   gbrain doctor
   ```
4. **行為檢查**（決定之後能不能直接指向正式資料夾）：
   - `diff -rq ~/notes/my-knowledge-base/wiki ~/gbrain-pilot/wiki`：副本是否被寫入新檔或被改了 frontmatter
   - `ls ~/.gbrain/`：資料實際存放位置
   - 記錄匯入時間與索引大小

✅ 完成條件：`doctor` 通過、`gbrain search "測試"` 有結果、知道 import 會不會寫入來源

## Phase 2：效果測試（Go／No-Go 關卡）

### 測試題組（15 題，使用者可替換成自己真的會問的）

| 類型 | 題數 | 例 |
|---|---|---|
| 模糊／口語 | 5 | 「之前看過一個說 skill 越寫越長反而變差的」 |
| 換句話說（用詞跟條目不同）| 5 | 「怎麼讓 AI 記得我上次做到哪」（條目用的是 handoff／跨 session 記憶）|
| 精確關鍵字 | 3 | 「OKF」、「TBRC」 |
| 中英混合 | 2 | 「Claude Code 的 context 管理技巧」 |

每題先由使用者（或我依 INDEX.md）標出**正解條目**，再比較：

| 方法 | 做法 |
|---|---|
| **Baseline** | 現行做法：Claude Code 用 grep＋讀 INDEX.md |
| **GBrain＋bge-m3** | `gbrain search "<題目>"` 取前 5 |
| **GBrain＋snowflake-arctic-embed2** | 另建一個 brain 對照（或確認 bge-m3 不行時才跑）|

### 通過門檻

- GBrain 前 5 命中率 **≥ 70%**（15 題至少 11 題）
- 在「模糊」與「換句話說」兩類中，**至少 3 題是 baseline 找不到、GBrain 找到的**
- 精確關鍵字類**不比 baseline 差**
- 單次搜尋 < 2 秒

❌ **任一不過 → 停在這裡，執行 Phase 5 撤除**，在本文件記下結果。

## Phase 3：正式整合（Go 才做）

1. **匯入範圍**：先 `wiki/`，穩定後再評估加入 `raw/` 的 `.md`（1,478 篇）。PDF／圖片不在範圍內。
   - 若 Phase 1 證實 import 會寫入來源：改為同步副本（rsync），gbrain 永遠讀副本
2. **Claude Code 接入（只限本專案）**：
   ```bash
   claude mcp add gbrain --scope project -- "$(command -v gbrain)" serve --surface starter
   ```
   在 `.claude/settings.json` 加上禁止寫入：
   ```json
   { "permissions": { "deny": ["mcp__gbrain__put_page", "mcp__gbrain__capture"] } }
   ```
   （實際工具名稱以 `/mcp` 列出的為準，寫入類全部擋掉）
3. **CLAUDE.md 加一條路由規則**：「找相關既有條目、或使用者描述模糊時，先用 gbrain search，再用 grep 補精確關鍵字」
4. **每晚同步**：用 launchd 在 04:00 跑 `gbrain sync`（確切指令依 Phase 1 的 `--help` 結果）；**不開 autopilot**
5. **Skill 接入（選做）**：knowledge-organizer 的「找相關既有條目」步驟加入 gbrain search。⚠️ 動到 SKILL.md 時，依 CLAUDE.md 要同步更新三處外部追蹤點

## Phase 4：觀察期（2 週）

- 記錄每次 gbrain 搜尋有沒有幫上忙（knowledge-organizer 執行時順手記）
- 兩週後檢討：沒幫上忙 → Phase 5 撤除

## Phase 5：撤除步驟

```bash
claude mcp remove gbrain --scope project
launchctl unload ~/Library/LaunchAgents/<gbrain-sync>.plist && rm 該 plist
rm -rf ~/.gbrain ~/gbrain-pilot
bun remove -g gbrain
ollama rm bge-m3 snowflake-arctic-embed2
```

並還原 `.claude/settings.json`、CLAUDE.md 的路由規則。

---

## 風險與未知

| 項目 | 影響 | 對策 |
|---|---|---|
| bge-m3 不被接受或維度出錯 | Phase 1 卡住 | 改用 snowflake-arctic-embed2 |
| import 寫入來源資料夾 | 污染 wiki、可能碰到 raw/ 唯讀規則 | 一律讀副本 |
| gbrain 改版快 | 指令或行為變動 | 用 `#latest-stable`；升級前先看 CHANGELOG |
| 模型 log／查詢資料留在 `~/.gbrain` | 隱私 | 全本機，無外送；撤除時一併刪除 |
| 磁碟剩 20 GB | 空間不足 | 只拉兩個模型；Phase 2 後刪掉落選的那個 |

## 進度紀錄

- 2026-11-03：開分支、完成計畫
- 2026-11-03：完成 Phase 0–2，**結果 No-Go**（詳見下節）
- 2026-11-03：**已撤除**。刪除 `~/gbrain-pilot`（兩個測試 brain、wiki 副本、測試紀錄），以 `bun remove -g gbrain` 移除 gbrain，以 `ollama rm` 刪除 bge-m3 與 snowflake-arctic-embed2，並關閉測試用的 Ollama server。`~/.gbrain` 從未建立；`~/.claude.json`、`~/.claude/CLAUDE.md`、`~/.claude/settings.json` 從未被寫入。Bun 1.4.4 保留

## Phase 2 測試結果（2026-11-03）

**環境**：gbrain 0.60.557.0、PGLite、Ollama bge-m3（1024 維）；wiki 副本 619 篇切成 4,729 段，匯入 11 秒，embedding 約 6 分鐘

**Phase 1 行為確認**
- `gbrain import` **不會寫入來源資料夾**：匯入前後比對副本的 hash，完全相同
- 資料都放在 `GBRAIN_HOME/.gbrain/`。init 只「詢問」要不要寫 `~/.claude.json`，不會自己寫
- `Links: 0`：裸 `[[slug]]` 連結**沒有被解析成圖譜邊**，證實先前的疑慮
- `gbrain init --embedding-model ollama:bge-m3 --embedding-dimensions 1024` 可用。但 `providers test --model ollama:bge-m3` 會誤報維度不符，不帶 `--model` 則正常

**命中率（前 5 筆含正解）**

| 題型 | Baseline（grep＋INDEX.md）| GBrain＋bge-m3 |
|---|---|---|
| 模糊口語（5）| 5/5 | 3/5 |
| 換句話說（5）| 5/5 | 1/5 |
| 精確關鍵字（3）| 3/3 | 3/3 |
| 中英混合（2）| 2/2 | 2/2 |
| **合計** | **15/15（每題都排第 1）** | **9/15（60%）** |

- 排除 `index`／`index-changelog` 頁後再跑一次，GBrain 仍是 9/15
- GBrain 單次搜尋 0.6–1.3 秒；baseline 由 agent 跑，15 題共 4 次 grep
- **門檻判定**：命中率 60% < 70% ❌；「baseline 找不到、GBrain 找到」0 題 < 3 ❌

**判讀**
- 這個 wiki 的 INDEX.md 每列都有高密度摘要，grep 加摘要已經等於一個人工編纂的語意索引。向量搜尋多不了東西，反而在「換句話說」類輸給它
- ⚠️ **偏差**：題目是看過 INDEX 摘要後才寫的，用詞可能偏向 baseline。換成使用者自己的真實問題，結果可能不同
- **snowflake-arctic-embed2 對照組：12/15（80%）**，明顯比 bge-m3 好。它在模糊口語類 5/5，換句話說類 3/5（Q3、Q8、Q10 落空），精確與混合類全中。命中率過了 70% 門檻，但「baseline 找不到、GBrain 找到」仍是 0 題，判定不變
- 若日後重測，embedding 模型首選 snowflake-arctic-embed2，不是 bge-m3

**結論**：依「效果不好就不加」原則，不進入 Phase 3。
