# Backend Developer Intern — Programme Guide
### VATEK Internship Program 2026 · Software Development Department

| | |
|---|---|
| **Position** | Backend Developer Intern (Fullstack-oriented) |
| **Plan scope** | Month 1 — Ramp-up & Foundation phase of the 3–6 month internship |
| **Format** | Full-time, onsite · **20 working days (4 weeks)** · ~8h/day |
| **Structure** | Week 1 fundamentals → Weeks 2–4 capstone project → Demo Day |
| **Exit state** | Intern is deployable onto a real client project as a supervised contributor |
| **Daily plans** | One track file per language in this folder — see §2 |

---

## 1. Programme at a Glance

| Week | Days | Theme | What the intern produces | Checkpoint |
|---|---|---|---|---|
| **1** | D1–D5 | **Core language & framework fundamentals** | 5 graded labs (L1–L5), ending in a two-service microservice pair | **Gate 1** |
| **2** | D6–D10 | **Capstone starts — the Core Service** | Running API: layering, DB, CRUD, auth, orders | **Gate 2** |
| **3** | D11–D15 | **Integration, performance & the second service** | Payments, caching, CI, plus the Ops Service on RabbitMQ | **Gate 3** |
| **4** | D16–D20 | **API Gateway, fullstack slice & ship** | API Gateway with aggregated Swagger, AI-assisted admin console, security & performance pass | **Demo Day** |

**The capstone project begins on Day 6 and runs to Day 20.** Week 1 is fundamentals and labs only — it is not project time, and its labs live in a separate folder.

---

## 2. How This Folder Works

| File | For | Contents |
|---|---|---|
| `README.md` *(this file)* | Mentors and team lead; interns read §1–§6 once on Day 1 | Programme overview, OrderHub spec, microservice & gateway rules, gates, grading, mentor playbook |
| [`DotNet.md`](DotNet.md) | Interns whose main language is .NET / C# | Day-by-day plan, stack, reading list, weekly deliverables |
| [`Java.md`](Java.md) | Interns whose main language is Java | Day-by-day plan, stack, reading list, weekly deliverables |
| [`NodeJS.md`](NodeJS.md) | Interns whose main language is Node.js (TypeScript) | Day-by-day plan, stack, reading list, weekly deliverables |
| [`Python.md`](Python.md) | Interns whose main language is Python | Day-by-day plan, stack, reading list, weekly deliverables |

All four tracks build the same OrderHub product (§5) and are graded on the same scorecard (§9). Each track file is independent and covers only its own language.

**How to use these documents**

- These documents are a **read-only reference** — requirements, exercises, stack and reading. Interns do not copy, fork or edit anything in this folder.
- Each intern builds everything — the Week 1 labs and the capstone — in **their own repository**, following their track file.
- Each week in a track file ends with the **deliverables** that must exist in the intern's own repository by that Friday's gate. The mentor reviews the intern's repository and pull requests against that list at the gate (§7).

---

## 3. Why This Plan Exists

The job description asks for a backend engineer who can **build a complete API service on their own and is comfortable touching the frontend**. This plan trains each intern deeply in **one main language** — the one they were hired on: one week hardening the core language and framework, then three weeks building one real microservice product in that language, plus a thin frontend slice.

From Week 2 the plan is deliberately **project-first**. No isolated tutorials — every day produces a commit against a single, running system.

### 3.1 Outcomes — what the intern can do on Day 20

1. Explain and use the **core constructs of their main language**: types, classes, interfaces, generics, collections, querying, async, error handling.
2. Explain and use the **core constructs of their web framework**: routing, the request pipeline and middleware, dependency injection, validation, configuration.
3. Design and ship a **production-shaped REST API** — auth, validation, error model, pagination, docs.
4. Design a **relational schema**, write migrations, and diagnose a slow query with `EXPLAIN ANALYZE`.
5. Integrate a **third-party service** with timeouts, retries and webhook handling.
6. Build and defend a **microservice system**: two independently deployable services, each owning its own data, integrated over REST + a message queue with retries and a dead-letter queue.
7. Stand up an **API Gateway** as the single entry point, with routing, edge CORS and rate limiting, correlation-ID propagation, and **one aggregated Swagger UI covering every service**.
8. Use **Docker Compose** for the whole system and **GitHub Actions** to test it on every PR.
9. Consume their own API from a **Next.js admin UI** — the fullstack-oriented requirement — using AI to build it and reviewing what AI produced.
10. Use an **AI coding assistant** as a daily accelerator — and defend every line of code they commit.

