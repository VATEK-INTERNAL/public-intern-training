# Backend Developer Intern — 1-Month Training Plan
### VATEK Internship Program 2026 · Software Development Department

| | |
|---|---|
| **Position** | Backend Developer Intern (Fullstack-oriented) |
| **Plan scope** | Month 1 — Ramp-up & Foundation phase of the 3–6 month internship |
| **Format** | Full-time, onsite · **20 working days (4 weeks)** · ~8h/day |
| **Structure** | Week 1 fundamentals → Weeks 2–4 capstone project → Demo Day |
| **Exit state** | Intern is deployable onto a real client project as a supervised contributor |

---

## 1. Programme at a Glance

| Week | Days | Theme | What the intern produces | Checkpoint |
|---|---|---|---|---|
| **1** | D1–D5 | **Core language & framework fundamentals** | 5 graded labs (L1–L5), ending in a two-service microservice pair | **Gate 1** |
| **2** | D6–D10 | **Capstone starts — the Core Service** | Running API: layering, DB, CRUD, auth, orders | **Gate 2** |
| **3** | D11–D15 | **Integration, performance & the second language** | Payments, caching, CI, plus the Ops Service on RabbitMQ | **Gate 3** |
| **4** | D16–D20 | **API Gateway, fullstack slice & ship** | API Gateway with aggregated Swagger, AI-assisted admin console, security & performance pass | **Demo Day** |

**The capstone project begins on Day 6 and runs to Day 20.** Week 1 is fundamentals and labs only — it is not project time, and its labs live in a separate folder.

---

## 2. Why This Plan Exists

The job description asks for a backend engineer who is **strong in one main language, productive in a second, and comfortable touching the frontend**. This plan builds exactly that: one week hardening the core language and framework, then three weeks building one real product across two languages plus a frontend slice.

From Week 2 the plan is deliberately **project-first**. No isolated tutorials — every day produces a commit against a single, running system.

### 2.1 Outcomes — what the intern can do on Day 20

1. Explain and use the **core constructs of their main language**: types, classes, interfaces, generics, collections, querying, async, error handling.
2. Explain and use the **core constructs of their web framework**: routing, the request pipeline and middleware, dependency injection, validation, configuration.
3. Design and ship a **production-shaped REST API** — auth, validation, error model, pagination, docs.
4. Design a **relational schema**, write migrations, and diagnose a slow query with `EXPLAIN ANALYZE`.
5. Integrate a **third-party service** with timeouts, retries and webhook handling.
6. Build and defend a **microservice system**: two services in two languages, independently deployable, owning their own data, integrated over REST + a message queue with retries and a dead-letter queue.
7. Stand up an **API Gateway** as the single entry point, with routing, edge CORS and rate limiting, correlation-ID propagation, and **one aggregated Swagger UI covering every service**.
8. Use **Docker Compose** for the whole system and **GitHub Actions** to test it on every PR.
9. Consume their own API from a **Next.js admin UI** — the fullstack-oriented requirement — using AI to build it and reviewing what AI produced.
10. Use an **AI coding assistant** as a daily accelerator — and defend every line of code they commit.

---

## 3. Track Selection (Day 1 decision)

The mentor assigns a track on Day 1 based on the technical interview result. **Main language ≈ 60% of the month; extension language ≈ 25%; frontend slice ≈ 15%.**

