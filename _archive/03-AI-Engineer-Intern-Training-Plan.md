# AI Engineer Intern — 1-Month Training Plan
### VATEK Internship Program 2026 · AI Research & Development Department

| | |
|---|---|
| **Position** | AI Engineer Intern |
| **Plan scope** | Month 1 — Ramp-up & Foundation phase of the 3–6 month internship |
| **Format** | Full-time, onsite · **20 working days (4 weeks)** · ~8h/day |
| **Structure** | Week 1 fundamentals → Weeks 2–4 capstone project → Demo Day |
| **Exit state** | Intern is deployable onto a real client AI project as a supervised contributor |

---

## 1. Programme at a Glance

| Week | Days | Theme | What the intern produces | Checkpoint |
|---|---|---|---|---|
| **1** | D1–D5 | **Core Python, API & LLM fundamentals** | 5 graded labs (L1–L5), ending in a working semantic search | **Gate 1** |
| **2** | D6–D10 | **Capstone starts — LLM service & ingestion** | FastAPI service with cost logging, structured outputs, streaming; corpus in a vector DB | **Gate 2** |
| **3** | D11–D15 | **RAG, agents & MCP** | Grounded answers with citations, an eval baseline, a tool-calling agent, an MCP server, guardrails | **Gate 3** |
| **4** | D16–D20 | **Evaluation, cost, latency & ship** | 50-question eval harness in CI, a measured improvement, a cost/latency pass, chat UI | **Demo Day** |

**The capstone project begins on Day 6 and runs to Day 20.** Week 1 is fundamentals and labs only — it is not project time, and its labs live in a separate folder.

---

## 2. Why This Plan Exists

The JD is explicit: **this is not a theory-research position.** The intern writes Python, calls LLM APIs, builds RAG pipelines and agents, measures output quality, and ships to production. This plan builds that path in order, on one real system.

Two disciplines separate an AI engineer from someone who can call an LLM API: **evaluation** (proving the system got better) and **cost/latency control** (making it affordable). Both are built in from Week 1, not bolted on at the end.

### 2.1 Outcomes — what the intern can do on Day 20

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

## 3. Stack & Environment

**Language & service:** Python 3.12 · FastAPI · Pydantic v2 · uv (or Poetry) · Ruff · pytest · Docker & Docker Compose.

**LLM:** the **Anthropic API via the official `anthropic` Python SDK** is the primary provider. `claude-opus-5` is the default working model; `claude-sonnet-5` and `claude-haiku-4-5` are the cheaper tiers used deliberately in the Week 4 cost work. Interns read one competing provider's docs in Week 4 to understand portability, but all capstone code targets the Anthropic SDK.

**Retrieval:** Qdrant (primary, via Docker) · pgvector on PostgreSQL (alternative) · an embedding model · BM25 for hybrid search · a cross-encoder or LLM reranker.

**Supporting:** MCP Python SDK · Streamlit (chat UI) · Git + GitHub PR flow · GitHub Actions · an AI coding assistant.

### 3.1 API-key and cost discipline (Day 1, non-negotiable)

- Keys live in `.env`, git-ignored from the first commit. `.env.example` is committed.
- Every intern gets a **budget cap** and a **daily spend limit** set by the mentor.
- From Week 2 onward, every LLM call in the capstone logs `model`, `input_tokens`, `output_tokens`, `cache_read_tokens`, `latency_ms` and estimated cost. **Untracked calls are a code-review rejection.**
- Never send credentials, PII or client data to any AI service — including the coding assistant. Understand the difference between an enterprise API's data policy and a consumer chat product's.

---

## 4. Week 1 — Core Python, API & LLM Fundamentals

**Theme:** Own the language and the mental model before touching retrieval. Mornings are concepts and reading; afternoons are graded micro-exercises (**labs**) in a `/labs` folder, kept separate from the capstone repository.

