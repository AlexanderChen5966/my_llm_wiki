---
title: 截圖到企業級前端：UIDL 四維分析 + 元件庫 Skill 架構設計
date: 2026-06-17
tags: [ai/agent, ai/claude, ai/prompt-engineering]
source: https://codelove.tw/@tony/post/xdbdka
type: article
status: processed
asset_value: 55
---

## T（核心論點）

解決 AI 生成前端程式碼的兩大根本問題：（1）**AI 讀不懂圖片**——輸出模糊的 300 字描述，缺欄位/對不上設計稿；（2）**AI 不知道你的元件庫**——生成藍紫漸層的「AI 味」頁面。解法是兩個串聯的 Skill：image-analyze-improvement（截圖→UIDL 結構化描述）→ vue-devui-practices（UIDL→符合元件庫規範的程式碼）。

## B（適用場景）

**適用：**
- 有固定企業級元件庫（Ant Design、Element Plus、DevUI 等）的前端團隊
- 需要從截圖/手繪稿/原型圖快速生成可維護前端程式碼
- 想設計「圖片→規範化程式碼」Skill Pipeline 的 Skill 設計師

**不適用：**
- 追求像素級完美還原（此方案優先元件化可維護性，不保證 100% 像素對齊）
- 沒有固定元件庫的個人專案（UIDL 方法論仍適用，但元件 Skill 需自行建立）

## R（完整方案）

### 三種 AI 生成前端方式比較

| 方式 | 優勢 | 劣勢 |
|------|------|------|
| 自然語言描述 | 門檻最低 | 300 字提示詞才能描述中等複雜頁面，還原度差 |
| Design-to-Code（Figma 原始檔）| 像素級還原（95%+）| 強依賴設計工具，不支援截圖/手繪，大量絕對定位 |
| 像素級視覺（v0.dev 等）| 門檻低，支援截圖 | 框架單一（React+Tailwind），不知道企業元件庫 |
| **UIDL + 元件庫 Skill（本文方案）** | 截圖即出碼 + 元件庫適配 + 風格可控 | 不保證像素對齊 |

### Skill 1：image-analyze-improvement（截圖→UIDL）

**四維分析模型（Position/Content/Interaction/Style）：**

| 維度 | 核心問題 | 描述規範重點 |
|------|----------|------------|
| **Position** | 元件在哪裡展示？| 使用精準方位詞（上方/左側/居中/右下角），禁用「旁邊/附近」 |
| **Content** | 展示哪些資訊？| 所有可見文字必須完整擷取 |
| **Interaction** | 內容如何動態變化？| 互動行為 + 狀態變更邏輯 |
| **Style** | 呈現何種視覺效果？| 具體數值（16px/十六進位色碼），禁用「大/小/藍色」等模糊詞 |

**分析順序：** 整體→局部，上→下，左→右

**層級關係描述：**
- 父子關係：「包含、位於…內部、嵌套於」
- 兄弟關係：「並列、相鄰、左側/右側」

**輸出格式：** Markdown（人類可讀）+ JSON（機器可讀的 UIDL），自動存為 `image-analysis-{pageName}.md`

**使用：** 上傳截圖時加上 `/image-analyze-improvement 分析圖片`

**核心洞見：** 先用 Mermaid（或人工）確定結構，再用 Nano Banana/Gemini 清稿──分工清楚，AI 只做「還原外觀」而不做「摘要結構」，避免關鍵元素被 AI 自行刪除。

### Skill 2：vue-devui-practices（UIDL→規範程式碼）

**Skill 目錄結構：**
```
vue-devui-practices/
├── SKILL.md
└── references/
    ├── DESIGNS.md          ← Design Token（顏色/圓角/間距/字體）
    ├── Form-表單.md
    ├── Input-輸入框.md
    ├── Icon-圖標.md
    └── demo/               ← 元件 Demo 檔案
```

**四步執行邏輯：**
1. 識別元件需求（從 UIDL 對應到元件庫清單）
2. 讀取 `DESIGNS.md`（禁止 AI 自由發揮設計規範）
3. 按需讀取對應元件 API/Demo 文件
4. 根據文件和最佳實踐生成頁面

**使用：** `/vue-devui-practices 使用 DevUI 元件生成一個表單頁面`

### 端到端三 Skill 組合

```
截圖上傳 → /image-analyze-improvement → UIDL
UIDL     → /vue-devui-practices        → 規範前端程式碼（元件+樣式）
後端API  → /api-call-generation         → 介面定義+呼叫+Mock
```

一次對話可完成：UI 頁面 + 介面呼叫 + Mock 機制（後端未就緒也不阻塞開發）

## C（關鍵行動）

- [ ] 製作自己專案的 `DESIGNS.md`（提取設計系統 Token：主題色/圓角/間距/字體）
- [ ] 把常用元件的 API/Demo 放進 `references/` 目錄，建立元件 Skill
- [ ] 從一個中等複雜度頁面試跑截圖→UIDL→程式碼完整流程，記錄哪個維度描述最容易失真

## 相關連結

- [[uiux-pro-max-skill]] — UIUX Pro Max：161 產業規則 + 67 風格的 AI 設計 Skill（互補）
- [[claude-code-design-skills-overview]] — 六款 Design Skill 精選（設計 Skill 生態概覽）
- [[taste-skill-anti-slop-frontend]] — taste-skill：三旋鈕調控防止 AI 生成通用模板
- [[frontend-agent-skills-30-picks]] — 前端 Skills 精選 30 個（七大類完整清單）
- [[seven-agent-software-factory]] — 七代理人軟體工廠（前端建造者代理人設計參考）