| Track | Main language (Core Service) | Extension language (Ops Service) | Core stack | Extension stack |
|---|---|---|---|---|
| **B1** | .NET / C# | Python **or** Node.js | ASP.NET Core Web API · EF Core · xUnit · FluentValidation | FastAPI · SQLAlchemy · Alembic · pytest **/** NestJS · Prisma · Jest |
| **B2** | Java | Python **or** Node.js | Spring Boot 3 · Spring Data JPA · JUnit 5 · Bean Validation | FastAPI · SQLAlchemy · Alembic · pytest **/** NestJS · Prisma · Jest |
| **B3** | Node.js | Python | NestJS · Prisma · Jest · class-validator | FastAPI · SQLAlchemy · Alembic · pytest |
| **B4** | Python | Node.js | FastAPI · SQLAlchemy · Alembic · pytest · Pydantic | NestJS · Prisma · Jest · class-validator |

> **Rule for choosing the extension:** pick the language the intern has *never* used in a real project. The point of Week 3 is the discomfort of a second paradigm, not a second victory lap.

### 3.1 Shared infrastructure (identical for every track)

PostgreSQL 16 · Redis 7 · RabbitMQ 3 · Docker & Docker Compose · Git + GitHub PR flow · OpenAPI/Swagger · GitHub Actions · Next.js 15 + TailwindCSS (Week 4 only) · an AI coding assistant.

---

## 4. Week 1 — Core Language & Framework Fundamentals

**Theme:** Before building the product, own the language. Mornings are concepts and reading; afternoons are graded micro-exercises (**labs**) in a `/labs` folder, kept separate from the capstone repository.

### 4.1 The five-day skeleton (identical shape for every track)

| Day | Concept block | Lab (afternoon) | Done when |
|---|---|---|---|
| **D1** | Language & runtime core: syntax, type system, project layout, build tool & package manager, debugger | **L1 — Inventory CLI.** Read a JSON file of products, filter, aggregate, print a report | Runs from the CLI; a debugger breakpoint is hit and stepped through in front of the mentor |
| **D2** | OOP & abstraction: class, interface, inheritance vs composition, generics, SOLID, dependency injection | **L2 — Refactor L1** behind an `IProductRepository` interface with two implementations (file, in-memory), wired by constructor injection | Swapping implementations requires no change inside the business logic |
| **D3** | Collections, querying & async: collection types, the language's query idiom, async/await, cancellation, error handling | **L3 — Async report.** Query and group the inventory, then fetch enrichment data for 20 items concurrently with a timeout | The concurrent version is measurably faster than sequential; timeouts are handled, not swallowed |
| **D4** | Web framework core: routing, controllers, the request pipeline (middleware / filters / interceptors), model binding & validation, DI container, configuration | **L4 — Bookstore API.** 5 REST endpoints + 3 custom middleware (request logging, correlation ID, global exception handler) | Middleware order is explained correctly; an unhandled exception returns a clean JSON error, never a stack trace |
| **D5** | Data access & testing: ORM basics, migrations, repository pattern, unit testing, mocking. **Microservice fundamentals:** bounded context, sync vs async communication, resilience, service discovery, API gateway | **L5 — Split Bookstore into two services:** `Catalog` and `Pricing`, communicating over HTTP with a timeout, retry and fallback. ≥8 unit tests with mocks | Killing `Pricing` degrades `Catalog` gracefully — it does not hang or return 500 |

> **L5 is the spine of the whole month.** The capstone in Weeks 2–4 is the same pattern at production scale: two services, two languages, synchronous calls plus asynchronous events. An intern who understands L5 understands Week 3 before arriving there.

### 4.2 Track B1 — .NET / C#

| Day | Topics | Official documentation |
|---|---|---|
| **D1** | C# syntax, value vs reference types, nullable reference types, records, pattern matching, `dotnet` CLI, NuGet, solution/project layout | [C# guide](https://learn.microsoft.com/en-us/dotnet/csharp/) · [C# fundamentals](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/) · [.NET CLI](https://learn.microsoft.com/en-us/dotnet/core/tools/) |
| **D2** | Classes, interfaces, abstract classes, inheritance vs composition, generics, extension methods, SOLID, DI container | [OOP in C#](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/) · [Interfaces](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/interfaces) · [Generics](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics) · [Dependency injection](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection) |
| **D3** | Collections, **LINQ** (method and query syntax, deferred execution, `IQueryable` vs `IEnumerable`), `async`/`await`, `Task`, `CancellationToken`, exception handling | [LINQ](https://learn.microsoft.com/en-us/dotnet/csharp/linq/) · [Collections](https://learn.microsoft.com/en-us/dotnet/standard/collections/) · [Async programming](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/) · [Exceptions](https://learn.microsoft.com/en-us/dotnet/standard/exceptions/) |
| **D4** | ASP.NET Core: minimal APIs vs controllers, routing, **middleware pipeline**, filters, model binding, validation, `IOptions` configuration, logging | [ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/) · [Web API](https://learn.microsoft.com/en-us/aspnet/core/web-api/) · [Middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/) · [Routing](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/routing) · [Configuration](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/configuration/) |
| **D5** | EF Core (DbContext, migrations, tracking, relationships), xUnit, Moq, FluentValidation, **microservices architecture**, resilience with Polly | [EF Core](https://learn.microsoft.com/en-us/ef/core/) · [Unit testing](https://learn.microsoft.com/en-us/dotnet/core/testing/unit-testing-with-dotnet-test) · [xUnit](https://xunit.net/) · [.NET microservices e-book](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/) · [Polly](https://www.pollydocs.org/) · [FluentValidation](https://docs.fluentvalidation.net/) |

### 4.3 Track B2 — Java / Spring Boot

| Day | Topics | Official documentation |
|---|---|---|
| **D1** | Java syntax, primitives vs objects, records, sealed types, `Optional`, Maven or Gradle, project layout | [dev.java learn](https://dev.java/learn/) · [Java SE API docs](https://docs.oracle.com/en/java/javase/21/docs/api/index.html) · [Maven guides](https://maven.apache.org/guides/) · [Gradle user manual](https://docs.gradle.org/current/userguide/userguide.html) |
| **D2** | Classes, interfaces, default methods, abstract classes, inheritance vs composition, generics, SOLID, Spring IoC container and bean scopes | [Java OOP tutorial](https://dev.java/learn/oop/) · [Spring Framework core (IoC & DI)](https://docs.spring.io/spring-framework/reference/core/beans.html) |
| **D3** | Collections framework, **Streams API** (map/filter/collect, lazy evaluation), `CompletableFuture`, virtual threads, exceptions | [Collections](https://dev.java/learn/api/collections-framework/) · [Streams](https://docs.oracle.com/en/java/javase/21/core/java-stream-api.html) · [Concurrency](https://dev.java/learn/concurrency/) |
| **D4** | Spring Boot: auto-configuration, `@RestController`, routing, **filters & interceptors**, `@Valid` bean validation, `application.yml` profiles, Actuator | [Spring Boot reference](https://docs.spring.io/spring-boot/index.html) · [Spring Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html) · [Validation](https://docs.spring.io/spring-framework/reference/core/validation.html) · [Actuator](https://docs.spring.io/spring-boot/reference/actuator/index.html) |
| **D5** | Spring Data JPA, entities and relationships, Flyway migrations, JUnit 5, Mockito, **microservices with Spring Cloud**, resilience patterns | [Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/) · [JUnit 5](https://junit.org/junit5/docs/current/user-guide/) · [Mockito](https://site.mockito.org/) · [Spring Cloud](https://spring.io/projects/spring-cloud) · [Resilience4j](https://resilience4j.readme.io/docs/getting-started) · [Flyway](https://documentation.red-gate.com/flyway) |

### 4.4 Track B3 — Node.js / NestJS

| Day | Topics | Official documentation |
|---|---|---|
| **D1** | Node runtime, event loop, modules (ESM vs CJS), npm/pnpm, **TypeScript** types, interfaces, generics, `tsconfig` | [Node.js learn](https://nodejs.org/en/learn) · [Node.js API](https://nodejs.org/docs/latest/api/) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) |
| **D2** | Classes, interfaces, decorators, composition, generics, SOLID, NestJS modules & providers, **dependency injection** | [NestJS providers](https://docs.nestjs.com/providers) · [NestJS modules](https://docs.nestjs.com/modules) · [TypeScript classes](https://www.typescriptlang.org/docs/handbook/2/classes.html) |
| **D3** | Array/iterator methods, `Map`/`Set`, Promises, `async`/`await`, `Promise.all` vs `allSettled`, `AbortController`, streams | [MDN — Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) · [MDN — Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) · [Node streams](https://nodejs.org/docs/latest/api/stream.html) |
| **D4** | NestJS controllers, routing, **middleware, guards, interceptors, pipes**, `class-validator`, `ConfigModule`, exception filters | [Controllers](https://docs.nestjs.com/controllers) · [Middleware](https://docs.nestjs.com/middleware) · [Guards](https://docs.nestjs.com/guards) · [Interceptors](https://docs.nestjs.com/interceptors) · [Validation](https://docs.nestjs.com/techniques/validation) · [Configuration](https://docs.nestjs.com/techniques/configuration) |
| **D5** | Prisma schema, migrations, relations; Jest + mocking; **NestJS microservices** (transports, message patterns), retries and timeouts | [Prisma](https://www.prisma.io/docs) · [Jest](https://jestjs.io/docs/getting-started) · [NestJS testing](https://docs.nestjs.com/fundamentals/testing) · [NestJS microservices](https://docs.nestjs.com/microservices/basics) |

### 4.5 Track B4 — Python / FastAPI

| Day | Topics | Official documentation |
|---|---|---|
| **D1** | Python typing, dataclasses, comprehensions, iterators & generators, context managers, decorators, packaging, `uv`, Ruff | [Python tutorial](https://docs.python.org/3/tutorial/) · [typing](https://docs.python.org/3/library/typing.html) · [uv](https://docs.astral.sh/uv/) · [Ruff](https://docs.astral.sh/ruff/) |
| **D2** | Classes, ABCs & protocols, inheritance vs composition, generics, SOLID, dependency injection patterns, **Pydantic v2** models | [Classes](https://docs.python.org/3/tutorial/classes.html) · [abc](https://docs.python.org/3/library/abc.html) · [Protocols (PEP 544)](https://peps.python.org/pep-0544/) · [Pydantic](https://docs.pydantic.dev/latest/) |
| **D3** | Collections, `itertools`, comprehension-based querying, `async`/`await`, `asyncio`, `TaskGroup`, timeouts, exceptions | [asyncio](https://docs.python.org/3/library/asyncio.html) · [collections](https://docs.python.org/3/library/collections.html) · [itertools](https://docs.python.org/3/library/itertools.html) · [Exceptions](https://docs.python.org/3/tutorial/errors.html) |
| **D4** | FastAPI routing, path/query/body params, **dependency injection**, middleware, response models, `BackgroundTasks`, settings, OpenAPI | [FastAPI](https://fastapi.tiangolo.com/) · [Dependencies](https://fastapi.tiangolo.com/tutorial/dependencies/) · [Middleware](https://fastapi.tiangolo.com/tutorial/middleware/) · [Settings](https://fastapi.tiangolo.com/advanced/settings/) |
| **D5** | SQLAlchemy 2.0 ORM, Alembic migrations, repository pattern, pytest + fixtures + mocking, **microservice communication**, `httpx` with retries | [SQLAlchemy ORM](https://docs.sqlalchemy.org/en/20/orm/) · [Alembic](https://alembic.sqlalchemy.org/en/latest/) · [pytest](https://docs.pytest.org/en/stable/) · [httpx](https://www.python-httpx.org/) · [FastAPI testing](https://fastapi.tiangolo.com/tutorial/testing/) |

**AI drill (Week 1).** Set up the assistant and write a project rules file (`CLAUDE.md` / `.cursorrules`) naming the stack and conventions. Then, on every lab: **write it yourself first, then ask AI to review it**, recording which suggestions you accepted and which you rejected with a reason. Reviewing beats generating as a way to learn a language.

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L4 and L5. The mentor asks: *"Add a fourth middleware that rate-limits by IP. Where in the pipeline does it go, and why there?"* and *"`Pricing` is down. Trace exactly what the user sees, and where in your code that behaviour is decided."* Passing requires answering without reading the code.

---

## 5. The Capstone Project — "OrderHub" (starts Day 6)

Every intern, on every track, builds the same product. This keeps mentoring, grading and peer review consistent, and lets interns on different tracks compare solutions directly.

> **OrderHub** — a mini order-management platform for a B2B merchant. A staff user signs in, manages products, creates and pays orders, uploads an invoice, and reads a daily sales report. The system emits an event when an order is paid; a separate service consumes it to build reports and send notifications.

> ### ⚠️ This exercise must be built as a microservice system.
> A monolith with two folders does not pass, however well it works. OrderHub is **three independently deployable services behind an API Gateway**, written in two different languages, communicating synchronously over HTTP and asynchronously over a message queue. The architecture is the assessment; the features are only the vehicle for it. §5.2 states the rules a mentor checks against.

### 5.1 Services

| Service | Language | Built in | Responsibility |
|---|---|---|---|
| **API Gateway** | Main language *or* a proxy (Traefik / Nginx / Kong) | Week 4 (D16) | The single public entry point. Routes to Core and Ops, terminates CORS, enforces edge rate limiting, forwards the JWT, and publishes **one aggregated Swagger UI** listing every service. See §5.3. |
| **Core API** | Main language | Weeks 2–3 | Auth (JWT), Users, Products, Orders, Payments, Invoice upload, Audit log. Publishes `order.paid` events. Owns the `users / products / orders / payments / invoices` tables. |
| **Ops Service** | Extension language | Week 3 | Consumes `order.paid`. Builds daily sales reports. Sends mock email/SMS notifications. Exposes `/reports` and `/notifications`. Runs a scheduled job. Owns the `reports / notifications` tables. |
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
                        │ (main lang) │   │ (ext. lang) │
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

> **This is L5 from Week 1 at production scale.** Rules 1, 2 and 5 are exactly what the `Catalog` / `Pricing` lab taught; rules 3, 4, 6, 7 and 8 are what production adds. Interns who understood L5 will recognise seven of these eight.

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
| **B1 .NET** | [YARP reverse proxy](https://microsoft.github.io/reverse-proxy/) | [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Kong](https://docs.konghq.com/) |
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

## 7. Week 2 — The Core Service *(capstone begins)*

**Theme:** From an empty repo to a secured, tested, documented API with a working order flow.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D6** | Repository setup · Git/PR flow · layered architecture (Controller → Service → Repository) · configuration & secrets · structured logging · Dockerfile · CI skeleton | Capstone repo scaffolded; `/health` and `/version` return 200; request-logging middleware with a correlation ID; **first PR merged**; CI runs build + lint | `docker build` succeeds; logs are JSON with a correlation ID; no hardcoded config |
| **D7** | Relational design · normalization · migrations · seeding | ERD for OrderHub (§5.4) + migrations applied to PostgreSQL + seed script (50 products, 5 users) | Migration runs from a clean database; the ERD is reviewed by the mentor |
| **D8** | CRUD · DTO vs entity · validation · error model · pagination, sorting, filtering · OpenAPI | Full `Products` endpoints + Swagger docs + `problem+json` error handler | Swagger UI is usable; invalid input returns 422 with field-level detail |
| **D9** | Authentication & authorization · password hashing · JWT access + refresh · role-based access · unit testing | `POST /auth/register`, `/auth/login`, `/auth/refresh`, `GET /me`; `Products` writes restricted to `ADMIN` | ≥10 unit tests pass; service-layer coverage ≥ 50% |
| **D10** | Order domain · transactions · optimistic concurrency · idempotency | `POST /orders` (multi-item, stock decrement inside a transaction), `GET /orders`, `GET /orders/{id}`, idempotency key on create | Two concurrent orders for the last unit → exactly one succeeds |

**Reading:** [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [JWT introduction](https://jwt.io/introduction) · [OAuth 2.0](https://oauth.net/2/) · [PostgreSQL docs](https://www.postgresql.org/docs/current/) · [Docker docs](https://docs.docker.com/) · [Conventional Commits](https://www.conventionalcommits.org/)

**AI drill (Week 2).** Use AI to generate boilerplate and to *explain* an unfamiliar open-source repository. **Every PR description must state which parts were AI-generated and what the intern changed after reviewing them.** Start the **AI Usage Journal** (`docs/ai-journal.md`) on D6.

**🚩 Gate 2 — Friday, 45 min.** Demo: register → login → create product → create an order → 401/403 behaviour. The mentor asks: *"Walk me through what happens between the HTTP request arriving and the order row being committed."* and *"Two customers buy the last unit at the same millisecond. What stops both succeeding, and where is that in your code?"*

---

## 8. Week 3 — Integration, Performance & the Second Language

**Theme:** Make the Core Service production-grade, then cross the language boundary and make two services talk. This is L5 at production scale.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D11** | Third-party integration · timeouts · retry with backoff · webhooks · signature verification · file upload | Mock payment provider: `POST /orders/{id}/pay` + `POST /webhooks/payment` moving the order to `PAID`; `POST /orders/{id}/invoice` with type and size validation | Webhook replay does not double-pay; a provider timeout does not hang the request; the 10 MB / MIME allow-list is enforced |
| **D12** | Caching (Redis, cache-aside) · N+1 detection · indexing · `EXPLAIN ANALYZE` · integration testing · CI | Cached product listing with explicit invalidation; before/after timings in `docs/performance.md`; ≥6 integration tests on the order flow; CI runs build → lint → unit → integration | `GET /products` p95 < 200 ms at 10k rows; the N+1 in order listing is found and fixed; CI green with a README badge |
| **D13** | Extension language crash course (runtime, package manager, project layout, typing, testing) · Ops Service scaffold | Ops Service running in Docker against the same PostgreSQL; `GET /reports/daily` returns seeded data | The service starts from compose; its logs match the Core API's JSON format; the intern lists 5 idiom differences vs their main language |
| **D14** | Messaging · RabbitMQ · publish/subscribe · retry & dead-letter queue | Core publishes `order.paid`; Ops consumes it and stores a report row; failures route to a DLQ | Killing Ops mid-flight loses no message; a poisoned message lands in the DLQ instead of looping forever |
| **D15** | Scheduled jobs · notification delivery · full-system composition · E2E smoke test · cross-language reflection | Scheduled report job + mock email/SMS sender + `GET /notifications`; `docker compose up` runs Core + Ops + Postgres + Redis + RabbitMQ; `scripts/smoke.sh` covers the happy path; `docs/language-comparison.md` | The job is idempotent; clean clone → compose up → smoke script passes with zero manual steps |

**Reading:** [PostgreSQL EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html) · [Redis docs](https://redis.io/docs/latest/) · [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials) · [Microservices patterns](https://microservices.io/patterns/index.html) · [Docker Compose](https://docs.docker.com/compose/) · [Testcontainers](https://testcontainers.com/) · [GitHub Actions](https://docs.github.com/en/actions) · [Stripe webhooks (reference design)](https://docs.stripe.com/webhooks) · plus the extension-language docs from §4.2–§4.5

**AI drill (Week 3).** Use AI to port code between languages — then **hunt the idiomatic gaps**. `docs/language-comparison.md` must name at least three places where the AI's translation was technically correct but not idiomatic, and how the intern fixed it. This is the month's strongest test of the JD's "critical thinking about AI output" requirement.

> **Week 3 is the densest week in the plan.** If the intern is behind by Wednesday, cut in this order: the scheduled job → mock notifications → the caching layer. **Never cut the message queue** — it is the graded microservice deliverable.

**🚩 Gate 3 — Friday, 45 min.** Live `docker compose up` from a fresh clone on the mentor's machine, then a walk through the **§5.2 microservice rules** one by one — the mentor checks rules 1–6 directly (rules 7 and 8 land with the gateway on D16). Then: *"Why this messaging pattern, and what breaks if the Ops Service is down for an hour?"* and *"Show me where the Ops Service reads order data. If it touches the `orders` table, explain why that is a problem."*

---

## 9. Week 4 — API Gateway, Fullstack Slice & Ship

**Theme:** Put the front door on the microservice system, close the loop to the user, then make it safe, observable and presentable.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D16** | **API Gateway** · reverse proxy & path routing · CORS at the edge · rate limiting · JWT pass-through · correlation-ID propagation · **aggregated Swagger UI** | The Gateway per §5.3.1: `/api/core/**` and `/api/ops/**` routed; `/docs` renders one Swagger UI with a service selector; only the gateway publishes a host port; all Swagger URLs listed in the README; an ADR justifying the gateway choice | A reviewer opens `http://localhost:8080/docs`, picks either service, and executes a request successfully. Core and Ops are unreachable from the host. |
| **D17** | Admin Console — **AI-assisted (§5.3.2)** · typed API client pointed at the gateway · loading / error / empty states | All four pages: login, orders list (filter + pagination), order detail, daily report | Every call goes through the gateway — no service port appears anywhere in the frontend. Login survives a refresh; all three states handled on every page; usable at 1280 px and 768 px |
| **D18** | Security hardening · OWASP API Security Top 10 · the gateway as the security boundary · dependency audit | `docs/security-review.md` applying the Top 10 to OrderHub + every finding fixed or ticketed | No high-severity dependency advisory; no secret in Git history; JWT expiry and rotation verified; services confirmed not directly reachable |
| **D19** | Observability & load testing · readiness/liveness across all three services · end-to-end correlation ID · bottleneck fix | Load test at 50 VUs **through the gateway** + `docs/performance.md` updated · **code freeze at 17:00** | One request ID traced gateway → Core → Ops consumer; one measured bottleneck identified and improved, with before/after numbers |
| **D20** | Documentation · rehearsal · **Demo Day** · evaluation | Final README (with the Swagger URL table), `docs/events.md`, ADRs in `docs/adr/`, the demo (§9.1), AI-usage retrospective, signed scorecard + Individual Development Plan for months 2–6 | Demo delivered; scorecard completed; IDP agreed |

**Reading:** [YARP](https://microsoft.github.io/reverse-proxy/) · [Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/reference/) · [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Swagger UI configuration](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/) · [springdoc-openapi](https://springdoc.org/) · [API gateway pattern](https://microservices.io/patterns/apigateway.html) · [Next.js docs](https://nextjs.org/docs) · [TailwindCSS](https://tailwindcss.com/docs) · [OWASP API Security Top 10](https://owasp.org/www-project-api-security/) · [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) · [Grafana k6](https://grafana.com/docs/k6/latest/)

**AI drill (Week 4).** D17 is the month's one sanctioned "let AI build it" day — generate the Admin Console, then review it as if a junior had written it and journal what you changed. Elsewhere, use AI to draft API documentation, generate the security checklist and propose refactors. Note the contrast in your retrospective: AI is excellent at the console, and much weaker at the gateway routing and Swagger aggregation, where the context is your specific system.

### 9.1 Demo Day agenda (20 minutes + 10 minutes questions)

| Minutes | Segment |
|---:|---|
| 0–2 | The problem and the **architecture diagram**: three services, two languages, one gateway |
| 2–5 | **Open the aggregated Swagger UI**, switch between services, execute a live request from the browser |
| 5–10 | End-to-end walkthrough through the gateway: login → order → payment → `order.paid` event → report → admin console |
| 10–13 | **Microservice evidence:** stop the Ops Service live and show the system degrade instead of break; show a message land in the DLQ; trace one correlation ID across three logs |
| 13–16 | Quality evidence: CI run, test coverage, performance before/after, security review |
| 16–18 | The two-language experience: what transferred, what did not |
| 18–20 | **AI usage retrospective:** where AI saved the most time (the console), where it was wrong, what changed in how you work |
| +10 | Mentor and team questions |

---

## 10. AI Adoption — Mandatory Standards

The JD makes AI adoption a hiring requirement, not a bonus. These rules are enforced in code review from Day 1.

### 10.1 Required practice

- Use one assistant daily (Claude Code / Cursor / GitHub Copilot or equivalent).
- Write **contextual prompts**: state the stack, the constraint and the desired output format. "Add pagination" is a weak prompt. "Add cursor-based pagination to `GET /products` in ASP.NET Core, returning `{items, nextCursor}`, keeping the existing `ProblemDetails` error shape" is a working prompt.
- Break large requests into steps. One prompt should map to roughly one PR-sized change.
- Use AI where it pays: boilerplate, unit tests, refactors, explaining unfamiliar code, reading stack traces, optimizing SQL, drafting API docs.

### 10.2 Hard rules — a violation blocks the merge

> **Never** paste credentials, customer data, or NDA-scope source code into a public AI tool.
> **Never** commit code you cannot explain line by line. Mentors will ask at random, in review.
> **Always** run and test AI-generated code before pushing. Generated tests that pass trivially score zero.
> **Always** disclose AI-generated sections in the PR description.

### 10.3 Graded artefact

`docs/ai-journal.md` — maintained from D6 to D20, reviewed at Gates 2 and 3 and at Demo Day. **Minimum 12 entries.** An entry that only says "used AI to write code" scores zero; each entry must name the failure mode the intern caught.

---

## 11. Evaluation

### 11.1 Scorecard (100 points)

| # | Criterion | Weight | What "excellent" looks like |
|---|---|---:|---|
| 1 | **Core language & framework fundamentals** (Week 1 labs) | 10 | All five labs complete; can explain the middleware pipeline and the L5 failure modes unaided |
| 2 | **Functional completeness** | 15 | All OrderHub features work end-to-end across both services |
| 3 | **Code quality & convention** | 12 | Consistent layering, meaningful names, no dead code, clean PR history |
| 4 | **API & data design** | 8 | Sensible schema and indexes, coherent REST contract, correct status codes |
| 5 | **Testing & CI** | 12 | Meaningful unit + integration tests, green pipeline, ≥60% service-layer coverage |
| 6 | **Microservice architecture** (§5.2 rules) | 10 | All eight rules hold; the Ops Service is idiomatic in its language, not a transliteration; messaging survives a failed consumer |
| 7 | **API Gateway & API documentation** | 8 | Single entry point enforced; routing, CORS, rate limiting and correlation ID working; **aggregated Swagger UI executable and every URL documented** |
| 8 | **Security & performance** | 5 | Top 10 checklist applied; a measured, documented performance improvement |
| 9 | **AI adoption & critical thinking** | 10 | Strong journal; can name concrete AI mistakes they caught — including in the AI-generated console |
| 10 | **Admin Console** | 5 | All four pages work through the gateway with complete loading/error/empty states; the intern can explain the generated code |
| 11 | **Communication, docs & demo** | 5 | Clear README and ADRs; confident, structured demo |

### 11.2 Decision bands

| Score | Outcome |
|---|---|
| **≥ 85** | **Strong pass.** Assign to a client project as a contributor; open the Fresher conversation early. |
| **70 – 84** | **Pass.** Continue to month 2 on a real project with normal supervision. |
| **55 – 69** | **Conditional.** A two-week targeted remediation plan on the weakest two criteria, then re-assess. |
| **< 55** | **Not passing.** Structured feedback and closure of the internship. |

**Non-negotiable minimums, regardless of total score:** CI green on `main` · no secrets in Git · the intern can explain any randomly selected file they committed.

---

## 12. Mentor Playbook

| Cadence | Duration | Purpose |
|---|---|---|
| Daily standup | 15 min | Unblock — not status theatre |
| PR review | ≤ 4h SLA | Teach through comments; ask for a rewrite rather than fixing it yourself |
| Pairing (Tue, Thu) | 60 min | Intern drives, mentor navigates |
| Gate review (Fri) | 45 min | Demo + verbal defence + written feedback |
| 1:1 (Fri) | 15 min | Wellbeing, motivation, career direction |

**Mentor preparation before Day 1:** working devcontainer or setup script · the OrderHub API contract and ERD reference solution · seed data · the Figma-free UI wireframe for Week 4 · GitHub repo template with CI · the track decision made from the interview notes.

**Escalate to the team lead the same day if:**

- The intern is more than 2 days behind the plan at any gate.
- The intern cannot explain their own code twice in one week.
- Any hard AI rule in §10.2 is violated.

**The mentor's most important habit:** when the intern is stuck, ask *"what have you already tried, and what did you expect to happen?"* before giving any answer.

---

## 13. Reference Library

> Every link points to primary/official documentation (vendor docs, MDN, W3C, RFCs). Documentation sites reorganize: if a URL 404s, search the same official domain rather than following a third-party mirror or a dated blog post.

### 13.1 .NET / C#
[C# guide](https://learn.microsoft.com/en-us/dotnet/csharp/) · [C# fundamentals](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/) · [OOP](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/object-oriented/) · [Interfaces](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/interfaces) · [Generics](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/types/generics) · [LINQ](https://learn.microsoft.com/en-us/dotnet/csharp/linq/) · [Async](https://learn.microsoft.com/en-us/dotnet/csharp/asynchronous-programming/) · [ASP.NET Core](https://learn.microsoft.com/en-us/aspnet/core/) · [Web API](https://learn.microsoft.com/en-us/aspnet/core/web-api/) · [Middleware](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/middleware/) · [DI](https://learn.microsoft.com/en-us/dotnet/core/extensions/dependency-injection) · [EF Core](https://learn.microsoft.com/en-us/ef/core/) · [Microservices e-book](https://learn.microsoft.com/en-us/dotnet/architecture/microservices/) · [xUnit](https://xunit.net/) · [FluentValidation](https://docs.fluentvalidation.net/) · [Polly](https://www.pollydocs.org/) · [Serilog](https://serilog.net/)

### 13.2 Java / Spring
[dev.java](https://dev.java/learn/) · [Java SE API](https://docs.oracle.com/en/java/javase/21/docs/api/index.html) · [Spring Boot](https://docs.spring.io/spring-boot/index.html) · [Spring Framework core](https://docs.spring.io/spring-framework/reference/core.html) · [Spring Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html) · [Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/) · [Spring Security](https://docs.spring.io/spring-security/reference/) · [Spring Cloud](https://spring.io/projects/spring-cloud) · [JUnit 5](https://junit.org/junit5/docs/current/user-guide/) · [Mockito](https://site.mockito.org/) · [Resilience4j](https://resilience4j.readme.io/docs/getting-started) · [Maven](https://maven.apache.org/guides/) · [Gradle](https://docs.gradle.org/current/userguide/userguide.html)

### 13.3 Node.js / NestJS / TypeScript
[Node.js learn](https://nodejs.org/en/learn) · [Node.js API](https://nodejs.org/docs/latest/api/) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [NestJS](https://docs.nestjs.com/) · [NestJS microservices](https://docs.nestjs.com/microservices/basics) · [Express](https://expressjs.com/) · [Prisma](https://www.prisma.io/docs) · [Jest](https://jestjs.io/docs/getting-started) · [BullMQ](https://docs.bullmq.io/) · [MDN JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

### 13.4 Python / FastAPI
[Python tutorial](https://docs.python.org/3/tutorial/) · [typing](https://docs.python.org/3/library/typing.html) · [asyncio](https://docs.python.org/3/library/asyncio.html) · [FastAPI](https://fastapi.tiangolo.com/) · [Pydantic](https://docs.pydantic.dev/latest/) · [SQLAlchemy](https://docs.sqlalchemy.org/en/20/) · [Alembic](https://alembic.sqlalchemy.org/en/latest/) · [pytest](https://docs.pytest.org/en/stable/) · [httpx](https://www.python-httpx.org/) · [uv](https://docs.astral.sh/uv/) · [Ruff](https://docs.astral.sh/ruff/) · [Celery](https://docs.celeryq.dev/en/stable/)

### 13.5 Data, infrastructure & messaging
[PostgreSQL](https://www.postgresql.org/docs/current/) · [PostgreSQL EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html) · [Redis](https://redis.io/docs/latest/) · [MongoDB](https://www.mongodb.com/docs/) · [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials) · [Apache Kafka](https://kafka.apache.org/documentation/) · [MinIO](https://min.io/docs/minio/linux/index.html) · [Docker](https://docs.docker.com/) · [Docker Compose](https://docs.docker.com/compose/) · [Testcontainers](https://testcontainers.com/) · [Grafana k6](https://grafana.com/docs/k6/latest/)

### 13.6 API Gateway & Swagger aggregation
[API gateway pattern](https://microservices.io/patterns/apigateway.html) · [YARP reverse proxy (.NET)](https://microsoft.github.io/reverse-proxy/) · [Spring Cloud Gateway (Java)](https://docs.spring.io/spring-cloud-gateway/reference/) · [http-proxy-middleware (Node)](https://github.com/chimurai/http-proxy-middleware) · [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Kong](https://docs.konghq.com/) · [Swagger UI configuration (`urls` for multiple specs)](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/) · [springdoc-openapi](https://springdoc.org/) · [Swashbuckle (.NET)](https://github.com/domaindrivendev/Swashbuckle.AspNetCore)

### 13.7 Architecture, API & practice
[Microservices patterns](https://microservices.io/patterns/index.html) · [Database per service](https://microservices.io/patterns/data/database-per-service.html) · [The Twelve-Factor App](https://12factor.net/) · [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [RFC 7807 Problem Details](https://datatracker.ietf.org/doc/html/rfc7807) · [MDN HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) · [JWT introduction](https://jwt.io/introduction) · [OAuth 2.0](https://oauth.net/2/) · [OWASP API Security Top 10](https://owasp.org/www-project-api-security/) · [OWASP Cheat Sheets](https://cheatsheetseries.owasp.org/) · [Git documentation](https://git-scm.com/doc) · [GitHub Actions](https://docs.github.com/en/actions) · [Conventional Commits](https://www.conventionalcommits.org/) · [Semantic Versioning](https://semver.org/) · [Google Engineering Practices — Code Review](https://google.github.io/eng-practices/review/)

### 13.8 Frontend slice (Week 4)
[Next.js](https://nextjs.org/docs) · [React](https://react.dev/learn) · [TailwindCSS](https://tailwindcss.com/docs) · [MDN CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

---

## 14. Appendix A — Definition of Done (every PR)

- [ ] Branch named `feat/…`, `fix/…` or `chore/…`; commits follow Conventional Commits
- [ ] PR description states: what, why, how to test, and **which parts were AI-generated**
- [ ] Unit tests added or updated; CI green
- [ ] No secret, no commented-out code, no `TODO` without a ticket reference
- [ ] OpenAPI updated if the contract changed — and the spec still loads in the aggregated Swagger UI
- [ ] No cross-service database access introduced (§5.2 rule 3); no service port hardcoded outside the gateway config (§5.2 rule 7)
- [ ] Diff self-reviewed before requesting review

## 15. Appendix B — Week-to-Date Planner

| Week | Days | Theme | Checkpoint | Dates (fill in) |
|---|---|---|---|---|
| 1 | D1–D5 | Core language & framework fundamentals | Gate 1 | ______ – ______ |
| 2 | D6–D10 | Capstone starts — the Core Service | Gate 2 | ______ – ______ |
| 3 | D11–D15 | Integration, performance & the second language | Gate 3 | ______ – ______ |
| 4 | D16–D20 | API Gateway, fullstack slice & ship | **Demo Day** | ______ – ______ |

## 16. Appendix C — Risks, Remediation & the Scope-Cut Ladder

### 16.1 Risks

| Risk | Early signal | Mentor action |
|---|---|---|
| Main language weaker than the interview suggested | Struggles on Week 1 D2–D3 | Compress D3, move L3 to homework; protect D4–D5 — the framework and microservice labs matter most |
| Week 1 feels too easy for a strong intern | L1–L3 finished by lunch | Skip ahead to L4/L5 and add a stretch lab: put an API gateway in front of the two L5 services |
| Extension language overwhelms the intern | D13 scaffold incomplete | Reduce the Ops Service to a consumer + one report endpoint; keep the queue |
| Over-reliance on AI, shallow understanding | Cannot explain a lab at Gate 1 | One mandatory AI-free day; the intern rebuilds a lab unaided |
| Environment and Docker problems eat days | D6 slips | Provide a prepared devcontainer; setup is mentor-owned, not intern-owned |
| Scope creep by an enthusiastic intern | Extra features, missing tests | Freeze scope at §5; excess goes to the §5.6 stretch backlog |

### 16.2 Scope-cut ladder — drop in this order if the intern falls behind

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
