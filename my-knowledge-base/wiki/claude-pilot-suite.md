---
title: Claude Pilot Suite：三模式執行手冊（成本優先 / 品質優先 / 天花板提升）
date: 2026-05-14
tags: [ai/claude, ai/agent]
source: https://github.com/zeuikli/claude-pilot-suite
type: article
status: processed
asset_value: 70
---

Claude Pilot Suite 是 Claude Code 的執行手冊 Skill 集合，提供三種模式：Haiku Pilot（成本優先，省 70-85%）、Sonnet Pilot（品質優先，省 55-65%）、Opus Pilot（天花板提升）。核心洞見：Prompt 結構 + Sub-agent 委派往往比換模型更有效。

## T（核心論點）
Haiku Pilot + 積極 Sub-agent 委派 ≈ 比單獨用 Opus 省 70-85% 成本但維持接近品質。研究依據：AgentOpt 論文顯示「Ministral 規劃者 + Opus 執行者 = 74.27%」遠優於「純 Opus = 31.71%」。

## B（適用場景）
**適用：** 重度 Claude Code 使用者優化成本；需要在 Haiku/Sonnet/Opus 之間動態切換的工作流
**不適用：** 輕度使用（直接用預設設定就夠）

## R（三種模式）

### Haiku Pilot（成本優先，預設）
觸發：`haiku` / `haiku-pilot`
- 弱規劃者 + 強委派 > 強規劃者單打
- 失敗 ≥3 次 → 升級 Sonnet；觸及架構/安全關鍵詞 → 升級 Opus

### Sonnet Pilot（品質優先）
觸發：`sonnet` / `sonnet-pilot`
- 深度 Context Engineering + 按需 Opus 顧問 ≈ 全 Opus 品質
- 強制 6 步驟前置檢查（Sonnet 的高信心偏誤容易跳過）
- 超過 800 字回應需重寫（非道歉）

### Opus Pilot（天花板提升）
觸發：`opus` / `opus-pilot`
- 5 個機制：逆向顧問迴路 / 平行假說合成 / CAR 明確性 / 決策日誌 / Meta-Harness 檔案系統迴路
- 硬規則：Opus 是**預設**天花板，子任務只能**向下**委派給 Sonnet/Haiku；特定高風險決策可短暫諮詢 Fable 5（見下方「Fable 5 決策點顧問」子機制），**不代表 Opus 不再是預設能力上限**

### Fable 5 決策點顧問（可選子機制，僅掛在 Opus Pilot）

> ⚠️ **成本警語**：Fable 5 目前為**限時可用**（Anthropic 曾於 2026-06-12 因出口管制全面暫停過一次，見 [[claude-fable-5-usage-guide]]），且定價明顯高於 Opus/Sonnet（輸入 $10/M、輸出 $50/M）。**不建議把 Fable 5 諮詢設計成常態路徑**，只適合當作可隨時關閉的臨時選配。

借用 [[fable-advisor-subagent-skill]] 的角色定位（Opus 統籌、Fable 5 只在決策點被諮詢），但補上該 skill 缺少的**硬性成本護欄**：

- **觸發範圍**：僅掛在 Opus Pilot，不影響 Haiku Pilot / Sonnet Pilot 的分級路由邏輯——只有觸及 Opus 天花板的任務才有諮詢更強模型的必要
- **次數上限**：每個任務最多 2 次諮詢（動手前一次、宣告完成前一次），硬性寫死，不是文字克制建議
- **Fallback**：Fable 5 存取權限或計費條件改變時，此子機制應可直接停用，不影響 Opus Pilot 原有 5 個機制的運作
- **總開關**：可整包關閉此子機制，避免長期綁死在一個不穩定計費/存取狀態的資源上

## C（關鍵行動）
- [ ] 大多數任務預設用 `haiku-pilot`，不要預設 Opus
- [ ] Sonnet Pilot 的品質保護是「保底」而非「提升」——防止複雜任務品質崩潰
- [ ] 安裝：clone 後用 symlink 連結到 `.claude/rules-pilot/` 和 `.claude/skills/`
- [ ] Fable 5 決策點顧問僅為 Opus Pilot 的選配子機制，啟用前先確認 Fable 5 當下的存取與計費狀態，不要當成常態依賴

## 相關連結
- [[claude-code-context-management]] — Sub-agent 委派策略
- [[dual-tool-strategy]] — Gemini CLI + Claude Code 的成本優化
- [[fable-advisor-subagent-skill]] — Fable 5 決策點顧問子機制借鑑的原始模式
- [[claude-fable-5-usage-guide]] — Fable 5 費用結構與限時可用性依據
- [[multi-model-orchestration-expansion-plan-2026-07-08]] — 本次更新的規劃來源
