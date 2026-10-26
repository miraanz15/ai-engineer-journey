# Skills Checklist — 26 topics, in learning order

⭐ = core, do not rush · 💥 = a "break it on purpose" drill exists for this

**Progress:** 0 / 26 topics complete

---

# PHASE 1 — FOUNDATION (Days 1–4)

## 00 · Environment, Git & GitHub — L1 `Day 1` ✅ COMPLETE
- [x] Python 3.11+ confirmed (3.13.5), VS Code
- [x] Terminal basics
- [x] venv — create, activate, why it exists
- [x] pip: install, list, freeze, `requirements.txt` — proven with a full destroy-and-rebuild test
- [x] `.env` + `python-dotenv`; `.gitignore` — incl. the "must exist before first commit" rule
- [x] Why version control exists
- [x] `git init` `status` `add` `commit` `log` `diff`
- [x] The three rooms: working area → staging → repository
- [x] Good commit messages — practiced, got feedback, revised
- [x] `branch` `switch` `merge`
- [x] 💥 Merge conflicts — caused and resolved twice, plus a clean three-way merge in between
- [x] `remote add` `push` `pull` `clone`
- [x] `fetch` vs `pull` — done hands-on via a real second clone, both ways
- [x] Undo: `restore` `restore --staged` `reset --soft` · `git revert` · full stash toolkit (push/show/apply/pop/drop)
- [x] `HEAD`, detached HEAD (risk + fix both proven live) · `git blame` · tags (lightweight + annotated)
- [x] GitHub: real repo created, real first push, under own identity
- [x] Docker concept: image vs container, daemon vs CLI, venv-vs-Docker scope
- [x] Run MongoDB in Docker — pulled, ran detached, verified port mapping, stopped/started, confirmed same container ID survives restart

## 01 · Python for AI `Day 2`
- [ ] Data types; list / tuple / dict / set — when to use each
- [ ] Control flow; functions (`*args`, `**kwargs`, defaults)
- [ ] f-strings; list & dict comprehensions
- [ ] Classes/OOP: `__init__`, `self`, methods, inheritance
- [ ] Modules, imports, `if __name__ == "__main__"`
- [ ] File I/O: `open`, `with`, `pathlib`; JSON `dumps`/`loads`
- [ ] Exceptions: try/except/finally, `raise`, custom exception classes
- [ ] Type hints: `str`, `list[str]`, `Optional`
- [ ] ⭐ **Pydantic v2**: `BaseModel`, `Field`, validators, nested models
- [ ] `logging` (never `print` in real code)
- [ ] `argparse`; decorators (read-level); `os.getenv`
- [ ] **L2 `Day 39`:** async/await, dependency injection, testability

## 02 · Jupyter, Pandas & NumPy `Day 3`
- [ ] Notebooks: cells, kernel, execution order
- [ ] 💥 Why out-of-order cells bite you
- [ ] Jupyter vs Google Colab (free GPU)
- [ ] ⭐ When to use a notebook vs a `.py` file
- [ ] NumPy: arrays vs lists, `dtype`, shapes
- [ ] Indexing, slicing, boolean masks
- [ ] Broadcasting; vectorised ops (why loops are slow)
- [ ] `reshape`, `concatenate`, axis operations
- [ ] ⭐ **Dot product & matrix multiply** — the maths behind attention and embeddings
- [ ] Pandas: `Series` vs `DataFrame`; `read_csv`/`read_json`
- [ ] `head` `info` `describe` `shape` `dtypes`
- [ ] Selecting: `[]`, `.loc`, `.iloc`, boolean filtering
- [ ] Missing data: `isna` `fillna` `dropna`
- [ ] `groupby` + aggregation; `sort_values`; `value_counts`
- [ ] `merge` / `join` / `concat`; `apply` + lambda
- [ ] DataFrame → JSON for LLM input
- [ ] **L2 `Day 38`:** analysing eval results with pandas

---

# PHASE 2 — HOW AI WORKS (Days 5–10)

## 03 · Machine Learning Foundations `Day 5`
- [ ] What ML is; AI vs ML vs DL vs GenAI
- [ ] Supervised vs unsupervised vs reinforcement
- [ ] Features, labels, training data
- [ ] ⭐ Train / validation / test split — why you never test on training data
- [ ] ⭐ **Overfitting vs underfitting**; bias–variance
- [ ] Loss functions — what "learning" means
- [ ] **Gradient descent** — the hill-walking analogy
- [ ] ⭐ **Accuracy, precision, recall, F1, confusion matrix**
- [ ] 💥 When accuracy lies (imbalanced data)
- [ ] Cross-validation
- [ ] scikit-learn: `fit` / `predict` / `score`, `train_test_split`
- [ ] Classification: logistic regression, decision tree, random forest
- [ ] Regression: linear regression, MSE/MAE/R²
- [ ] Clustering: k-means — connects to embeddings on Day 18
- [ ] Feature scaling & encoding

