# Documentation Index

Welcome to the Advanced RAG Pipeline documentation! This index will guide you to the right resource.

## 🎯 Getting Started

**New to this project?** Start here:
1. 📖 [README.md](../README.md) - Project overview and quick start
2. 🗺️ [Complete Journey](complete_journey.md) - Visual roadmap with timeline
3. 📋 [Workflow Guide](workflow_guide.md) - Step-by-step instructions

## 🏗️ Architecture & Design

**Understanding how it works:**
- 🏛️ [**System Architecture**](architecture.md) - **Complete system overview with all components**
- 🔧 [Basic RAG Architecture](basic_rag_architecture.md) - Simple implementation details
- ⚡ [Advanced RAG Architecture](advanced_rag_architecture.md) - Enhanced implementation details
- 📊 [Architecture Comparison](architecture_comparison.md) - Side-by-side analysis with metrics

## 📚 Deep Dives

**Detailed analysis:**
- 🔍 [RAG vs Pro Comparison](rag_vs_pro.md) - In-depth feature comparison
  - Ingestion differences
  - Query pipeline differences  
  - Cost-benefit analysis
  - When to use each

## 🎓 By Learning Goal

### "I want to understand the basics"
1. Read: [README.md](../README.md) overview
2. Review: [System Architecture](architecture.md) for complete picture
3. Study: [Basic RAG Architecture](basic_rag_architecture.md)
4. Run: Basic implementation

### "I want to build both systems"
1. Follow: [Workflow Guide](workflow_guide.md)
2. Reference: [Complete Journey](complete_journey.md) for troubleshooting
3. Compare: Your results with expected metrics

### "I want to understand why advanced is better"
1. Overview: [System Architecture](architecture.md) - See both flows
2. Study: [RAG vs Pro Comparison](rag_vs_pro.md)
3. Review: [Architecture Comparison](architecture_comparison.md)
4. Analyze: Your evaluation metrics

### "I want to optimize for production"
1. Review: Trade-off analysis in [Architecture Comparison](architecture_comparison.md)
2. Study: Cost analysis in [RAG vs Pro Comparison](rag_vs_pro.md)
3. Plan: Based on your requirements

## 📂 Quick Reference

### Code Files
```
src/rag-pipeline/
├── implementation/          # Basic RAG
│   ├── ingest.py           # Simple chunking
│   └── answer.py           # Direct retrieval
├── pro_implementation/      # Advanced RAG  
│   ├── ingest.py           # LLM chunking
│   └── answer.py           # Multi-stage retrieval
├── app.py                   # Gradio UI
└── evaluator.py             # Evaluation dashboard
```

### Documentation Files
```
docs/
├── README.md (this file)              # Documentation index
├── architecture.md                    # Complete system architecture (NEW!)
├── complete_journey.md                # Visual roadmap
├── workflow_guide.md                  # Step-by-step guide
├── basic_rag_architecture.md          # Basic implementation
├── advanced_rag_architecture.md       # Advanced implementation
├── architecture_comparison.md         # Side-by-side comparison
└── rag_vs_pro.md                      # Detailed analysis
```

## 🔍 By Topic

