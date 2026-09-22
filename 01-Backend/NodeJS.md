# Backend Track — Node.js / NestJS
### VATEK Internship Program 2026 · Backend Developer Intern (Fullstack-oriented)

| | |
|---|---|
| **Language** | Node.js / NestJS — the whole month: labs, both microservices and the API Gateway |
| **Capstone** | OrderHub — order microservices behind an API Gateway with aggregated Swagger |
| **Duration** | 20 working days · Week 1 fundamentals → Weeks 2–4 OrderHub capstone → Demo Day |
| **Programme guide** | [README.md](README.md) — capstone spec, microservice rules, gates, grading, AI rules |

> **Read-only reference.** This document describes what to learn and build, week by week. Do all of the work in your own repository — nothing here needs to be copied or edited. Each week ends with the deliverables your mentor reviews at that Friday's gate; the rules you are graded against are in the [programme guide](README.md).

---

## Your Stack

| Layer | Technology |
|---|---|
| **Core API** | NestJS · TypeScript (strict) · Prisma · class-validator · Passport + `@nestjs/jwt` · Jest · `@nestjs/swagger` · pino |
| **Ops Service** | The same stack — a separate project, its own database tables, its own container |
| **API Gateway** | NestJS gateway app + http-proxy-middleware — or Traefik / Nginx |
| **Admin Console** | Next.js 15 · TypeScript · TailwindCSS — AI-assisted (README §5.3.2) |
| **Shared infrastructure** | PostgreSQL 16 · Redis 7 · RabbitMQ 3 · Docker Compose · GitHub Actions |

---

## Before Day 1 — Environment Checklist

- Node.js 22 LTS + pnpm
- VS Code (+ ESLint, Prettier, Prisma extensions)
- NestJS CLI: `pnpm add -g @nestjs/cli`
- The Week 1 lab dataset — `orders.json` and the other files in [`datasets/`](../datasets/README.md)
- Docker Desktop (with Docker Compose v2) — `docker compose version` works
- Git configured with your company identity; SSH key added to GitHub
- A REST client: Postman, Bruno, or VS Code REST Client
- Your AI coding assistant installed and signed in (Claude Code / Cursor / Copilot)
- Read the programme guide [README.md](README.md) §1–§6 once, end to end

---

## Week 1 — Core Language & Framework Fundamentals (D1–D5)

**Theme:** Before building the product, own the language. Mornings are concepts and reading; afternoons are graded micro-exercises (**labs**) in a `/labs` folder, kept separate from the capstone repository.

| Day | Topics — Node.js | Lab (afternoon) | Done when |
|---|---|---|---|
| **D1** | Node runtime, event loop, modules (ESM vs CJS), npm/pnpm, **TypeScript** types, interfaces, generics, `tsconfig` | **L1 — Order report CLI.** Read `orders.json` (200 orders with customer, status, line items), filter by status and date range, compute revenue per day and per status, print a report | Runs from the CLI; totals match the answer key; a debugger breakpoint is hit and stepped through in front of the mentor |
| **D2** | Classes, interfaces, decorators, composition, generics, SOLID, NestJS modules & providers, **dependency injection** | **L2 — Refactor L1** behind an `IOrderRepository` interface with two implementations (JSON file, in-memory), plus an `OrderTotalCalculator` wired by constructor injection | Swapping repository implementations requires no change inside the report logic |
| **D3** | Array/iterator methods, `Map`/`Set`, Promises, `async`/`await`, `Promise.all` vs `allSettled`, `AbortController`, streams | **L3 — Async order enrichment.** Group orders by customer with the language's query idiom, then fetch shipping status for 20 orders concurrently from a mock endpoint, with a timeout | The concurrent version is measurably faster than sequential; a slow shipping call times out and is reported, not swallowed |
| **D4** | NestJS controllers, routing, **middleware, guards, interceptors, pipes**, `class-validator`, `ConfigModule`, exception filters | **L4 — Mini Orders API.** 5 REST endpoints — list, get, create, change status, cancel order — + 3 custom middleware (request logging, correlation ID, global exception handler) | Middleware order is explained correctly; an invalid status transition returns a clean JSON error, never a stack trace |
| **D5** | Prisma schema, migrations, relations; Jest + mocking; **NestJS microservices** (transports, message patterns), retries and timeouts | **L5 — Split the Orders API into two services:** `Orders` and `Pricing` (line-item totals, discounts, tax), communicating over HTTP with a timeout, retry and fallback. ≥8 unit tests with mocks | Killing `Pricing` degrades `Orders` gracefully — orders still list, with totals marked `pending` — instead of hanging or returning 500 |

