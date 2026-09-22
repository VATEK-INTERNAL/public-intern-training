# AI Engineer Intern — Programme Guide
### VATEK Internship Program 2026 · AI Research & Development Department

| | |
|---|---|
| **Position** | AI Engineer Intern |
| **Plan scope** | Month 1 — Ramp-up & Foundation phase of the 3–6 month internship |
| **Format** | Full-time, onsite · **20 working days (4 weeks)** · ~8h/day |
| **Structure** | Week 1 fundamentals → Weeks 2–4 capstone project → Demo Day |
| **Exit state** | Intern is deployable onto a real client project as a supervised contributor |
| **Daily plans** | One track file per language in this folder — see §2 |

---

## 1. Programme at a Glance

| Week | Days | Theme | What the intern produces | Checkpoint |
|---|---|---|---|---|
| **1** | D1–D5 | **Core Python, API & LLM fundamentals** | 5 graded labs (L1–L5), ending in a working order-policy search | **Gate 1** |
| **2** | D6–D10 | **Capstone starts — LLM service & ingestion** | FastAPI service with cost logging, purchase-order → `Order` extraction, streaming; the order corpus in a vector DB | **Gate 2** |
| **3** | D11–D15 | **RAG, agents & MCP** | Cited answers to order questions, an eval baseline, an order agent with tools, an MCP server, guardrails | **Gate 3** |
| **4** | D16–D20 | **Evaluation, cost, latency & ship** | 50-question eval harness in CI, a measured improvement, a cost/latency pass, chat UI | **Demo Day** |

**The capstone project begins on Day 6 and runs to Day 20.** Week 1 is fundamentals and labs only — it is not project time, and its labs live in a separate folder.

---

## 2. How This Folder Works

| File | For | Contents |
|---|---|---|
| `README.md` *(this file)* | Mentors and team lead; interns read §1–§6 once on Day 1 | Programme overview, stack and cost discipline, OrderHub AI Assistant spec, gates, grading, mentor playbook, full reference library |
| [`Python.md`](Python.md) | AI Engineer interns | Day-by-day plan, weekly reading, weekly deliverables |

Every AI intern builds the same OrderHub AI Assistant (§5) against the same corpus and is graded on the same scorecard (§9).

**How to use these documents**

- These documents are a **read-only reference** — requirements, exercises, stack and reading. Interns do not copy, fork or edit anything in this folder.
- Each intern builds everything — the Week 1 labs and the capstone — in **their own repository**, following their track file.
- Each week in a track file ends with the **deliverables** that must exist in the intern's own repository by that Friday's gate. The mentor reviews the intern's repository and pull requests against that list at the gate (§7).

---

## 3. Why This Plan Exists

The JD is explicit: **this is not a theory-research position.** The intern writes Python, calls LLM APIs, builds RAG pipelines and agents, measures output quality, and ships to production. This plan builds that path in order, on one real system — and, like the Backend and Frontend plans, every exercise lives in the **Order** domain: order data, purchase orders, order policies and order actions.

Two disciplines separate an AI engineer from someone who can call an LLM API: **evaluation** (proving the system got better) and **cost/latency control** (making it affordable). Both are built in from Week 1, not bolted on at the end.

### 3.1 Outcomes — what the intern can do on Day 20

1. Write **idiomatic, typed, async Python** with proper packaging, linting and tests.
2. Build a **production-shaped FastAPI service** with dependency injection, streaming, error handling, retries and Docker packaging.
3. Explain **how an LLM works** at the level the job needs: tokenization, embeddings, context window, sampling, why hallucination happens.
4. Write **structured prompts** and get **schema-validated JSON** back reliably.
5. Build a complete **RAG pipeline**: ingest → parse → chunk → embed → store → hybrid retrieve → rerank → answer with citations.
6. Operate a **vector database** (Qdrant / pgvector): collections, payload filters, ANN tuning.
7. Build an **AI agent** with tool calling, multi-step reasoning, memory and loop-safety limits.
8. Build and consume an **MCP server** connecting an LLM to internal systems.
9. Build an **evaluation harness** with a golden set, retrieval metrics and an LLM judge — and use it to prove an improvement.
10. Measure and reduce **token cost and latency**: model tiering, prompt caching, effort tuning, batching.
11. Design **guardrails**: prompt-injection defence, PII redaction, grounding checks, human confirmation on write actions.

