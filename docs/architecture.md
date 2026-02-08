# System Architecture - Advanced RAG Pipeline

This document provides a comprehensive overview of the entire RAG pipeline system architecture, including both implementations and the evaluation framework.

## 📐 High-Level System Architecture

```mermaid
graph TB
    subgraph "Knowledge Base"
        KB[📚 Markdown Documents<br/>knowledge-base/]
    end
    
    subgraph "Implementation Choice"
        CHOICE{Choose Implementation}
        CHOICE -->|Basic| BASIC[implementation/]
        CHOICE -->|Advanced| ADV[pro_implementation/]
    end
    
    subgraph "Ingestion Pipeline"
        KB --> INGEST[Run ingest.py]
        BASIC --> INGEST
        ADV --> INGEST
        INGEST --> VDB[(Vector Database<br/>ChromaDB)]
    end
    
    subgraph "User Interface"
        USER[👤 User] --> UI[Gradio Web App<br/>app.py]
    end
    
    subgraph "Query Pipeline"
        UI --> ANSWER[answer.py]
        VDB -.->|Retrieve| ANSWER
        ANSWER --> LLM[🤖 LLM Generation]
        LLM --> RESPONSE[Answer + Context]
        RESPONSE --> UI
    end
    
    subgraph "Evaluation Framework"
        EVAL[evaluator.py] --> EVAL_IMPL[evaluation/eval.py]
        EVAL_IMPL --> TEST[Test Cases<br/>evaluation/test.py]
        EVAL_IMPL --> ANSWER
        EVAL_IMPL --> METRICS[📊 Metrics Dashboard]
    end
    
    style KB fill:#e3f2fd
    style VDB fill:#fff3e0
    style USER fill:#e8f5e9
    style METRICS fill:#f3e5f5
    style CHOICE fill:#fff9c4
```

---

## 🔄 Complete System Flow

### End-to-End Journey

```mermaid
sequenceDiagram
    participant Admin as System Admin
    participant Docs as Knowledge Base
    participant Ingest as Ingestion
    participant VDB as Vector Database
    participant User as End User
    participant UI as Web Interface
    participant Query as Query Pipeline
    participant LLM as Language Model
    participant Eval as Evaluator
    
    Note over Admin,Ingest: Phase 1: Setup
    Admin->>Docs: Add/Update Documents
    Admin->>Ingest: Run ingest.py
    Ingest->>Docs: Load Documents
    Ingest->>Ingest: Chunk & Process
    Ingest->>VDB: Store Embeddings
    
    Note over User,LLM: Phase 2: User Interaction
    User->>UI: Ask Question
    UI->>Query: Forward Question
    Query->>VDB: Retrieve Context
    VDB-->>Query: Return Chunks
    Query->>LLM: Generate with Context
    LLM-->>Query: Return Answer
    Query-->>UI: Answer + Sources
    UI-->>User: Display Response
    
    Note over Admin,Eval: Phase 3: Evaluation
    Admin->>Eval: Run Evaluation
    Eval->>Query: Test Multiple Questions
    Query-->>Eval: Return Results
    Eval->>Eval: Calculate Metrics
    Eval-->>Admin: Display Dashboard
```

---

## 🏗️ Detailed Component Architecture

### 1. Ingestion Pipeline Architecture

```mermaid
graph TB
    subgraph "Input Layer"
        FILES[📄 Markdown Files<br/>knowledge-base/*/*.md]
    end
    
    subgraph "Processing Layer"
        LOAD[Document Loader<br/>DirectoryLoader]
        FILES --> LOAD
        
        LOAD --> ROUTE{Implementation?}
        
        ROUTE -->|Basic| SIMPLE[Simple Chunking<br/>RecursiveCharacterTextSplitter<br/>500 chars, 200 overlap]
        ROUTE -->|Advanced| SMART[LLM Chunking<br/>GPT-4.1 Semantic Split<br/>+ Headlines + Summaries]
        
        SIMPLE --> CHUNKS1[Plain Text Chunks]
        SMART --> CHUNKS2[Enhanced Chunks<br/>headline + summary + text]
    end
    
    subgraph "Embedding Layer"
        CHUNKS1 --> EMB[OpenAI Embeddings<br/>text-embedding-3-large<br/>3072 dimensions]
        CHUNKS2 --> EMB
        EMB --> VECTORS[Vector Representations]
    end
    
    subgraph "Storage Layer"
        VECTORS --> STORE{Storage Path}
        STORE -->|Basic| DB1[(vector_db/<br/>~180 vectors)]
        STORE -->|Advanced| DB2[(preprocessed_db/<br/>~245 vectors)]
    end
    
    style FILES fill:#e3f2fd
    style SIMPLE fill:#ffccbc
    style SMART fill:#c8e6c9
    style DB1 fill:#fff3e0
    style DB2 fill:#fff3e0
```

