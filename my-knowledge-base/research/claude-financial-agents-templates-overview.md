---
title: Claude 金融 AI 代理範本：10 款官方 Agent 與三種部署方式
date: 2026-07-24
tags: [ai/agent, ai/claude]
source: https://www.bnext.com.tw/article/91588/anthropic-claude-financial-agents-guide
type: note
status: processed
asset_value: 27
---

Anthropic 推出開源的 10 款金融業 Claude AI 代理範本（Apache 2.0），涵蓋投行、股票研究、私募、財富管理四大場景，皆須先安裝 `financial-analysis` 核心模組（共用技能與 MCP 資料連接器），其餘代理才有底層工具可用。

**10 款 Agent**：Pitch Agent（提案簡報）、Meeting Prep Agent（會議行前包）、Market Researcher（產業研究）、Earnings Reviewer（財報比對更新模型）、Model Builder（DCF/收購/三表建模）、Valuation Reviewer（GP 估值檢核）、General Ledger Reconciler（總帳對帳）、Month-End Closer（月結）、Statement Auditor（財報稽核）、KYC Screener（開戶文件解析）。

**三種部署方式**：Claude Cowork（視窗操作，適合非技術分析師）／Claude Code（`claude plugin install` 指令碼，適合工程團隊）／Claude Managed Agents（企業後台 API 自動化部署，含子代理委派，目前為 Research Preview）。連接器透過 MCP 協定串接 Daloopa、FactSet、Moody's、PitchBook 等資料供應商，但企業仍需自行付費訂閱。

**邊界限制**：所有產出皆為待人工審核初稿，不做投資/法律建議，不執行交易或入帳；子代理委派功能仍在 Research Preview。企業客製化走「換連接器指向、灌企業脈絡進 Skill、套品牌 PPT 範本、改 agent 權限範圍、擴充新工作流」五個方向，全部是 Markdown/YAML 純文字檔，免編譯。

## 為何列為研究筆記而非 wiki 條目

內容偏向產品發表與功能總覽（資料來源本身也標註「本文初稿為 AI 編撰」），缺乏可驗證的實測案例或深度技術拆解，asset_value 27 未達 wiki 門檻。核心結構（Agents/Skills/Connectors 三元件 + 三種部署路徑）與既有 [[ai-agent-architecture-10-modules]]、[[api-cli-skills-agent-architecture]] 的通用 Agent 架構重疊度高，暫不重複建 wiki 條目；未來若有實際導入案例或深度技術評測，可再評估獨立成篇。

## 相關連結

- [[api-cli-skills-agent-architecture]] — API+CLI+Skills 讓產品 Agent-Accessible 的通用架構，可對照本篇 Agents/Skills/Connectors 三元件設計
- [[ai-agent-architecture-10-modules]] — 通用 Agent 十模組拆解，本篇的 Managed-agent wrappers 對應其 SubAgent 概念
