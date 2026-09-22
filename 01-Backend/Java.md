# Backend Track — Java / Spring Boot
### VATEK Internship Program 2026 · Backend Developer Intern (Fullstack-oriented)

| | |
|---|---|
| **Language** | Java / Spring Boot — the whole month: labs, both microservices and the API Gateway |
| **Capstone** | OrderHub — order microservices behind an API Gateway with aggregated Swagger |
| **Duration** | 20 working days · Week 1 fundamentals → Weeks 2–4 OrderHub capstone → Demo Day |
| **Programme guide** | [README.md](README.md) — capstone spec, microservice rules, gates, grading, AI rules |

> **Read-only reference.** This document describes what to learn and build, week by week. Do all of the work in your own repository — nothing here needs to be copied or edited. Each week ends with the deliverables your mentor reviews at that Friday's gate; the rules you are graded against are in the [programme guide](README.md).

---

## Your Stack

| Layer | Technology |
|---|---|
| **Core API** | Spring Boot 3 · Spring Web · Spring Data JPA · Spring Security · Flyway · JUnit 5 · Mockito · springdoc-openapi |
| **Ops Service** | The same stack — a separate project, its own database tables, its own container |
| **API Gateway** | Spring Cloud Gateway — or Traefik / Nginx |
| **Admin Console** | Next.js 15 · TypeScript · TailwindCSS — AI-assisted (README §5.3.2) |
| **Shared infrastructure** | PostgreSQL 16 · Redis 7 · RabbitMQ 3 · Docker Compose · GitHub Actions |

---

## Before Day 1 — Environment Checklist