> **L5 is the spine of the whole month.** The capstone in Weeks 2–4 is the same pattern at production scale: two independently deployable order services, synchronous calls plus asynchronous events. An intern who understands L5 understands Week 3 before arriving there.

### 📚 Week 1 reading — official documentation

| Day | Documentation |
|---|---|
| **D1** | [Node.js learn](https://nodejs.org/learn) · [Node.js API](https://nodejs.org/docs/latest/api/) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) |
| **D2** | [NestJS providers](https://docs.nestjs.com/providers) · [NestJS modules](https://docs.nestjs.com/modules) · [TypeScript classes](https://www.typescriptlang.org/docs/handbook/2/classes.html) |
| **D3** | [MDN — Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) · [MDN — Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) · [Node streams](https://nodejs.org/docs/latest/api/stream.html) |
| **D4** | [Controllers](https://docs.nestjs.com/controllers) · [Middleware](https://docs.nestjs.com/middleware) · [Guards](https://docs.nestjs.com/guards) · [Interceptors](https://docs.nestjs.com/interceptors) · [Validation](https://docs.nestjs.com/techniques/validation) · [Configuration](https://docs.nestjs.com/techniques/configuration) |
| **D5** | [Prisma](https://www.prisma.io/docs) · [Jest](https://jestjs.io/docs/getting-started) · [NestJS testing](https://docs.nestjs.com/fundamentals/testing) · [NestJS microservices](https://docs.nestjs.com/microservices/basics) |

**AI drill (Week 1).** Set up the assistant and write a project rules file (`CLAUDE.md` / `.cursorrules`) naming the stack and conventions. Then, on every lab: **write it yourself first, then ask AI to review it**, recording which suggestions you accepted and which you rejected with a reason. Reviewing beats generating as a way to learn a language.

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L4 and L5. The mentor asks: *"Add a fourth middleware that rate-limits by IP. Where in the pipeline does it go, and why there?"* and *"`Pricing` is down. Trace exactly what the user sees, and where in your code that behaviour is decided."* Passing requires answering without reading the code.

### ✅ Week 1 deliverables

What must exist in your own repository by Gate 1 (end of Week 1):

- **D1** — L1 — Order report CLI
- **D2** — L2 — `IOrderRepository` + DI refactor
- **D3** — L3 — async order enrichment
- **D4** — L4 — Mini Orders API + 3 middleware
- **D5** — L5 — `Orders` + `Pricing` microservices

---

## Week 2 — The Core Service *(capstone begins)* (D6–D10)

**Theme:** From an empty repo to a secured, tested, documented API with a working order flow.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D6** | Repository setup · Git/PR flow · layered architecture (Controller → Service → Repository) · configuration & secrets · structured logging · Dockerfile · CI skeleton | Capstone repo scaffolded; `/health` and `/version` return 200; request-logging middleware with a correlation ID; **first PR merged**; CI runs build + lint | `docker build` succeeds; logs are JSON with a correlation ID; no hardcoded config |
| **D7** | Relational design · normalization · migrations · seeding | ERD for OrderHub (README §5.4) + migrations applied to PostgreSQL + seed script (50 products, 5 users) | Migration runs from a clean database; the ERD is reviewed by the mentor |
| **D8** | CRUD · DTO vs entity · validation · error model · pagination, sorting, filtering · OpenAPI | Full `Products` endpoints — the catalog that every order line item references — + Swagger docs + `problem+json` error handler | Swagger UI is usable; invalid input returns 422 with field-level detail |
| **D9** | Authentication & authorization · password hashing · JWT access + refresh · role-based access · unit testing | `POST /auth/register`, `/auth/login`, `/auth/refresh`, `GET /me`; `Products` writes restricted to `ADMIN` | ≥10 unit tests pass; service-layer coverage ≥ 50% |
| **D10** | Order domain · transactions · optimistic concurrency · idempotency | `POST /orders` (multi-item, stock decrement inside a transaction), `GET /orders`, `GET /orders/{id}`, idempotency key on create | Two concurrent orders for the last unit → exactly one succeeds |

### 🔧 Your stack this week — Node.js

| Day | Tools & commands |
|---|---|
| D6 | `nest new` · nestjs-pino JSON logging · `@nestjs/config` with env validation · multi-stage Dockerfile (`node:22-alpine`) · GitHub Actions: `pnpm build && pnpm lint` |
| D7 | Prisma schema · `prisma migrate dev` / `prisma migrate deploy` · `prisma db seed` |
| D8 | Controllers + DTOs · global `ValidationPipe` with class-validator · exception filter returning problem+json · `@nestjs/swagger` · skip/take pagination |
| D9 | `@nestjs/jwt` + Passport JWT strategy · bcrypt · `RolesGuard` + `@Roles('ADMIN')` · Jest with mocked providers · `jest --coverage` |
| D10 | Prisma interactive transaction (`$transaction`) · optimistic concurrency via a `version` column + conditional `updateMany` · an `idempotency_keys` table |

**Reading:** [OpenAPI Specification](https://spec.openapis.org/oas/latest.html) · [JWT introduction](https://jwt.io/introduction) · [OAuth 2.0](https://oauth.net/2/) · [PostgreSQL docs](https://www.postgresql.org/docs/current/) · [Docker docs](https://docs.docker.com/) · [Conventional Commits](https://www.conventionalcommits.org/)

**AI drill (Week 2).** Use AI to generate boilerplate and to *explain* an unfamiliar open-source repository. **Every PR description must state which parts were AI-generated and what the intern changed after reviewing them.** Start the **AI Usage Journal** (`docs/ai-journal.md`) on D6.

**🚩 Gate 2 — Friday, 45 min.** Demo: register → login → create product → create an order → 401/403 behaviour. The mentor asks: *"Walk me through what happens between the HTTP request arriving and the order row being committed."* and *"Two customers buy the last unit at the same millisecond. What stops both succeeding, and where is that in your code?"*

### ✅ Week 2 deliverables

What must exist in your own repository by Gate 2 (end of Week 2):

- **D6** — Repo scaffold · `/health` · JSON logging · Dockerfile · CI skeleton · first PR merged
- **D7** — ERD · migrations · seed data
- **D8** — Product catalog CRUD (order line items) · validation · problem+json · Swagger
- **D9** — Auth (JWT + roles) · ≥10 unit tests
- **D10** — Orders · transaction · concurrency · idempotency

---

## Week 3 — Integration, Performance & the Second Service (D11–D15)

**Theme:** Make the Core Service production-grade, then build the second service and make the two talk. This is L5 at production scale.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D11** | Third-party integration · timeouts · retry with backoff · webhooks · signature verification · file upload | Mock payment provider: `POST /orders/{id}/pay` + `POST /webhooks/payment` moving the order to `PAID`; `POST /orders/{id}/invoice` with type and size validation | Webhook replay does not double-pay; a provider timeout does not hang the request; the 10 MB / MIME allow-list is enforced |
| **D12** | Caching (Redis, cache-aside) · N+1 detection · indexing · `EXPLAIN ANALYZE` · integration testing · CI | Cached product listing with explicit invalidation; before/after timings in `docs/performance.md`; ≥6 integration tests on the order flow; CI runs build → lint → unit → integration | `GET /products` p95 < 200 ms at 10k rows; the N+1 in order listing is found and fixed; CI green with a README badge |
| **D13** | Service boundaries · a second independently deployable service · shared conventions (logging, config, error model) **without** shared code | Ops Service running in Docker with its **own** tables (`reports`, `notifications`); `GET /reports/daily` returns seeded data | The service starts from compose on its own; its logs match the Core API's JSON format; it shares no source code and no tables with the Core API |
| **D14** | Messaging · RabbitMQ · publish/subscribe · retry & dead-letter queue | Core publishes `order.paid`; Ops consumes it and stores a report row; failures route to a DLQ | Killing Ops mid-flight loses no message; a poisoned message lands in the DLQ instead of looping forever |
| **D15** | Scheduled jobs · notification delivery · full-system composition · E2E smoke test · architecture write-up | Scheduled report job + mock email/SMS sender + `GET /notifications`; `docker compose up` runs Core + Ops + Postgres + Redis + RabbitMQ; `scripts/smoke.sh` covers the happy path; `docs/architecture.md` (service boundaries, event contract, failure modes) | The job is idempotent; clean clone → compose up → smoke script passes with zero manual steps |

### 🔧 Your stack this week — Node.js

| Day | Tools & commands |
|---|---|
| D11 | `@nestjs/axios` + RxJS `timeout` / `retry` · HMAC check with `crypto.timingSafeEqual` · `FileInterceptor` (Multer) with size/MIME limits |
| D12 | `@nestjs/cache-manager` + Redis store · Prisma `include` + query logging · Supertest e2e + Testcontainers for Node.js |
| D13 | A second NestJS application, `ops-service`, with its own Prisma schema (`reports`, `notifications` only) · nestjs-pino · Dockerfile |
| D14 | **Core:** `ClientProxy.emit('order.paid', …)` over the RMQ transport after commit<br>**Ops:** `@EventPattern('order.paid')` with `noAck: false`, an explicit `channel.ack`, retry and a dead-letter exchange |
| D15 | `@nestjs/schedule` `@Cron` nightly order report (idempotent upsert) · mock email/SMS provider · one compose file for both services |

**Reading:** [PostgreSQL EXPLAIN](https://www.postgresql.org/docs/current/using-explain.html) · [Redis docs](https://redis.io/docs/latest/) · [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials) · [Microservices patterns](https://microservices.io/patterns/index.html) · [Docker Compose](https://docs.docker.com/compose/) · [Testcontainers](https://testcontainers.com/) · [GitHub Actions](https://docs.github.com/en/actions) · [Stripe webhooks (reference design)](https://docs.stripe.com/webhooks)

**AI drill (Week 3).** Give AI your ERD and both services' responsibilities and ask it to review the service boundary — then check every suggestion against the README §5.2 rules. AI reviewers often recommend a shared database or a shared model library, and both break the microservice rules. `docs/architecture.md` must record at least three AI suggestions you rejected, and why. This is the month's strongest test of the JD's "critical thinking about AI output" requirement.

> **Week 3 is the densest week in the plan.** If the intern is behind by Wednesday, cut in this order: the scheduled job → mock notifications → the caching layer. **Never cut the message queue** — it is the graded microservice deliverable.

**🚩 Gate 3 — Friday, 45 min.** Live `docker compose up` from a fresh clone on the mentor's machine, then a walk through the **README §5.2 microservice rules** one by one — the mentor checks rules 1–6 directly (rules 7 and 8 land with the gateway on D16). Then: *"Why this messaging pattern, and what breaks if the Ops Service is down for an hour?"* and *"Show me where the Ops Service reads order data. If it touches the `orders` table, explain why that is a problem."*

### ✅ Week 3 deliverables

What must exist in your own repository by Gate 3 (end of Week 3):

- **D11** — Payment + webhook · invoice upload
- **D12** — Redis cache · N+1 fix · integration tests · CI green
- **D13** — Ops Service scaffold — second service, own tables
- **D14** — `order.paid` published (Core) and consumed (Ops) · retry · DLQ
- **D15** — Scheduled order report · notifications · full compose · smoke test · `docs/architecture.md`

---

## Week 4 — API Gateway, Fullstack Slice & Ship (D16–D20)

**Theme:** Put the front door on the microservice system, close the loop to the user, then make it safe, observable and presentable.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D16** | **API Gateway** · reverse proxy & path routing · CORS at the edge · rate limiting · JWT pass-through · correlation-ID propagation · **aggregated Swagger UI** | The Gateway per README §5.3.1: `/api/core/**` and `/api/ops/**` routed; `/docs` renders one Swagger UI with a service selector; only the gateway publishes a host port; all Swagger URLs listed in the README; an ADR justifying the gateway choice | A reviewer opens `http://localhost:8080/docs`, picks either service, and executes a request successfully. Core and Ops are unreachable from the host. |
| **D17** | Admin Console — **AI-assisted (README §5.3.2)** · typed API client pointed at the gateway · loading / error / empty states | All four pages: login, orders list (filter + pagination), order detail, daily report | Every call goes through the gateway — no service port appears anywhere in the frontend. Login survives a refresh; all three states handled on every page; usable at 1280 px and 768 px |
| **D18** | Security hardening · OWASP API Security Top 10 · the gateway as the security boundary · dependency audit | `docs/security-review.md` applying the Top 10 to OrderHub + every finding fixed or ticketed | No high-severity dependency advisory; no secret in Git history; JWT expiry and rotation verified; services confirmed not directly reachable |
| **D19** | Observability & load testing · readiness/liveness across all three services · end-to-end correlation ID · bottleneck fix | Load test at 50 VUs **through the gateway** + `docs/performance.md` updated · **code freeze at 17:00** | One request ID traced gateway → Core → Ops consumer; one measured bottleneck identified and improved, with before/after numbers |
| **D20** | Documentation · rehearsal · **Demo Day** · evaluation | Final README (with the Swagger URL table), `docs/events.md`, ADRs in `docs/adr/`, the demo (README §7.5), AI-usage retrospective, signed scorecard + Individual Development Plan for months 2–6 | Demo delivered; scorecard completed; IDP agreed |

### 🔧 Your stack this week — Node.js

| Day | Tools & commands |
|---|---|
| D16 | **Native:** a small NestJS gateway app using http-proxy-middleware; Swagger UI via swagger-ui-express with `swaggerOptions.urls` listing both specs<br>**Or:** Traefik or Nginx as the gateway + the `swaggerapi/swagger-ui` container with `URLS` listing both specs |
| D17 | Next.js 15 App Router · TypeScript · TailwindCSS · your AI assistant to scaffold the pages · `NEXT_PUBLIC_API_BASE_URL` = the **gateway** URL |
| D18 | `pnpm audit` · helmet · `@nestjs/throttler` if not rate-limiting at the gateway |
| D19 | `@nestjs/terminus` health checks (Prisma + RabbitMQ) · k6 · pino correlation IDs |
| D20 | README with the Swagger URL table · `docs/events.md` · `docs/architecture.md` · ADRs in `docs/adr/` · an architecture diagram (Mermaid or draw.io) |

**Reading:** [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Swagger UI configuration](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/) · [API gateway pattern](https://microservices.io/patterns/apigateway.html) · [Next.js docs](https://nextjs.org/docs) · [TailwindCSS](https://tailwindcss.com/docs) · [OWASP API Security Top 10](https://owasp.github.io/API-Security/) · [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS) · [Grafana k6](https://grafana.com/docs/k6/latest/)

**AI drill (Week 4).** D17 is the month's one sanctioned "let AI build it" day — generate the Admin Console, then review it as if a junior had written it and journal what you changed. Elsewhere, use AI to draft API documentation, generate the security checklist and propose refactors. Note the contrast in your retrospective: AI is excellent at the console, and much weaker at the gateway routing and Swagger aggregation, where the context is your specific system.

### Demo Day agenda (20 minutes + 10 minutes questions)

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

### ✅ Week 4 deliverables

What must exist in your own repository by Demo Day:

- **D16** — API Gateway · aggregated Swagger UI · single entry point
- **D17** — Admin Console (AI-assisted), calling the gateway only
- **D18** — Security review · dependency audit
- **D19** — Health checks · load test via gateway · correlation-ID trace · code freeze
- **D20** — README (Swagger URLs) · ADRs · `docs/events.md` · Demo Day

---

## Track Reference Library

> Shared infrastructure, architecture and practice references are in the programme guide, [README §11](README.md). Every link points to official documentation; if one moves, search the same official domain.

### Node.js / NestJS — core

[Node.js learn](https://nodejs.org/learn) · [Node.js API](https://nodejs.org/docs/latest/api/) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [NestJS](https://docs.nestjs.com/) · [NestJS microservices](https://docs.nestjs.com/microservices/basics) · [Express](https://expressjs.com/) · [Prisma](https://www.prisma.io/docs) · [Jest](https://jestjs.io/docs/getting-started) · [BullMQ](https://docs.bullmq.io/) · [MDN JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

### Node.js / NestJS — the libraries used in this track

[NestJS OpenAPI](https://docs.nestjs.com/openapi/introduction) · [Authentication](https://docs.nestjs.com/security/authentication) · [Caching](https://docs.nestjs.com/techniques/caching) · [HTTP module](https://docs.nestjs.com/techniques/http-module) · [File upload](https://docs.nestjs.com/techniques/file-upload) · [RabbitMQ transport](https://docs.nestjs.com/microservices/rabbitmq) · [Rate limiting](https://docs.nestjs.com/security/rate-limiting) · [Terminus health checks](https://docs.nestjs.com/recipes/terminus) · [Prisma Migrate](https://www.prisma.io/docs/orm/migrations/how-migrations-work) · [Testcontainers for Node.js](https://node.testcontainers.org/) · [amqplib](https://amqp-node.github.io/amqplib/) · [pino](https://getpino.io/) · [helmet](https://helmet.js.org/) · [http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware) · [swagger-ui-express](https://github.com/scottie1984/swagger-ui-express) · [NestJS task scheduling](https://docs.nestjs.com/techniques/task-scheduling) · [RabbitMQ dead-letter exchanges](https://www.rabbitmq.com/docs/dlx)

### API Gateway & Swagger aggregation

[http-proxy-middleware](https://github.com/chimurai/http-proxy-middleware) · [swagger-ui-express](https://github.com/scottie1984/swagger-ui-express) · [API gateway pattern](https://microservices.io/patterns/apigateway.html) · [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Kong](https://developer.konghq.com/) · [Swagger UI configuration (`urls` for multiple specs)](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/)

---

*VATEK Internship Program 2026 · Backend Developer Intern · Track — Node.js / NestJS*
