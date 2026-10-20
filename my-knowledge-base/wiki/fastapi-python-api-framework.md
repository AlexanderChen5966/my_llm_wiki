---
title: 為什麼越來越多人使用 FastAPI？（Java 開發者視角）
date: 2026-07-03
tags: [dev/architecture, ai/workflow, type/concept]
source: https://codelove.tw/@tony/post/3bpYrq
type: article
status: processed
asset_value: 52
---

FastAPI 是 Python 高效能 API 框架（GitHub 80K+ Star），主打「三高」：高開發效率、高執行效能、高型別安全。特別適合 **AI 模型部署、資料處理、微服務**。與 Spring Boot 的關係像「跑車 vs SUV」——輕快靈活 vs 穩重紮實，各有場景。

## T（核心論點）

FastAPI 快的根本原因是 **ASGI**（非同步非阻塞）取代傳統 WSGI（同步、每請求獨占執行緒），能輕鬆處理數千併發。配合 Pydantic 在「門口」就攔下不合格請求（自動回 422），大減業務層防禦性程式碼——「宣告式程式設計」：只宣告「要什麼」，框架處理「怎麼驗證」。

## B（適用場景）

- **適合**：AI 模型推論服務（高併發+自動文件）、微服務、資料 API、WebSocket 即時應用、快速原型
- **不適合**：需強 Admin/完善 ORM/內建認證的大型全端（Django 較合）；CPU 密集計算（Spring Boot JIT 更穩）；純 Java 團隊（引入 Python 有學習/維運成本）

## R（主要內容）

**三引擎架構**：路由系統（`@app.get` 裝飾器，路徑匹配比 Flask Werkzeug 快 40%）、依賴注入（`Depends`，認證/DB session 與業務分離）、資料驗證引擎（Pydantic BaseModel）。

**核心能力**：原生 async/await、Pydantic 型別自動驗證、自動生成互動式文件（Swagger `/docs` + ReDoc `/redoc`，寫程式=寫文件）、依賴注入、中介軟體、統一回應/全域例外處理、非同步 SQLAlchemy。

**效能**：TechEmpower JSON 序列化達 Django 8 倍，接近 Go 的 Gin；約 Flask 的 2-3 倍。真實案例：某電商重構訂單服務開發週期 6 週→2 週、錯誤率降 72%、支援 2000+ QPS。

**與 Spring Boot 真實對比**：同業務各寫一遍跑半年——FastAPI 開發快（2 天跑起來）、首輪效能領先（45ms vs 80ms），但**最終 Java 同事贏**，因維運體系/監控告警/日誌聚合/錯誤追蹤這些「非功能需求」Spring Boot 生態更成熟。啟示：技術選型不能只看開發速度與效能，還要看團隊技術棧與長期維護成本。

## C（對自身的啟發）

- [ ] 部署 AI 模型/工具服務時，FastAPI 是高併發+自動文件的好選擇（對照 AgentCore/RAG 服務化）
- [ ] 「非功能需求決定長期成敗」呼應 production-agent-architecture 的可觀測性/評估
- [ ] Spring Boot 背景者可把 FastAPI 加入工具箱做 AI 部署快速原型

## 相關連結

- [[production-agent-architecture]] — 生產級 Agent（服務化/可觀測性）
- [[mcp-fastmcp-server]] — FastMCP Server（Python）
- [[minewatch-app-store-release-architecture]] — FastAPI 在真實 App Store 上架專案的架構落地（worker/beat 分離、API 版本化、Expand-Contract）
