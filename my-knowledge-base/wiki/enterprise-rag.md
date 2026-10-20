---
title: 企業私有知識庫 RAG 實作指南
date: 2026-05-14
tags: [ai/agent, dev/architecture]
source: https://fordige.com/blog/openclaw-rag-knowledge-base-guide
type: article
status: processed
asset_value: 65
---

RAG（檢索增強生成）讓 AI 擁有「企業記憶」：把公司文件切成小塊、轉成向量存進資料庫，提問時先找最相關片段再讓 AI 回答。本地部署方案（OpenClaw + 本地 Embedding）確保資料不外流，滿足企業資安要求。

## T（核心論點）
企業 AI 的問題不是 AI 不夠聰明，而是 AI 不知道你的公司事。RAG 就是把「企業知識」餵給 AI，讓它從「完全沒有記憶的實習生」變成「有公司記憶的智能員工」。

## B（適用場景）
**適用：** 企業內部 FAQ / SOP 自動化；需要 AI 回答公司特定問題；資安要求資料不外流
**不適用：** 個人知識庫（用 Karpathy 方法更輕量）；公開資訊查詢（直接用 LLM）

## R（RAG 實作步驟）
1. **整理文件**：PDF / TXT / Markdown / CSV，標題清楚、段落完整、術語統一
2. **選擇 Embedding 模型**：
   - 雲端（簡單）：OpenAI `text-embedding-3-small` / Google `gemini-embedding`
   - 本地（資安）：選用本地可跑的 Embedding 模型
3. **建立向量資料庫**：存儲文件向量
4. **串接查詢流程**：使用者提問 → 向量搜索相關片段 → 連同問題送給 LLM → 回答

## C（關鍵行動）
- [ ] 整理原則：像寫給新進員工的手冊那樣整理文件，AI 效果會大幅提升
- [ ] 資安優先：本地部署方案讓資料不離開公司伺服器
- [ ] Karpathy 方法是個人知識庫的輕量替代：wiki/ 目錄即 retrieval 層，不需向量資料庫

## 相關連結
- [[karpathy-knowledge-base-method]] — 個人 RAG 的輕量版（無需向量資料庫）
- [[cognitive-hub]] — 企業知識外部化的設計模式
- [[agentcore-rag-agent-tutorial]] — AWS AgentCore 低程式碼建 RAG+Agent 實作