---

## 4. Stack & Environment

**Language & service:** Python 3.12 · FastAPI · Pydantic v2 · uv (or Poetry) · Ruff · pytest · Docker & Docker Compose.

**LLM:** the **Anthropic API via the official `anthropic` Python SDK** is the primary provider. `claude-opus-5` is the default working model; `claude-sonnet-5` and `claude-haiku-4-5` are the cheaper tiers used deliberately in the Week 4 cost work. Interns read one competing provider's docs in Week 4 to understand portability, but all capstone code targets the Anthropic SDK.

**Retrieval:** Qdrant (primary, via Docker) · pgvector on PostgreSQL (alternative) · an embedding model · BM25 for hybrid search · a cross-encoder or LLM reranker.

**Supporting:** MCP Python SDK · Streamlit (chat UI) · Git + GitHub PR flow · GitHub Actions · an AI coding assistant.

### 4.1 API-key and cost discipline (Day 1, non-negotiable)

- Keys live in `.env`, git-ignored from the first commit. `.env.example` is committed.
- Every intern gets a **budget cap** and a **daily spend limit** set by the mentor.
- From Week 2 onward, every LLM call in the capstone logs `model`, `input_tokens`, `output_tokens`, `cache_read_tokens`, `latency_ms` and estimated cost. **Untracked calls are a code-review rejection.**
- Never send credentials, PII or client data to any AI service — including the coding assistant. Understand the difference between an enterprise API's data policy and a consumer chat product's.

> **Track file:** there is one track for this role — [`Python.md`](Python.md).

---

## 5. The Capstone Project — "OrderHub AI Assistant" (starts Day 6)

> **OrderHub AI Assistant** — an AI assistant for the order-operations team of the same OrderHub platform the Backend and Frontend interns build. It turns messy purchase orders into structured orders, answers order-policy questions **with citations**, refuses when it has no evidence, and performs order actions through tools — with a human confirmation before anything is written.

**Three order jobs, one system:**

| Job | Example | Built on |
|---|---|---|
| **Extract** | A customer emails a scanned purchase order → the assistant returns a validated `Order` JSON (customer, lines, quantities, unit prices, requested delivery date) ready to be created in OrderHub | D7 |
| **Answer** | *"Can a B2B customer cancel an order after it is PAID?"* → a cited answer from the current cancellation policy — or an explicit refusal if the corpus does not say | D11–D12 |
| **Act** | *"Where is ORD-2026-0142? Open a return for the damaged line."* → `lookup_order`, then `create_return_request` only after the user confirms | D13–D15 |

### 5.1 Components

| # | Component | Built on | What it does |
|---|---|---|---|
| 1 | **LLM service** | D6–D8 | FastAPI endpoints with cost logging, **purchase-order → `Order` extraction** with structured outputs, SSE streaming chat, retries and prompt caching |
| 2 | **Ingestion pipeline** | D9 | Parses order policies, SOPs, contracts, catalog sheets and purchase orders (PDF, DOCX, HTML, Markdown); cleans; extracts metadata (`doc_type`, customer, effective date); chunks |
| 3 | **Vector store** | D10 | Qdrant collection with payload filters on `doc_type`, customer and effective date; idempotent upsert |
| 4 | **Retrieval layer** | D11 | Hybrid search (vector + BM25 — essential for exact order codes and SKUs), metadata filtering, reranking, citation assembly |
| 5 | **Answer service** | D12 | Grounded, cited answers to order questions; **refuses when evidence is insufficient**; resolves policy versions by effective date; eval baseline committed |
| 6 | **Order agent** | D13–D14 | Tools `search_order_docs`, `lookup_order`, `list_orders_by_customer`, `create_return_request`, `get_today`; multi-step reasoning; conversation memory; step and budget limits |
| 7 | **MCP server** | D14 | Exposes `lookup_order` and `search_order_docs` over Model Context Protocol so any MCP client — including Claude Code — can query orders |
| 8 | **Guardrails** | D15 | Prompt-injection defence (including instructions hidden inside customer PO emails), redaction of customer contact data, output validation, human confirmation before `create_return_request` |
| 9 | **Evaluation harness** | D16–D17 | 50 order questions (recall@k, MRR, faithfulness and correctness via an LLM judge) **plus** field-level extraction accuracy on 20 held-out purchase orders; runs in CI |
| 10 | **Chat UI & deployment** | D19 | Streamlit order-ops chat with streaming, visible citations, a PO upload box and a per-message cost readout; full Docker Compose |

