# RAG Pipeline

## RAG has two main phases that run at different times:

Offline / Indexing Pipeline (done once or periodically when data changes)
Online / Query-Time Pipeline (runs every time a user asks something)

## 1. Indexing Pipeline (Data → Vector Store)
This prepares your knowledge base.
```
[Raw Data Sources]
   ↓
[1. Ingestion / Loading]
   ↓
[2. Pre-processing & Cleaning]
   ↓
[3. Chunking / Splitting]
   ↓
[4. Embedding Generation]
   ↓
[5. Metadata Enrichment (optional but very useful)]
   ↓
[6. Vector Store / Index Creation + Upsert]
```


## 2. Query-Time / Inference Pipeline (User Question → Answer)
   
```
User Question
   ↓
[1. Query Pre-processing (optional: rewrite / decompose)]
   ↓
[2. Query Embedding]  ← same embedding model as indexing
   ↓
[3. Retrieval]
      ├─→ Vector similarity search (top-k)
      ├─→ Optional: Hybrid (BM25 + vector)
      ├─→ Optional: Re-ranking (Cohere Rerank, bge-reranker, flashrank)
      └─→ Optional: Query transformation (HyDE, multi-query)
   ↓
[4. Context Assembly / Post-retrieval]
      ↓ (merge chunks, compress context if too long, add citations)
   ↓
[5. Prompt Construction]
      ↓ (system prompt + retrieved context + user question)
   ↓
[6. LLM Generation]  ← Mistral 7B, Llama-3.1 8B, Qwen 2.5, etc.
   ↓
[7. Optional: Post-processing]
      ↓ (guardrails, citation check, self-reflection / corrective RAG)
   ↓
Final Answer (+ sources / confidence)
```

## Combined High-Level Flow Diagram (Text Version) :

```
                ┌───────────────────────┐
                │   Indexing Pipeline   │               (run once / on update)
                │   (Offline / Batch)   │
                └───────────┬───────────┘
                            │
             ┌──────────────┼──────────────┐
             │                             │
   ┌──────────────────┐           ┌──────────────────┐
   │ Raw Documents    │           │ Vector Database  │
   │ PDFs, Markdown,  │           │ (Chroma / Qdrant)│
   │ Web, DB, etc.    │◄───┐      └────────┬─────────┘
   └────────┬─────────┘    │               ▲
            ▼              │               │
   ┌──────────────────┐    │   Retrieval   │
   │ Load → Clean     │    │               │
   │ → Chunk → Embed  ├────┘               │
   └──────────────────┘                    │
                                           │
                             ┌─────────────┼─────────────┐
                             │   Query-Time Pipeline       │
                             │      (Online / Real-time)   │
                             └─────────────┬─────────────┘
                                           │
                                 User Question
                                           ↓
                             ┌─────────────────────────────┐
                             │ Embed query → Retrieve →    │
                             │ Rerank → Build prompt → LLM │
                             └──────────────┬──────────────┘
                                            ↓
                                      Grounded Answer
```

## Quick Start Stack Recommendation (2026, local GPU, Mistral 7B)

- Framework → LlamaIndex or LangChain + LangGraph (for more advanced flows)
- Embedding → bge-m3 or snowflake-arctic-embed-m (fast & good quality)
- Vector DB → Chroma or Qdrant (persistent mode)
- LLM → Mistral-7B-Instruct-v0.3 (or Nemo 12B / Llama-3.2 3B for faster speed)
- Reranker (strongly recommended) → bge-reranker-large or flashrank
- Local inference → Ollama / vLLM / llama.cpp

## Advanced Additions (Production 2026)

- Parent-document retrieval / hierarchical indexing
- Self-RAG / Corrective RAG / Adaptive RAG
- GraphRAG (entities + knowledge graph)
- Agentic flows (query routing, fallback to web search)
- Evaluation loop (RAGAS, TruLens, DeepEval)
