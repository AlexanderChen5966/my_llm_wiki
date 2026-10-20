---
title: Claude Skills 從指令到資產的系統化構建
date: 2026-05-13
tags: [ai/claude, ai/agent, pkm/framework]
source: https://www.axtonliu.ai/newsletters/ai-2/posts/claude-agent-skills-maps-framework
type: article
status: processed
asset_value: 82
---

## T（核心論點）
Skill 不是「更好的 Prompt」，而是把隱性知識變成**可復用資產**的工程化過程——用 MAPS 框架設計架構，用漸進式披露機制讓 Skill 在需要時才載入，避免 Context 被消耗。

## B（適用場景）
**適用：** 已有重複使用的 Prompt，想升級成 Skill；想系統化管理多個 Skill
**不適用：** 還沒有穩定的重複場景（先用 Prompt 驗證再升級）

## R（操作步驟）
1. 用 MAPS 評估：M（值不值得做）→ A（架構設計）→ P（SKILL.md 內容）→ S（迭代計劃）
2. SKILL.md 結構：frontmatter（name/description/trigger/version）+ 核心邏輯 + 輸出模板
3. 漸進式披露：trigger 關鍵字觸發時才載入，平時不佔 Context
4. 用 5A+ 迭代：MVP → 觀察 → 調整 → 擴展

## C（關鍵行動）
- [ ] Skill vs Subagent vs MCP vs Project：先判斷用哪個，再決定怎麼建
- [ ] trigger 關鍵字要具體，避免意外觸發
- [ ] SKILL.md 寫完先測試邊界條件

## 原始摘要
Axton Liu 對 Skills 的深度架構分析。核心洞見：Skills 的護城河不在於內容，而在於「判斷邏輯的封裝密度」。Skills vs MCP 的選擇矩陣：靜態知識/流程 → Skills；動態工具/即時資料 → MCP。漸進式披露機制讓大型 Skill 庫不會一次塞爆 Context。

## 相關連結
- [[claude-skills-guide]] — Skills 兩種類型的實測指南
- [[maps-framework]] — MAPS 框架原厂定義
- [[agent-skills-vs-mcp]] — 與 MCP 的比較

## 補充資料（來源：Claude Skills： 从指令到资产的系统化构建指南.md）

### MAPS 四維度拆解 Claude Skills

**心智層（Mindset）：資產化思維**
- 從「每次現編 Prompt」→「一次封裝永久復用」
- 隱性知識顯性化：把你的判斷標準、領域知識封裝成 AI 可執行的模塊
- 「你只停留在『這個工具怎麼用』這一層，就很難在 AI 時代建立護城河」

**架構層（Architecture）：能力包 vs 軟編排**

| 類型 | 特點 | 適用 |
|------|------|------|
| 能力包型 | 封裝判斷邏輯，Claude 在主 Context 執行 | 重複性判斷工作（整理筆記、品質審查）|
| 軟編排型 | 定義 SOP，Task 工具調度多個 Subagent | 複雜流程（字幕→文章，需要多階段隔離）|

**提示詞層（Prompt）：description 決定觸發**

description 四要素：
1. **明確輸入類型**：「SRT 格式的字幕」、「Markdown 文章」
2. **說明核心功能**：與日常使用指令一致（「把字幕轉換成文章」）
3. **列舉關鍵觸發詞**：中英文都列（「字幕轉文章 / convert subtitle to article」）
4. **強調特殊能力**：這個 Skill 獨特的能力

**系統層（Systems）：5A+ 完整閉環**
- AIM → ACQUIRE → ATTEMPT（MVS 最小可行 Skill）→ ADJUST → APPLY → 進階（MCP 整合、API 串接）

### 軟編排四條工程級核心規則

1. **職責清晰**：主 Skill 只定義 SOP，主 Claude 只負責調配，不親自執行
2. **最小權限**：每個 Subagent 只拿必要的工具白名單
3. **文件傳遞**：Subagent 間透過固定命名的中間文件傳遞，不走對話（避免上下文污染）
4. **失敗記錄**：任何階段失敗寫入總報告，支援調試和回滾

### 資源庫管理建議

隨著 Skill 數量增加，建議用筆記工具（Notion / Obsidian）建立 AI 資源庫，記錄：
- Skill 用途與觸發詞
- 版本迭代歷史
- 依賴的 Subagent 和 MCP 清單

可用 Notion MCP 讓 Skill 自動同步到資源庫（Skill 調用 Skill 的典型案例）。
