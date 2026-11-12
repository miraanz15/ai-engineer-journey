# Master Plan — 45 Days to Interview-Ready AI Engineer

**7 hrs/day ≈ 315 hours · 26 topics · 8 phases**

**Every topic is learned twice — Level 1 (basics) then Level 2 (intermediate).**

---

## What this plan is built around

My senior reviewed the first version and said: **do topics 00–23, defer the rest**, and the ones that actually matter are **RAG · Agentic AI · Embeddings · LangGraph · LangChain · Transformers · LLM**.

So I cut 7 topics, and **put every freed hour into those core topics.**

| | v1 | v2 (this plan) |
|---|---|---|
| Topics | 31 | **26** |
| Days | 35 | **45** |
| RAG | 28 hrs | **38 hrs** |
| Agents + LangGraph | 28 hrs | **45 hrs** |
| Transformers | buried | **its own 14-hr topic** |
| Core as % of plan | 29% | **48%** |

**Why 45 and not 35:**
- Cutting 7 topics saved **3 days**
- Deepening the core (RAG, Agents, LangGraph, Transformers) cost **10 days**
- Giving **every** topic a real Level 2 cost **3 more days** (ML, Deep Learning, FastAPI each needed a second day)

**= +10 days.** Every one went into either the core six or a missing Level 2.

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
⭐⭐⭐ RAG       the #1 skill — 7 days
     ↓
⭐⭐⭐ AGENTS    5 days
     ↓
FRAMEWORKS      LangChain → ⭐ LangGraph → MCP
     ↓
