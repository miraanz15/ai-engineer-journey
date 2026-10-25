# Topics Index

**26 topics, in learning order. Every topic is learned twice — Level 1 (basics), then Level 2 (intermediate).**

Every folder has the same 8 files — learn the pattern once, never hunt for anything again.

```
README.md  ·  1_theory.md  ·  2_summary.md  ·  3_examples.md
4_drills.md  ·  5_interview_qa.md  ·  6_flashcards.md  ·  7_resources.md
```

---

## ⭐ The Core Six — the destination

| Topic | Hours | Why |
|---|---|---|
| **15 · RAG** | **38** | 89% of JDs. The #1 skill. |
| **17 · Agents + 20 · LangGraph** | **45** | Agentic jobs grew 280%; demand:supply 3.2:1 |
| **07 · LLM Fundamentals** | **14** | Everything sits on this |
| **06 · Transformers** | **14** | The "why" under LLMs |
| **14 · Embeddings & Vector DBs** | **14** | 71% of JDs. RAG needs it. |
| **19 · LangChain** | **16** | 82% of JDs |

**141 hours = 45% of the plan.**

---

## The learning order

```
FOUNDATION      Git → Python → Pandas/NumPy
     ↓
HOW AI WORKS    ML → Deep Learning → NLP → ⭐ TRANSFORMERS
     ↓
LLM CORE        ⭐ LLM Fundamentals → APIs → Prompting → Structured Output
     ↓
DATA LAYER      Databases → Doc Ingestion → Streamlit → ⭐ EMBEDDINGS
     ↓
⭐⭐⭐ RAG       7 days — the #1 skill
     ↓
⭐⭐⭐ AGENTS    5 days
     ↓
FRAMEWORKS      LangChain → ⭐ LangGraph → MCP
     ↓
MEASURE & SHIP  Evals → FastAPI → Testing → Docker → Git for Teams
```

**Why this order:**
1. **Transformers before LLM Fundamentals** — can't understand context windows or temperature without knowing what's underneath
2. **Embeddings before RAG** — RAG is "embed, retrieve, stuff into a prompt." Learning it without embeddings is memorising
3. **Frameworks last** — build RAG and the agent loop by hand first, so LangChain is a convenience, not magic

---

## All 26 topics — both levels, every one

| # | Topic | **L1 (basics)** | **L2 (intermediate)** | Hrs |
|---|---|---|---|---|
| | **PHASE 1 — FOUNDATION** | | | |
| 00 | Environment, Git & GitHub | Day 1 — solo Git | Day 43 — teams, PRs, rebase | 8 |
| 01 | Python for AI | Day 2 | Day 42 — async, DI, testability | 7 |
| 02 | Jupyter, Pandas & NumPy | Day 3 | Day 40 — analysing eval results | 7 |
| | **PHASE 2 — HOW AI WORKS** | | | |
| 03 | ML Foundations | Day 5 — concepts | Day 6 — feature eng, tuning, imbalanced data | 14 |
| 04 | Deep Learning & PyTorch | Day 7 — training loop | Day 8 — optimizers, **transfer learning** | 14 |
| 05 | NLP & HuggingFace | Day 9 | Day 11 | 7 |
| 06 | ⭐ **Transformers** | Day 10 — architecture | Day 11 — attention & internals | **14** |
| | **PHASE 3 — LLM CORE** | | | |
| 07 | ⭐ **LLM Fundamentals** | Day 13 | Day 21 — routing, caching, budgets | **14** |
| 08 | APIs, HTTP & Postman | Day 14 | Day 42 | 7 |
| 09 | Prompt Engineering | Day 15 | Day 21 — prompt modules, A/B | 11 |
| 10 | Structured Output | Day 16 | Day 21 | 7 |
| | **PHASE 4 — DATA LAYER** | | | |
| 11 | Databases: SQL & MongoDB | Day 18 | Day 27 — pgvector | 6 |
| 12 | Document Ingestion | Day 19 | Day 27 — incremental re-indexing | 4 |
| 13 | Streamlit | Day 19 — widgets, session_state | Day 24 — streaming, upload, multipage | 5 |
| 14 | ⭐ **Embeddings & Vector DBs** | Day 20 | Day 21 — HNSW, ANN tuning | **14** |
| | **PHASE 5 — RAG** | | | |
| 15 | ⭐⭐⭐ **RAG** | Days 22–24 | Days 26–27 — hybrid, rerank, agentic | **38** |
| | **PHASE 6 — AGENTS** | | | |
| 16 | Tools & Function Calling | Day 29 | Days 36–38 — tools inside agents | 8 |
| 17 | ⭐⭐⭐ **Agents & Agentic AI** | Days 30–31 | Day 32 — planning, context engineering | **24** |
| 18 | Speech AI | Day 33 — Whisper, TTS | Day 33 — streaming, VAD, latency | 5 |
| | **PHASE 7 — FRAMEWORKS** | | | |
| 19 | **LangChain** | Day 34 — LCEL | Day 35 — retrievers, tools, callbacks | **16** |
| 20 | ⭐⭐ **LangGraph & Multi-Agent** | Day 36 — state, cycles | Days 37–38 — HITL, **subagents** | **21** |
| 21 | MCP | Day 39 — a simple server | Day 39 — resources, prompts, auth | 5 |
| | **PHASE 8 — MEASURE & SHIP** | | | |
| 22 | ⭐ **Evals** | Day 25 — golden dataset | Day 40 — RAGAS, LLM-judge, agent evals | 9 |
| 23 | FastAPI & Flask | Day 41 — routes, Pydantic | Day 42 — async, DI, SSE, Flask | 14 |
| 24 | Testing | Day 16 | Day 42 — mocking the LLM | 6 |
| 25 | Docker | Day 1 — concept | Day 43 — Dockerfile, compose | 8 |

**~315 hours over 45 days.**

---

## 🚩 Deferred — `_later/`

Six topics moved out, **not deleted**. These are my second 6 weeks.

| Topic | Hrs | Learn it |
|---|---|---|
| AWS · Deployment · CI/CD | 15 | Right after Day 45 — highest-value deferred item |
| System Design for AI | 11 | Week 7–8, before serious interviewing |
| Security & AI Security | 6 | Week 8 *(basics stay in Days 1, 15, 18)* |
| Production AI Patterns | 4 | Week 8 |
| Observability (Langfuse) | 3 | Week 7 |
| Fine-tuning / LoRA hands-on | 1 | Week 9+ *(the "RAG vs fine-tuning" answer stays in Day 27)* |