| Day | Concept block | Lab (afternoon) | Done when |
|---|---|---|---|
| **D1** | **Python core:** type hints, dataclasses, comprehensions, iterators & generators, context managers, decorators, exception design, modules & packaging, virtual environments, `uv`, Ruff, the debugger | **L1 — Document toolkit.** A typed CLI that walks a folder, reads text files, normalizes and word-counts them, and writes a JSON report | Runs from the CLI; Ruff clean; type checker clean; a debugger breakpoint stepped through with the mentor |
| **D2** | **Python advanced:** `async`/`await`, `asyncio`, `TaskGroup`, concurrency vs parallelism, timeouts and cancellation, **Pydantic v2** models and validators, structured `logging` | **L2 — Async fetcher.** Fetch 50 URLs concurrently with a semaphore, per-request timeout, retry with backoff, results validated into Pydantic models | The concurrent version is measurably faster than sequential; a hung URL does not stall the batch; failures are logged, not swallowed |
| **D3** | **Data handling:** pandas basics, JSON/CSV, parsing **PDF, DOCX, HTML**, text cleaning and normalization, regex, encoding pitfalls; SQL refresher (joins, aggregation, indexes) | **L3 — Messy corpus parser.** Extract clean text + metadata from 20 mixed PDF/DOCX/HTML files, including two that are broken | Every file either parses or is explicitly reported as unsupported — silent drops are a defect |
| **D4** | **Web API core:** FastAPI routing, path/query/body params, **dependency injection**, middleware, response models, `BackgroundTasks`, streaming responses, OpenAPI, error handling; testing with pytest + `httpx`; Docker fundamentals | **L4 — Document API.** Wrap L3 as a REST service: `POST /documents` (upload + parse), `GET /documents`, `GET /documents/{id}`, with validation, a global error handler and 6 tests. Dockerized | `docker compose up` serves it; Swagger UI is usable; tests pass inside the container |
| **D5** | **LLM & ML foundations:** tokenization, **embeddings** and vector space, cosine similarity, context window, sampling (temperature, top-p) and why output is non-deterministic, transformers & attention *conceptually*, what causes hallucination, prompt-engineering primitives | **L5 — Naive semantic search.** Embed the L3 corpus in memory, implement cosine similarity by hand, return top-k for a query. Write `labs/where-this-breaks.md` | Search returns sensible results; the note correctly predicts what a real vector DB and chunking will be needed for |

### 4.1 Week 1 reference documentation