MEASURE & SHIP  Evals → FastAPI → Testing → Docker → Git for Teams
```

1. **Transformers before LLM Fundamentals** — can't understand context windows or temperature without knowing what's underneath
2. **Embeddings before RAG** — RAG is "embed, retrieve, stuff into a prompt"
3. **Frameworks last** — build it by hand first, so LangChain is convenience, not magic

---

# PHASE 1 — FOUNDATION (Days 1–4)

| Day | Topic | Build | Done |
|---|---|---|---|
| 1 | Environment · **Git L1** · Docker concept | My Learning Repo (on GitHub) | - [ ] |
| 2 | **Python L1** | Contact Book CLI | - [ ] |
| 3 | **Pandas/NumPy L1** · Jupyter | Dataset Explorer notebook | - [ ] |
| 4 | 🔁 Review + 🧩 **Integration 1** + **Mock #1** | Data Cleaning Tool | - [ ] |

# PHASE 2 — HOW AI WORKS (Days 5–12)

| Day | Topic | Build | Done |
|---|---|---|---|
| 5 | **ML Foundations L1** — the concepts | ⭐ Sklearn classifier w/ held-out test metrics | - [ ] |
| 6 | **ML Foundations L2** — feature engineering · tuning · imbalanced data · pipelines | Tuned model, measured improvement | - [ ] |
| 7 | **Deep Learning L1** — networks & training loop | PyTorch training loop, by hand | - [ ] |
| 8 | **Deep Learning L2** — optimizers · regularization · **transfer learning** | Fine-tune a pretrained model | - [ ] |
| 9 | **NLP & HuggingFace L1** | Tokenizer comparison + spaCy NER | - [ ] |
| 10 | ⭐ **Transformers L1** — architecture | Attention by hand: paper → NumPy | - [ ] |
| 11 | ⭐ **Transformers L2** + **NLP L2** — attention & internals | Inspect a real HF model's layers | - [ ] |
| 12 | 🔁 Review + **Mock #2** | Blank-page rebuild | - [ ] |

# PHASE 3 — LLM CORE (Days 13–17)

| Day | Topic | Build | Done |
|---|---|---|---|
| 13 | ⭐ **LLM Fundamentals L1** | LLM Parameter Explorer | - [ ] |
| 14 | **APIs · HTTP · Postman L1** · first Groq calls | Chatbot with memory | - [ ] |
| 15 | **Prompt Engineering L1** *(incl. prompt injection)* | Prompt lab — 5 styles, measured | - [ ] |
| 16 | **Structured Output L1** + **Testing L1** | Resume → JSON extractor + tests | - [ ] |
| 17 | 🔁 Review + 🧩 **Integration 2** + **Mock #3** | Smart CLI Assistant | - [ ] |

# PHASE 4 — DATA LAYER (Days 18–21)

| Day | Topic | Build | Done |
|---|---|---|---|
| 18 | **Databases L1** — SQL + MongoDB *(+ secrets hygiene)* | Chat history store | - [ ] |
| 19 | **Doc Ingestion L1** + **Streamlit L1** | Doc loader + chat UI | - [ ] |
| 20 | ⭐ **Embeddings & Vector DBs L1** | Embedding playground | - [ ] |
| 21 | ⭐ **Embeddings L2** + **LLM L2** + **Prompting L2** + **Structured L2** | Vector search tuned & measured | - [ ] |

# PHASE 5 — ⭐⭐⭐ RAG (Days 22–28) — *the heart of the plan*

| Day | Topic | Build | Done |
|---|---|---|---|
| 22 | **RAG L1 Pt 1** — the full pipeline | First working RAG | - [ ] |
| 23 | **RAG L1 Pt 2** — chunking deep dive | Chunking lab: 4 strategies | - [ ] |
| 24 | **RAG L1 Pt 3** — citations · guards + **Streamlit L2** | **AskMyDocs** | - [ ] |
| 25 | ⭐ **Evals L1** — how do you know it's good? | Golden dataset + eval harness | - [ ] |
| 26 | **RAG L2 Pt 1** — hybrid search + reranking | Measured vs Day-24 baseline | - [ ] |
| 27 | **RAG L2 Pt 2** + **DB L2 (pgvector)** + **Doc Ingestion L2** | Production RAG | - [ ] |
| 28 | 🔁 Review + 🏆 **CAPSTONE 1** + **Mock #4** | **DocBrain** | - [ ] |

# PHASE 6 — ⭐⭐⭐ AGENTS (Days 29–33)

| Day | Topic | Build | Done |
|---|---|---|---|
| 29 | **Tools & Function Calling L1** | Tool Box — 4 tools + dispatcher | - [ ] |
| 30 | **Agents L1 Pt 1** — ReAct loop, by hand | Hand-built agent | - [ ] |
| 31 | **Agents L1 Pt 2** — memory · stopping · cost | Agent with real memory | - [ ] |
| 32 | **Agents L2** — planning patterns · context engineering | Multi-step research agent | - [ ] |
| 33 | **Speech AI L1 + L2** — Whisper/TTS, then streaming · VAD · latency | Voice agent | - [ ] |

# PHASE 7 — FRAMEWORKS (Days 34–39)

| Day | Topic | Build | Done |
|---|---|---|---|
| 34 | **LangChain L1** — LCEL · runnables · prompts · parsers | Chains, hands-on | - [ ] |
| 35 | **LangChain L2** — retrievers · chains · tools · callbacks | **RAG rebuilt in LangChain** | - [ ] |
| 36 | ⭐ **LangGraph L1** — state · nodes · edges · cycles | First graph with a loop | - [ ] |
| 37 | ⭐ **LangGraph L2 Pt 1** — checkpointing · HITL · self-correction | Self-correcting RAG graph | - [ ] |
| 38 | ⭐ **LangGraph L2 Pt 2** — multi-agent · **subagents** · supervisor | Supervisor + subagents | - [ ] |
| 39 | **MCP L1 + L2** — server, then resources · prompts · auth + **Mock #5** | MCP server for my RAG | - [ ] |

# PHASE 8 — MEASURE & SHIP (Days 40–45)

| Day | Topic | Build | Done |
|---|---|---|---|
| 40 | ⭐ **Evals L2** — RAGAS · LLM-judge · **agent evals** + **Pandas L2** | Full eval suite | - [ ] |
| 41 | **FastAPI L1** — routes · Pydantic models · `/docs` | RAG agent as an API | - [ ] |
| 42 | **FastAPI L2** — async · DI · SSE + **Flask** + **Testing L2** + **APIs L2** + **Python L2** | Tested, streaming API | - [ ] |
| 43 | **Docker L2** + **Git L2 (teams: branching, PRs, review, rebase)** | Containerised stack | - [ ] |
| 44 | 🏆 **CAPSTONE 2** | **ScreenerAI-Lite** | - [ ] |
| 45 | **Mock #6, #7, #8** + portfolio · resume · pitch | Job-ready | - [ ] |

---

## 🏆 The two capstones

**CAPSTONE 1 — "DocBrain" `Day 28`**
A RAG system built **entirely by hand — no frameworks.** Streamlit + Groq + ChromaDB + MongoDB + hybrid search + reranking + citations + grounding guard + a real eval suite.
> *"I built RAG from scratch — chunking, embeddings, retrieval, reranking, evals — before I ever touched LangChain."*

**CAPSTONE 2 — "ScreenerAI-Lite" `Day 44`**
My real work project, rebuilt. FastAPI + **LangGraph multi-agent with subagents** + hybrid RAG + pgvector + Whisper/TTS + Docker + pytest + full eval suite.
> *"I rebuilt my company's AI interview platform with a LangGraph agent system — here's the repo."*

---

## Weekly rhythm

| Day | Special |
|---|---|
| **4 · 12 · 17 · 28 · 39** | Review day + mock interview |
| **7 · 14 · 21 · 28 · 35 · 42** | Sunday — 45-min **blank-page rebuild** from memory |
| **4 · 17 · 28 · 44** | Integration project or capstone |

**8 mock interviews total:** Days 4 · 12 · 17 · 28 · 39 · 45 (×3)

## Every day, 7 hours

```
0:00–0:30  🔁 Revision      flashcards: Days −1, −3, −7, −14, −30
0:30–2:30  📘 Learn         analogy → diagram → demo
2:30–3:30  🏋️ Drills        45 min today + 10 min throwback
3:30–5:30  🔨 Build         I write the code
5:30–6:30  ❓ Interview     questions answered out loud
6:30–7:00  ✍️ Write up      fill topic files, tick boxes, commit, push
```

---

## 🚩 Deferred to weeks 7–9 (in `01_TOPICS/_later/`)

| Topic | Hrs | When |
|---|---|---|
| AWS · Deployment · CI/CD | 15 | Right after Day 45 |
| System Design for AI | 11 | Week 7–8, before serious interviewing |
| Observability (Langfuse) | 3 | Week 7 |
| Security & AI Security | 6 | Week 8 *(basics stay in Days 1, 15, 18)* |
| Production AI Patterns | 4 | Week 8 |
| Fine-tuning / LoRA hands-on | 1 | Week 9+ *(the "RAG vs fine-tuning" answer stays in Day 27)* |

## Running in parallel (on my own)

**DSA** — ~100 LeetCode easy/medium. Arrays · strings · hashmaps · two pointers · sliding window · binary search · recursion · trees · BFS/DFS · basic DP.
Needed at roughly half of companies.

---

## The honest expectation

| Milestone | Where I'll be |
|---|---|
| **Day 45** | Interview-ready on the core. Will pass many interviews, will fail some |
| **+2 weeks** (AWS + system design) | Genuinely solid across the board |
| **Day 90** | Comfortably intermediate |

Start applying around **Day 34** — real interviews are the best practice.
