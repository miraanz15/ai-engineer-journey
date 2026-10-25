# Revision Schedule

## Why this works — in simple English

My brain throws away anything it thinks I don't need. If I learn RAG on Day 14 and never touch it again, by Day 30 most of it is gone.

**But every time I recall something just as I'm about to forget it, my brain keeps it much longer.** That is spaced repetition. So I revisit each topic at growing gaps:

```
Learn ──► 1 day ──► 3 days ──► 7 days ──► 14 days ──► 30 days ──► it's mine
```

---

## The 5 layers

| Layer | When | Time | What |
|---|---|---|---|
| 1. Daily flashcards | every morning | 30 min | Cards from Days −1, −3, −7, −14, −30 |
| 2. Throwback drills | inside daily drills | 10 min | 2 exercises from an old topic |
| 3. Throwback questions | inside interview drill | 10 min | 2 old questions mixed in |
| 4. Weekly full sweep | Days 8 · 18 · 27 · 32 | full day | Re-run old projects, fix what broke |
| 5. Blank-page rebuild | Sundays | 45 min | Rebuild from memory, no notes |

---

## The rule

**Today I revise: yesterday · 3 days back · 7 days back · 14 days back · 30 days back.**

| Today | Revise these days |
|---|---|
| Day 2 | 1 |
| Day 3 | 2 |
| Day 4 | 3 · 1 |
| Day 5 | 4 · 2 |
| Day 6 | 5 · 3 |
| Day 7 | 6 · 4 |
| Day 8 | 7 · 5 · 1 |
| Day 9 | 8 · 6 · 2 |
| Day 10 | 9 · 7 · 3 |
| Day 11 | 10 · 8 · 4 |
| Day 12 | 11 · 9 · 5 |
| Day 13 | 12 · 10 · 6 |
| Day 14 | 13 · 11 · 7 |
| Day 15 | 14 · 12 · 8 · 1 |
| Day 16 | 15 · 13 · 9 · 2 |
| Day 17 | 16 · 14 · 10 · 3 |
| Day 18 | 17 · 15 · 11 · 4 |
| Day 19 | 18 · 16 · 12 · 5 |
| Day 20 | 19 · 17 · 13 · 6 |
| Day 21 | 20 · 18 · 14 · 7 |
| Day 22 | 21 · 19 · 15 · 8 |
| Day 23 | 22 · 20 · 16 · 9 |
| Day 24 | 23 · 21 · 17 · 10 |
| Day 25 | 24 · 22 · 18 · 11 |
| Day 26 | 25 · 23 · 19 · 12 |
| Day 27 | 26 · 24 · 20 · 13 |
| Day 28 | 27 · 25 · 21 · 14 |
| Day 29 | 28 · 26 · 22 · 15 |
| Day 30 | 29 · 27 · 23 · 16 |
| Day 31 | 30 · 28 · 24 · 17 · 1 |
| Day 32 | 31 · 29 · 25 · 18 · 2 |
| Day 33 | 32 · 30 · 26 · 19 · 3 |
| Day 34 | 33 · 31 · 27 · 20 · 4 |
| Day 35 | 34 · 32 · 28 · 21 · 5 |
| Day 36 | 35 · 33 · 29 · 22 · 6 |
| Day 37 | 36 · 34 · 30 · 23 · 7 |
| Day 38 | 37 · 35 · 31 · 24 · 8 |
| Day 39 | 38 · 36 · 32 · 25 · 9 |
| Day 40 | 39 · 37 · 33 · 26 · 10 |
| Day 41 | 40 · 38 · 34 · 27 · 11 |
| Day 42 | 41 · 39 · 35 · 28 · 12 |
| Day 43 | 42 · 40 · 36 · 29 · 13 |
| Day 44 | 43 · 41 · 37 · 30 · 14 |
| Day 45 | **EVERYTHING — final sweep** |

---

## Sunday blank-page rebuilds

Open a blank file. **Close all notes.** Type it from memory.

| Week | Day | Rebuild from memory | Done |
|---|---|---|---|
| 1 | 7 | A sklearn train/test split + metrics, from memory | - [ ] |
| 2 | 14 | **Attention, by hand** — Q, K, V for 4 words | - [ ] |
| 3 | 21 | A Pydantic model + a validated LLM call | - [ ] |
| 4 | 28 | **The whole RAG pipeline**, start to finish | - [ ] |
| 5 | 35 | **The ReAct agent loop** | - [ ] |
| 6 | 42 | **A LangGraph with a conditional edge and a cycle** | - [ ] |

**If I can't do it — that's the point.** I just found exactly what to revise. It goes straight into `weak_areas.md`.

---

## Weekly project re-run

Dead code is forgotten code. Every Sunday, one old project gets run end to end.
If it breaks (new library version, expired key, moved file) — **I fix it. That debugging is the revision.**

| Week | Day | Project re-run | Still works? |
|---|---|---|---|
| 1 | 7 | Day 2 Contact Book CLI | - [ ] |
| 2 | 14 | Day 5 Sklearn classifier + Day 8 fine-tuned model | - [ ] |
| 3 | 21 | Day 14 Chatbot + Day 20 Embedding playground | - [ ] |
| 4 | 28 | Day 24 AskMyDocs + Day 25 eval harness | - [ ] |
| 5 | 35 | Day 28 **Capstone 1 — DocBrain** | - [ ] |
| 6 | 42 | Day 32 research agent + Day 35 LangChain RAG | - [ ] |
| 7 | 45 | **Both capstones, from a clean clone** | - [ ] |

---

## Scoring

Every flashcard gets one of:

| Score | What happens |
|---|---|
| **Easy** | Next review at the longer gap |
| **OK** | Stays on schedule |
| **Forgot** | ⚠️ Goes to `weak_areas.md` → **comes back tomorrow**, not in 7 days |

## Health check (every Sunday)

- [ ] No card has been stuck in `weak_areas.md` for more than 7 days
- [ ] Every topic is at confidence 3+ within two weeks of learning it
- [ ] Last Sunday's blank-page rebuild got logged
