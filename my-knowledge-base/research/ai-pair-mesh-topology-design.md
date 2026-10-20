---
title: AI-Pair v4.5 Mesh 網狀共識辯論模式設計研究
date: 2026-06-05
tags: [ai/agent, ai/claude]
source: 重構建議-v4.5-mesh互動.md + Claude Code 分析
type: research
status: processed
asset_value: 72
---

## 研究目標

> ⏱️ **基準版本註記**：本文是基於 v4.2.5 的 **v4.5 前瞻設計研究**，文中 `gemini-reviewer (gemini-2.5-flash)` 等為當時模型名。實際已於 **v4.3 將底層 CLI 由 `gemini` 遷移至 `agy`（Antigravity）**，模型對應為 `Gemini 3.5 Flash (Low/Medium)`／`Gemini 3.1 Pro (High)`，角色名 `gemini-reviewer` 不變。Mesh 拓撲設計本身不受 CLI 遷移影響。詳見 [[ai-pair-skill]]。

將 ai-pair Skill 的 Review 拓撲從 Star/Fan-out（Reviewer 互不可見）升級為 Mesh（Reviewer 可互相辯論），提升 Code Review 精準度。本文整合原始重構提案（v4.5-mesh互動.md）與補充分析，產出完整設計規格。

## 一、現狀 vs 目標

| | Star（v4.2.5 現行） | Mesh（v4.5 目標） |
|---|---|---|
| 資訊流向 | Team Lead → 3 Reviewers（單向、平行、互不可見） | Round 1 獨立審 → Round 2 看到彼此意見後辯論 |
| 回合數 | 1 回合 | 1-2 回合（共識一致時跳過 Round 2） |
| VERDICT | 各自獨立 | 辯論後可修改 |
| Team Lead 角色 | 收集＋翻譯 | 共識檢查＋辯論主持＋收集＋翻譯 |
| 誤判率 | 較高（單模型盲點） | 極低（交叉辯論過濾） |

### 理論依據

根據 [[context-engineering-fundamentals]] 中 AI Agent (2-3) 課程的多 Agent 拓撲研究，Mesh 和 Random 拓撲效果優於 Chain/Tree。關鍵在於 Agent 間的資訊交換能觸發「同儕啟發」——一個 Reviewer 的發現可以激發另一個 Reviewer 注意到自己原本忽略的問題。

## 二、核心機制：兩輪共識協商（2-Round Consensus Debate）

```
[Round 1: 獨立初審]
Team Lead → (平行) → copilot-reviewer / claude-reviewer / gemini-reviewer
        │
        ▼
[共識檢查 Consensus Check]
├─ 意見一致 → 直接輸出（省 token）
└─ 意見分歧（PASS vs BLOCK / WARN）
        │
        ▼
[Round 2: Mesh 辯論]
Team Lead 彙整初審 → DEBATE_FILE → (平行) → 各 Reviewer 重審
        │
        ▼
[最終判定]
Team Lead 彙整 Round 2 → 標注辯論過程 → 呈現 User
```

### 共識檢查規則（原提案已有，補強邊界條件）

| Round 1 結果 | 動作 |
|---|---|
| 全部 PASS | 直接 auto PASS，跳過 Round 2 |
| 全部 BLOCK | 直接 BLOCK，跳過 Round 2（共識已達成） |
| 全部 WARN | 直接 WARN，跳過 Round 2 |
| 混合（如 PASS + BLOCK） | 進入 Round 2 辯論 |
| 混合（如 WARN + BLOCK） | 進入 Round 2 辯論 |

### 辯論輪數限制

固定 1 輪辯論（初審 + 1 輪 = 2 輪完成），防止模型間無限跳針。理由：
- 實測大多數分歧在 1 輪辯論後即收斂
- 多輪辯論的 token 成本呈線性增長，邊際收益遞減
- 若 Round 2 仍分歧，交由 User 裁決比再跑 Round 3 更有效

## 三、Inter-Agent Protocol 擴充

### 3.1 DEBATE_FILE 格式（Team Lead → Reviewers, Round 2）

```markdown
# Peer Review Debate Info

We have conflicting reviews for this change. Please review the peer opinions and the code below.

Instructions:
1. Evaluate each peer's findings against the actual code diff
2. AGREE or DISAGREE with specific findings, with reasoning
3. Add any NEW issues you missed in Round 1
4. Update your VERDICT if peer evidence changed your assessment

## Round 1 Peer Opinions:

### copilot-reviewer (gpt-5.4-mini) — VERDICT: BLOCK
C|car_edit_page.dart:142|vehicleData null→crash|add null check before access
W|car_response.dart:58|fromJson empty array unhandled|add isEmpty guard

### claude-reviewer — VERDICT: PASS
S|NA|extract validation to mixin

### gemini-reviewer (gemini-2.5-flash) — VERDICT: WARN
W|NA|missing edge case: batch size = 0 unhandled

---
[原始 diff 內容接在下方]
```

### 3.2 DEBATE_RESPONSE 格式（Reviewer → Team Lead, Round 2）