### 2. Query Pipeline Architecture

#### Basic RAG Query Flow

```mermaid
graph LR
    Q[❓ User Question] --> H[📝 History Concat]
    H --> E[🧮 Embed Query]
    E --> S[🔍 Vector Search<br/>Similarity Top-10]
    S --> DB1[(vector_db)]
    DB1 -.->|Chunks| C[📋 Format Context]
    C --> P[🎯 Build Prompt<br/>System + Context + Question]
    P --> L[🤖 LLM Generate<br/>GPT-4.1-nano]
    L --> A[✅ Answer]
    
    style Q fill:#e8f5e9
    style S fill:#fce4ec
    style L fill:#e1f5fe
    style A fill:#c8e6c9
```

#### Advanced RAG Query Flow

```mermaid
graph TB
    Q[❓ User Question] --> R[🔄 LLM Rewrite<br/>Extract key terms]
    
    R --> S1[🔍 Search #1<br/>Original Query<br/>Top-20]
    R --> S2[🔍 Search #2<br/>Rewritten Query<br/>Top-20]
    
    S1 --> DB[(preprocessed_db)]
    S2 --> DB
    
    DB -.->|Chunks| M[🔗 Merge & Dedupe<br/>~30-40 unique chunks]
    
    M --> RK[🎯 LLM Rerank<br/>Semantic Relevance<br/>GPT-4.1-nano]
    
    RK --> T[📊 Select Top-10<br/>Best Quality]
    
    T --> P[🎯 Build Prompt<br/>System + Context + Question]
    
    P --> L[🤖 LLM Generate<br/>With Enhanced Context]
    
    L --> A[✅ Answer + Sources]
    
    style Q fill:#e8f5e9
    style R fill:#fff9c4
    style RK fill:#fff9c4
    style L fill:#fff9c4
    style A fill:#c8e6c9
    style M fill:#e1bee7
```

### 3. Evaluation Architecture

```mermaid
graph TB
    subgraph "Test Suite"
        TC[Test Cases<br/>evaluation/test.py<br/>Questions + Expected Results]
    end
    
    subgraph "Evaluation Engine"
        TC --> EVAL[evaluator.py<br/>Gradio Dashboard]
        EVAL --> CHOICE{Evaluation Type}
        
        CHOICE -->|Retrieval| RET[evaluate_all_retrieval]
        CHOICE -->|Answer| ANS[evaluate_all_answers]
        
        RET --> IMPL[Implementation<br/>ingest/answer modules]
        ANS --> IMPL
        
        IMPL --> VDB[(Vector DB)]
        VDB -.-> IMPL
    end
    
    subgraph "Metrics Calculation"
        RET --> MRR[MRR<br/>Mean Reciprocal Rank]
        RET --> NDCG[nDCG<br/>Normalized DCG]
        RET --> COV[Coverage<br/>Keyword Match %]
        
        ANS --> LLM_JUDGE[🤖 LLM as Judge<br/>GPT-4 Scoring]
        LLM_JUDGE --> ACC[Accuracy<br/>1-5 scale]
        LLM_JUDGE --> COMP[Completeness<br/>1-5 scale]
        LLM_JUDGE --> REL[Relevance<br/>1-5 scale]
    end
    
    subgraph "Visualization"
        MRR --> DASH[📊 Dashboard<br/>Color-coded Metrics]
        NDCG --> DASH
        COV --> DASH
        ACC --> DASH
        COMP --> DASH
        REL --> DASH
        
        DASH --> CHART[📈 Category Charts<br/>Bar Plots]
    end
    
    style TC fill:#e3f2fd
    style DASH fill:#f3e5f5
    style CHART fill:#c5cae9
    style LLM_JUDGE fill:#fff9c4
```

---

## 🔀 Implementation Comparison Architecture

