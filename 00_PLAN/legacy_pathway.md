# Zero to AI Engineer — Gemini API + Claude Pro Pathway

Stack: Python 3.11+, `google-genai` SDK, VS Code, Gemini API key, Claude (chat) as tutor/reviewer.

---

## Phase 0 — Environment

**0.1 — Install & verify**
- Install Python 3.11+, VS Code + Python extension.
- Terminal: `python3 --version`, `pip --version`.
- `mkdir ai-pathway && cd ai-pathway && python3 -m venv venv`
- Activate: `source venv/bin/activate` (Mac/Linux) or `venv\Scripts\activate` (Windows).
- `pip install google-genai python-dotenv`
- Create `hello.py`:
```python
print("Environment works")
```
- Run: `python hello.py`. **Done when:** it prints without error and `pip list` shows `google-genai`.

---

## Phase 1 — First API calls

**1.1 — Single hardcoded call**
File: `01_first_call.py`
- Create `.env` with `GEMINI_API_KEY=your_key_here`
- Use `python-dotenv` to load it, `google.genai.Client(api_key=...)`
- Call `client.models.generate_content(model="gemini-2.5-flash", contents="Explain recursion in 2 sentences.")`
- Print `response.text`.
- **Done when:** you get real text output, not an error.

**1.2 — Interactive loop**
File: `02_chat_loop.py`
- `while True:` read `input("You: ")`, exit on `"exit"`.
- Send each input as a fresh, independent call (no memory yet).
- Print `Gemini: {response.text}` each turn.