原提案沿用現有壓縮格式，但缺乏結構化辯論欄位。補充定義：

```
SRC:{model_id}
ROUND:2
AGREE:{peer_model}|{finding_summary}|{optional_comment}
DISAGREE:{peer_model}|{finding_summary}|{reason}
NEW|{severity}|{file:line_or_NA}|{issue}|{fix}
{severity}|{file:line_or_NA}|{issue}|{fix}
VERDICT:{PASS|WARN|BLOCK}
VERDICT_CHANGED:{yes|no}
```

範例：
```
SRC:claude
ROUND:2
AGREE:gpt-5.4-mini|null crash at :142|confirmed, vehicleData can be null when offline
DISAGREE:gpt-5.4-mini|fromJson empty array|array case handled by CarResponse.empty() factory
NEW|W|car_edit_page.dart:89|race condition on concurrent saves|add mutex or debounce
VERDICT:BLOCK
VERDICT_CHANGED:yes
```

**設計決策**：
- `AGREE/DISAGREE` 用 peer model id 而非 finding index，因為 index 跨不同 Reviewer 報告容易混淆
- `VERDICT_CHANGED` 讓 Team Lead 明確知道哪些 Reviewer 改了主意，方便翻譯層標注
- 保持壓縮格式一致性，Round 1 報告不需改動

### 3.3 Team Lead 指令擴充

```
REVIEW:{path}    # Round 1（不變）
DEBATE:{path}    # Round 2（新增）
```

## 四、Round 2 後最終判定邏輯

原提案未定義 Round 2 後仍分歧的處理。補充完整決策表：

| Round 2 結果 | 判定 | 動作 |
|---|---|---|
| 全部 PASS | auto PASS | 通知 User |
| 全部 BLOCK | BLOCK | User 決定修復 |
| 全部 WARN | WARN | 列出所有 warning |
| 多數 BLOCK（≥2/3）| BLOCK（共識） | User 決定修復 |
| 1 BLOCK + 2 DISAGREE | **CONTESTED** | 呈現雙方論點，User 仲裁 |
| WARN + PASS 混合 | WARN | 列出 warning，User 決定 |

### CONTESTED 狀態（新增）

當 Round 2 後 Reviewer 間仍存在根本分歧（1 BLOCK vs 2 PASS/WARN），標記為 CONTESTED：
- Team Lead 不做自動判定
- 呈現正反論點摘要，由 User 最終裁決
- 這避免了少數 Reviewer 誤判導致錯誤 BLOCK

## 五、翻譯層呈現格式

### 無辯論（Round 1 共識一致）

沿用現行格式，不變。

### 有辯論（Round 2 觸發）

```markdown
## Review 結果（Mesh 辯論模式）

### Round 1 — 初審

#### GPT Review（bugs / security / performance）
❌ CRITICAL `car_edit_page.dart:142` — vehicleData null crash
⚠️ WARNING `car_response.dart:58` — fromJson empty array unhandled
**判定：BLOCK**

#### Claude Review（architecture / maintainability）
💡 SUGGESTION — extract validation to mixin
**判定：PASS**

#### Gemini Review（spec compliance）
⚠️ WARNING — batch size = 0 unhandled
**判定：WARN**

---

### Round 2 — 辯論

#### Claude（PASS → BLOCK 🔄）
- ✅ 同意 GPT：`car_edit_page.dart:142` null crash 確認存在（離線模式觸發）
- ❌ 反駁 GPT：`car_response.dart:58` empty array 已由 `CarResponse.empty()` 處理
- 🆕 新發現：`car_edit_page.dart:89` 並行儲存 race condition

#### GPT（維持 BLOCK）
- ✅ 同意 Gemini：batch=0 edge case
- 接受 Claude 對 :58 的反駁，降級為 SUGGESTION

#### Gemini（維持 WARN）
- ✅ 同意 GPT 的 CRITICAL
- 補充：batch=0 在 spec v2.3 §4.1 中有明確要求

---

**綜合判定：BLOCK（2/3 共識）**
- ❌ CRITICAL × 1：null crash（GPT 發現，Claude 確認）
- ⚠️ WARNING × 2：race condition（Claude 新發現）、batch=0（Gemini）
- 💡 SUGGESTION × 1：extract validation（Claude）
```

### CONTESTED 呈現

```markdown
**綜合判定：⚖️ CONTESTED — 需要您的裁決**

爭議焦點：GPT 認為 `car_response.dart:58` 是 CRITICAL（crash on empty array），
Claude 和 Gemini 認為已由 `CarResponse.empty()` factory 處理（非 bug）。

→ 請檢查 `CarResponse.empty()` 是否涵蓋所有 empty array 路徑後決定。
```

## 六、Review Cache 更新

現行 `CACHE_FILE` 格式需擴充以記錄辯論結果：

```bash
task_id: {task_id}
review_level: {--quick | default | --deep}
reviewer_set: {copilot,claude,gemini}
mesh_mode: true
changed_files: {file1,file2,...}
timestamp: {ISO8601}
round_1_findings:
{Round 1 SRC/findings lines}
round_2_debate:
{Round 2 DEBATE_RESPONSE lines, including AGREE/DISAGREE}
final_verdict: {PASS|WARN|BLOCK|CONTESTED}
```