### Side-by-Side System Comparison

```mermaid
graph TB
    subgraph "Basic RAG System"
        B_IN[📄 Documents] --> B_CHUNK[✂️ Character Split<br/>Fixed 500]
        B_CHUNK --> B_EMB[🧮 Embed]
        B_EMB --> B_DB[(vector_db<br/>180 vectors)]
        
        B_Q[❓ Question] --> B_SEARCH[🔍 Single Search<br/>K=10]
        B_DB -.-> B_SEARCH
        B_SEARCH --> B_LLM[🤖 LLM]
        B_LLM --> B_ANS[Answer]
        
        B_TIME[⏱️ 1-2s latency]
        B_COST[💰 $0.001/query]
        B_ACC[🎯 75% accuracy]
    end
    
    subgraph "Advanced RAG System"
        A_IN[📄 Documents] --> A_CHUNK[🤖 LLM Chunk<br/>+ Metadata]
        A_CHUNK --> A_EMB[🧮 Embed<br/>Parallel x3]
        A_EMB --> A_DB[(preprocessed_db<br/>245 vectors)]
        
        A_Q[❓ Question] --> A_RW[🔄 Rewrite]
        A_RW --> A_S1[🔍 Search 1<br/>K=20]
        A_RW --> A_S2[🔍 Search 2<br/>K=20]
        A_DB -.-> A_S1
        A_DB -.-> A_S2
        A_S1 --> A_MERGE[🔗 Merge]
        A_S2 --> A_MERGE
        A_MERGE --> A_RANK[🎯 Rerank]
        A_RANK --> A_TOP[Top 10]
        A_TOP --> A_LLM[🤖 LLM]
        A_LLM --> A_ANS[Answer]
        
        A_TIME[⏱️ 3-5s latency]
        A_COST[💰 $0.01/query]
        A_ACC[🎯 92% accuracy]
    end
    
    B_ANS -.->|Compare| EVAL[📊 Evaluation]
    A_ANS -.->|Compare| EVAL
    
    style B_CHUNK fill:#ffccbc
    style A_CHUNK fill:#c8e6c9
    style A_RW fill:#fff9c4
    style A_RANK fill:#fff9c4
    style EVAL fill:#f3e5f5
```

---

## 🗂️ Data Flow Architecture

### Document Processing Flow

```mermaid
graph LR
    subgraph "Source"
        MD[Markdown Files<br/>.md format]
    end
    
    subgraph "Ingestion"
        MD --> L[Load<br/>UTF-8 encoding]
        L --> M[Metadata<br/>source + type]
    end
    
    subgraph "Chunking"
        M --> C{Method}
        C -->|Basic| C1[Character-based<br/>~500 chars]
        C -->|Advanced| C2[LLM-based<br/>Semantic boundaries]
    end
    
    subgraph "Enhancement"
        C1 --> E1[Plain Text]
        C2 --> E2[Headline<br/>Summary<br/>Original]
    end
    
    subgraph "Vectorization"
        E1 --> V[OpenAI Embedding API<br/>3072-dim vectors]
        E2 --> V
    end
    
    subgraph "Storage"
        V --> S[ChromaDB<br/>Persistent storage]
    end
    
    style MD fill:#e3f2fd
    style C2 fill:#fff9c4
    style E2 fill:#c8e6c9
    style S fill:#fff3e0
```

### Query Processing Flow

```mermaid
graph TB
    USER[User Input] --> PARSE[Parse Question]
    PARSE --> HIST{Has History?}
    
    HIST -->|Yes| MERGE[Merge Context]
    HIST -->|No| DIRECT[Use Direct]
    
    MERGE --> PROC[Process]
    DIRECT --> PROC
    
    PROC --> METHOD{Implementation}
    
    METHOD -->|Basic| BASIC_PATH[Direct Embedding<br/>→ Single Search<br/>→ Top K=10]
    
    METHOD -->|Advanced| ADV_PATH[Rewrite Question<br/>→ Dual Search<br/>→ Merge<br/>→ Rerank<br/>→ Top K=10]
    
    BASIC_PATH --> FORMAT[Format Context]
    ADV_PATH --> FORMAT
    
    FORMAT --> PROMPT[Build Prompt<br/>System + Context + Q]
    PROMPT --> LLM[LLM API Call]
    LLM --> POST[Post-process]
    POST --> RETURN[Return Answer + Sources]
    
    style USER fill:#e8f5e9
    style ADV_PATH fill:#fff9c4
    style LLM fill:#e1f5fe
    style RETURN fill:#c8e6c9
```