**1.3 — Add real memory**
File: `03_chat_with_memory.py`
- Use `client.chats.create(model="gemini-2.5-flash")` (Gemini's native chat session object — this handles history for you).
- Use `chat.send_message(user_input)` in the loop instead of `generate_content`.
- **Test:** tell it your name in turn 1, ask "what's my name?" in turn 3. It must answer correctly.

**1.4 — System prompt + params**
File: `04_configured_chat.py`
- Pass a `system_instruction` (e.g. "You are a terse Python tutor. Max 3 sentences.") via `config=types.GenerateContentConfig(system_instruction=..., temperature=0.2, max_output_tokens=200)`.
- Run the same question at `temperature=0.0` vs `temperature=1.0`, three times each. Save outputs to `temp_comparison.txt`. Note in a comment which was more consistent.

---

## Phase 2 — Structured output & real tools

**2.1 — JSON extraction**
File: `05_receipt_extractor.py`
- Hardcode a messy string: `"Coffee shop receipt: 2x Latte $4.50 each, 1x Croissant $3.25, total with tax $12.75, paid 2024-03-14"`
- Prompt Gemini to return **only** JSON matching: `{"items": [{"name": str, "qty": int, "unit_price": float}], "total": float, "date": "YYYY-MM-DD"}`
- Better: use `config=types.GenerateContentConfig(response_mime_type="application/json", response_schema=YourPydanticModel)` — define the schema with Pydantic (`pip install pydantic`).
- Parse with `json.loads`, wrap in `try/except json.JSONDecodeError`.
- **Done when:** it correctly extracts 2 items, total 12.75, date 2024-03-14, every run, 5 runs in a row.

**2.2 — File summarizer CLI**
File: `06_summarize.py`
- Use `argparse`: `python 06_summarize.py notes.txt`
- Read file, send to Gemini with prompt "Summarize in 3 bullet points."
- Write result to `notes_summary.txt`.
- Add `--bullets N` flag to control bullet count.

**2.3 — Prompt engineering comparison**
File: `07_prompt_styles.py`
- Task: "A cafe has 3 tables of 4 and 2 tables of 6. If each seat holds one person, how many people fit total?"
- Run it 3 ways: (a) direct question, (b) few-shot with 1 similar worked example first, (c) append "Think step by step before answering."
- Print all 3 outputs. Write 3–4 sentences in `reflection.md` on which was most reliable and why.

---

## Phase 3 — UI + persistence

**3.1 — Streamlit chat UI**
- `pip install streamlit`
- File: `08_app.py` — wrap your Phase 1.3 chat session in Streamlit: `st.chat_input`, `st.chat_message`, keep history in `st.session_state`.
- Run: `streamlit run 08_app.py`.

**3.2 — Persistent history (SQLite)**
- `pip install` nothing extra — `sqlite3` is built-in.
- File: `09_persistent_chat.py`
- Create table `messages(id, role, content, timestamp)`.
- On every message, insert to DB. On app start, load last session's messages and re-render them in Streamlit before accepting new input.
- **Test:** close the app, reopen — prior conversation must still show.

---

## Phase 4 — RAG (Retrieval-Augmented Generation)

**4.1 — Embeddings + chunking**
- `pip install chromadb`
- Gather 3–5 real `.txt` files (your own notes, a Wikipedia article, whatever).
- File: `10_build_index.py`
- Chunk each file into ~300-word pieces (simple split, no fancy libraries needed yet).
- Embed each chunk with `client.models.embed_content(model="gemini-embedding-001", contents=chunk)`.
- Store chunks + embeddings in a local Chroma collection (`chromadb.PersistentClient(path="./chroma_db")`).

**4.2 — Q&A over your documents**
- File: `11_rag_query.py`
- Take a user question → embed it → `collection.query(query_embeddings=..., n_results=3)` → get top 3 chunks.
- Build prompt: `"Answer using ONLY this context:\n{chunks}\n\nQuestion: {question}"`
- **Test:** ask a question whose answer is *only* in your files (not general knowledge). Confirm Gemini uses the retrieved text, not outside knowledge. Then ask something NOT in your files and confirm it says it doesn't know (add that instruction to the prompt).

---

## Phase 5 — Tool use / agents

**5.1 — Function calling**
- File: `12_function_calling.py`
- Define a real Python function `get_weather(city: str) -> dict` (can be fake/mocked data, or call a real free weather API).
- Declare it as a tool via `types.Tool(function_declarations=[...])`, pass in `config.tools`.
- Ask: "What's the weather in Tokyo?" — confirm Gemini emits a `function_call` part, you execute your real Python function, and send the result back via `send_message` with a `function_response` part to get the final natural-language answer.

**5.2 — Multi-tool mini agent**
- File: `13_mini_agent.py`
- Add a second tool: a calculator function `calculate(expression: str) -> float` (use `eval` carefully with a restricted namespace, or `numexpr`).
- Ask a question requiring both: "What's the weather in Paris, and what's 15% of 340?" — confirm it calls both tools correctly in one turn.

---

## Phase 6 — Engineering rigor

**6.1 — Retries & error handling**
- File: `14_robust_client.py`
- Wrap your API call in a retry decorator (`pip install tenacity`) — retry on rate-limit/5xx errors, exponential backoff, max 3 attempts.
- Simulate failure by temporarily using a bad API key and confirm retries happen (check printed logs) then fail gracefully with a clear error message.

**6.2 — Mini eval harness**
- File: `15_eval.py`
- Write 10 test cases as a list of dicts: `{"prompt": ..., "check": lambda output: ...}` (e.g. "output contains word X", "output is valid JSON", "output length < 50 words").
- Run all 10 against two different prompt versions of the same task. Print a pass/fail table and score (e.g. 8/10 vs 6/10).

**6.3 — FastAPI wrapper**
- `pip install fastapi uvicorn`
- File: `16_api.py`
- Expose your Phase 4 RAG Q&A as `POST /ask {"question": str}` → `{"answer": str, "sources": [...]}`.
- Run: `uvicorn 16_api:app --reload`. Test with `curl -X POST localhost:8000/ask -d '{"question":"..."}' -H "Content-Type: application/json"`.

**6.4 — Capstone**
- Combine: FastAPI backend (RAG + the two tools from 5.2) + Streamlit frontend calling that API + SQLite persistence.
- Deliverable: one folder, `README.md` with setup instructions, runs locally with two terminal commands (`uvicorn ...` and `streamlit run ...`).

---

## How to use Claude alongside this
Paste your code here after each assignment and ask me to: review for bugs, explain an error message, or suggest what to try if a test fails. I won't write the assignments for you — but I'll debug, explain concepts, and review your solutions.