- JDK 21 (Eclipse Temurin)
- IntelliJ IDEA (Community or Ultimate)
- Maven 3.9+ or Gradle 8+
- The Next.js toolchain for the Week 4 Admin Console — follow the [Next.js installation guide](https://nextjs.org/docs/app/getting-started/installation)
- The Week 1 lab dataset — `orders.json` and the other files in [`datasets/`](../datasets/README.md)
- Docker Desktop (with Docker Compose v2) — `docker compose version` works
- Git configured with your company identity; SSH key added to GitHub
- A REST client: Postman, Bruno, or VS Code REST Client
- Your AI coding assistant installed and signed in (Claude Code / Cursor / Copilot)
- Read the programme guide [README.md](README.md) §1–§6 once, end to end

---

## Week 1 — Core Language & Framework Fundamentals (D1–D5)

**Theme:** Before building the product, own the language. Mornings are concepts and reading; afternoons are graded micro-exercises (**labs**) in a `/labs` folder, kept separate from the capstone repository.

| Day | Topics — Java | Lab (afternoon) | Done when |
|---|---|---|---|
| **D1** | Java syntax, primitives vs objects, records, sealed types, `Optional`, Maven or Gradle, project layout | **L1 — Order report CLI.** Read `orders.json` (200 orders with customer, status, line items), filter by status and date range, compute revenue per day and per status, print a report | Runs from the CLI; totals match the answer key; a debugger breakpoint is hit and stepped through in front of the mentor |
| **D2** | Classes, interfaces, default methods, abstract classes, inheritance vs composition, generics, SOLID, Spring IoC container and bean scopes | **L2 — Refactor L1** behind an `IOrderRepository` interface with two implementations (JSON file, in-memory), plus an `OrderTotalCalculator` wired by constructor injection | Swapping repository implementations requires no change inside the report logic |
| **D3** | Collections framework, **Streams API** (map/filter/collect, lazy evaluation), `CompletableFuture`, virtual threads, exceptions | **L3 — Async order enrichment.** Group orders by customer with the language's query idiom, then fetch shipping status for 20 orders concurrently from a mock endpoint, with a timeout | The concurrent version is measurably faster than sequential; a slow shipping call times out and is reported, not swallowed |
| **D4** | Spring Boot: auto-configuration, `@RestController`, routing, **filters & interceptors**, `@Valid` bean validation, `application.yml` profiles, Actuator | **L4 — Mini Orders API.** 5 REST endpoints — list, get, create, change status, cancel order — + 3 custom middleware (request logging, correlation ID, global exception handler) | Middleware order is explained correctly; an invalid status transition returns a clean JSON error, never a stack trace |
| **D5** | Spring Data JPA, entities and relationships, Flyway migrations, JUnit 5, Mockito, **microservices with Spring Cloud**, resilience patterns | **L5 — Split the Orders API into two services:** `Orders` and `Pricing` (line-item totals, discounts, tax), communicating over HTTP with a timeout, retry and fallback. ≥8 unit tests with mocks | Killing `Pricing` degrades `Orders` gracefully — orders still list, with totals marked `pending` — instead of hanging or returning 500 |

> **L5 is the spine of the whole month.** The capstone in Weeks 2–4 is the same pattern at production scale: two independently deployable order services, synchronous calls plus asynchronous events. An intern who understands L5 understands Week 3 before arriving there.

### 📚 Week 1 reading — official documentation

| Day | Documentation |
|---|---|
| **D1** | [dev.java learn](https://dev.java/learn/) · [Java SE API docs](https://docs.oracle.com/en/java/javase/21/docs/api/index.html) · [Maven guides](https://maven.apache.org/guides/) · [Gradle user manual](https://docs.gradle.org/current/userguide/userguide.html) |
| **D2** | [Java OOP tutorial](https://dev.java/learn/oop/) · [Spring Framework core (IoC & DI)](https://docs.spring.io/spring-framework/reference/core/beans.html) |
| **D3** | [Collections](https://dev.java/learn/api/collections-framework/) · [Streams](https://dev.java/learn/api/streams/) · [Concurrency](https://docs.oracle.com/en/java/javase/21/core/concurrency.html) |
| **D4** | [Spring Boot reference](https://docs.spring.io/spring-boot/index.html) · [Spring Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html) · [Validation](https://docs.spring.io/spring-framework/reference/core/validation.html) · [Actuator](https://docs.spring.io/spring-boot/reference/actuator/index.html) |
| **D5** | [Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/) · [JUnit user guide](https://docs.junit.org/current/user-guide/) · [Mockito](https://site.mockito.org/) · [Spring Cloud](https://spring.io/projects/spring-cloud) · [Resilience4j](https://resilience4j.readme.io/docs/getting-started) · [Flyway](https://documentation.red-gate.com/flyway) |

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

### 🔧 Your stack this week — Java

| Day | Tools & commands |
|---|---|
| D6 | Spring Initializr (Web, Actuator, Validation) · logstash-logback-encoder for JSON logs · `application.yml` + environment overrides · layered Dockerfile or Jib · GitHub Actions: `mvn -B verify` |
| D7 | JPA entities and relationships · Flyway `V1__init.sql` migrations · seed via `R__seed.sql` or a `CommandLineRunner` |
| D8 | `@RestController` · Bean Validation `@Valid` · `@RestControllerAdvice` returning `ProblemDetail` · springdoc-openapi · `Pageable` |
| D9 | Spring Security `SecurityFilterChain` · OAuth2 Resource Server (JWT) or a custom JWT filter · `BCryptPasswordEncoder` · `@PreAuthorize("hasRole('ADMIN')")` · JUnit 5 + Mockito · JaCoCo |
| D10 | `@Transactional` · `@Version` optimistic locking · `ObjectOptimisticLockingFailureException` · an `idempotency_keys` table |

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

### 🔧 Your stack this week — Java

| Day | Tools & commands |
|---|---|
| D11 | `RestClient` + Resilience4j (TimeLimiter, Retry) · HMAC webhook signature check · `MultipartFile` size/MIME validation |
| D12 | Spring Cache + Spring Data Redis · `@EntityGraph` / `JOIN FETCH` against N+1 · Hibernate SQL logging · `@SpringBootTest` + Testcontainers |
| D13 | A second Spring Boot application, `ops-service`, with its own schema and Flyway migrations (`reports`, `notifications` only) · Dockerfile or Jib |
| D14 | **Core:** `RabbitTemplate` publishing `order.paid` after commit (`@TransactionalEventListener`)<br>**Ops:** `@RabbitListener` with manual ack, Spring AMQP listener retry and a dead-letter exchange |
| D15 | `@Scheduled(cron = …)` nightly order report (idempotent upsert) · mock `NotificationSender` · one compose file for both services |

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

### 🔧 Your stack this week — Java

| Day | Tools & commands |
|---|---|
| D16 | **Native:** Spring Cloud Gateway — routes in `application.yml`; springdoc `springdoc.swagger-ui.urls` aggregating both specs<br>**Or:** Traefik or Nginx as the gateway + the `swaggerapi/swagger-ui` container with `URLS` listing both specs |
| D17 | Next.js 15 App Router · TypeScript · TailwindCSS · your AI assistant to scaffold the pages · `NEXT_PUBLIC_API_BASE_URL` = the **gateway** URL |
| D18 | OWASP Dependency-Check Maven/Gradle plugin · gateway `RequestRateLimiter` (Redis) · verify JWT expiry handling |
| D19 | Spring Boot Actuator health groups (liveness / readiness) · Micrometer · k6 |
| D20 | README with the Swagger URL table · `docs/events.md` · `docs/architecture.md` · ADRs in `docs/adr/` · an architecture diagram (Mermaid or draw.io) |

**Reading:** [Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/reference/) · [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Swagger UI configuration](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/) · [springdoc-openapi](https://springdoc.org/) · [API gateway pattern](https://microservices.io/patterns/apigateway.html) · [Next.js docs](https://nextjs.org/docs) · [TailwindCSS](https://tailwindcss.com/docs) · [OWASP API Security Top 10](https://owasp.github.io/API-Security/) · [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS) · [Grafana k6](https://grafana.com/docs/k6/latest/)

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

### Java / Spring Boot — core

[dev.java](https://dev.java/learn/) · [Java SE API](https://docs.oracle.com/en/java/javase/21/docs/api/index.html) · [Spring Boot](https://docs.spring.io/spring-boot/index.html) · [Spring Framework core](https://docs.spring.io/spring-framework/reference/core.html) · [Spring Web MVC](https://docs.spring.io/spring-framework/reference/web/webmvc.html) · [Spring Data JPA](https://docs.spring.io/spring-data/jpa/reference/) · [Spring Security](https://docs.spring.io/spring-security/reference/) · [Spring Cloud](https://spring.io/projects/spring-cloud) · [JUnit user guide](https://docs.junit.org/current/user-guide/) · [Mockito](https://site.mockito.org/) · [Resilience4j](https://resilience4j.readme.io/docs/getting-started) · [Maven](https://maven.apache.org/guides/) · [Gradle](https://docs.gradle.org/current/userguide/userguide.html)

### Java / Spring Boot — the libraries used in this track

[Spring Security](https://docs.spring.io/spring-security/reference/) · [OAuth2 Resource Server — JWT](https://docs.spring.io/spring-security/reference/servlet/oauth2/resource-server/jwt.html) · [REST clients](https://docs.spring.io/spring-framework/reference/integration/rest-clients.html) · [Cache abstraction](https://docs.spring.io/spring-framework/reference/integration/cache.html) · [Spring Data Redis](https://docs.spring.io/spring-data/redis/reference/) · [Spring AMQP](https://docs.spring.io/spring-amqp/reference/) · [Task scheduling](https://docs.spring.io/spring-framework/reference/integration/scheduling.html) · [Testcontainers for Java](https://java.testcontainers.org/) · [springdoc-openapi](https://springdoc.org/) · [Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/reference/) · [OWASP Dependency-Check](https://jeremylong.github.io/DependencyCheck/) · [logstash-logback-encoder](https://github.com/logfellow/logstash-logback-encoder) · [Spring AMQP — resilience & retry](https://docs.spring.io/spring-amqp/reference/amqp/resilience-recovering-from-errors-and-broker-failures.html)

### API Gateway & Swagger aggregation

[Spring Cloud Gateway](https://docs.spring.io/spring-cloud-gateway/reference/) · [springdoc-openapi](https://springdoc.org/) · [API gateway pattern](https://microservices.io/patterns/apigateway.html) · [Traefik](https://doc.traefik.io/traefik/) · [Nginx](https://nginx.org/en/docs/) · [Kong](https://developer.konghq.com/) · [Swagger UI configuration (`urls` for multiple specs)](https://swagger.io/docs/open-source-tools/swagger-ui/usage/configuration/)

---

*VATEK Internship Program 2026 · Backend Developer Intern · Track — Java / Spring Boot*