### 5.2 The corpus and order data (provided Day 1)

~200 order-operations documents, deliberately messy:

- **Policies & SOPs** — order cancellation, returns & refunds, shipping SLAs, payment terms, discount rules. **Two versions of the refund policy contradict each other** (old vs current effective date); resolving that correctly is part of the assessment.
- **Customer contracts** with special order terms that override the standard policy.
- **Product catalog & price sheets** — the SKUs that order lines reference.
- **~60 purchase orders** as PDF (some scanned), DOCX and HTML emails — 40 for building, **20 held out** for extraction evaluation.
- **A synthetic orders database** on the same OrderHub schema as the Backend track (`orders`, `order_items`, `payments`) — the data behind `lookup_order`.

### 5.3 Non-functional requirements (graded on Day 20)

- `docker compose up` brings up API + Qdrant + UI from a clean clone, with no manual steps.
- Every policy answer carries citations resolvable back to a source document and chunk.
- An out-of-corpus order question produces an explicit refusal, **never** a fabricated policy.
- Purchase-order extraction reaches **≥ 90% field-level accuracy** on the 20 held-out POs and never invents a line item.
- `create_return_request` never executes without explicit user confirmation.
- Evaluation runs with one command and prints a scored report.
- p95 end-to-end latency for a typical question: **< 8 s** streaming, time-to-first-token **< 2 s**.
- Cost per answered question and per extracted PO is measured and reported.

### 5.4 Stretch backlog (only after the required scope is green)

OCR for scanned POs · creating the extracted order in a Backend intern's OrderHub API (with confirmation) · parent–child chunking · query rewriting and multi-query retrieval · contract-override reasoning across customers · a local model comparison via Ollama · a LangChain/LlamaIndex reimplementation for contrast.

---

## 6. Daily Rhythm

| Time | Activity |
|---|---|
| 09:00 – 09:15 | Standup: yesterday / today / blockers |
| 09:15 – 12:00 | Focused block (Week 1: concepts · Weeks 2–4: build) |
| 13:00 – 16:30 | Build block 2 · pairing / mentor review window |
| 16:30 – 17:15 | Self-review, push PR, write the daily log |
| 17:15 – 17:30 | Paper or documentation reading · AI-usage journal entry |

**Fixed weekly events:** Tue & Thu 60-min pairing · Wed 45-min **paper/doc share** (the intern presents one paper, doc or model release) · Fri 45-min **Gate Review** · Fri 15-min 1:1.

**Code review SLA:** the mentor responds to any PR within 4 working hours. Nothing merges without one approval.

---

## 7. Weekly Checkpoints — Gates & Demo Day

Day-by-day content, tools and reading live in the track files. This section is the mentor's reference for what each checkpoint verifies. Every gate is a live demo **plus** a verbal defence: passing requires answering without reading the code.

### 7.1 Week 1 labs (shared by every track)

| Day | Lab | Done when |
|---|---|---|
| **D1** | **L1 — Order document toolkit.** A typed CLI that walks a folder of order emails and notes, normalizes the text, extracts every order code (`ORD-YYYY-NNNN`) with a regex, and writes a JSON report of which documents mention which orders | Runs from the CLI; Ruff clean; type checker clean; a debugger breakpoint stepped through with the mentor |
| **D2** | **L2 — Async order fetcher.** Fetch 50 orders concurrently from a mock OrderHub API with a semaphore, per-request timeout and retry with backoff; validate each into a Pydantic `Order` model (lines, totals, status) | The concurrent version is measurably faster than sequential; a hung request does not stall the batch; malformed orders fail validation and are logged, not swallowed |
| **D3** | **L3 — Purchase-order parser.** Extract clean text + metadata (customer, PO number, date) from 20 mixed PDF/DOCX/HTML purchase orders, including two that are broken | Every file either parses or is explicitly reported as unsupported — silent drops are a defect |
| **D4** | **L4 — Purchase-order API.** Wrap L3 as a REST service: `POST /purchase-orders` (upload + parse), `GET /purchase-orders`, `GET /purchase-orders/{id}`, with validation, a global error handler and 6 tests. Dockerized | `docker compose up` serves it; Swagger UI is usable; tests pass inside the container |
| **D5** | **L5 — Naive order-policy search.** Embed a small set of order policies (cancellation, returns, refunds, shipping) plus the L3 purchase orders in memory, implement cosine similarity by hand, and answer queries such as *"refund rule for damaged goods"* with top-k results. Write `labs/where-this-breaks.md` | Search returns sensible results; the note correctly predicts that exact order codes (`ORD-2026-0142`) are where pure vector search fails — the case for hybrid search in Week 3 |

