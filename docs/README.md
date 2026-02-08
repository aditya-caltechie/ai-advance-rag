# Documentation Index

Comprehensive guide to the Advanced RAG Pipeline project documentation.

## 🎯 Getting Started

**New to this project?**
1. [Main README](../README.md) - Project overview and quick start
2. [Workflow Guide](workflow_guide.md) - Step-by-step implementation instructions
3. [System Architecture](architecture.md) - Complete system overview

## 📚 Documentation Structure

### Core Documentation
- **[System Architecture](architecture.md)** - Complete system design with flow diagrams
- **[Workflow Guide](workflow_guide.md)** - Hands-on implementation guide with examples
- **[Switching Guide](SWITCHING.md)** - Easy mode switching between basic and pro
- **[Architecture Comparison](architecture_comparison.md)** - Side-by-side analysis with metrics
- **[RAG vs Pro Comparison](rag_vs_pro.md)** - Detailed feature and cost comparison
- **[Advanced RAG Techniques](advancedRagTechniques.md)** - 10 advanced RAG techniques explained

### Implementation Details
- **[Basic RAG Architecture](basic_rag_architecture.md)** - Simple implementation flow
- **[Advanced RAG Architecture](advanced_rag_architecture.md)** - Enhanced implementation flow

## 🗂️ Repository Structure

```
src/rag-pipeline/
├── implementation/          # Basic RAG (rule-based)
│   ├── ingest.py           # Simple text chunking
│   └── answer.py           # Direct retrieval
├── pro_implementation/      # Advanced RAG (LLM-powered)
│   ├── ingest.py           # Semantic chunking + metadata
│   └── answer.py           # Multi-query + reranking
├── evaluation/              # Test framework
├── app.py                   # Gradio web interface
└── evaluator.py             # Metrics dashboard

docs/
├── architecture.md          # System design
├── workflow_guide.md        # Implementation guide
├── architecture_comparison.md  # Performance analysis
├── rag_vs_pro.md           # Feature comparison
├── basic_rag_architecture.md
└── advanced_rag_architecture.md
```

## 🎯 Quick Navigation by Role

| Role | Start Here | Then Review |
|------|-----------|-------------|
| **Student/Learner** | [Workflow Guide](workflow_guide.md) | [System Architecture](architecture.md) |
| **Developer/Engineer** | [System Architecture](architecture.md) | [Architecture Comparison](architecture_comparison.md) |
| **Decision Maker** | [Main README](../README.md) | [Architecture Comparison](architecture_comparison.md) (costs) |
| **Researcher** | [RAG vs Pro](rag_vs_pro.md) | [Architecture Comparison](architecture_comparison.md) |

## 📖 Key Topics

### Implementation
- **Chunking**: [System Architecture](architecture.md#1-ingestion-pipeline-architecture) | [RAG vs Pro](rag_vs_pro.md#ingestpy)
- **Retrieval**: [System Architecture](architecture.md#2-query-pipeline-architecture) | [Architecture Comparison](architecture_comparison.md#query-comparison)
- **Evaluation**: [Workflow Guide](workflow_guide.md#understanding-the-metrics) | [Architecture Comparison](architecture_comparison.md#evaluation-results-expected)

### Analysis
- **Performance**: [Architecture Comparison](architecture_comparison.md#detailed-comparison-table)
- **Costs**: [RAG vs Pro](rag_vs_pro.md#performance-metrics-estimated)
- **Trade-offs**: [Architecture Comparison](architecture_comparison.md)

## 🐛 Troubleshooting

- [Workflow Guide - Troubleshooting Section](workflow_guide.md#troubleshooting)
- Common issues: Rate limits, ChromaDB errors, memory issues

## 📊 Visual Aids

All documents include Mermaid diagrams for:
- System flows (ingestion & query pipelines)
- Component interactions
- Performance comparisons
- Decision trees

## ⚡ Quick Start Commands

```bash
# Setup
uv sync
cp .env.example .env
# Edit .env: Set RAG_MODE=basic or RAG_MODE=pro

# Basic RAG
cd src/rag-pipeline
uv run implementation/ingest.py
uv run app.py

# Advanced RAG (switch RAG_MODE=pro in .env)
uv run pro_implementation/ingest.py
uv run app.py

# Evaluation (uses mode from .env)
uv run evaluator.py
```

**Note**: See [Switching Guide](SWITCHING.md) for easy mode switching.

## 📝 Document Versions

| Document | Status | Description |
|----------|--------|-------------|
| architecture.md | ✅ | Complete system design |
| workflow_guide.md | ✅ | Implementation walkthrough |
| architecture_comparison.md | ✅ | Performance analysis |
| rag_vs_pro.md | ✅ | Feature comparison |
| basic_rag_architecture.md | ✅ | Basic flow details |
| advanced_rag_architecture.md | ✅ | Advanced flow details |

*Last updated: February 2026*

---

**Need Help?** Start with the [Workflow Guide](workflow_guide.md) for step-by-step instructions.