---

## 🎨 User Interface Architecture

```mermaid
graph TB
    subgraph "Frontend - Gradio UI"
        USER[👤 User Browser]
        
        USER --> CHAT[💬 Chatbot Panel<br/>Conversation History]
        USER --> INPUT[⌨️ Text Input<br/>Question Box]
        
        CHAT --> DISPLAY[Display Messages]
        INPUT --> SUBMIT[Submit Handler]
    end
    
    subgraph "Backend Logic"
        SUBMIT --> PROCESS[Process Message]
        PROCESS --> BACKEND[answer_question]
        
        BACKEND --> IMPL{Which Implementation?}
        IMPL -->|Basic| B_ANSWER[implementation/answer.py]
        IMPL -->|Advanced| A_ANSWER[pro_implementation/answer.py]
        
        B_ANSWER --> RESULT[Result Handler]
        A_ANSWER --> RESULT
    end
    
    subgraph "Response Display"
        RESULT --> UPD_CHAT[Update Chat<br/>Add Assistant Message]
        RESULT --> UPD_CTX[Update Context Panel<br/>Show Retrieved Docs]
        
        UPD_CHAT --> DISPLAY
        UPD_CTX --> CTX_PANEL[📚 Context Panel<br/>Source Documents]
    end
    
    style USER fill:#e8f5e9
    style CHAT fill:#e1f5fe
    style CTX_PANEL fill:#fff3e0
    style IMPL fill:#fff9c4
```

---

## 💾 Database Architecture

### Vector Database Schema

```mermaid
graph TB
    subgraph "ChromaDB Structure"
        COL[Collection: 'docs']
        
        COL --> DOC[Documents<br/>Text content]
        COL --> EMB[Embeddings<br/>3072-dim vectors]
        COL --> META[Metadata<br/>source, type]
        COL --> ID[IDs<br/>Unique identifiers]
    end
    
    subgraph "Basic Implementation"
        DB1[(vector_db/)]
        DB1 --> C1[Collection: vector_db]
        C1 --> D1[~180 documents<br/>Plain text chunks]
    end
    
    subgraph "Advanced Implementation"
        DB2[(preprocessed_db/)]
        DB2 --> C2[Collection: docs]
        C2 --> D2[~245 documents<br/>Enhanced chunks<br/>headline + summary + text]
    end
    
    style COL fill:#fff3e0
    style DB1 fill:#ffccbc
    style DB2 fill:#c8e6c9
```

---

## 🔌 API & Integration Architecture

```mermaid
graph LR
    subgraph "External APIs"
        OPENAI[OpenAI API<br/>Embeddings + LLM]
        LITELLM[LiteLLM<br/>Unified interface]
        GROQ[Groq<br/>Fast inference]
    end
    
    subgraph "Application Layer"
        APP[Application Code<br/>Python]
        
        APP --> EMB_CALL[Embedding Calls<br/>text-embedding-3-large]
        APP --> LLM_CALL[LLM Calls<br/>GPT-4.1 / Groq]
        
        EMB_CALL --> OPENAI
        LLM_CALL --> LITELLM
        LITELLM --> OPENAI
        LITELLM --> GROQ
    end
    
    subgraph "Local Storage"
        VDB[(ChromaDB<br/>Local Files)]
        ENV[.env<br/>API Keys]
    end
    
    APP --> VDB
    APP -.->|Read| ENV
    
    style OPENAI fill:#e1f5fe
    style LITELLM fill:#f3e5f5
    style VDB fill:#fff3e0
    style ENV fill:#ffccbc
```

---

## 📦 Module Dependency Architecture