### 7.2 Gate 1 — end of Week 1 (D5)

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L2, L4 and L5. The mentor asks: *"One of the 50 order requests hangs forever. Trace exactly what happens in your code and where you decided that behaviour."* and *"Why do two identical calls to the same model return different text, and what would you do if a caller needed determinism?"* Passing requires answering without reading the code.

### 7.3 Gate 2 — end of Week 2 (D10)

**🚩 Gate 2 — Friday, 45 min.** Demo purchase-order extraction on an unseen PO, streaming chat and a filtered search (e.g. current refund policies only), then open the log and walk through one request's token and cost breakdown. The mentor asks: *"This prompt costs X per call. Show me two ways to make it cheaper without losing quality, and which you'd try first."*

### 7.4 Gate 3 — end of Week 3 (D15)

**🚩 Gate 3 — Friday, 45 min.** The mentor asks three unseen order questions — one clearly answerable, one that depends on the contradictory refund-policy versions, one entirely out of corpus — then submits a purchase order carrying a hidden prompt-injection instruction. Then: *"Show me a question your system gets wrong, and tell me whether it's a retrieval problem or a generation problem — and how you know."*

### 7.5 Demo Day agenda — end of Week 4 (D20)

| Minutes | Segment |
|---:|---|
| 0–2 | The problem and the architecture diagram: ingest → retrieve → rerank → answer → agent |
| 2–7 | Live: upload an unseen purchase order and show the extracted `Order` JSON; then three order questions — one answerable, one from the contradictory refund policies, one out of corpus (the refusal) |
| 7–10 | The order agent and MCP: *"Where is ORD-…? Open a return"* with the confirmation step, then the mentor's machine looking up an order through your MCP server |
| 10–14 | **Evaluation:** baseline vs final scores, what the failure taxonomy revealed, what you fixed |
| 14–17 | **Cost & latency:** before/after numbers, which levers you pulled and why in that order |
| 17–20 | Guardrails: a PO email with a hidden injection, blocked · **AI usage retrospective:** where AI was confidently wrong and what you now verify by default |
| +10 | Mentor and team questions |

---

## 8. AI Adoption — Mandatory Standards

For this role the JD is direct: **AI is not a support tool, it is the core competency.** These standards are enforced from Day 1.

### 8.1 Required practice

- Use one coding assistant daily (Claude Code / Cursor / GitHub Copilot or equivalent).
- **Working-level prompt engineering:** structure a system prompt, use few-shot examples, decompose a task, specify an output schema, and debug systematically when the model returns something wrong.
- Use AI to accelerate your own work: generate test datasets, write eval scripts, analyse inference logs, summarize papers and documentation, compare architecture options.
- **Understand the limits and design for them:** hallucination, non-determinism, context limits, prompt injection. Every limit needs a matching guardrail in the system you build.
- **Cost and performance awareness:** pick the model that fits the task, trim tokens, cache aggressively, and balance quality against latency consciously rather than by accident.
- Stay current and share: the Wednesday paper/doc share is mandatory.

### 8.2 Hard rules — a violation blocks the merge

> **Never** send credentials, PII or client data to any AI service, including your coding assistant.
> **Never** trust model output without verification. Every generated fact in the capstone must be traceable to a retrieved source.
> **Never** commit code you cannot explain. Mentors will ask at random, in review.
> **Never** commit an untracked LLM call — every call logs model, tokens, latency and cost.
> **Always** disclose AI-generated sections in the PR description.

### 8.3 Graded artefact

`docs/ai-journal.md` — maintained from D6 to D20, reviewed at Gates 2 and 3 and at Demo Day. **Minimum 12 entries.** Each entry names a task, the prompt approach, **the failure mode observed**, and the fix. For this role, entries documenting *how the model failed* are worth more than entries documenting what it produced.

