# Projects

All the code I write. **I write it — Claude reviews and debugs.**

```
pass1/        Days 1–18, built by hand, no frameworks
pass2/        Days 19–32, frameworks + production
integration/  the weekly "join everything" projects
notebooks/    Jupyter work from Days 3–5
```

## Mini-projects

| Day | Project | Proves |
|---|---|---|
| 1 | My Learning Repo | Git, venv, project setup |
| 2 | Contact Book CLI | Python, Pydantic, argparse, logging |
| 3 | Dataset Explorer notebook | Jupyter, Pandas, NumPy |
| 4 | ⭐ Sklearn classifier | Real ML with held-out test metrics |
| 5 | PyTorch loop + HF NER | Deep learning, transformers |
| 6 | LLM Parameter Explorer | Temperature, tokens, cost |
| 7 | Chatbot with memory | API calls, retries, history |
| 9 | Prompt lab | 5 prompt styles, measured |
| 10 | Resume → JSON extractor | Structured output + tests |
| 11 | Chat history store | SQLite + MongoDB |
| 12 | Doc loader + Streamlit | PDF/HTML ingestion + UI |
| 13 | Embedding playground | Vectors, similarity, Chroma |
| 14 | Index Builder | Chunking + embedding pipeline |
| 15 | ⭐ AskMyDocs | Full working RAG + eval |
| 16 | Tool Box | Function calling |
| 17 | ⭐ ReAct agent + voice | Agent loop, Whisper, TTS |
| 19 | Prompt Engine v2 | Prompt modules, caching, routing |
| 21 | RAG in LangChain | LCEL, retrievers |
| 23 | ⭐ Multi-agent + subagents | LangGraph, supervisor, MCP |
| 24 | RAG as a REST API | FastAPI + pytest |
| 26 | Hybrid search + reranking | pgvector, BM25, RRF |
| 28 | ⭐ Deployed on AWS | EC2, ECR, S3, CI |
| 29 | Eval Dashboard | RAGAS, LLM-judge, Langfuse |
| 30 | Hardened Agent API | AI security, fallbacks, cost gates |

## Integration

| Day | Project | Joins |
|---|---|---|
| 8 | Smart CLI Assistant | Days 1–7 |
| 27 | Dockerized Agent Service | Days 19–26 |

## 🏆 Capstones

**Day 18 — VoiceDocs.** Talk to my own documents. Streamlit + Groq + ChromaDB
+ MongoDB + Whisper + TTS + hand-written agent loop. **Zero frameworks.**

**Day 32 — ScreenerAI-Lite.** My real work project rebuilt: FastAPI +
LangGraph multi-agent + hybrid RAG + pgvector + Docker + CI + Langfuse,
**deployed live on AWS.**

## Rule for every project

Each one ships with a `README.md` saying: what it does, how to run it,
what it proves, and what I'd do differently. A project a stranger can't
run is not finished.
