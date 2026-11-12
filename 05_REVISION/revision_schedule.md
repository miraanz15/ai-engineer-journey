# Revision Schedule

## Why this works — in simple English

My brain throws away anything it thinks I don't need. If I learn RAG on Day 14 and never touch it again, by Day 30 most of it is gone.

**I chose full cumulative revision, every day, with no sampling.** This means: every morning, I revise every topic from Day 1 through the day before, not a sampled few. Nothing is ever permanently left out of the daily pass, no matter how many days have built up.

```
Day 1 ─┐
Day 2 ─┤
Day 3 ─┼──► ALL of these get a quick flashcard pass, every single morning, forever
 ...   │
Day N-1┘
```

**Honest trade-off, agreed to upfront:** this grows over time. By Day 20, that's ~19 days of flashcards every morning. The way this stays feasible is **layer 1 below being fast recall, not full re-reading** — and if it ever starts crowding out the day's new learning, I say so and we adjust.

---

## The 5 layers

| Layer | When | What |
|---|---|---|
| 1. **Full cumulative flashcards** | every morning | Every card in `flashcards_all.md` from Day 1 to yesterday — fast recall, Easy/OK/Forgot, not full re-reads |
| 2. Throwback drills | inside daily drills | 2 exercises from an old topic, for deeper hands-on practice beyond flashcards |
| 3. Throwback questions | inside interview drill | 2 old questions mixed into today's interview drill |
| 4. Weekly full sweep | Days 8 · 18 · 27 · 32 | Re-run old projects end to end, fix what broke |
| 5. Blank-page rebuild | Sundays | Rebuild something from memory, no notes |

---

## The rule — simple now, not a lookup table

**Today, before starting the new lesson: go through every flashcard from Day 1 through the last completed day, in `05_REVISION/flashcards_all.md`.** No day is ever skipped once it's been learned. As the deck grows, this is done as fast recall (cover the answer, say it, score it) — not re-reading full theory files each time.

**Anything scored "Forgot" goes to `weak_areas.md` and gets real re-teaching time the next day** — this is where actual depth-revision time should go, not spread thin across everything already solid.

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
