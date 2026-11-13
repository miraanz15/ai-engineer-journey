# My 45-Day AI Engineer Journey

**Goal:** become an intermediate AI Engineer, ready for interviews, in **45 days**.
**Study time:** 7 hours a day · **26 topics, each learned twice** · ~315 hours.

Built around my senior's advice: **focus on the core AI skills, defer the rest.**
48% of this plan is RAG, Agents, LangGraph, Transformers, Embeddings and LLM.

---

## ⭐ Start here every morning

1. Open `00_PLAN/MASTER_PLAN.md` → see what today is
2. Do the 30-minute revision from `05_REVISION/revision_schedule.md`
3. Learn today's topic in `01_TOPICS/<topic>/1_theory.md`
4. At the end of the day, tick your boxes in `00_PLAN/SKILLS_CHECKLIST.md`

---

## Where everything lives

| Folder | What's inside |
|---|---|
| `00_PLAN/` | The 45-day schedule, the skills checklist, my progress log |
| `01_TOPICS/` | **The main folder.** 26 topics in learning order, each with the same 8 files (+ `_later/`) |
| `02_DAILY/` | A short log for each day, linking into the topic folders |
| `03_PROJECTS/` | All the code I write — mini-projects, integrations, capstones |
| `04_INTERVIEW_PREP/` | All questions, cheat sheets, system design, mock interviews |
| `05_REVISION/` | Flashcards, revision schedule, weak areas, confidence tracker |
| `06_RESOURCES/` | Every YouTube video and doc link, grouped by topic |
| `_archive/` | Old plans I'm not using right now |

---

## Every topic folder has the same 8 files

Learn this pattern once and you will never hunt for anything:

```
01_TOPICS/15_rag/
  README.md          what this topic is, which days, my confidence
  1_theory.md        📘 the explanation, in simple English, with diagrams
  2_summary.md       📝 ONE PAGE — the 10 things to remember
  3_examples.md      💡 worked examples + "the wrong way"
  4_drills.md        🏋️ practice exercises + solutions
  5_interview_qa.md  ❓ all the questions, with detailed answers
  6_flashcards.md    🔁 quick Q→A for revision
  7_resources.md     📺 videos and docs for this topic only
```

---

## Quick navigation

| I want to… | Go to |
|---|---|
| Know what to do today | `00_PLAN/MASTER_PLAN.md` |
| Learn today's topic | `01_TOPICS/<topic>/1_theory.md` |
| Revise something fast | `01_TOPICS/<topic>/2_summary.md` |
| Practise | `01_TOPICS/<topic>/4_drills.md` |
| Prepare interview answers | `01_TOPICS/<topic>/5_interview_qa.md` |
| Find a video | `01_TOPICS/<topic>/7_resources.md` or `06_RESOURCES/by_topic.md` |
| See my progress | `00_PLAN/SKILLS_CHECKLIST.md` |
| See what I'm forgetting | `05_REVISION/weak_areas.md` |
| Revise everything (last week) | `04_INTERVIEW_PREP/top_50_must_know.md` |

---

## The 26 topics — L1 then L2, in learning order

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

| # | Topic | L1 | L2 | Hrs |
|---|---|---|---|---|
| 00 | Environment, Git & GitHub | Day 1 | Day 43 | 8 |
| 01 | Python for AI | Day 2 | Day 42 | 7 |
| 02 | Jupyter, Pandas & NumPy | Day 3 | Day 40 | 7 |
| 03 | ML Foundations | Day 5 | Day 6 | 14 |
| 04 | Deep Learning & PyTorch | Day 7 | Day 8 | 14 |
| 05 | NLP & HuggingFace | Day 9 | Day 11 | 7 |
| 06 | ⭐ **Transformers** | Day 10 | Day 11 | **14** |
| 07 | ⭐ **LLM Fundamentals** | Day 13 | Day 21 | **14** |
| 08 | APIs, HTTP & Postman | Day 14 | Day 42 | 7 |
| 09 | Prompt Engineering | Day 15 | Day 21 | 11 |
| 10 | Structured Output | Day 16 | Day 21 | 7 |
| 11 | Databases: SQL & MongoDB | Day 18 | Day 27 | 6 |
| 12 | Document Ingestion | Day 19 | Day 27 | 4 |
| 13 | Streamlit | Day 19 | Day 24 | 5 |
| 14 | ⭐ **Embeddings & Vector DBs** | Day 20 | Day 21 | **14** |
| 15 | ⭐⭐⭐ **RAG** | Days 22–24 | Days 26–27 | **38** |
| 16 | Tools & Function Calling | Day 29 | Days 36–38 | 8 |
| 17 | ⭐⭐⭐ **Agents & Agentic AI** | Days 30–31 | Day 32 | **24** |
| 18 | Speech AI | Day 33 | Day 33 | 5 |
| 19 | **LangChain** | Day 34 | Day 35 | **16** |
| 20 | ⭐⭐ **LangGraph & Multi-Agent** | Day 36 | Days 37–38 | **21** |
| 21 | MCP | Day 39 | Day 39 | 5 |
| 22 | ⭐ **Evals** | Day 25 | Day 40 | 9 |
| 23 | FastAPI & Flask | Day 41 | Day 42 | 14 |
| 24 | Testing | Day 16 | Day 42 | 6 |
| 25 | Docker | Day 1 | Day 43 | 8 |

**~315 hours. Every topic learned twice.** The core six = **141 hours (45%)**.


## 🚩 Deferred to weeks 7–9 — `01_TOPICS/_later/`

AWS/Deployment · System Design · Observability · Security · Production Patterns · Fine-tuning hands-on.
**Nothing deleted** — these are my second 6 weeks.


---

## My two capstone projects

**🏆 Capstone 1 — DocBrain** *(Day 28)*
A full RAG system built **entirely by hand — zero frameworks.** Chunking, embeddings,
hybrid search, reranking, citations, grounding guard, and a real eval suite.
→ *"I built RAG from scratch before I ever touched LangChain."*

**🏆 Capstone 2 — ScreenerAI-Lite** *(Day 44)*
My real work project rebuilt: FastAPI + **LangGraph multi-agent with subagents** +
hybrid RAG + pgvector + Whisper/TTS + Docker + full eval suite.
→ *"I rebuilt my company's AI interview platform with a LangGraph agent system."*

---

## The rules I'm holding myself to

1. I write the code. Claude teaches, reviews and debugs — it does not do my assignments.
2. Every day: revision → learn → drills → build → interview drill → write it up.
3. Every drill set includes one **"break it on purpose"** exercise.
4. Nothing learned is allowed to fade — spaced repetition every single morning.
5. Under 70% on a drill means slow down, not push on.
6. **I also do DSA on my own**, alongside this plan. ~100 LeetCode easy/medium.