---

## 4. Track Selection (Day 1 decision)

The mentor assigns a track on Day 1 from the intern's main language in the technical interview. **The whole month is built in that one language** — every lab, both microservices and the API Gateway. The only other technology is the thin, AI-assisted Next.js Admin Console in Week 4.

| Track | Language | Stack | Track file |
|---|---|---|---|
| **.NET** | C# | ASP.NET Core Web API · EF Core · xUnit · FluentValidation · YARP | [`DotNet.md`](DotNet.md) |
| **Java** | Java | Spring Boot 3 · Spring Data JPA · JUnit 5 · Bean Validation · Spring Cloud Gateway | [`Java.md`](Java.md) |
| **Node.js** | TypeScript | NestJS · Prisma · Jest · class-validator | [`NodeJS.md`](NodeJS.md) |
| **Python** | Python | FastAPI · SQLAlchemy · Alembic · pytest · Pydantic | [`Python.md`](Python.md) |

> **Each track file is independent.** It covers only its own language, from the Week 1 fundamentals to Demo Day. An intern needs exactly two documents: this programme guide and their track file.

### 4.1 Shared infrastructure (identical for every track)

PostgreSQL 16 · Redis 7 · RabbitMQ 3 · Docker & Docker Compose · Git + GitHub PR flow · OpenAPI/Swagger · GitHub Actions · Next.js 15 + TailwindCSS (Week 4 only) · an AI coding assistant.

---

## 5. The Capstone Project — "OrderHub" (starts Day 6)

Every intern, on every track, builds the same product. This keeps mentoring, grading and peer review consistent, and lets interns on different tracks compare solutions directly.

> **OrderHub** — a mini order-management platform for a B2B merchant. A staff user signs in, manages products, creates and pays orders, uploads an invoice, and reads a daily sales report. The system emits an event when an order is paid; a separate service consumes it to build reports and send notifications.

> ### ⚠️ This exercise must be built as a microservice system.
> A monolith with two folders does not pass, however well it works. OrderHub is **three independently deployable services behind an API Gateway**, communicating synchronously over HTTP and asynchronously over a message queue. The architecture is the assessment; the features are only the vehicle for it. §5.2 states the rules a mentor checks against.

### 5.1 Services

| Service | Language | Built in | Responsibility |
|---|---|---|---|
| **API Gateway** | Main language *or* a proxy (Traefik / Nginx / Kong) | Week 4 (D16) | The single public entry point. Routes to Core and Ops, terminates CORS, enforces edge rate limiting, forwards the JWT, and publishes **one aggregated Swagger UI** listing every service. See §5.3. |
| **Core API** | Main language | Weeks 2–3 | Auth (JWT), Users, Products, Orders, Payments, Invoice upload, Audit log. Publishes `order.paid` events. Owns the `users / products / orders / payments / invoices` tables. |
| **Ops Service** | Main language | Week 3 | Consumes `order.paid`. Builds daily sales reports. Sends mock email/SMS notifications. Exposes `/reports` and `/notifications`. Runs a scheduled job. Owns the `reports / notifications` tables. |
| **Admin Console** | Next.js + TypeScript | Week 4 (D17) | Login, order list (filter + pagination), order detail, report page. **May be AI-generated** — see §5.3.2. Calls only the Gateway, never a service directly. |

