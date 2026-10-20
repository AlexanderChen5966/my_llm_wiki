---
title: LangChain RAG 三種架構：2-Step / Agentic / Hybrid
date: 2026-06-16
tags: [ai/agent, dev/architecture]
source: https://ithelp.ithome.com.tw/articles/10391500
type: article
status: processed
asset_value: 62
---

LangChain v1.0 提供三種 RAG（Retrieval-Augmented Generation）架構，核心差異在「誰決定何時檢索」：固定流程、模型自主決策、或兩者混合。

## T（核心論點）

RAG 的瓶頸從「能不能檢索」轉為「要不要檢索、何時檢索、用什麼策略檢索」。LangChain v1.0 把這三個問題分別對應到三種架構，讓開發者依任務複雜度選擇適合的 RAG 模式。

## B（適用場景）

| 架構 | 適用情境 | 不適用 |
|------|----------|--------|
| 2-Step（固定流程）| 查詢模式固定、文件庫穩定 | 需要根據問題動態調整策略 |
| Agentic RAG | 複雜問題、需多次檢索 | 對延遲敏感的場景 |
| Hybrid RAG | 需兼顧效率與靈活性 | 簡單單次查詢 |

## R（三種架構詳解）

### 架構一：2-Step RAG（固定流水線）

```
使用者問題 → 向量檢索 → 填入 Prompt → LLM 生成回答
```

**特點：** 流程固定，每次必定執行檢索步驟，無論問題是否需要。

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter
from langchain_google_genai import GoogleGenerativeAIEmbeddings
from langchain_core.vectorstores import InMemoryVectorStore
from langchain.retrievers import VectorStoreRetriever

# 文件分塊
splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = splitter.split_documents(docs)

# 向量化 + 儲存
embeddings = GoogleGenerativeAIEmbeddings(model="models/embedding-001")
vectorstore = InMemoryVectorStore.from_documents(chunks, embeddings)

# MMR 檢索（最大邊際相關性，確保多樣性）
retriever = vectorstore.as_retriever(
    search_type="mmr",
    search_kwargs={"k": 4, "fetch_k": 20}
)

# Prompt + LLM 串接
from langchain_core.prompts import ChatPromptTemplate
prompt = ChatPromptTemplate.from_template(
    "根據以下內容回答：{context}\n\n問題：{question}"
)
chain = retriever | prompt | llm
```

### 架構二：Agentic RAG（模型自主決策）

```
使用者問題 → Agent 判斷 → [是否需要檢索？] → 回答
                              ↓ 是
                          呼叫 @tool retriever → 取得資料 → 回答
```

**特點：** 把 retriever 包裝成 `@tool`，Agent 自行判斷是否需要使用。

```python
from langchain_core.tools import tool

@tool
def search_knowledge_base(query: str) -> str:
    """在知識庫中搜尋相關資訊。
    Args:
        query: 搜尋問題
    """
    docs = retriever.invoke(query)
    return "\n\n".join(doc.page_content for doc in docs)

# Agent 自主決策是否呼叫 search_knowledge_base
agent = create_agent(
    model=llm,
    tools=[search_knowledge_base],
    system_prompt="你是知識庫助理，複雜問題才需要搜尋資料庫。"
)
```

**優點：** 簡單問題不需要檢索，減少不必要的延遲和成本。

### 架構三：Hybrid RAG（混合策略）

結合 2-Step 的穩定性與 Agentic 的靈活性：

```
簡單問題 → 直接回答（不檢索）
複雜問題 → Agentic 決定是否多次檢索
標準問題 → 固定 2-Step 流水線
```

用 LangGraph 的條件邊（conditional edge）實作：
```python
def should_retrieve(state) -> str:
    if "查詢" in state["question"] or "找" in state["question"]:
        return "retrieve"
    return "direct_answer"

graph.add_conditional_edges("classify", should_retrieve, {
    "retrieve": "retrieve_node",
    "direct_answer": "answer_node"
})
```

### 三種架構比較

| 面向 | 2-Step | Agentic | Hybrid |
|------|--------|---------|--------|
| 延遲 | 固定較高 | 動態較低 | 中等 |
| 成本 | 固定 | 變動（通常更低）| 中等 |
| 可預測性 | 高 | 低 | 中 |
| 適合規模 | 小型 | 大型 | 中大型 |

## C（關鍵行動）

- [ ] 新專案預設用 2-Step RAG 驗證效果，再依需求升級
- [ ] 文件分塊選 `RecursiveCharacterTextSplitter`（自動找語義邊界）+ `MMR` 檢索（確保多樣性）
- [ ] Agentic RAG 要在 tool docstring 寫清楚「什麼時候應該用」，幫助 Agent 正確判斷

## 相關連結

- [[langchain-langgraph-basics]] — LangChain v1.0 六大模組 + LangGraph 基礎
- [[langchain-tool-use]] — @tool 包裝 RAG retriever 的工具化方法
- [[context-engineering-fundamentals]] — Context Engineering 的壓縮與記憶機制
- [[anythingllm-ollama-rag-local]] — 本地 RAG 實作（Ollama + AnythingLLM）