## 04 · Deep Learning & PyTorch `Day 6`
- [ ] What a neural network is (neurons, layers, weights)
- [ ] Activation functions (ReLU, sigmoid, softmax) — why non-linearity
- [ ] Forward pass → loss → **backpropagation** → weight update
- [ ] Epochs, batches, learning rate
- [ ] 💥 Learning rate too high → watch it diverge
- [ ] Why deep learning beat classical ML for text
- [ ] PyTorch tensors — creation, shapes, ops, GPU vs CPU
- [ ] `autograd` & gradients; `nn.Module`
- [ ] Loss function + optimizer
- [ ] ⭐ **Write a training loop yourself**
- [ ] `torch.no_grad()`, eval mode, saving/loading
- [ ] Embedding layers — the bridge to Day 7

## 05 · NLP & HuggingFace `Day 7`
- [ ] Why text is hard for computers
- [ ] **Tokenization**: word → subword → BPE / WordPiece
- [ ] 💥 Tokenize the same sentence with 3 tokenizers, compare
- [ ] Bag-of-words → TF-IDF → **Word2Vec** → contextual embeddings
- [ ] Why static embeddings fail ("bank" the river vs "bank" the money)
- [ ] **NER with spaCy** *(ScreenerAI uses this)*
- [ ] Text classification; sentiment analysis
- [ ] HuggingFace **Hub** — models, datasets, Spaces
- [ ] `transformers`: `pipeline()` in 3 lines
- [ ] `AutoModel` / `AutoTokenizer`
- [ ] Model cards — judging a model before using it
- [ ] `datasets` library; caching models; model size vs RAM

## 06 · ⭐ TRANSFORMERS `Days 8–9` — *senior-flagged core*

### Day 8 — Architecture
- [ ] The problem before transformers: RNNs, LSTMs, and why they failed
- [ ] ⭐ **"Attention Is All You Need"** — what changed in 2017
- [ ] ⭐ **What attention actually is** — plain English, then maths
- [ ] **Query, Key, Value** — the library analogy
- [ ] Scaled dot-product attention, step by step
- [ ] 💥 Compute attention for 4 words **by hand on paper**, then in NumPy
- [ ] **Multi-head attention** — why more than one head
- [ ] Self-attention vs cross-attention
- [ ] **Positional encoding** — why transformers need it at all
- [ ] Feed-forward layers; residual connections; layer normalisation
- [ ] Putting one full block together

### Day 9 — Internals & the real thing
- [ ] **Encoder vs decoder vs encoder-decoder** — BERT vs GPT vs T5
- [ ] ⭐ Why GPT is **decoder-only** and what that means
- [ ] **Causal / masked attention** — why a model can't see the future
- [ ] Stacking layers — what each layer learns
- [ ] The output head: logits → softmax → next token
- [ ] **Sampling**: greedy vs top-k vs top-p vs temperature *(connects to Day 11)*
- [ ] Context window — **where the limit physically comes from** (O(n²) attention)
- [ ] KV cache — why generation speeds up after the first token
- [ ] Attention variants (awareness): flash attention, sliding window, MoE
- [ ] 💥 Load a real HF model, print its layers, inspect attention weights
- [ ] ⭐ **Explain a transformer in 2 minutes with no notes**

---

# PHASE 3 — LLM CORE (Days 11–15)

## 07 · ⭐ LLM Fundamentals `Day 11` (L1) · `Day 19` (L2)

### L1 `Day 11`
- [ ] What an LLM is — now that I know transformers
- [ ] ⭐ **Tokens** & tokenization — why you pay per token
- [ ] ⭐ **Context window** — what fits, what gets cut, where the limit comes from
- [ ] Parameters (7B / 70B) — what the number means
- [ ] **Pre-training → instruction tuning → RLHF**
- [ ] ⭐ **Temperature**, top_p, top_k — connected back to Day 9 sampling
- [ ] `max_tokens`; stop sequences
- [ ] Roles: system / user / assistant
- [ ] ⭐ **How "memory" really works** — history is re-sent every time
- [ ] Streaming vs non-streaming
- [ ] ⭐ **Hallucination** — what & why, from first principles
- [ ] 💥 Make it hallucinate on purpose, then stop it
- [ ] 💥 Overflow the context window and watch what breaks
- [ ] Knowledge cutoff; token cost math — estimate a monthly bill
- [ ] Open-weight vs closed; choosing a model (speed vs quality vs cost)