```mermaid
graph TB
    subgraph "Core Dependencies"
        LANGCHAIN[langchain<br/>RAG framework]
        CHROMADB[chromadb<br/>Vector DB]
        OPENAI_SDK[openai<br/>API client]
        GRADIO[gradio<br/>Web UI]
    end
    
    subgraph "Implementation Modules"
        IMPL[implementation/]
        IMPL_IN[ingest.py]
        IMPL_ANS[answer.py]
        
        PRO[pro_implementation/]
        PRO_IN[ingest.py]
        PRO_ANS[answer.py]
        
        IMPL --> IMPL_IN
        IMPL --> IMPL_ANS
        PRO --> PRO_IN
        PRO --> PRO_ANS
    end
    
    subgraph "Application Layer"
        APP[app.py]
        EVAL[evaluator.py]
        EVAL_LIB[evaluation/eval.py]
    end
    
    IMPL_IN --> LANGCHAIN
    IMPL_IN --> CHROMADB
    IMPL_ANS --> LANGCHAIN
    
    PRO_IN --> OPENAI_SDK
    PRO_IN --> CHROMADB
    PRO_ANS --> OPENAI_SDK
    
    APP --> IMPL_ANS
    APP --> PRO_ANS
    APP --> GRADIO
    
    EVAL --> EVAL_LIB
    EVAL --> GRADIO
    EVAL_LIB --> IMPL_ANS
    EVAL_LIB --> PRO_ANS
    
    style LANGCHAIN fill:#e1f5fe
    style CHROMADB fill:#fff3e0
    style GRADIO fill:#f3e5f5
    style PRO fill:#c8e6c9
```

---

## 🔄 State Management

### System State Flow

```mermaid
stateDiagram-v2
    [*] --> Uninitialized
    
    Uninitialized --> Ingesting: Run ingest.py
    Ingesting --> Indexed: Vector DB Created
    
    Indexed --> Ready: Start app.py
    Ready --> Processing: User asks question
    
    Processing --> Retrieving: Search vector DB
    Retrieving --> Generating: Found context
    Generating --> Responding: LLM completes
    Responding --> Ready: Show answer
    
    Ready --> Evaluating: Run evaluator.py
    Evaluating --> Testing: Run test suite
    Testing --> Calculating: Process results
    Calculating --> Visualizing: Compute metrics
    Visualizing --> Ready: Show dashboard
    
    Indexed --> Updating: Update documents
    Updating --> Ingesting: Re-ingest
```

---

## 🎯 Performance Characteristics

### Resource Usage Architecture

```mermaid
graph TB
    subgraph "Compute Resources"
        CPU[CPU<br/>Document processing<br/>Parallel workers]
        MEM[Memory<br/>Vector storage<br/>Embedding cache]
        DISK[Disk<br/>ChromaDB persistence<br/>~100MB-1GB]
    end
    
    subgraph "Network Resources"
        API_EMB[Embedding API<br/>~$0.0001/1K tokens]
        API_LLM[LLM API<br/>~$0.001-0.01/query]
        BANDWIDTH[Data Transfer<br/>Embeddings + Responses]
    end
    
    subgraph "Performance Metrics"
        LATENCY[Latency<br/>Basic: 1-2s<br/>Advanced: 3-5s]
        THROUGHPUT[Throughput<br/>~10-30 queries/min]
        ACCURACY[Accuracy<br/>Basic: 75%<br/>Advanced: 92%]
    end
    
    CPU --> LATENCY
    API_LLM --> LATENCY
    MEM --> THROUGHPUT
    API_EMB --> ACCURACY
    
    style LATENCY fill:#fff9c4
    style ACCURACY fill:#c8e6c9
```

---

## 🔒 Security & Configuration

### Configuration Architecture

```mermaid
graph TB
    subgraph "Environment Config"
        ENV[.env File]
        ENV --> API_KEY[OPENAI_API_KEY]
        ENV --> MODEL[MODEL_CONFIG]
        ENV --> DB_PATH[DB_PATHS]
    end
    
    subgraph "Application Config"
        CODE[Python Modules]
        CODE --> EMBED_MODEL[Embedding Model<br/>text-embedding-3-large]
        CODE --> LLM_MODEL[LLM Model<br/>gpt-4.1-nano]
        CODE --> CHUNK_SIZE[Chunk Parameters<br/>size, overlap]
        CODE --> RETRIEVAL_K[Retrieval K<br/>10 or 20]
    end
    
    subgraph "Runtime Config"
        USER_CHOICE[User Switches Imports]
        USER_CHOICE --> IMPL_SELECT{Implementation}
        IMPL_SELECT -->|Basic| BASIC_CONFIG[Basic Settings]
        IMPL_SELECT -->|Advanced| ADV_CONFIG[Advanced Settings]
    end
    
    ENV -.-> CODE
    CODE -.-> USER_CHOICE
    
    style ENV fill:#ffccbc
    style IMPL_SELECT fill:#fff9c4
```