下次 re-review 時，`Previous findings` section 應包含 Round 2 的最終共識結果（而非 Round 1 的原始分歧），讓 Reviewer 直接驗證修復而非重走辯論流程。

## 七、Review Level 與 --mesh 旗標

### 命令格式

```bash
/ai-pair dev-team [project] --mesh            # Level 2 + Mesh 辯論
/ai-pair dev-team [project] --mesh --deep     # Level 3 + Mesh 辯論（最強品質）
/ai-pair dev-team [project] --mesh --quick    # 無效組合（Level 1 只有 1 個 Reviewer，無法辯論）
```

### 時間與成本估算

| 模式 | Reviewers | 回合數 | 預估時間 | Token 倍率 |
|---|---|---|---|---|
| Level 2（Star） | 2 | 1 | ~2 min | 1× |
| Level 2 --mesh（共識） | 2 | 1 | ~2 min | 1× |
| Level 2 --mesh（辯論） | 2 | 2 | ~3-4 min | 1.5-1.8× |
| Level 3 --deep（Star） | 3 | 1 | ~3 min | 1× |
| Level 3 --deep --mesh（共識） | 3 | 1 | ~3 min | 1× |
| Level 3 --deep --mesh（辯論） | 3 | 2 | ~5-6 min | 1.5-2.2× |

## 八、Agent Prompt 修改規格

三個 Reviewer 的 prompt 需增加 Round 2 處理邏輯。以 claude-reviewer 為例：

```
You are claude-reviewer in {project}-dev team.
Provide architectural review. Read the file path sent by team-lead.

Protocol:
1. Wait for SendMessage from team-lead containing file path.
   - Format "REVIEW:{path}" → Round 1 (independent review)
   - Format "DEBATE:{path}" → Round 2 (peer debate)

2. For Round 1 (REVIEW):
   - Read the file, review for architecture/design/maintainability
   - Output compressed format: SRC / findings / VERDICT

3. For Round 2 (DEBATE):
   - Read the file, which contains a "Peer Review Debate Info" section
   - Evaluate each peer's findings against the actual code diff
   - For each peer finding: state AGREE or DISAGREE with reasoning
   - Add NEW issues you missed in Round 1 (peer opinions may trigger new insights)
   - Update your VERDICT if peer evidence warrants it
   - Output DEBATE_RESPONSE compressed format:
     SRC:{model_id}
     ROUND:2
     AGREE:{peer}|{finding}|{comment}
     DISAGREE:{peer}|{finding}|{reason}
     NEW|{severity}|{location}|{issue}|{fix}
     VERDICT:{PASS|WARN|BLOCK}
     VERDICT_CHANGED:{yes|no}

Do NOT Read/Glob/Grep source files directly — the provided file is your only input.
```

## 九、實作順序

```
前置：確認 v4.4 成本優化重構完成
Step 1：定義 DEBATE_FILE + DEBATE_RESPONSE protocol 格式 → 更新 SKILL.md
Step 2：修改 reference/agents-prompts.md，三位 Reviewer 增加 Round 2 prompt
Step 3：修改 SKILL.md Workflow 流程（加入共識檢查 + Round 2 循環 + CONTESTED 邏輯）
Step 4：修改翻譯層呈現格式（辯論過程 + CONTESTED 呈現）
Step 5：更新 Review Cache 格式（round_1 + round_2 + mesh_mode 欄位）
Step 6：修改 reference/cli-invocation-ref.md，更新底層 CLI prompt
Step 7：更新 README.md，增加 --mesh 參數說明
```

## 十、與原提案的差異對照

| 項目 | 原提案（v4.5-mesh互動.md） | 本研究補充 |
|---|---|---|
| 共識快捷路徑 | ✅ 有（很好的設計） | 擴充邊界條件（全 BLOCK / 全 WARN 也跳過） |
| DEBATE_RESPONSE 格式 | ❌ 沿用現有壓縮格式 | ✅ 新增結構化 AGREE/DISAGREE/VERDICT_CHANGED |
| CONTESTED 狀態 | ❌ 未定義 Round 2 後仍分歧的處理 | ✅ 新增 CONTESTED → User 仲裁 |
| 翻譯層 template | ❌ 只提到「呈現辯論脈絡」 | ✅ 完整 Markdown template（含 CONTESTED） |
| Review Cache | ❌ 未更新 | ✅ 擴充 mesh_mode / round_2_debate 欄位 |
| --mesh + --quick | ❌ 未處理 | ✅ 標記為無效組合（單人無法辯論） |

## 相關連結

- [[ai-pair-skill]] — AI-Pair Skill 核心 wiki 條目
- [[context-engineering-fundamentals]] — 多 Agent 拓撲理論（Mesh/Random 優於 Chain/Tree）
- [[synthesis-agent-framework-philosophy]] — 框架穩定性設計哲學
- [[claude-code-community-workflow-2026]] — 社群 Code Review 最佳實踐