### L2 `Day 19`
- [ ] Context-window management & history trimming strategies
- [ ] ⭐ **Model routing** — cheap model first, expensive only when needed
- [ ] Streaming internals (SSE, chunk assembly)
- [ ] Caching: exact + **semantic cache**
- [ ] Multi-provider abstraction; handling provider outages
- [ ] Token budgeting in a real app; measuring cost per request

## 08 · APIs, HTTP & Postman `Day 12`
- [ ] What an API is; client–server
- [ ] Methods: GET POST PUT PATCH DELETE
- [ ] Status codes: 200 201 400 401 403 404 **429** 500 503
- [ ] Headers; `Authorization: Bearer`; query vs path vs body; JSON; REST
- [ ] `requests` library; `curl` basics
- [ ] **Groq API**: key, first call, SDK, streaming
- [ ] OpenAI-compatible clients — swapping `base_url`
- [ ] Lazy singleton client with timeout + max_retries
- [ ] 💥 Send a bad API key; trigger a 429; force a timeout
- [ ] Rate limits; retries + exponential backoff (`tenacity`)
- [ ] **Postman**: collections, environments, variables

## 09 · Prompt Engineering `Day 13` (L1) · `Day 19` (L2)

### L1 `Day 13`
- [ ] ⭐ Zero-shot · **Few-shot** · **Chain-of-Thought**
- [ ] Role / persona prompting; delimiters (XML tags, `###`)
- [ ] Specifying output format; clarity & specificity
- [ ] Positive vs negative instructions
- [ ] **BAD/GOOD contrast pairs** *(ScreenerAI pattern)*
- [ ] Prompt templates with variables; prompt chaining
- [ ] System prompt design; guardrails in prompts
- [ ] ⭐ **Prompt injection** — attack & defence
- [ ] 💥 Write an injection that defeats my own system prompt, then block it
- [ ] The 6 most common prompt mistakes

### L2 `Day 19`
- [ ] ⭐ **Prompts as versioned Python modules**, not inline strings
- [ ] ⭐ **Pre-flight validation** — raise before you spend money
- [ ] Difficulty calibration ladders *(ScreenerAI)*
- [ ] Prompt versioning + **A/B testing with real metrics**
- [ ] Defence in depth against injection