```
                          ┌──────────────────────┐
   Browser ──────────────►│    API Gateway       │  /docs  → aggregated Swagger UI
   (Admin Console)        │  routing · CORS      │  /api/core/*  →  Core API
                          │  rate limit · JWT    │  /api/ops/*   →  Ops Service
                          └───────┬──────┬───────┘
                                  │      │
                     HTTP ────────┘      └──────── HTTP
                                  ▼             ▼
                        ┌─────────────┐   ┌─────────────┐
                        │  Core API   │   │ Ops Service │
                        │ (main lang) │   │ (main lang) │
                        └──────┬──────┘   └──────▲──────┘
                               │  order.paid     │
                               └───► RabbitMQ ───┘
```

### 5.2 Microservice rules (checked at Gate 3 and on Day 20)

These are the architectural constraints the intern is graded against. They are what turn "two projects in one repo" into a microservice system.

| # | Rule | How the mentor verifies it |
|---|---|---|
| 1 | **Independently deployable.** Each service has its own Dockerfile, its own config, its own port, and starts on its own. | `docker compose up core-api` alone starts and serves `/health`. |
| 2 | **Separate codebases.** No shared source folder, no shared build. Each service has its own dependency file and test suite. | Deleting the Ops Service folder must not break the Core API build. |
| 3 | **Owned data.** A service reads and writes only its own tables. **No cross-service database access.** If Ops needs order data, it gets it from the event payload or by calling the Core API. | Grep the Ops Service for `orders`/`order_items` table access — there must be none. |
| 4 | **Contract over coupling.** Cross-service calls go through a documented HTTP contract (in OpenAPI) or a documented event schema — never a shared ORM entity class. | The event payload schema is written down in `docs/events.md`. |
| 5 | **Resilient synchronous calls.** Every service-to-service HTTP call has a timeout, a retry policy and a fallback. | Stopping Ops must not make a Core API request hang. |
| 6 | **Reliable asynchronous messaging.** `order.paid` is published by Core and consumed by Ops with retry and a dead-letter queue. | Killing Ops mid-flight loses no message; a poisoned message lands in the DLQ. |
| 7 | **One entry point.** All external traffic goes through the API Gateway. The services are not exposed publicly. | In `docker-compose.yml`, only the gateway publishes a host port. |
| 8 | **Independently observable.** Each service logs in the same JSON format and propagates the same correlation ID end to end. | One request ID is traceable from the gateway log through Core to the Ops consumer log. |

> **This is L5 from Week 1 at production scale.** Rules 1, 2 and 5 are exactly what the `Orders` / `Pricing` lab taught; rules 3, 4, 6, 7 and 8 are what production adds. Interns who understood L5 will recognise seven of these eight.

### 5.3 The API Gateway (required) and the Admin Console (AI-assisted)

#### 5.3.1 API Gateway — a graded deliverable

The Gateway is **not optional and not stretch scope**. It is built on D16 and must provide:

| Requirement | Detail |
|---|---|
| **Single entry point** | Exactly one host port published in `docker-compose.yml`. Core and Ops are reachable only on the internal Docker network. |
| **Path-based routing** | `/api/core/**` → Core API · `/api/ops/**` → Ops Service. Prefixes are stripped or rewritten consistently and documented. |
| **Aggregated Swagger UI** | `http://localhost:8080/docs` renders a **single Swagger UI with a service selector** listing every service's spec. Both specs load and both are executable from the browser. |
| **Full Swagger URLs published** | The README lists every documentation URL explicitly (see the table below) — a reviewer must never have to guess a port. |
| **CORS** | Configured once, at the gateway. Individual services do not each implement CORS. |
| **Edge rate limiting** | Applied at the gateway on `/api/core/auth/**`. |
| **Auth pass-through** | The `Authorization` header is forwarded unchanged; the gateway does not re-issue tokens. Token *validation* stays in the Core API. |
| **Correlation ID** | The gateway generates `X-Correlation-Id` if absent and forwards it to every downstream call. |

**Swagger URLs to publish in the README** (fill in the actual ports used):

| Purpose | URL |
|---|---|
| Aggregated Swagger UI (the one the reviewer opens) | `http://localhost:8080/docs` |
| Core API spec, served through the gateway | `http://localhost:8080/api/core/openapi.json` |
| Ops Service spec, served through the gateway | `http://localhost:8080/api/ops/openapi.json` |
| Core API Swagger UI, direct (internal / dev only) | `http://localhost:5001/swagger` |
| Ops Service Swagger UI, direct (internal / dev only) | `http://localhost:5002/docs` |