### Chunking Strategies
- [System Architecture](architecture.md#1-ingestion-pipeline-architecture) - Overview of both approaches
- [Basic RAG Architecture](basic_rag_architecture.md) - RecursiveCharacterTextSplitter
- [Advanced RAG Architecture](advanced_rag_architecture.md) - LLM semantic chunking
- [RAG vs Pro](rag_vs_pro.md#ingestpy) - Detailed comparison

### Retrieval Techniques
- [System Architecture](architecture.md#2-query-pipeline-architecture) - Overview of query flows
- [Basic RAG Architecture](basic_rag_architecture.md) - Single-query vector search
- [Advanced RAG Architecture](advanced_rag_architecture.md) - Multi-stage with reranking
- [Architecture Comparison](architecture_comparison.md#query-comparison) - Performance impact

### Evaluation Metrics
- [Workflow Guide](workflow_guide.md#understanding-the-metrics) - Metric definitions
- [Architecture Comparison](architecture_comparison.md#evaluation-results-expected) - Expected results
- [Complete Journey](complete_journey.md#success-indicators) - Success criteria

### Cost Analysis
- [RAG vs Pro](rag_vs_pro.md#performance-metrics-estimated) - Cost per operation
- [Architecture Comparison](architecture_comparison.md#detailed-comparison-table) - Component costs
- [Complete Journey](complete_journey.md#cost-expectations) - Total project costs

## 🐛 Troubleshooting

Having issues? Check:
1. [Workflow Guide - Troubleshooting](workflow_guide.md#troubleshooting)
2. [Complete Journey - Common Pitfalls](complete_journey.md#common-pitfalls--solutions)
3. [Complete Journey - Verification Checklist](complete_journey.md#verification-checklist)

## 📊 Diagrams & Visuals

All documents include Mermaid diagrams that render on GitHub:
- Flow diagrams (ingestion & query pipelines)
- Comparison charts (basic vs advanced)
- Decision trees (when to use each)
- Timeline diagrams (project workflow)

## 🎯 Quick Links by Role

### Student / Learner
Start → [Complete Journey](complete_journey.md) → [Workflow Guide](workflow_guide.md)

### Developer / Engineer  
Start → [System Architecture](architecture.md) → [Architecture Comparison](architecture_comparison.md) → [RAG vs Pro](rag_vs_pro.md)

### Decision Maker / PM
Start → [README.md](../README.md) → [System Architecture](architecture.md) (overview) → [Architecture Comparison](architecture_comparison.md) (cost section)

### Researcher / Optimizer
Start → [RAG vs Pro](rag_vs_pro.md) → [Architecture Comparison](architecture_comparison.md)

## 📈 Evaluation & Metrics

### Running Evaluations
1. [Workflow Guide - Step 3](workflow_guide.md#step-3-run-evaluation) - How to run
2. [Complete Journey - Success Indicators](complete_journey.md#success-indicators) - What to look for

### Understanding Results
1. [Workflow Guide - Metrics](workflow_guide.md#understanding-the-metrics) - Metric definitions
2. [Architecture Comparison - Results](architecture_comparison.md#evaluation-results-expected) - Expected ranges

### Comparing Performance
1. [Architecture Comparison - Tables](architecture_comparison.md#detailed-comparison-table) - Component-level
2. [RAG vs Pro - Performance](rag_vs_pro.md#performance-metrics-estimated) - System-level

## 🚀 Next Steps

After reviewing the documentation:
1. ✅ Set up your environment ([README.md](../README.md))
2. ✅ Run Phase 1: Basic RAG ([Workflow Guide](workflow_guide.md))
3. ✅ Run Phase 2: Advanced RAG ([Workflow Guide](workflow_guide.md))
4. ✅ Compare results ([Architecture Comparison](architecture_comparison.md))
5. ✅ Experiment and optimize ([Complete Journey - Experimentation](complete_journey.md#1-experimentation-phase))

## 🤝 Contributing

Found something unclear? Have suggestions?
- Review the existing docs to ensure it's not already covered
- Consider contributing improvements to the documentation
- Share your evaluation results and findings

## 📝 Document Status

| Document | Status | Last Updated | Purpose |
|----------|--------|--------------|---------|
| README.md | ✅ Complete | 2026-02 | Project overview |
| architecture.md | ✅ Complete | 2026-02 | Complete system architecture |
| complete_journey.md | ✅ Complete | 2026-02 | Visual roadmap |
| workflow_guide.md | ✅ Complete | 2026-02 | Step-by-step guide |
| basic_rag_architecture.md | ✅ Complete | 2026-02 | Basic design |
| advanced_rag_architecture.md | ✅ Complete | 2026-02 | Advanced design |
| architecture_comparison.md | ✅ Complete | 2026-02 | Side-by-side |
| rag_vs_pro.md | ✅ Complete | 2026-02 | Detailed analysis |

---

**Happy Learning! 🎓**

For questions or issues, start with the [Workflow Guide](workflow_guide.md) or [Complete Journey](complete_journey.md).