| Topic | Official documentation |
|---|---|
| Python language | [Python tutorial](https://docs.python.org/3/tutorial/) · [Language reference](https://docs.python.org/3/reference/) · [typing](https://docs.python.org/3/library/typing.html) · [dataclasses](https://docs.python.org/3/library/dataclasses.html) · [Errors & exceptions](https://docs.python.org/3/tutorial/errors.html) |
| Async Python | [asyncio](https://docs.python.org/3/library/asyncio.html) · [Coroutines & tasks](https://docs.python.org/3/library/asyncio-task.html) |
| Tooling | [uv](https://docs.astral.sh/uv/) · [Ruff](https://docs.astral.sh/ruff/) · [pytest](https://docs.pytest.org/en/stable/) · [Python packaging guide](https://packaging.python.org/en/latest/) |
| Validation | [Pydantic v2](https://docs.pydantic.dev/latest/) · [Pydantic Settings](https://docs.pydantic.dev/latest/concepts/pydantic_settings/) |
| Data handling | [pandas](https://pandas.pydata.org/docs/) · [pypdf](https://pypdf.readthedocs.io/en/stable/) · [python-docx](https://python-docx.readthedocs.io/en/latest/) · [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) · [re module](https://docs.python.org/3/library/re.html) |
| Web API | [FastAPI](https://fastapi.tiangolo.com/) · [FastAPI dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/) · [FastAPI testing](https://fastapi.tiangolo.com/tutorial/testing/) · [httpx](https://www.python-httpx.org/) · [Docker](https://docs.docker.com/) |
| LLM foundations | [Anthropic documentation](https://docs.claude.com/) · [Prompt engineering overview](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview) · [Hugging Face NLP course](https://huggingface.co/learn/nlp-course/chapter1/1) · [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) |
| Embeddings & similarity | [Sentence-Transformers](https://sbert.net/) · [Okapi BM25 (concept)](https://en.wikipedia.org/wiki/Okapi_BM25) |

**AI drill (Week 1).** Set up the assistant with a project rules file (`CLAUDE.md` / `.cursorrules`) naming the stack and conventions. Then, on every lab: **write it yourself first, then ask AI to review it**, recording which suggestions you accepted and rejected. On D5, additionally ask the assistant about a *recent* SDK parameter and observe the failure mode — a confident, wrong answer. **Write that observation down.** Understanding *how* a model is wrong is this role's core skill, and it starts on day one.

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L2, L4 and L5. The mentor asks: *"One of the 50 URLs hangs forever. Trace exactly what happens in your code and where you decided that behaviour."* and *"Why do two identical calls to the same model return different text, and what would you do if a caller needed determinism?"* Passing requires answering without reading the code.

---

## 5. The Capstone Project — "VATEK Knowledge Agent" (starts Day 6)

> **VATEK Knowledge Agent** — an internal assistant over the company's own documents. Staff ask questions in natural language; the system answers **with citations**, refuses when it has no evidence, and can take limited actions on internal systems through tools.

### 5.1 Components

| # | Component | Built on | What it does |
|---|---|---|---|
| 1 | **LLM service** | D6–D8 | FastAPI endpoints with cost logging, structured outputs, SSE streaming, retries and prompt caching |
| 2 | **Ingestion pipeline** | D9 | Parses PDF, DOCX, HTML and Markdown; cleans; extracts metadata; chunks |
| 3 | **Vector store** | D10 | Qdrant collections, embeddings, payload filters, idempotent upsert |
| 4 | **Retrieval layer** | D11 | Hybrid search (vector + BM25), metadata filtering, reranking, citation assembly |
| 5 | **Answer service** | D12 | Grounded, cited answers; **refuses when evidence is insufficient**; eval baseline committed |
| 6 | **Agent** | D13–D14 | Tool calling over `search_docs`, `lookup_order`, `create_ticket`, `get_today`; multi-step reasoning; memory; step and budget limits |
| 7 | **MCP server** | D14 | Exposes the internal tools over Model Context Protocol so any MCP client can use them |
| 8 | **Guardrails** | D15 | Prompt-injection defence, PII redaction, output validation, human confirmation before write actions |
| 9 | **Evaluation harness** | D16–D17 | 50-question golden set; recall@k and MRR; faithfulness and correctness via an LLM judge; runs in CI |
| 10 | **Chat UI & deployment** | D19 | Streamlit chat with streaming, visible citations, per-message cost readout; full Docker Compose |

### 5.2 The corpus (provided Day 1)

~200 mixed documents: internal handbooks, product specs, meeting notes, and a small synthetic order database. Deliberately messy — scanned PDFs, inconsistent headings, near-duplicate versions, and a few documents that contradict each other. **The contradictions are intentional**: handling them is part of the assessment.

### 5.3 Non-functional requirements (graded on Day 20)

- `docker compose up` brings up API + Qdrant + UI from a clean clone, with no manual steps.
- Every answer carries citations resolvable back to a source document and chunk.
- An out-of-corpus question produces an explicit refusal, **never** a fabricated answer.
- Evaluation runs with one command and prints a scored report.
- p95 end-to-end latency for a typical question: **< 8 s** streaming, time-to-first-token **< 2 s**.
- Cost per answered question is measured and reported.

### 5.4 Stretch backlog (only after the required scope is green)

Parent–child chunking · query rewriting and multi-query retrieval · conversational memory summarization · OCR for scanned PDFs · a second MCP client integration · a local model comparison via Ollama · a LangChain/LlamaIndex reimplementation for contrast.

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

## 7. Week 2 — LLM Service & Ingestion *(capstone begins)*

**Theme:** A real service that calls a real model, with tokens and cost visible from the first commit — and the corpus in a vector database by Friday.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D6** | Repository setup · Git/PR flow · **secret and cost discipline (§3.1)** · Docker · Anthropic SDK first calls · the Messages API · pricing math · `count_tokens` | FastAPI skeleton, `GET /health`, `POST /complete`, `.env.example`, **first PR merged**, token/cost logging middleware | `docker build` succeeds; no key in Git; every LLM call logs model, token counts and estimated cost |
| **D7** | Prompt engineering: system prompt structure, few-shot, task decomposition · **structured outputs** with a Pydantic schema | `POST /extract` — pulls structured fields out of messy documents and returns a schema-validated object | 20/20 test documents return valid schema; malformed model output is caught, not crashed on |
| **D8** | Streaming (SSE) · errors, retries, timeouts, rate limits · **prompt caching** · thinking and effort levels | `POST /chat` streaming endpoint + retry/backoff on 429 and 5xx + cache-hit rate in the log line | Cancelling the client stream cancels the call; cache-read tokens are non-zero on repeated prefixes |
| **D9** | Ingestion at scale: parsing PDF/DOCX/HTML/Markdown, cleaning, metadata, duplicates · **chunking strategies** (fixed, recursive, semantic, parent–child), overlap | `ingest` CLI producing clean, chunked text for all ~200 corpus documents + a written comparison of two chunking strategies on the same 10 documents | Every document parses or is explicitly logged as unsupported; the comparison shows measured differences, not opinions |
| **D10** | Embeddings in production: model choice, dimensionality, batching · **Qdrant**: collections, distance metrics, payload filters, upsert, ANN parameters | Full corpus embedded and upserted to Qdrant; `POST /search` with metadata filters | Re-running ingestion is idempotent — no duplicated chunks; embedding is batched, not one call per document |

**Reading:** [Anthropic API — Messages](https://docs.claude.com/en/api/messages) · [Anthropic Python SDK](https://github.com/anthropics/anthropic-sdk-python) · [Prompt engineering](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview) · [Structured outputs](https://docs.claude.com/en/docs/build-with-claude/structured-outputs) · [Streaming](https://docs.claude.com/en/docs/build-with-claude/streaming) · [Prompt caching](https://docs.claude.com/en/docs/build-with-claude/prompt-caching) · [Token counting](https://docs.claude.com/en/docs/build-with-claude/token-counting) · [Anthropic pricing](https://www.anthropic.com/pricing) · [Qdrant](https://qdrant.tech/documentation/) · [Sentence-Transformers](https://sbert.net/) · [MDN — Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events)

**AI drill (Week 2).** Use AI to generate the FastAPI scaffolding, Pydantic schemas and test fixtures. Start the **AI Usage Journal** (`docs/ai-journal.md`) on D6: task, prompt approach, **the failure mode observed**, the fix.

**🚩 Gate 2 — Friday, 45 min.** Demo structured extraction, streaming chat and a filtered vector search, then open the log and walk through one request's token and cost breakdown. The mentor asks: *"This prompt costs X per call. Show me two ways to make it cheaper without losing quality, and which you'd try first."*

---

## 8. Week 3 — RAG, Agents & MCP

**Theme:** From "the model guesses" to "the model answers from our documents, with citations" — and then takes actions, safely.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D11** | Retrieval quality: top-k tuning, **hybrid search** (BM25 + vector), metadata filtering, **reranking**, citation assembly | Hybrid retrieval + reranker + a retrieval response carrying document title, chunk id and score | Hybrid + rerank measurably beats vector-only on 20 hand-labelled questions |
| **D12** | Grounded answering · citation formatting · **refusal when evidence is insufficient** · first evaluation baseline | `POST /chat` answers from retrieved context with inline citations; a 20-question baseline scored and committed to `evals/baseline.json` | An out-of-corpus question is refused; the baseline numbers are in Git |
| **D13** | **Tool / function calling:** schema design, strict tools, parallel tool use, `tool_result` handling · the agent loop · **loop safety** (max steps, token budget, timeout) | Agent with 4 tools, a hand-written loop, step limit, budget limit and structured trace logging | The intern can draw the request → `tool_use` → execute → `tool_result` → response cycle on a whiteboard; a deliberately looping prompt terminates cleanly at the step limit |
| **D14** | Conversation memory & state · **MCP**: protocol concepts, building an MCP server, connecting a client, tools vs resources vs prompts | Per-session conversation persistence + an **MCP server** exposing `search_docs` and `lookup_order`, verified from an MCP client | The mentor connects to the intern's MCP server from their own machine and calls a tool successfully |
| **D15** | **Guardrails:** prompt injection, input/output validation, PII redaction, grounding checks, human confirmation on write actions | Injection test suite (≥10 attacks) + PII redaction on ingest and output + a confirmation gate before `create_ticket` | Every attack in the suite is blocked or safely refused; the results are documented |

**Reading:** [Qdrant hybrid queries](https://qdrant.tech/documentation/concepts/hybrid-queries/) · [Cross-encoder reranking](https://sbert.net/examples/applications/cross-encoder/README.html) · [Anthropic — search & retrieval](https://docs.claude.com/en/docs/build-with-claude/search-and-retrieval/overview) · [Tool use overview](https://docs.claude.com/en/docs/agents-and-tools/tool-use/overview) · [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) · [Model Context Protocol](https://modelcontextprotocol.io/) · [MCP specification](https://modelcontextprotocol.io/specification) · [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) · [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) · [Reducing hallucinations](https://docs.claude.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations)

**AI drill (Week 3).** Use AI to draft golden-set questions from the corpus and to generate the prompt-injection attack suite — then **verify every one by hand**. An eval set contaminated by hallucinated ground truth is worse than no eval at all, and an AI-suggested "guardrail" is often security theatre. Journal both cases.

> **Week 3 is the densest week in the plan.** If the intern is behind by Wednesday, cut in this order: conversation memory → the reranker → the fourth agent tool. **Never cut the eval baseline (D12) or the MCP server (D14)** — the baseline gates all of Week 4, and MCP is an explicit JD requirement.

**🚩 Gate 3 — Friday, 45 min.** The mentor asks three unseen questions — one clearly answerable, one answerable only from a contradictory pair of documents, one entirely out of corpus — then attempts a live prompt-injection attack against the agent. Then: *"Show me a question your system gets wrong, and tell me whether it's a retrieval problem or a generation problem — and how you know."*

---

## 9. Week 4 — Evaluation, Cost, Latency & Ship

**Theme:** Prove it works, prove it got better, prove it is affordable.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D16** | Evaluation harness: golden-set design, retrieval metrics (recall@k, MRR), **LLM-as-judge** for faithfulness and correctness, judge calibration, CI integration | 50-question golden set + `make eval` producing a scored report + eval running in GitHub Actions | The judge is calibrated against 15 human-labelled examples and agrees ≥ 85% of the time |
| **D17** | Failure analysis: error taxonomy, root-causing retrieval vs generation failures, targeted fixes, regression discipline | Every failing question categorised; the top 3 failure modes fixed; eval re-run with a recorded delta | A measured improvement over the D12 baseline, with the numbers committed |
| **D18** | **Cost & latency engineering:** model tiering, prompt caching, effort tuning, batching, context trimming, streaming UX | `docs/cost-latency.md` with measured p50/p95 latency and cost per question, before and after | ≥ 30% cost reduction **or** ≥ 30% latency reduction, with eval scores held within 2 points |
| **D19** | Chat UI · deployment · observability · **code freeze at 17:00** | Streamlit chat with streaming, citations and a per-message cost readout; `docker compose up` runs API + Qdrant + UI; structured inference logs | Clean clone → compose up → a working conversation, with zero manual steps |
| **D20** | Technical write-up · rehearsal · **Demo Day** + tech talk · evaluation | `docs/technical-report.md` (architecture, decisions, eval results, cost analysis, known limits) + the demo (§9.2) + signed scorecard + Individual Development Plan for months 2–6 | Demo and tech talk delivered; scorecard completed; IDP agreed |

### 9.1 Model tiering guidance for D18

Use the strongest model where reasoning quality decides the outcome (the agent's planning turns, the LLM judge), and a cheaper tier where the task is mechanical (query rewriting, classification, summarizing a chunk). Measure before and after — a cheaper model that needs two retries is not cheaper. **Enable prompt caching on the stable system-prompt and tool-definition prefix before touching model choice:** caching is a free win, a model downgrade is a quality trade.

**Reading:** [Define success criteria](https://docs.claude.com/en/docs/test-and-evaluate/define-success) · [Creating strong empirical evaluations](https://docs.claude.com/en/docs/test-and-evaluate/develop-tests) · [Batch processing](https://docs.claude.com/en/docs/build-with-claude/batch-processing) · [RAGAS](https://docs.ragas.io/) · [DeepEval](https://github.com/confident-ai/deepeval) · [LangSmith](https://docs.smith.langchain.com/) · [Streamlit](https://docs.streamlit.io/)

**AI drill (Week 4).** Use AI to generate additional eval cases, write analysis scripts, summarize inference logs into failure patterns, and draft the technical report.

### 9.2 Demo Day agenda (20 minutes + 10 minutes questions)

| Minutes | Segment |
|---:|---|
| 0–2 | The problem and the architecture diagram: ingest → retrieve → rerank → answer → agent |
| 2–7 | Live: three questions — one answerable, one from contradictory sources, one out of corpus (the refusal) |
| 7–10 | The agent and MCP: a multi-step tool call, then the mentor's machine calling your MCP server |
| 10–14 | **Evaluation:** baseline vs final scores, what the failure taxonomy revealed, what you fixed |
| 14–17 | **Cost & latency:** before/after numbers, which levers you pulled and why in that order |
| 17–20 | Guardrails: a live injection attempt, blocked · **AI usage retrospective:** where AI was confidently wrong and what you now verify by default |
| +10 | Mentor and team questions |

---

## 10. AI Adoption — Mandatory Standards

For this role the JD is direct: **AI is not a support tool, it is the core competency.** These standards are enforced from Day 1.

### 10.1 Required practice

- Use one coding assistant daily (Claude Code / Cursor / GitHub Copilot or equivalent).
- **Working-level prompt engineering:** structure a system prompt, use few-shot examples, decompose a task, specify an output schema, and debug systematically when the model returns something wrong.
- Use AI to accelerate your own work: generate test datasets, write eval scripts, analyse inference logs, summarize papers and documentation, compare architecture options.
- **Understand the limits and design for them:** hallucination, non-determinism, context limits, prompt injection. Every limit needs a matching guardrail in the system you build.
- **Cost and performance awareness:** pick the model that fits the task, trim tokens, cache aggressively, and balance quality against latency consciously rather than by accident.
- Stay current and share: the Wednesday paper/doc share is mandatory.

### 10.2 Hard rules — a violation blocks the merge

> **Never** send credentials, PII or client data to any AI service, including your coding assistant.
> **Never** trust model output without verification. Every generated fact in the capstone must be traceable to a retrieved source.
> **Never** commit code you cannot explain. Mentors will ask at random, in review.
> **Never** commit an untracked LLM call — every call logs model, tokens, latency and cost.
> **Always** disclose AI-generated sections in the PR description.

### 10.3 Graded artefact

`docs/ai-journal.md` — maintained from D6 to D20, reviewed at Gates 2 and 3 and at Demo Day. **Minimum 12 entries.** Each entry names a task, the prompt approach, **the failure mode observed**, and the fix. For this role, entries documenting *how the model failed* are worth more than entries documenting what it produced.

---

## 11. Evaluation

### 11.1 Scorecard (100 points)

| # | Criterion | Weight | What "excellent" looks like |
|---|---|---:|---|
| 1 | **Core Python & API fundamentals** (Week 1 labs) | 10 | All five labs complete; clean typed async Python; can explain non-determinism and tokenization unaided |
| 2 | **RAG pipeline quality** | 18 | Robust ingestion, well-reasoned chunking, hybrid retrieval + rerank, accurate citations, correct refusals |
| 3 | **Agent, tool calling & MCP** | 18 | Reliable multi-step agent with loop safety; a working MCP server another machine can call |
| 4 | **Evaluation rigor** | 14 | Human-verified golden set, calibrated judge, measured improvement over a committed baseline, eval in CI |
| 5 | **Guardrails & safety** | 10 | Injection suite passing, PII redaction, grounding checks, confirmation before write actions |
| 6 | **Code quality, API design & testing** | 12 | Clean FastAPI service, typed models, meaningful tests, green CI, one-command startup |
| 7 | **Cost & latency engineering** | 8 | Measured before/after; deliberate model tiering and caching; quality held |
| 8 | **AI adoption & critical thinking** | 5 | Strong journal centred on observed failure modes |
| 9 | **Communication, docs & tech talk** | 5 | Clear technical report; a tech talk the team actually learns from |

### 11.2 Decision bands

| Score | Outcome |
|---|---|
| **≥ 85** | **Strong pass.** Assign to a client AI project as a contributor; open the Fresher conversation early. |
| **70 – 84** | **Pass.** Continue to month 2 on a real project with normal supervision. |
| **55 – 69** | **Conditional.** A two-week targeted remediation plan on the weakest two criteria, then re-assess. |
| **< 55** | **Not passing.** Structured feedback and closure of the internship. |

**Non-negotiable minimums, regardless of total score:** no key or PII in Git · the system refuses out-of-corpus questions instead of fabricating · the eval runs and reports · the intern can explain any randomly selected file they committed.

---

## 12. Mentor Playbook

| Cadence | Duration | Purpose |
|---|---|---|
| Daily standup | 15 min | Unblock — not status theatre |
| PR review | ≤ 4h SLA | Teach through comments; ask for a rewrite rather than fixing it yourself |
| Pairing (Tue, Thu) | 60 min | Intern drives, mentor navigates |
| Paper/doc share (Wed) | 45 min | Intern presents; builds the habit of tracking a fast-moving field |
| Gate review (Fri) | 45 min | Demo + adversarial questioning + written feedback |
| 1:1 (Fri) | 15 min | Wellbeing, motivation, budget check, career direction |

**Mentor preparation before Day 1:** the ~200-document corpus with its deliberate contradictions · a scoped API key with a hard budget cap · 15 human-labelled examples for judge calibration · a synthetic order database for `lookup_order` · a GitHub repo template with CI · a Qdrant container image ready to pull.

**Escalate to the team lead the same day if:**

- The intern is more than 2 days behind the plan at any gate.
- Spend exceeds 70% of the monthly budget before D15.
- The intern accepts model output without verification twice in one week.
- Any hard AI rule in §10.2 is violated.

**The mentor's most important habit:** at every gate, ask the intern to show you a case where **their own system is wrong** — and to explain why. An AI engineer who can only demo the happy path is not yet an AI engineer.

---

## 13. Reference Library

> Every link points to primary/official documentation. Documentation sites reorganize: if a URL 404s, search the same official domain rather than following a third-party mirror or a dated blog post. AI documentation changes faster than any other area in this programme — treat a six-month-old blog post as a hypothesis, not a fact.

### 13.1 Python language & tooling
[Python tutorial](https://docs.python.org/3/tutorial/) · [Language reference](https://docs.python.org/3/reference/) · [typing](https://docs.python.org/3/library/typing.html) · [dataclasses](https://docs.python.org/3/library/dataclasses.html) · [asyncio](https://docs.python.org/3/library/asyncio.html) · [logging](https://docs.python.org/3/library/logging.html) · [uv](https://docs.astral.sh/uv/) · [Ruff](https://docs.astral.sh/ruff/) · [pytest](https://docs.pytest.org/en/stable/) · [Python packaging guide](https://packaging.python.org/en/latest/)

### 13.2 API service
[FastAPI](https://fastapi.tiangolo.com/) · [FastAPI dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/) · [FastAPI testing](https://fastapi.tiangolo.com/tutorial/testing/) · [Flask](https://flask.palletsprojects.com/) · [Pydantic v2](https://docs.pydantic.dev/latest/) · [httpx](https://www.python-httpx.org/) · [Uvicorn](https://www.uvicorn.org/) · [Docker](https://docs.docker.com/) · [Docker Compose](https://docs.docker.com/compose/)

### 13.3 LLM & the Anthropic API
[Anthropic documentation](https://docs.claude.com/) · [Messages API](https://docs.claude.com/en/api/messages) · [Python SDK](https://github.com/anthropics/anthropic-sdk-python) · [Prompt engineering](https://docs.claude.com/en/docs/build-with-claude/prompt-engineering/overview) · [Structured outputs](https://docs.claude.com/en/docs/build-with-claude/structured-outputs) · [Streaming](https://docs.claude.com/en/docs/build-with-claude/streaming) · [Prompt caching](https://docs.claude.com/en/docs/build-with-claude/prompt-caching) · [Token counting](https://docs.claude.com/en/docs/build-with-claude/token-counting) · [Batch processing](https://docs.claude.com/en/docs/build-with-claude/batch-processing) · [Pricing](https://www.anthropic.com/pricing) · [Anthropic Cookbook](https://github.com/anthropics/anthropic-cookbook)

### 13.4 Agents, tools & MCP
[Tool use overview](https://docs.claude.com/en/docs/agents-and-tools/tool-use/overview) · [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) · [Model Context Protocol](https://modelcontextprotocol.io/) · [MCP specification](https://modelcontextprotocol.io/specification) · [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk) · [Claude Agent SDK](https://code.claude.com/docs/en/agent-sdk)

### 13.5 Retrieval & vector databases
[Qdrant](https://qdrant.tech/documentation/) · [Qdrant hybrid queries](https://qdrant.tech/documentation/concepts/hybrid-queries/) · [pgvector](https://github.com/pgvector/pgvector) · [Chroma](https://docs.trychroma.com/) · [Pinecone](https://docs.pinecone.io/) · [Weaviate](https://weaviate.io/developers/weaviate) · [Sentence-Transformers](https://sbert.net/) · [Cross-encoder reranking](https://sbert.net/examples/applications/cross-encoder/README.html) · [Okapi BM25](https://en.wikipedia.org/wiki/Okapi_BM25)

### 13.6 Frameworks (read to understand the abstractions — build without them first)
[LangChain](https://python.langchain.com/docs/introduction/) · [LangGraph](https://langchain-ai.github.io/langgraph/) · [LlamaIndex](https://docs.llamaindex.ai/)

### 13.7 Evaluation & observability
[Define success criteria](https://docs.claude.com/en/docs/test-and-evaluate/define-success) · [Creating strong empirical evaluations](https://docs.claude.com/en/docs/test-and-evaluate/develop-tests) · [Reduce hallucinations](https://docs.claude.com/en/docs/test-and-evaluate/strengthen-guardrails/reduce-hallucinations) · [RAGAS](https://docs.ragas.io/) · [DeepEval](https://github.com/confident-ai/deepeval) · [LangSmith](https://docs.smith.langchain.com/)

### 13.8 Data handling & storage
[pandas](https://pandas.pydata.org/docs/) · [Polars](https://docs.pola.rs/) · [pypdf](https://pypdf.readthedocs.io/en/stable/) · [python-docx](https://python-docx.readthedocs.io/en/latest/) · [Beautiful Soup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) · [Tesseract OCR](https://tesseract-ocr.github.io/tessdoc/) · [PostgreSQL](https://www.postgresql.org/docs/current/) · [Apache Airflow](https://airflow.apache.org/docs/)

### 13.9 ML foundations & local models
[Hugging Face NLP course](https://huggingface.co/learn/nlp-course/chapter1/1) · [Hugging Face docs](https://huggingface.co/docs) · [The Illustrated Transformer](https://jalammar.github.io/illustrated-transformer/) · [scikit-learn](https://scikit-learn.org/stable/) · [PyTorch](https://pytorch.org/docs/stable/index.html) · [Ollama](https://github.com/ollama/ollama) · [vLLM](https://docs.vllm.ai/)

### 13.10 Security & practice
[OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) · [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/) · [The Twelve-Factor App](https://12factor.net/) · [Git documentation](https://git-scm.com/doc) · [GitHub Actions](https://docs.github.com/en/actions) · [Conventional Commits](https://www.conventionalcommits.org/) · [Streamlit](https://docs.streamlit.io/)

> **Deliberate constraint for Weeks 2–3:** build the RAG pipeline **without** a framework. An intern who starts with LangChain learns the framework; an intern who starts with the SDK learns retrieval. Frameworks appear in the §5.4 stretch backlog as a comparison, not as a foundation.

---

## 14. Appendix A — Definition of Done (every PR)

- [ ] Branch named `feat/…`, `fix/…` or `chore/…`; commits follow Conventional Commits
- [ ] PR description states: what, why, how to test, and **which parts were AI-generated**
- [ ] No key, no PII, no client data in the diff or in Git history
- [ ] Every new LLM call logs model, token counts, latency and estimated cost
- [ ] Tests added or updated; CI green
- [ ] If the change could affect answer quality, the eval was re-run and the delta is in the PR

## 15. Appendix B — Week-to-Date Planner

| Week | Days | Theme | Checkpoint | Dates (fill in) |
|---|---|---|---|---|
| 1 | D1–D5 | Core Python, API & LLM fundamentals | Gate 1 | ______ – ______ |
| 2 | D6–D10 | Capstone starts — LLM service & ingestion | Gate 2 | ______ – ______ |
| 3 | D11–D15 | RAG, agents & MCP | Gate 3 | ______ – ______ |
| 4 | D16–D20 | Evaluation, cost, latency & ship | **Demo Day** | ______ – ______ |

## 16. Appendix C — Risks, Remediation & the Scope-Cut Ladder

### 16.1 Risks

| Risk | Early signal | Mentor action |
|---|---|---|
| Weak Python or async fundamentals | L2 or L4 incomplete | Compress D3, move L3 to homework; protect D4–D5 |
| Week 1 feels too easy for a strong intern | L1–L3 finished by lunch | Skip to L4/L5 and add a stretch lab: implement BM25 by hand and compare it to embeddings on the same queries |
| Corpus parsing eats the week | D9 incomplete | Provide pre-parsed text for 100 documents; the intern parses the remaining messy 100 |
| Budget burned by careless agent loops | Spend spikes on D13–D14 | Enforce the step limit and budget cap in code before any further agent work |
| RAG "works" but is never measured | No baseline committed by D12 | Stop feature work; the eval baseline is a hard gate, not a Week 4 task |
| Chasing frameworks instead of understanding | Reaches for LangChain on D9 | Enforce the §13 constraint; frameworks are stretch scope only |
| Over-reliance on AI, shallow understanding | Cannot explain the agent loop at Gate 3 | One mandatory AI-free day; the intern rebuilds the loop unaided |
| Eval set contaminated by AI-generated ground truth | Suspiciously high baseline scores | Re-verify the golden set against source documents by hand before any further tuning |

### 16.2 Scope-cut ladder — drop in this order if the intern falls behind

1. The Streamlit chat UI (D19) — a working `curl` demo is acceptable at Demo Day.
2. Conversation memory (D14) — keep single-turn agent calls and the MCP server.
3. The reranker (D11) — keep hybrid search.
4. The fourth agent tool (D13) — three tools are enough to demonstrate the loop.
5. The cost/latency optimization pass (D18) — but **still report the measurements**, even without the optimization.

**Never cut:** the eval baseline (D12) · the eval harness (D16–D17) · the MCP server (D14) · the guardrails suite (D15) · the refusal behaviour. Those carry 60 of the 100 scorecard points and are explicit JD requirements.

---

*Prepared for the VATEK Internship Program 2026 · AI Engineer Intern · Month 1 of 3–6.*