## 10 · Structured Output `Day 14`
- [ ] Why JSON output matters; prompting for JSON (and why it's fragile)
- [ ] **JSON mode / `response_format`**
- [ ] ⭐ **Pydantic schema → guaranteed structure**
- [ ] 💥 Force invalid JSON, then make the code survive it
- [ ] Retry-on-validation-failure loop
- [ ] Enums & constrained choices; nested structures
- [ ] Tolerant schemas (`extra="allow"`) *(ScreenerAI)*
- [ ] Robust free-text parsers (strip numbering, enforce counts)
- [ ] Output schema vs function schema

## 24 · Testing — L1 `Day 14`
- [ ] What a test is; `pytest` basics; `assert`
- [ ] First test on the extractor

---

# PHASE 4 — DATA LAYER (Days 16–19)

## 11 · Databases: SQL & MongoDB `Day 16`
- [ ] ⭐ **SQL vs NoSQL — when to pick which**
- [ ] `CREATE TABLE`; CRUD; `WHERE` `ORDER BY` `LIMIT`
- [ ] JOINs; `GROUP BY` + aggregates; keys; indexes
- [ ] Python `sqlite3`; parameterized queries
- [ ] 💥 Write a SQL-injectable query, exploit it, fix it
- [ ] MongoDB document model: database → collection → document
- [ ] PyMongo: `insert_one/many`, `find`, `update_one`, `delete_one`
- [ ] Operators: `$set` `$inc` `$push` `$in`; `find_one_and_update`
- [ ] Indexes; upsert & idempotent writes
- [ ] ObjectId/datetime → JSON serialization
- [ ] Graceful DB-down handling *(ScreenerAI)*
- [ ] **Secrets hygiene:** `.env`, never commit keys ⚠️ *(the ScreenerAI mistake)*

## 12 · Document Ingestion `Day 17`
- [ ] Why AI engineers load documents — building RAG corpora
- [ ] ⭐ **PDF extraction with `pdfplumber`**
- [ ] 💥 Try a scanned PDF and watch it fail — then know why
- [ ] **BeautifulSoup4**: `find`, `find_all`, `get_text`
- [ ] `requests` + User-Agent + encoding fixes
- [ ] Markdown / CSV / DOCX loaders
- [ ] Cleaning text: whitespace, zero-width chars, boilerplate
- [ ] Normalising to one flat schema; deduplication
- [ ] `robots.txt`, ToS & licensing risk

## 13 · Streamlit `Day 17`
- [ ] The rerun model — why variables reset
- [ ] Inputs: `text_input` `button` `selectbox` `file_uploader`
- [ ] `st.chat_input` + `st.chat_message`
- [ ] ⭐ **`st.session_state`** — what everyone gets wrong
- [ ] 💥 Forget `session_state` and watch chat reset every keystroke
- [ ] `sidebar` · `spinner` · caching (`cache_data` vs `cache_resource`)
- [ ] Streaming LLM output; `st.audio`

## 14 · ⭐ Embeddings & Vector DBs `Day 18` (L1) · `Day 19` (L2)

### L1 `Day 18`
- [ ] ⭐ **What an embedding is** — now that I know transformers
- [ ] Where embeddings come from inside a model
- [ ] Vectors & dimensions; semantic vs keyword search
- [ ] ⭐ **Cosine similarity**, dot product, Euclidean distance — and when each differs
- [ ] Normalisation — why cosine ≈ dot product on normalised vectors
- [ ] Embedding models; `sentence-transformers` + `all-MiniLM-L6-v2`
- [ ] Choosing an embedding model: size vs quality vs speed
- [ ] 💥 Embed Hindi with an English model, watch quality drop
- [ ] What a **vector database** is; why a normal DB won't do
- [ ] **ChromaDB**: client, collection, `add`, `query`, persistence
- [ ] Metadata + filtering; top-k; similarity thresholds

### L2 `Day 19`
- [ ] ⭐ **ANN vs exact search** — the speed/accuracy trade-off
- [ ] **HNSW** — how it actually works; `ef_search`, `M`
- [ ] IVF, product quantization (awareness)
- [ ] Tuning index parameters and measuring the effect
- [ ] Landscape: Pinecone vs FAISS vs Chroma vs Weaviate vs Qdrant vs pgvector
- [ ] Embedding drift; re-embedding when you change models
- [ ] Multi-vector & late-interaction retrieval (awareness: ColBERT)

---

# PHASE 5 — ⭐⭐⭐ RAG (Days 20–26) — *the #1 skill, 38 hours*

## 15 · RAG — Day 20: the pipeline
- [ ] ⭐ What RAG is — **open-book exam** analogy
- [ ] ⭐⭐ **RAG vs fine-tuning vs long context** *(asked in nearly every interview)*
- [ ] The full architecture: load → chunk → embed → store → retrieve → augment → generate
- [ ] Indexing time vs query time — the two separate pipelines
- [ ] Build one end to end, start to finish
- [ ] 💥 Ask something not in the docs, watch it invent an answer

## 15 · RAG — Day 21: chunking
- [ ] ⭐ **Chunking strategies**: fixed · sentence · recursive · semantic · document-structure
- [ ] ⭐ **Chunk size & overlap** — how to actually choose (256–512 tokens, 10–20%)
- [ ] Token-based vs character-based chunking
- [ ] Metadata on chunks — what to attach and why
- [ ] 💥 Compare 4 chunking strategies on the same corpus, measure the difference
- [ ] 💥 Set overlap to 0% then 50%, see what breaks
- [ ] Handling tables, code blocks, headers

## 15 · RAG — Day 22: quality & trust
- [ ] Prompt augmentation / context stuffing
- [ ] ⭐ **Citations & source attribution**
- [ ] ⭐ **Grounding guard** — making it say *"I don't know"*
- [ ] Confidence gate on retrieval score
- [ ] "Lost in the middle" — where in the context the model actually looks
- [ ] Context ordering — best chunk first or last?
- [ ] ⭐ **Debugging retrieval** — always print what came back
- [ ] The 6 ways RAG fails and how to spot each

## 22 · ⭐ Evals — L1 `Day 23`
- [ ] Why you must measure, not guess
- [ ] ⭐ Building a **golden dataset** from your own documents
- [ ] Deterministic checks: contains, regex, valid JSON, length
- [ ] Retrieval metrics: hit rate, MRR, recall@k
- [ ] Generation checks: did it use the context? did it cite?
- [ ] 💥 Sabotage retrieval, watch the scores drop
- [ ] Building a repeatable eval harness

## 15 · RAG — Day 24: better retrieval
- [ ] ⭐⭐ **Hybrid search** (BM25 + vector) + **Reciprocal Rank Fusion**
- [ ] Why keyword search still matters (names, IDs, codes)
- [ ] ⭐⭐ **Reranking** with a cross-encoder — bi-encoder vs cross-encoder
- [ ] Two-stage retrieval: retrieve wide, rerank narrow
- [ ] Measuring the gain vs the latency cost
- [ ] 💥 Rerank with the wrong model, watch quality drop

## 15 · RAG — Day 25: production
- [ ] ⭐ Query transformation: rewriting, **HyDE**, multi-query
- [ ] Metadata filtering & self-query
- [ ] **Postgres + pgvector**: `vector` column, HNSW index, `<->` `<=>`
- [ ] Migrating Chroma → pgvector, verifying identical results
- [ ] ⭐ **Agentic RAG** — the agent decides when and what to retrieve
- [ ] Multi-hop questions
- [ ] ⭐ **Tuning chunk size using evals, not vibes**
- [ ] Index updates, deletions, incremental re-indexing
- [ ] Multi-tenant RAG (per-user isolation)
- [ ] End-to-end cost & latency of a RAG request
- [ ] 📌 **Fine-tuning concept (30 min)** — LoRA/QLoRA, and *"prompt → RAG → fine-tune, in that order"*

---

# PHASE 6 — ⭐⭐⭐ AGENTS (Days 27–31)

## 16 · Tools & Function Calling `Day 27`
- [ ] What function / tool calling is — the bridge from GenAI to Agentic AI
- [ ] Tool schema (JSON Schema); writing good tool descriptions
- [ ] 💥 Vague vs precise description — measure call accuracy
- [ ] `tool_choice`: auto / required / none
- [ ] ⭐ **The full round trip**: model → tool_call → execute → return → answer
- [ ] Multiple tools; parallel tool calls
- [ ] 💥 Make a tool raise an error, handle it gracefully
- [ ] Tool design: granularity, naming, when a tool is too broad
- [ ] Tool result formatting — what the model can actually use

## 17 · ⭐ Agents & Agentic AI — Day 28: the loop
- [ ] ⭐ **GenAI vs Agentic AI** — ask-and-stop vs plan-act-observe-repeat
- [ ] What an AI agent is (vs a chain / workflow)
- [ ] ⭐⭐ **ReAct loop**: Thought → Action → Observation → repeat
- [ ] The agent's system prompt & scratchpad
- [ ] Trace a ReAct loop on paper before coding it
- [ ] **Write the whole loop by hand, no framework**
- [ ] 💥 Remove the max-iteration cap, watch it loop forever, then cap it

## 17 · Agents — Day 29: memory, control, cost
- [ ] **Memory architectures**: short-term (scratchpad) · long-term (vector) · episodic · semantic
- [ ] Conversation summarisation when history overflows
- [ ] Stopping conditions; max iterations; timeouts
- [ ] 💥 Give the agent a tool that always fails — recovery behaviour
- [ ] Error handling and retries inside the loop
- [ ] ⭐ **Cost control** — token burn in agent loops, and how to cap it
- [ ] Latency: why agents feel slow and what to do

## 17 · Agents — Day 30: planning & context
- [ ] ⭐ **Planning patterns**: ReAct · Plan-and-Execute · Reflexion · Tree-of-Thought
- [ ] Self-reflection / self-correction
- [ ] ⭐⭐ **Context engineering** — what goes in the window, what gets summarised, what gets dropped
- [ ] Tool selection when you have 20 tools (retrieval over tools)
- [ ] ⭐ **Guardrails & permissions** — least-privilege tools, approval gates
- [ ] ⭐⭐ **When NOT to use an agent**
- [ ] Agent failure modes and how to debug a bad run

## 18 · Speech AI `Day 31`
- [ ] Whisper STT — model sizes, `faster-whisper` + Groq Whisper API
- [ ] Audio formats, sample rates, why Whisper needs a real file
- [ ] 💥 Transcribe noisy audio, measure accuracy
- [ ] TTS — voices, streaming audio back
- [ ] Full voice loop: record → transcribe → LLM → speak
- [ ] Latency & cost of audio vs text

---

# PHASE 7 — FRAMEWORKS (Days 32–37)

## 19 · LangChain `Days 32–33`

### Day 32 — core
- [ ] Why frameworks exist; the ecosystem map
- [ ] ⭐ **LCEL** and the pipe `|`
- [ ] Runnables: `invoke` `batch` `stream` `astream`
- [ ] ChatModels & messages; `ChatPromptTemplate`, `MessagesPlaceholder`
- [ ] Output parsers: `StrOutputParser`, `PydanticOutputParser`, `JsonOutputParser`
- [ ] 💥 Feed a malformed output into a parser, handle it
- [ ] Custom runnables: `RunnableLambda`, `RunnableParallel`, `RunnablePassthrough`

### Day 33 — retrieval & tools
- [ ] Document loaders; **`RecursiveCharacterTextSplitter`**
- [ ] Embeddings & vector-store wrappers
- [ ] ⭐ **Retrievers**: `as_retriever`, `search_type`, `k`, **MMR**
- [ ] Retrieval chain / stuff-documents chain
- [ ] Memory & history trimming
- [ ] `@tool` decorator; LangChain agents; callbacks; streaming
- [ ] 💥 Swap the vector store without touching the chain
- [ ] ⭐ **When LangChain is overkill** — and what it costs you

## 20 · ⭐⭐ LangGraph & Multi-Agent `Days 34–36`

### Day 34 — the graph
- [ ] Why LangGraph exists (LangChain agents were hard to control)
- [ ] Graph vs chain
- [ ] ⭐ **State** (TypedDict / Pydantic), reducers, `Annotated`
- [ ] **Nodes**; **edges**; ⭐ **conditional edges**; `START` / `END`
- [ ] Compile & invoke; `stream` vs `invoke`
- [ ] ⭐⭐ **Cycles / loops** — the whole point of LangGraph
- [ ] 💥 Create an infinite loop, then add the recursion limit

### Day 35 — control & reliability
- [ ] ⭐ **Checkpointing & persistence** (threads, `MemorySaver`)
- [ ] Resuming a run after a crash
- [ ] ⭐ **Human-in-the-loop / interrupts** — approval before a destructive action
- [ ] Streaming graph events; `astream_events`
- [ ] ⭐ **Reflection / self-correction pattern**
- [ ] ⭐ **Corrective RAG (CRAG)** — grade retrieval, rewrite query, retry
- [ ] Debugging a graph run; visualising the graph

### Day 36 — multi-agent & subagents
- [ ] ⭐ **Patterns**: supervisor · hierarchical · sequential pipeline · network
- [ ] ⭐⭐ **Subagents** — a supervisor delegating to specialised children
- [ ] **Subgraphs** in LangGraph
- [ ] Agent **handoffs** and communication protocols
- [ ] Shared vs isolated state between agents
- [ ] ⭐ **When a subagent helps vs when it just adds cost and latency**
- [ ] 💥 Run the same task with 1 agent vs 3 — compare cost and time
- [ ] CrewAI (1 hr): Agent, Task, Crew, Process — enough to discuss it
- [ ] ⭐⭐ **When one agent beats five**

## 21 · MCP `Day 37`
- [ ] What MCP is ("USB-C for AI tools") & the problem it solves
- [ ] Architecture: host, client, server
- [ ] Primitives: tools, resources, prompts
- [ ] Transports: stdio vs HTTP/SSE
- [ ] Build a simple MCP server in Python
- [ ] Connect it to a client
- [ ] MCP vs plain function calling; security considerations

---

# PHASE 8 — MEASURE & SHIP (Days 38–42)

## 22 · ⭐ Evals — L2 `Day 38`
- [ ] ⭐⭐ **LLM-as-a-judge** and its biases (position, verbosity, self-preference)
- [ ] **Calibration anchors in rubrics** ("9–10 should be rare") *(ScreenerAI)*
- [ ] Decoupling scoring dimensions *(ScreenerAI)*
- [ ] ⭐⭐ **RAG metrics**: faithfulness · answer relevance · context precision · context recall
- [ ] **RAGAS** library, hands-on
- [ ] Hallucination detection
- [ ] ⭐ **Agent evals**: trajectory scoring · tool-call correctness · task completion
- [ ] Regression testing prompts; A/B testing
- [ ] Offline vs online eval; human eval
- [ ] Analysing eval results with pandas *(Topic 02 L2)*

## 23 · FastAPI & Flask `Day 39`
- [ ] ASGI vs WSGI; routes; path & query params
- [ ] ⭐ **Pydantic request/response models**; `response_model`
- [ ] ⭐ **Async endpoints — when `async` helps and when it hurts**
- [ ] 💥 Put a blocking call in an async endpoint, watch throughput collapse
- [ ] ⭐ **Dependency injection (`Depends`)**; middleware; **CORS**
- [ ] Exception hierarchy → HTTP status mapping *(ScreenerAI)*
- [ ] Background tasks; ⭐ **streaming responses / SSE** for LLM tokens
- [ ] File upload; `/docs` + OpenAPI; routers & project structure
- [ ] `uvicorn`; `pydantic-settings`; health checks
- [ ] **Flask (2 hrs, read-level):** app factory + Blueprints, `before_request`, `errorhandler`, Gunicorn — enough to work on ScreenerAI

## 24 · Testing — L2 `Day 39`
- [ ] Fixtures, `parametrize`, `monkeypatch`, `unittest.mock`
- [ ] ⭐⭐ **Mocking the LLM call** so tests are fast & deterministic
- [ ] Dependency injection for testability
- [ ] `TestClient` for FastAPI; test pyramid; coverage
- [ ] ⭐ **What to test in an AI app**: prompt builders, parsers, fallback branches
- [ ] Postman tests + Newman

## 25 · Docker — L2 `Day 40`
- [ ] Dockerfile: `FROM` `WORKDIR` `COPY` `RUN` `CMD` `ENTRYPOINT` `EXPOSE` `ENV`
- [ ] ⭐ **Layers & build cache** — reorder and halve the rebuild time
- [ ] Installing heavy ML deps efficiently *(ScreenerAI)*
- [ ] 💥 Forget `.dockerignore`, watch the image balloon
- [ ] Core commands; port mapping; **volumes**
- [ ] Caching model downloads with a volume *(ScreenerAI)*
- [ ] Secrets in containers; **multi-stage builds**; image size
- [ ] ⭐ **docker-compose**: services, `depends_on`, networks, volumes
- [ ] Service-to-service calls by container name
- [ ] The 5 Docker errors I will actually hit

## 00 · Git for TEAMS & ORGANISATIONS — L2 `Day 40`
*Day 1 was Git for working alone. This is Git for working with other people.*

### Branching strategies
- [ ] Why teams need a strategy at all
- [ ] **GitHub Flow** — one `main`, short-lived feature branches
- [ ] **Git Flow** — `main` / `develop` / `feature` / `release` / `hotfix`
- [ ] **Trunk-based development** — and why big teams move to it
- [ ] Branch naming conventions; short-lived vs long-lived branches

### Pull Requests
- [ ] ⭐ Opening a good PR: title, description, what/why
- [ ] Draft PRs; linking a PR to a ticket
- [ ] Keeping PRs small — why a 1,000-line PR never gets reviewed properly
- [ ] ⭐⭐ **Reviewing someone else's code** — what to look for, how to comment kindly
- [ ] Requesting changes vs approving vs commenting
- [ ] Responding to review comments; re-requesting review

### Merging
- [ ] ⭐ **Merge commit vs Squash merge vs Rebase merge**
- [ ] ⭐⭐ **`merge` vs `rebase`** — the real difference, with diagrams
- [ ] ⚠️ The golden rule: **never rebase a branch others are using**
- [ ] `git pull --rebase`; fast-forward vs no-ff

### Shared repos
- [ ] ⭐ Keeping your branch up to date with `main`
- [ ] ⭐ Resolving conflicts in a team setting (and how to avoid them)
- [ ] Protected branches; `CODEOWNERS`; required reviews & CI checks
- [ ] Forks vs branches; syncing a fork

### History & recovery
- [ ] ⭐ **`git reflog`** — recovering work you thought you destroyed
- [ ] ⭐ **`git cherry-pick`**; `git bisect`
- [ ] `git rebase -i` — squash, reword, drop, reorder
- [ ] `--amend` and when not to; `revert` on a shared branch
- [ ] ⚠️ `--force` vs `--force-with-lease`

### Conventions & secrets
- [ ] ⭐ **Conventional Commits** (`feat:`, `fix:`, `chore:`)
- [ ] Tags & **semantic versioning**; GitHub Releases
- [ ] `pre-commit` hooks; `.gitattributes`; Git LFS (awareness)
- [ ] ⭐ **`git rm --cached`** — and why `.gitignore` alone does NOT remove a committed secret
- [ ] ⚠️ **Applied to the real ScreenerAI repo** and its committed `.env`

---

# PROJECTS

## Mini-projects
- [ ] D1 — My Learning Repo (pushed to my GitHub)
- [ ] D2 — Contact Book CLI
- [ ] D3 — Dataset Explorer notebook
- [ ] D5 — ⭐ Sklearn classifier with held-out test metrics
- [ ] D6 — PyTorch training loop, by hand
- [ ] D7 — Tokenizer comparison + spaCy NER
- [ ] D8 — ⭐ Attention by hand (paper → NumPy)
- [ ] D9 — ⭐ Inspect a real transformer's layers
- [ ] D11 — LLM Parameter Explorer
- [ ] D12 — Chatbot with memory
- [ ] D13 — Prompt lab, 5 styles measured
- [ ] D14 — Resume → JSON extractor + tests
- [ ] D16 — Chat history store (SQLite + MongoDB)
- [ ] D17 — Doc loader + Streamlit chat UI
- [ ] D18 — Embedding playground
- [ ] D19 — Vector search tuned & measured
- [ ] D20 — ⭐ First working RAG
- [ ] D21 — Chunking lab: 4 strategies compared
- [ ] D22 — ⭐ AskMyDocs (citations + grounding guard)
- [ ] D23 — ⭐ Golden dataset + eval harness
- [ ] D24 — Hybrid search + reranking
- [ ] D25 — Production RAG on pgvector
- [ ] D27 — Tool Box (4 tools + dispatcher)
- [ ] D28 — ⭐ Hand-built ReAct agent
- [ ] D29 — Agent with real memory
- [ ] D30 — Multi-step research agent
- [ ] D31 — Voice agent
- [ ] D32 — LCEL chains
- [ ] D33 — RAG rebuilt in LangChain
- [ ] D34 — First LangGraph with a cycle
- [ ] D35 — Self-correcting RAG graph
- [ ] D36 — ⭐ Supervisor + subagents
- [ ] D37 — MCP server for my RAG
- [ ] D38 — Full eval suite
- [ ] D39 — RAG agent as a tested API
- [ ] D40 — Containerised stack

## Integration projects
- [ ] D4 — Data Cleaning Tool
- [ ] D15 — Smart CLI Assistant

## 🏆 Capstones
- [ ] **D26 — CAPSTONE 1: DocBrain** (RAG by hand, zero frameworks)
- [ ] **D41 — CAPSTONE 2: ScreenerAI-Lite** (LangGraph multi-agent)

## Mock interviews
- [ ] #1 Day 4 · [ ] #2 Day 10 · [ ] #3 Day 15 · [ ] #4 Day 26
- [ ] #5 Day 37 · [ ] #6 #7 #8 Day 42

**Target: ≥70% by Day 26 · ≥80% by Day 42**

---

# 🎯 THE CORE-SIX GATE — Day 42, no notes

- [ ] Draw the RAG pipeline and explain every box
- [ ] Explain **attention** and why transformers beat RNNs
- [ ] Explain what an **embedding** is and how similarity search works
- [ ] Write a **ReAct loop** from a blank file
- [ ] Explain **when to use an agent — and when not to**
- [ ] Explain what **LangChain and LangGraph** each give you, and when they're overkill
- [ ] Explain **when a subagent helps** and when it just adds cost
- [ ] Answer *"how do you know your RAG is good?"* with real metrics
- [ ] Answer *"RAG vs fine-tuning vs long context?"*
- [ ] Explain **chunk size and overlap** choices with numbers

---

# 🚩 DEFERRED — weeks 7–9 (`01_TOPICS/_later/`)

- [ ] **AWS · Deployment · CI/CD** (15 hrs) — right after Day 42, highest value
- [ ] **System Design for AI** (11 hrs) — week 7–8, before serious interviewing
- [ ] **Observability / Langfuse** (3 hrs) — week 7
- [ ] **Security & AI Security** (6 hrs) — week 8 *(basics stay in Days 1, 13, 16)*
- [ ] **Production AI Patterns** (4 hrs) — week 8
- [ ] **Fine-tuning / LoRA hands-on** (1 hr) — week 9+ *(the concept stays in Day 25)*

---

# RUNNING IN PARALLEL (on my own)

## DSA — needed at ~half of companies
- [ ] Arrays · [ ] Strings · [ ] Hashmaps · [ ] Two pointers
- [ ] Sliding window · [ ] Binary search · [ ] Recursion
- [ ] Trees · [ ] BFS/DFS · [ ] Basic DP
- [ ] ~100 LeetCode easy/medium completed
