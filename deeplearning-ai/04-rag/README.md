# 🔍 RAG - Retrieval-Augmented Generation

## Tổng quan
Kỹ thuật kết hợp retrieval (tìm kiếm) với generation (sinh text) để cung cấp cho LLM thông tin chính xác và cập nhật.

## RAG là gì?
```
Query → Retrieval (tìm tài liệu liên quan) → Augment (thêm vào prompt) → Generate (LLM trả lời)
```

## Các thành phần chính

### 1. Document Processing
- Text Splitting / Chunking strategies
- Document Loaders (PDF, HTML, Docs, DB...)
- Metadata extraction

### 2. Embedding & Vector Store
- Embedding Models (OpenAI, Cohere, BGE, E5...)
- Vector Databases (Pinecone, Weaviate, Qdrant, ChromaDB, pgvector)
- Indexing strategies

### 3. Retrieval
- Semantic Search (vector similarity)
- Keyword Search (BM25)
- Hybrid Search (kết hợp cả hai)
- Re-ranking (Cohere Rerank, ColBERT)

### 4. Advanced RAG Patterns
- **Naive RAG** - Cơ bản: embed → retrieve → generate
- **Advanced RAG** - Thêm re-ranking, query transformation
- **Modular RAG** - Pipeline tùy chỉnh
- **Agentic RAG** - Agent quyết định khi nào cần retrieve
- **GraphRAG** - Sử dụng knowledge graphs

## 📎 Tài liệu
- [RAG Survey Paper](https://arxiv.org/abs/2312.10997)
- [LlamaIndex RAG Guide](https://docs.llamaindex.ai/en/stable/)
- [LangChain RAG Tutorial](https://python.langchain.com/docs/tutorials/rag/)

---
*Thêm ghi chú vào thư mục `notes/` bên dưới.*