**Gateway options by track** — pick one and justify the choice in an ADR:

| Track | Native option | Proxy option (any track) |
|---|---|---|
| **B1 .NET** | [YARP reverse proxy](https://microsoft.github.io/reverse-proxy/) | [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Kong](https://developer.konghq.com/) |
| **B2 Java** | [Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/reference/) | as above |
| **B3 Node.js** | A NestJS gateway app + [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware) | as above |
| **B4 Python** | A FastAPI gateway app + [httpx](https://www.python-httpx.org/) | as above |

For aggregating the two specs into one Swagger UI, use the Swagger UI [`urls` configuration option](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/) — .NET does this through Swashbuckle's multiple `SwaggerEndpoint` calls, Java through [springdoc-openapi](https://springdoc.org/) grouped configs.

#### 5.3.2 Admin Console — AI-assisted by design

The Admin Console exists to prove the API is usable end to end and to satisfy the JD's fullstack-oriented requirement. It is **not** where a backend intern should spend their judgement, so:

- **Using an AI assistant to generate the dashboard is explicitly encouraged.** Scaffold it, generate the tables and forms, let AI write the Tailwind. One day (D17) is budgeted for all four pages precisely because AI is doing the heavy lifting.
- **But the same rules still apply.** The intern must be able to explain the generated code, must check it renders at 1280 px and 768 px, and must handle loading, error and empty states on every page. "AI wrote it" is not a defence at Demo Day.
- **It must call the Gateway only.** Its API base URL is the gateway's, and it must not know that Core and Ops are separate services. If the intern hardcodes a service port, that is a microservice-rule violation, not a frontend bug.
- It is graded lightly — 5 of 100 points — while the Gateway and the microservice rules it exercises are graded at 18.

### 5.4 Data model (target ERD)

```
users        (id, email, password_hash, role, created_at)
products     (id, sku, name, price, stock, is_active, created_at, updated_at)
orders       (id, code, user_id → users, status, total_amount, created_at, updated_at)
order_items  (id, order_id → orders, product_id → products, qty, unit_price)
payments     (id, order_id → orders, provider, provider_ref, amount, status, paid_at)
invoices     (id, order_id → orders, storage_key, mime_type, size_bytes, uploaded_at)
audit_logs   (id, actor_id, entity, entity_id, action, payload_json, created_at)
```

`orders.status` ∈ `DRAFT → PENDING_PAYMENT → PAID → FULFILLED → CANCELLED`

### 5.5 Non-functional requirements (graded on Day 20)

- `docker compose up` brings the entire system up from a clean clone, with no manual steps.
- **Only the API Gateway publishes a host port.** Core and Ops are internal-network only.
- **The aggregated Swagger UI at the gateway lists every service, and every endpoint is executable from it.** All Swagger URLs are written in the README.
- Errors returned in one consistent shape ([RFC 7807 Problem Details](https://datatracker.ietf.org/doc/html/rfc7807)), including errors the gateway itself returns.
- One correlation ID is traceable from the gateway log through Core to the Ops consumer log.
- No secret committed to Git; all configuration via environment variables ([12-Factor](https://12factor.net/)).
- CI green on `main`: build + lint + unit tests + integration tests.
- `GET /products` responds in **< 200 ms p95** with 10,000 seeded rows, measured **through the gateway**.

### 5.6 Stretch backlog (only after the required scope is green)

Presigned-URL object storage via MinIO · transactional outbox pattern · service discovery instead of static routes · circuit breaker at the gateway · distributed tracing (OpenTelemetry) across all three services · refresh-token rotation with revocation · GraphQL read endpoint · Kubernetes manifests.

---

## 6. Daily Rhythm

| Time | Activity |
|---|---|
| 09:00 – 09:15 | Standup: yesterday / today / blockers |
| 09:15 – 12:00 | Focused block (Week 1: concepts · Weeks 2–4: build) |
| 13:00 – 16:30 | Build block 2 · pairing / mentor review window |
| 16:30 – 17:15 | Self-review, push PR, write the daily log |
| 17:15 – 17:30 | Reading (from the linked docs) · AI-usage journal entry |

**Fixed weekly events:** Tue & Thu 60-min pairing · Wed 30-min tech reading share · Fri 45-min **Gate Review** · Fri 15-min 1:1.

**Code review SLA:** the mentor responds to any PR within 4 working hours. Nothing merges without one approval.

---

## 7. Weekly Checkpoints — Gates & Demo Day

Day-by-day content, tools and reading live in the track files. This section is the mentor's reference for what each checkpoint verifies. Every gate is a live demo **plus** a verbal defence: passing requires answering without reading the code.

### 7.1 Week 1 labs (shared by every track)

| Day | Concept block | Lab (afternoon) | Done when |
|---|---|---|---|
| **D1** | Language & runtime core: syntax, type system, project layout, build tool & package manager, debugger | **L1 — Order report CLI.** Read `orders.json` (200 orders with customer, status, line items), filter by status and date range, compute revenue per day and per status, print a report | Runs from the CLI; totals match the answer key; a debugger breakpoint is hit and stepped through in front of the mentor |
| **D2** | OOP & abstraction: class, interface, inheritance vs composition, generics, SOLID, dependency injection | **L2 — Refactor L1** behind an `IOrderRepository` interface with two implementations (JSON file, in-memory), plus an `OrderTotalCalculator` wired by constructor injection | Swapping repository implementations requires no change inside the report logic |
| **D3** | Collections, querying & async: collection types, the language's query idiom, async/await, cancellation, error handling | **L3 — Async order enrichment.** Group orders by customer with the language's query idiom, then fetch shipping status for 20 orders concurrently from a mock endpoint, with a timeout | The concurrent version is measurably faster than sequential; a slow shipping call times out and is reported, not swallowed |
| **D4** | Web framework core: routing, controllers, the request pipeline (middleware / filters / interceptors), model binding & validation, DI container, configuration | **L4 — Mini Orders API.** 5 REST endpoints — list, get, create, change status, cancel order — + 3 custom middleware (request logging, correlation ID, global exception handler) | Middleware order is explained correctly; an invalid status transition returns a clean JSON error, never a stack trace |
| **D5** | Data access & testing: ORM basics, migrations, repository pattern, unit testing, mocking. **Microservice fundamentals:** bounded context, sync vs async communication, resilience, service discovery, API gateway | **L5 — Split the Orders API into two services:** `Orders` and `Pricing` (line-item totals, discounts, tax), communicating over HTTP with a timeout, retry and fallback. ≥8 unit tests with mocks | Killing `Pricing` degrades `Orders` gracefully — orders still list, with totals marked `pending` — instead of hanging or returning 500 |

> **L5 is the spine of the whole month.** The capstone in Weeks 2–4 is the same pattern at production scale: two independently deployable order services, synchronous calls plus asynchronous events. An intern who understands L5 understands Week 3 before arriving there.

### 7.2 Gate 1 — end of Week 1 (D5)

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L4 and L5. The mentor asks: *"Add a fourth middleware that rate-limits by IP. Where in the pipeline does it go, and why there?"* and *"`Pricing` is down. Trace exactly what the user sees, and where in your code that behaviour is decided."* Passing requires answering without reading the code.

### 7.3 Gate 2 — end of Week 2 (D10)

**🚩 Gate 2 — Friday, 45 min.** Demo: register → login → create product → create an order → 401/403 behaviour. The mentor asks: *"Walk me through what happens between the HTTP request arriving and the order row being committed."* and *"Two customers buy the last unit at the same millisecond. What stops both succeeding, and where is that in your code?"*

### 7.4 Gate 3 — end of Week 3 (D15)

**🚩 Gate 3 — Friday, 45 min.** Live `docker compose up` from a fresh clone on the mentor's machine, then a walk through the **§5.2 microservice rules** one by one — the mentor checks rules 1–6 directly (rules 7 and 8 land with the gateway on D16). Then: *"Why this messaging pattern, and what breaks if the Ops Service is down for an hour?"* and *"Show me where the Ops Service reads order data. If it touches the `orders` table, explain why that is a problem."*

### 7.5 Demo Day agenda — end of Week 4 (D20)

| Minutes | Segment |
|---:|---|
| 0–2 | The problem and the **architecture diagram**: three services, one gateway |
| 2–5 | **Open the aggregated Swagger UI**, switch between services, execute a live request from the browser |
| 5–10 | End-to-end walkthrough through the gateway: login → order → payment → `order.paid` event → report → admin console |
| 10–13 | **Microservice evidence:** stop the Ops Service live and show the system degrade instead of break; show a message land in the DLQ; trace one correlation ID across three logs |
| 13–16 | Quality evidence: CI run, test coverage, performance before/after, security review |
| 16–18 | Service boundaries: what you split, what you kept together, and what you would change |
| 18–20 | **AI usage retrospective:** where AI saved the most time (the console), where it was wrong, what changed in how you work |
| +10 | Mentor and team questions |

---

## 8. AI Adoption — Mandatory Standards

The JD makes AI adoption a hiring requirement, not a bonus. These rules are enforced in code review from Day 1.

### 8.1 Required practice

- Use one assistant daily (Claude Code / Cursor / GitHub Copilot or equivalent).
- Write **contextual prompts**: state the stack, the constraint and the desired output format. "Add pagination" is a weak prompt. "Add cursor-based pagination to `GET /products` in ASP.NET Core, returning `{items, nextCursor}`, keeping the existing `ProblemDetails` error shape" is a working prompt.
- Break large requests into steps. One prompt should map to roughly one PR-sized change.
- Use AI where it pays: boilerplate, unit tests, refactors, explaining unfamiliar code, reading stack traces, optimizing SQL, drafting API docs.

### 8.2 Hard rules — a violation blocks the merge

> **Never** paste credentials, customer data, or NDA-scope source code into a public AI tool.
> **Never** commit code you cannot explain line by line. Mentors will ask at random, in review.
> **Always** run and test AI-generated code before pushing. Generated tests that pass trivially score zero.
> **Always** disclose AI-generated sections in the PR description.

### 8.3 Graded artefact

`docs/ai-journal.md` — maintained from D6 to D20, reviewed at Gates 2 and 3 and at Demo Day. **Minimum 12 entries.** An entry that only says "used AI to write code" scores zero; each entry must name the failure mode the intern caught.

---

## 9. Evaluation

### 9.1 Scorecard (100 points)

| # | Criterion | Weight | What "excellent" looks like |
|---|---|---:|---|
| 1 | **Core language & framework fundamentals** (Week 1 labs) | 10 | All five labs complete; can explain the middleware pipeline and the L5 failure modes unaided |
| 2 | **Functional completeness** | 15 | All OrderHub features work end-to-end across both services |
| 3 | **Code quality & convention** | 12 | Consistent layering, meaningful names, no dead code, clean PR history |
| 4 | **API & data design** | 8 | Sensible schema and indexes, coherent REST contract, correct status codes |
| 5 | **Testing & CI** | 12 | Meaningful unit + integration tests, green pipeline, ≥60% service-layer coverage |
| 6 | **Microservice architecture** (§5.2 rules) | 10 | All eight rules hold; clean service boundaries with no shared code or tables; messaging survives a failed consumer |
| 7 | **API Gateway & API documentation** | 8 | Single entry point enforced; routing, CORS, rate limiting and correlation ID working; **aggregated Swagger UI executable and every URL documented** |
| 8 | **Security & performance** | 5 | Top 10 checklist applied; a measured, documented performance improvement |
| 9 | **AI adoption & critical thinking** | 10 | Strong journal; can name concrete AI mistakes they caught — including in the AI-generated console |
| 10 | **Admin Console** | 5 | All four pages work through the gateway with complete loading/error/empty states; the intern can explain the generated code |
| 11 | **Communication, docs & demo** | 5 | Clear README and ADRs; confident, structured demo |

### 9.2 Decision bands

| Score | Outcome |
|---|---|
| **≥ 85** | **Strong pass.** Assign to a client project as a contributor; open the Fresher conversation early. |
| **70 – 84** | **Pass.** Continue to month 2 on a real project with normal supervision. |
| **55 – 69** | **Conditional.** A two-week targeted remediation plan on the weakest two criteria, then re-assess. |
| **< 55** | **Not passing.** Structured feedback and closure of the internship. |

**Non-negotiable minimums, regardless of total score:** CI green on `main` · no secrets in Git · the intern can explain any randomly selected file they committed.

---

## 10. Mentor Playbook

| Cadence | Duration | Purpose |
|---|---|---|
| Daily standup | 15 min | Unblock — not status theatre |
| PR review | ≤ 4h SLA | Teach through comments; ask for a rewrite rather than fixing it yourself |
| Pairing (Tue, Thu) | 60 min | Intern drives, mentor navigates |
| Gate review (Fri) | 45 min | Demo + verbal defence + written feedback |
| 1:1 (Fri) | 15 min | Wellbeing, motivation, career direction |

**Mentor preparation before Day 1:** nothing for the Week 1 data — it is provided in [`datasets/`](../datasets/README.md) (`orders.json`, `answer-key.json` for checking L1 totals, and `shipments.json` served by `json-server` as the L3 shipping-status endpoint) · working devcontainer or setup script · the OrderHub API contract and ERD reference solution · seed data · the Figma-free UI wireframe for Week 4 · GitHub repo template with CI · the track decision made from the interview notes.

**Escalate to the team lead the same day if:**

- The intern is more than 2 days behind the plan at any gate.
- The intern cannot explain their own code twice in one week.
- Any hard AI rule in §8.2 is violated.

**The mentor's most important habit:** when the intern is stuck, ask *"what have you already tried, and what did you expect to happen?"* before giving any answer.

---

## 11. Shared Reference Library

> Every link points to primary/official documentation (vendor docs, MDN, W3C, RFCs). Documentation sites reorganize: if a URL 404s, search the same official domain rather than following a third-party mirror or a dated blog post.

Language- and framework-specific references are in each track file, next to the day they are used.

### 11.1 Data, infrastructure & messaging

[PostgreSQL](https://www.postgresql.org/docs/current/) · [PostgreSQL EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html) · [Redis](https://redis.io/docs/latest/) · [MongoDB](https://www.mongodb.com/docs/) · [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials) · [Apache Kafka](https://kafka.apache.org/documentation/) · [MinIO](https://github.com/minio/minio) · [Docker](https://docs.docker.com/) · [Docker Compose](https://docs.docker.com/compose/) · [Testcontainers](https://testcontainers.com/) · [Grafana k6](https://grafana.com/docs/k6/latest/)

### 11.2 API Gateway & Swagger aggregation

[API gateway pattern](https://microservices.io/patterns/apigateway.html) · [YARP reverse proxy (.NET)](https://microsoft.github.io/reverse-proxy/) · [Spring Cloud Gateway (Java)](https://docs.spring.io/spring-cloud-gateway/reference/) · [http-proxy-middleware (Node)](https://github.com/chimurai/http-proxy-middleware) · [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Kong](https://developer.konghq.com/) · [Swagger UI configuration (`urls` for multiple specs)](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/) · [springdoc-openapi](https://springdoc.org/) · [Swashbuckle (.NET)](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)

### 11.3 Architecture, API & practice

[Microservices patterns](https://microservices.io/patterns/index.html) · [Database per service](https://microservices.io/patterns/data/database-per-service.html) · [The Twelve-Factor App](https://12factor.net/) · [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [RFC 7807 Problem Details](https://datatracker.ietf.org/doc/html/rfc7807) · [MDN HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) · [JWT introduction](https://jwt.io/introduction) · [OAuth 2.0](https://oauth.net/2/) · [OWASP API Security Top 10](https://owasp.github.io/API-Security/) · [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/) · [Git documentation](https://git-scm.com/doc) · [GitHub Actions](https://docs.github.com/en/actions) · [Conventional Commits](https://www.conventionalcommits.org/) · [Semantic Versioning](https://semver.org/) · [Google Engineering Practices — Code Review](https://google.github.io/eng-practices/review/)

### 11.4 Frontend slice (Week 4)

[Next.js](https://nextjs.org/docs) · [React](https://react.dev/learn) · [TailwindCSS](https://tailwindcss.com/docs) · [MDN CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)

---

## 12. Appendix A — Definition of Done (every PR)

- Branch named `feat/…`, `fix/…` or `chore/…`; commits follow Conventional Commits
- PR description states: what, why, how to test, and **which parts were AI-generated**
- Unit tests added or updated; CI green
- No secret, no commented-out code, no `TODO` without a ticket reference
- OpenAPI updated if the contract changed — and the spec still loads in the aggregated Swagger UI
- No cross-service database access introduced (§5.2 rule 3); no service port hardcoded outside the gateway config (§5.2 rule 7)
- Diff self-reviewed before requesting review

---

## 13. Appendix B — Programme Calendar

| Week | Days | Theme | Checkpoint |
|---|---|---|---|
| 1 | D1–D5 | Core language & framework fundamentals | Gate 1 |
| 2 | D6–D10 | Capstone starts — the Core Service | Gate 2 |
| 3 | D11–D15 | Integration, performance & the second service | Gate 3 |
| 4 | D16–D20 | API Gateway, fullstack slice & ship | **Demo Day** |

---

## 14. Appendix C — Risks, Remediation & the Scope-Cut Ladder

### 14.1 Risks

| Risk | Early signal | Mentor action |
|---|---|---|
| Main language weaker than the interview suggested | Struggles on Week 1 D2–D3 | Compress D3, move L3 to homework; protect D4–D5 — the framework and microservice labs matter most |
| Week 1 feels too easy for a strong intern | L1–L3 finished by lunch | Skip ahead to L4/L5 and add a stretch lab: put an API gateway in front of the two L5 order services |
| The second service slips | D13 scaffold incomplete | Reduce the Ops Service to a consumer + one report endpoint; keep the queue |
| Over-reliance on AI, shallow understanding | Cannot explain a lab at Gate 1 | One mandatory AI-free day; the intern rebuilds a lab unaided |
| Environment and Docker problems eat days | D6 slips | Provide a prepared devcontainer; setup is mentor-owned, not intern-owned |
| Scope creep by an enthusiastic intern | Extra features, missing tests | Freeze scope at §5; excess goes to the §5.6 stretch backlog |

### 14.2 Scope-cut ladder — drop in this order if the intern falls behind

1. Load testing and the observability pass (D19) — keep the security review.
2. The Admin Console's report and order-detail pages (D17) — keep login and the orders list. It is AI-generated and worth 5 points; do not let it eat gateway time.
3. Mock notifications and the scheduled job (D15).
4. Redis caching (D12) — keep the query optimization and `EXPLAIN ANALYZE`.
5. Invoice upload (D11).
6. Gateway edge rate limiting — keep routing, CORS, correlation ID and the aggregated Swagger UI.

**Never cut:** auth · the order transaction · the message queue between the two services · the **API Gateway with its aggregated Swagger UI** · the §5.2 microservice rules · integration tests and CI · the Demo Day retrospective. Those carry 65 of the 100 scorecard points and are the reason this exercise exists.

> **If the gateway is at risk on D16, drop the native implementation and use Traefik or Nginx with a static config.** A 40-line proxy config that satisfies §5.3.1 scores full marks; a half-finished custom gateway scores none. The learning objective is the *pattern*, not the library.

---

*Prepared for the VATEK Internship Program 2026 · Backend Developer Intern (Fullstack-oriented) · Month 1 of 3–6.*

---

*VATEK Internship Program 2026 · Backend Developer Intern (Fullstack-oriented) · Programme Guide · Month 1 of 3–6.*