---

## 📊 Monitoring & Observability

### Evaluation & Monitoring Architecture

```mermaid
graph TB
    subgraph "Metrics Collection"
        SYSTEM[Running System]
        SYSTEM --> COLLECT[Collect Metrics]
        
        COLLECT --> RET_MET[Retrieval Metrics<br/>MRR, nDCG, Coverage]
        COLLECT --> ANS_MET[Answer Metrics<br/>Accuracy, Completeness]
        COLLECT --> PERF_MET[Performance<br/>Latency, Cost]
    end
    
    subgraph "Analysis"
        RET_MET --> AGG[Aggregate by Category]
        ANS_MET --> AGG
        PERF_MET --> CALC[Calculate Averages]
        
        AGG --> COMPARE[Compare Baseline]
        CALC --> TREND[Trend Analysis]
    end
    
    subgraph "Visualization"
        COMPARE --> DASH[Dashboard UI<br/>Color-coded metrics]
        TREND --> CHART[Bar Charts<br/>Category breakdown]
        
        DASH --> ALERT{Threshold Check}
        ALERT -->|Green| GOOD[✅ Excellent]
        ALERT -->|Amber| WARN[⚠️ Review]
        ALERT -->|Red| BAD[❌ Action Needed]
    end
    
    style COLLECT fill:#e1f5fe
    style DASH fill:#f3e5f5
    style GOOD fill:#c8e6c9
    style WARN fill:#fff9c4
    style BAD fill:#ffccbc
```

---

## 🚀 Deployment Architecture

### Recommended Deployment Flow

```mermaid
graph TB
    subgraph "Development"
        DEV[Local Development]
        DEV --> TEST_BASIC[Test Basic RAG]
        TEST_BASIC --> EVAL_BASIC[Evaluate Baseline]
        EVAL_BASIC --> TEST_ADV[Test Advanced RAG]
        TEST_ADV --> EVAL_ADV[Evaluate Advanced]
    end
    
    subgraph "Decision"
        EVAL_ADV --> COMPARE[Compare Results]
        COMPARE --> DECIDE{Good Enough?}
        DECIDE -->|Basic OK| DEPLOY_BASIC[Deploy Basic<br/>Lower cost]
        DECIDE -->|Need Better| DEPLOY_ADV[Deploy Advanced<br/>Higher quality]
    end
    
    subgraph "Production"
        DEPLOY_BASIC --> PROD[Production Server]
        DEPLOY_ADV --> PROD
        
        PROD --> MONITOR[Monitor Metrics]
        MONITOR --> OPTIMIZE[Optimize<br/>Fine-tune parameters]
        OPTIMIZE --> UPDATE[Update Models/Data]
        UPDATE --> PROD
    end
    
    style COMPARE fill:#fff9c4
    style DECIDE fill:#fff9c4
    style PROD fill:#c8e6c9
```

---

## 📝 Summary

### System Characteristics

| Component | Basic RAG | Advanced RAG |
|-----------|-----------|--------------|
| **Ingestion** | Simple, fast | Complex, thorough |
| **Chunking** | Rule-based | LLM-powered |
| **Retrieval** | Single query | Dual query + rerank |
| **Latency** | 1-2s | 3-5s |
| **Cost/Query** | $0.001 | $0.01-0.02 |
| **Accuracy** | 75% | 92% |
| **Use Case** | Prototyping, simple docs | Production, complex docs |

### Architecture Principles

1. **Modularity**: Implementations are swappable
2. **Evaluation-Driven**: Built-in metrics and testing
3. **Progressive Enhancement**: Start basic, upgrade as needed
4. **Cost-Aware**: Clear trade-offs documented
5. **Production-Ready**: Both paths viable for deployment

---

## 🔗 Related Documentation

- [Basic RAG Architecture](basic_rag_architecture.md) - Detailed basic flow
- [Advanced RAG Architecture](advanced_rag_architecture.md) - Detailed advanced flow
- [Architecture Comparison](architecture_comparison.md) - Side-by-side comparison
- [Workflow Guide](workflow_guide.md) - How to use the system
- [Complete Journey](complete_journey.md) - User journey map

---

*Last Updated: 2026-02*