---

## 9. Evaluation

### 9.1 Scorecard (100 points)

| # | Criterion | Weight | What "excellent" looks like |
|---|---|---:|---|
| 1 | **Core Python & API fundamentals** (Week 1 labs) | 10 | All five labs complete; clean typed async Python; can explain non-determinism and tokenization unaided |
| 2 | **RAG pipeline quality** | 18 | Robust ingestion, well-reasoned chunking, hybrid retrieval + rerank, accurate citations, correct refusals |
| 3 | **Agent, tool calling & MCP** | 18 | Reliable multi-step agent with loop safety; a working MCP server another machine can call |
| 4 | **Evaluation rigor** | 14 | Human-verified golden set, calibrated judge, measured improvement over a committed baseline, field-level PO extraction accuracy, eval in CI |
| 5 | **Guardrails & safety** | 10 | Injection suite passing, PII redaction, grounding checks, confirmation before write actions |
| 6 | **Code quality, API design & testing** | 12 | Clean FastAPI service, typed models, meaningful tests, green CI, one-command startup |
| 7 | **Cost & latency engineering** | 8 | Measured before/after; deliberate model tiering and caching; quality held |
| 8 | **AI adoption & critical thinking** | 5 | Strong journal centred on observed failure modes |
| 9 | **Communication, docs & tech talk** | 5 | Clear technical report; a tech talk the team actually learns from |

### 9.2 Decision bands

| Score | Outcome |
|---|---|
| **≥ 85** | **Strong pass.** Assign to a client AI project as a contributor; open the Fresher conversation early. |
| **70 – 84** | **Pass.** Continue to month 2 on a real project with normal supervision. |
| **55 – 69** | **Conditional.** A two-week targeted remediation plan on the weakest two criteria, then re-assess. |
| **< 55** | **Not passing.** Structured feedback and closure of the internship. |

**Non-negotiable minimums, regardless of total score:** no key or PII in Git · the system refuses out-of-corpus questions instead of fabricating · no order action runs without user confirmation · the eval runs and reports · the intern can explain any randomly selected file they committed.

---

## 10. Mentor Playbook

| Cadence | Duration | Purpose |
|---|---|---|
| Daily standup | 15 min | Unblock — not status theatre |
| PR review | ≤ 4h SLA | Teach through comments; ask for a rewrite rather than fixing it yourself |
| Pairing (Tue, Thu) | 60 min | Intern drives, mentor navigates |
| Paper/doc share (Wed) | 45 min | Intern presents; builds the habit of tracking a fast-moving field |
| Gate review (Fri) | 45 min | Demo + adversarial questioning + written feedback |
| 1:1 (Fri) | 15 min | Wellbeing, motivation, budget check, career direction |

**Mentor preparation before Day 1:** the Week 1 lab documents (order emails for L1, 20 purchase orders for L3 — the L2 mock OrderHub API is `db.json` in [`datasets/`](../datasets/README.md), served by `json-server`) · the ~200-document order-operations corpus with its deliberate refund-policy contradiction · ~60 purchase orders, with hand-labelled `Order` JSON for the 20 held out · the orders database for `lookup_order`, seeded from `orders.json` in [`datasets/`](../datasets/README.md) · a scoped API key with a hard budget cap · 15 human-labelled examples for judge calibration · a GitHub repo template with CI · a Qdrant container image ready to pull.

**Escalate to the team lead the same day if:**

- The intern is more than 2 days behind the plan at any gate.
- Spend exceeds 70% of the monthly budget before D15.
- The intern accepts model output without verification twice in one week.
- Any hard AI rule in §8.2 is violated.

**The mentor's most important habit:** at every gate, ask the intern to show you a case where **their own system is wrong** — and to explain why. An AI engineer who can only demo the happy path is not yet an AI engineer.

---

## 11. Reference Library

> Every link points to primary/official documentation. Documentation sites reorganize: if a URL 404s, search the same official domain rather than following a third-party mirror or a dated blog post. AI documentation changes faster than any other area in this programme — treat a six-month-old blog post as a hypothesis, not a fact.

### 11.1 Python language & tooling

[Python tutorial](https://docs.python.org/3/tutorial/) · [Language reference](https://docs.python.org/3/reference/) · [typing](https://docs.python.org/3/library/typing.html) · [dataclasses](https://docs.python.org/3/library/dataclasses.html) · [asyncio](https://docs.python.org/3/library/asyncio.html) · [logging](https://docs.python.org/3/library/logging.html) · [uv](https://docs.astral.sh/uv/) · [Ruff](https://docs.astral.sh/ruff/) · [pytest](https://docs.pytest.org/en/stable/) · [Python packaging guide](https://packaging.python.org/en/latest/)

### 11.2 API service

[FastAPI](https://fastapi.tiangolo.com/) · [FastAPI dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/) · [FastAPI testing](https://fastapi.tiangolo.com/tutorial/testing/) · [Flask](https://flask.palletsprojects.com/en/stable/) · [Pydantic v2](https://pydantic.dev/docs/validation/latest/get-started/) · [httpx](https://www.python-httpx.org/) · [Uvicorn](https://uvicorn.dev/) · [Docker](https://docs.docker.com/) · [Docker Compose](https://docs.docker.com/compose/)

### 11.3 LLM & the Anthropic API

[Anthropic documentation](https://platform.claude.com/docs/en/home) · [Messages API](https://platform.claude.com/docs/en/api/messages) · [Python SDK](https://github.com/anthropics/anthropic-sdk-python) · [Prompt engineering](https://platform.claude.com/docs/en/build-with-claude/prompt-engineering/overview) · [Structured outputs](https://platform.claude.com/docs/en/build-with-claude/structured-outputs) · [Streaming](https://platform.claude.com/docs/en/build-with-claude/streaming) · [Prompt caching](https://platform.claude.com/docs/en/build-with-claude/prompt-caching) · [Token counting](https://platform.claude.com/docs/en/build-with-claude/token-counting) · [Batch processing](https://platform.claude.com/docs/en/build-with-claude/batch-processing) · [Pricing](https://claude.com/pricing) · [Claude Cookbooks](https://github.com/anthropics/claude-cookbooks)

### 11.4 Agents, tools & MCP

[Tool use overview](https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview) · [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) · [Model Context Protocol](https://modelcontextprotocol.io/) · [MCP specification](https://modelcontextprotocol.io/specification) · [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) · [Claude Agent SDK](https://code.claude.com/docs/en/agent-sdk)

### 11.5 Retrieval & vector databases

[Qdrant](https://qdrant.tech/documentation/) · [Qdrant hybrid queries](https://qdrant.tech/documentation/search/hybrid-queries/) · [pgvector](https://github.com/pgvector/pgvector) · [Chroma](https://docs.trychroma.com/docs/overview/introduction) · [Pinecone](https://docs.pinecone.io/guides/get-started/overview) · [Weaviate](https://docs.weaviate.io/weaviate) · [Sentence-Transformers](https://sbert.net/) · [Cross-encoder reranking](https://sbert.net/examples/cross_encoder/applications/README.html) · [Okapi BM25](https://en.wikipedia.org/wiki/Okapi_BM25)

### 11.6 Frameworks (read to understand the abstractions — build without them first)

[LangChain](https://docs.langchain.com/oss/python/langchain/overview) · [LangGraph](https://langchain-ai.github.io/langgraph/) · [LlamaIndex](https://developers.llamaindex.ai/python/framework/)

### 11.7 Evaluation & observability

[Creating strong empirical evaluations](https://platform.claude.com/docs/en/test-and-evaluate/develop-tests) · [Reduce hallucinations](https://platform.claude.com/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) · [RAGAS](https://docs.ragas.io/en/stable/) · [DeepEval](https://github.com/confident-ai/deepeval) · [LangSmith](https://docs.langchain.com/langsmith/observability)

### 11.8 Data handling & storage

[pandas](https://pandas.pydata.org/docs/) · [Polars](https://docs.pola.rs/) · [pypdf](https://pypdf.readthedocs.io/en/stable/) · [python-docx](https://python-docx.readthedocs.io/en/latest/) · [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) · [Tesseract OCR](https://tesseract-ocr.github.io/tessdoc/) · [PostgreSQL](https://www.postgresql.org/docs/current/) · [Apache Airflow](https://airflow.apache.org/docs/)

### 11.9 ML foundations & local models

[Hugging Face LLM course](https://huggingface.co/learn/llm-course/chapter1/1) · [Hugging Face docs](https://huggingface.co/docs) · [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) · [scikit-learn](https://scikit-learn.org/stable/) · [PyTorch](https://docs.pytorch.org/docs/stable/index.html) · [Ollama](https://github.com/ollama/ollama) · [vLLM](https://docs.vllm.ai/en/latest/)

### 11.10 Security & practice

[OWASP Top 10 for LLM Applications](https://owasp.org/projects/top-10-for-large-language-model-applications) · [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/) · [The Twelve-Factor App](https://12factor.net/) · [Git documentation](https://git-scm.com/doc) · [GitHub Actions](https://docs.github.com/en/actions) · [Conventional Commits](https://www.conventionalcommits.org/) · [Streamlit](https://docs.streamlit.io/)

> **Deliberate constraint for Weeks 2–3:** build the RAG pipeline **without** a framework. An intern who starts with LangChain learns the framework; an intern who starts with the SDK learns retrieval. Frameworks appear in the §5.4 stretch backlog as a comparison, not as a foundation.

---

## 12. Appendix A — Definition of Done (every PR)

- Branch named `feat/…`, `fix/…` or `chore/…`; commits follow Conventional Commits
- PR description states: what, why, how to test, and **which parts were AI-generated**
- No key, no PII, no client data in the diff or in Git history
- Every new LLM call logs model, token counts, latency and estimated cost
- Tests added or updated; CI green
- If the change could affect answer quality, the eval was re-run and the delta is in the PR

---

## 13. Appendix B — Programme Calendar

| Week | Days | Theme | Checkpoint |
|---|---|---|---|
| 1 | D1–D5 | Core Python, API & LLM fundamentals | Gate 1 |
| 2 | D6–D10 | Capstone starts — LLM service & ingestion | Gate 2 |
| 3 | D11–D15 | RAG, agents & MCP | Gate 3 |
| 4 | D16–D20 | Evaluation, cost, latency & ship | **Demo Day** |

---

## 14. Appendix C — Risks, Remediation & the Scope-Cut Ladder

### 14.1 Risks

| Risk | Early signal | Mentor action |
|---|---|---|
| Weak Python or async fundamentals | L2 or L4 incomplete | Compress D3, move L3 to homework; protect D4–D5 |
| Week 1 feels too easy for a strong intern | L1–L3 finished by lunch | Skip to L4/L5 and add a stretch lab: implement BM25 by hand and compare it to embeddings on the same queries |
| Corpus parsing eats the week | D9 incomplete | Provide pre-parsed text for 100 documents; the intern parses the remaining messy 100 |
| Budget burned by careless agent loops | Spend spikes on D13–D14 | Enforce the step limit and budget cap in code before any further agent work |
| RAG "works" but is never measured | No baseline committed by D12 | Stop feature work; the eval baseline is a hard gate, not a Week 4 task |
| Chasing frameworks instead of understanding | Reaches for LangChain on D9 | Enforce the §11 constraint; frameworks are stretch scope only |
| Over-reliance on AI, shallow understanding | Cannot explain the agent loop at Gate 3 | One mandatory AI-free day; the intern rebuilds the loop unaided |
| Eval set contaminated by AI-generated ground truth | Suspiciously high baseline scores | Re-verify the golden set against source documents by hand before any further tuning |

### 14.2 Scope-cut ladder — drop in this order if the intern falls behind

1. The Streamlit chat UI (D19) — a working `curl` demo is acceptable at Demo Day.
2. Conversation memory (D14) — keep single-turn agent calls and the MCP server.
3. The reranker (D11) — keep hybrid search.
4. The fourth agent tool (D13) — three tools are enough to demonstrate the loop.
5. The cost/latency optimization pass (D18) — but **still report the measurements**, even without the optimization.

**Never cut:** the eval baseline (D12) · the eval harness (D16–D17) · the MCP server (D14) · the guardrails suite (D15) · the refusal behaviour. Those carry 60 of the 100 scorecard points and are explicit JD requirements.

---

*Prepared for the VATEK Internship Program 2026 · AI Engineer Intern · Month 1 of 3–6.*

---

*VATEK Internship Program 2026 · AI Engineer Intern · Programme Guide · Month 1 of 3–6.*
