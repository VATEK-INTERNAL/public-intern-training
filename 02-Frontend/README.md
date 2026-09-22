# Frontend Developer Intern — Programme Guide
### VATEK Internship Program 2026 · Software Development Department

| | |
|---|---|
| **Position** | Frontend Developer Intern (Fullstack Frontend) |
| **Plan scope** | Month 1 — Ramp-up & Foundation phase of the 3–6 month internship |
| **Format** | Full-time, onsite · **20 working days (4 weeks)** · ~8h/day |
| **Structure** | Week 1 fundamentals → Weeks 2–4 capstone project → Demo Day |
| **Exit state** | Intern is deployable onto a real client project as a supervised contributor |
| **Daily plans** | One track file per language in this folder — see §2 |

---

## 1. Programme at a Glance

| Week | Days | Theme | What the intern produces | Checkpoint |
|---|---|---|---|---|
| **1** | D1–D5 | **Core language & framework fundamentals** | 5 graded labs (L1–L5), ending in an order builder with shared state | **Gate 1** |
| **2** | D6–D10 | **Capstone starts — design system & app shell** | Deployed app: tokens, component library, app shell, first order screen | **Gate 2** |
| **3** | D11–D15 | **Order data, state, forms & testing** | All 8 order screens working against a mock API, with tests in CI | **Gate 3** |
| **4** | D16–D20 | **Realtime, real API & ship** | Live order updates, integration with the real OrderHub API, bulk order actions, performance/a11y pass, production deploy | **Demo Day** |

**The capstone project begins on Day 6 and runs to Day 20.** Week 1 is fundamentals and labs only — it is not project time, and its labs live in a separate folder.

---

## 2. How This Folder Works

| File | For | Contents |
|---|---|---|
| `README.md` *(this file)* | Mentors and team lead; interns read §1–§6 once on Day 1 | Programme overview, Order Admin Console spec, gates, grading, mentor playbook |
| [`React.md`](React.md) | Interns whose main framework is React + Next.js | Day-by-day plan, stack, reading list, weekly deliverables |
| [`Vue.md`](Vue.md) | Interns whose main framework is Vue 3 + Nuxt | Day-by-day plan, stack, reading list, weekly deliverables |
| [`Angular.md`](Angular.md) | Interns whose main framework is Angular | Day-by-day plan, stack, reading list, weekly deliverables |
| [`ReactNative.md`](ReactNative.md) | Interns whose main framework is React Native | Day-by-day plan (mobile variant), stack, reading list, weekly deliverables |

All four tracks build the same Order Admin Console (§5) against the same API contract, each in its own framework, and are graded on the same scorecard (§9). Each track file is independent and covers only its own framework.

**How to use these documents**

- These documents are a **read-only reference** — requirements, exercises, stack and reading. Interns do not copy, fork or edit anything in this folder.
- Each intern builds everything — the Week 1 labs and the capstone — in **their own repository**, following their track file.
- Each week in a track file ends with the **deliverables** that must exist in the intern's own repository by that Friday's gate. The mentor reviews the intern's repository and pull requests against that list at the gate (§7).

---

## 3. Why This Plan Exists

The JD asks for a frontend engineer who can **turn a Figma file into a fast, accessible, production UI** with ReactJS / Next.js, VueJS or Angular — with React Native as a plus. This plan trains each intern deeply in **one framework**, the one they were hired on: one week on the language and the framework's real mental model, then three weeks building the Order Admin Console in it.

From Week 2 the plan is **project-first**. Every day produces a commit against one real application.

### 3.1 Outcomes — what the intern can do on Day 20

1. Use **modern JavaScript and TypeScript** confidently: closures, the event loop, async/await, generics, narrowing, utility types.
2. Explain **how their framework actually renders** — reactivity, re-render or change-detection triggers, keys — and use its primitives correctly rather than by imitation.
3. Choose and implement the right **state solution** — local, shared, or a global store — and justify the choice.
4. Read a Figma file and reproduce it **pixel-accurately and responsively** with TailwindCSS.
5. Build an application with layouts, protected routes and lazy-loaded pages using their framework's router.
6. Build a **reusable component library** with variants, typed props, and accessibility built in.
7. Integrate a REST API properly: typed client, caching, pagination, optimistic updates, complete loading / error / empty states.
8. Build **complex, validated forms**, including a multi-step order form and file upload with progress.
9. Write **component tests and one E2E flow**, running in CI on every PR.
10. Handle **realtime order updates** and integrate with the **real OrderHub API** through the API Gateway.
11. Hit **Lighthouse ≥ 90** on performance and accessibility (React Native: the mobile performance budget), and explain every fix that got them there.
12. Use an **AI coding assistant** daily — and verify every generated UI for responsiveness, a11y, design fidelity and render cost.

---

## 4. Track Selection (Day 1 decision)

The mentor assigns a track on Day 1 from the intern's main framework in the technical interview. **The whole month is built in that one framework** — every lab, every screen, every test. Each track file is independent: it covers only its own framework, from the Week 1 fundamentals to Demo Day.

| Track | Framework | State & data | Forms | Tests | Deploy | Track file |
|---|---|---|---|---|---|---|
| **React** | React 19 · Next.js 15 (App Router) | Redux Toolkit or Zustand · TanStack Query | React Hook Form + Zod | Vitest + React Testing Library · Playwright | Vercel | [`React.md`](React.md) |
| **Vue** | Vue 3 · Nuxt 4 | Pinia · `useFetch` / TanStack Query | VeeValidate + Zod | Vitest + Vue Test Utils · Playwright | Vercel / Netlify | [`Vue.md`](Vue.md) |
| **Angular** | Angular 18+ (standalone, signals) | Signal services / NgRx SignalStore · HttpClient + RxJS | Reactive Forms | Jest + Angular Testing Library · Playwright | Vercel / Netlify / Firebase | [`Angular.md`](Angular.md) |
| **React Native** | React Native · Expo · Expo Router | Zustand or Redux Toolkit · TanStack Query | React Hook Form + Zod | Jest + React Native Testing Library · Maestro | EAS | [`ReactNative.md`](ReactNative.md) |

> **React Native track.** The capstone is the same eight order screens delivered as a phone app — tab bar instead of sidebar, mobile performance budgets instead of Lighthouse, push notifications in the realtime week. The track file carries every mobile-specific difference.

### 4.1 Shared toolchain (all tracks)

TypeScript (strict) · MSW for the mock API · ESLint + Prettier · Figma · Git + GitHub PR flow · GitHub Actions · an AI coding assistant.

---

## 5. The Capstone Project — "OrderHub Admin Console" (starts Day 6)

Every intern builds the same application, in their own framework, against the same API contract. This keeps mentoring, grading and peer review consistent — and it is the **same product the Backend interns build**: on D17 every frontend track switches from the mock API to the real OrderHub API.

> **OrderHub Admin Console** — the internal web console for a B2B order-management platform. Staff sign in, browse and filter orders, open an order and change its status, upload an invoice, manage products, and read a daily sales report.

### 5.1 Screens to build

| # | Screen | Built on | Key requirements |
|---|---|---|---|
| 1 | **Login** | D7 | Form validation, error states, redirect after login, "remember me" |
| 2 | **App shell** | D9 | Sidebar + topbar, active-route highlighting, user menu, responsive drawer on mobile |
| 3 | **Orders list** | D10–D11 | Data table, server-side pagination, filter by status and date range, search, sortable columns, skeleton loading |
| 4 | **Order detail** | D14 | Summary card, line-item table, status change with confirmation, invoice upload with progress, activity timeline |
| 5 | **Create Order** | D13 | Multi-step form: choose customer → add line items from a searchable product picker → review totals and discount → submit; Zod validation per step; optimistic insert into the orders list |
| 6 | **Daily report** | D14 | Date-range picker, KPI tiles, one chart (Recharts), CSV export |
| 7 | **Order preferences** | D12 | Default status filter and page size for the orders list, theme toggle (light/dark), language toggle (vi/en) |
| 8 | **Not-found / error** | D9 | Proper 404 and error boundary pages |

**React Native track:** the same eight screens as a phone app — a tab bar replaces the sidebar, and Order detail uses the camera or file picker for the invoice.

### 5.2 API

A mock API is provided via **MSW** from Day 10, so frontend work never blocks on backend availability. The contract is fixed and published in `docs/api-contract.md`. Order status changes are pushed as events (Server-Sent Events or WebSocket) — mocked until D16. **On D17 the app is pointed at the real OrderHub API through the API Gateway** (a Backend intern's build or the mentor's reference build) — same contract, no code change beyond the base URL. If the real API has no event stream yet, the app falls back to polling.

### 5.3 Non-functional requirements (graded on Day 20)

- Responsive and usable at **375 px, 768 px, 1280 px, 1920 px**.
- **Lighthouse ≥ 90** on Performance, Accessibility, Best Practices, SEO (production build).
- Fully keyboard-navigable; visible focus states; no axe-core critical violations.
- No layout shift on data load — skeletons reserve space (CLS < 0.1).
- Zero TypeScript errors, zero ESLint errors on `main`.
- Deployed and reachable on a public URL.
- **React Native track:** the web-only items above are replaced by the mobile budget in the track file — cold start < 3 s on a mid-range Android, 60 fps list scrolling, a TalkBack / VoiceOver pass, and an EAS preview build.

### 5.4 Stretch backlog (only after the required scope is green)

Bulk order import from CSV · saved order views · Storybook · visual regression tests · offline order queue · advanced animation choreography.

---

## 6. Daily Rhythm

| Time | Activity |
|---|---|
| 09:00 – 09:15 | Standup: yesterday / today / blockers |
| 09:15 – 12:00 | Focused block (Week 1: concepts · Weeks 2–4: build) |
| 13:00 – 16:30 | Build block 2 · pairing / mentor review window |
| 16:30 – 17:15 | Self-review against the Figma file, push PR, daily log |
| 17:15 – 17:30 | Reading (from the linked docs) · AI-usage journal entry |

**Fixed weekly events:** Tue & Thu 60-min pairing · Wed 30-min UI/UX critique session (the whole team reviews one screen) · Fri 45-min **Gate Review** · Fri 15-min 1:1.

**Code review SLA:** the mentor responds to any PR within 4 working hours. Nothing merges without one approval **and one screenshot or screen recording in the PR description**.

---

## 7. Weekly Checkpoints — Gates & Demo Day

Day-by-day content, tools and reading live in the track files. This section is the mentor's reference for what each checkpoint verifies. Every gate is a live demo **plus** a verbal defence: passing requires answering without reading the code.

### 7.1 Week 1 labs (shared by every track)

| Day | Lab | Done when |
|---|---|---|
| **D1** | **L1 — Typed order utilities** — `Order` types + 8 pure functions with tests | Strict type-check clean; no `any`; every function tested |
| **D2** | **L2 — Order list without a framework** (React Native: an order fetch script) — debounced search, stale requests cancelled | Stale requests cancelled; failures show a message |
| **D3** | **L3 — The order list in your framework**, keyed by order ID | Can explain exactly what triggers re-rendering or change detection |
| **D4** | **L4 — Order hooks / composables / services**, then refactor L3 | No side effect where derived state belongs; tested |
| **D5** | **L5 — Order builder with shared state**, built two or three ways + `labs/state-comparison.md` | The comparison names a concrete scenario where each approach wins |

### 7.2 Gate 1 — end of Week 1 (D5)

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L4 and L5. The mentor asks: *"This order list re-renders (or re-runs change detection) on every keystroke. Show me why, and give me two different fixes."* and *"Why this state solution for the order builder, and what does it cost you?"* Passing requires answering without reading the code. Each track file phrases these questions for its own framework.

### 7.3 Gate 2 — end of Week 2 (D10)

**🚩 Gate 2 — Friday, 45 min.** Demo the app shell, component catalogue and orders list on desktop and a phone (React Native: phone and tablet), then navigate the whole UI with the keyboard only (React Native: with a screen reader). The mentor asks one question about the framework's rendering or routing boundary — each track file names it — and *"Where does this spacing value come from?"* (the answer must be a token, not a number).

### 7.4 Gate 3 — end of Week 3 (D15)

**🚩 Gate 3 — Friday, 45 min.** Demo the full authenticated flow with the network throttled to Slow 3G, then break the mock API live and show the failure handling. The mentor asks: *"What is cached right now, for how long, and what invalidates it?"*

### 7.5 Demo Day agenda — end of Week 4 (D20)

| Minutes | Segment |
|---:|---|
| 0–2 | The product and the design system: tokens, components, why they are reusable |
| 2–8 | Live walkthrough of all 8 screens — desktop, then the same flow on a phone |
| 8–11 | Failure modes: slow network, API down, expired session, empty results |
| 11–14 | Quality evidence: Lighthouse scores (React Native: startup and scroll measurements), accessibility audit, CI run with tests |
| 14–17 | Realtime and real API: push an order status change live, then show the app running against the real OrderHub API and the contract differences you fixed |
| 17–20 | **AI usage retrospective:** where AI saved the most time, where it produced UI that looked right but failed on responsiveness or a11y, what you now check by reflex |
| +10 | Mentor and team questions |

---

## 8. AI Adoption — Mandatory Standards

The JD makes AI adoption a hiring requirement, not a bonus. These rules are enforced in code review from Day 1.

### 8.1 Required practice

- Use one assistant daily (Claude Code / Cursor / GitHub Copilot or equivalent).
- Give AI the **project context**, not a generic request. "Make a card component" produces generic output. "Create a `Card` component using our `cva` variant pattern and the `surface`/`border` tokens in `tailwind.config.ts`, matching the `OrderSummary` frame in Figma, with `header`/`body`/`footer` slots" produces mergeable output.
- Use AI where it pays: Figma-to-component conversion, repetitive markup, complex Tailwind and grid layouts, mock data, Zod schemas, component tests, refactors, TypeScript error diagnosis.

### 8.2 Hard rules — a violation blocks the merge

> **Never** paste credentials, customer data, or NDA-scope design assets or source into a public AI tool.
> **Never** merge UI you have not personally checked for **responsiveness, accessibility, design fidelity and render cost**. AI-generated UI looks convincing and fails quietly at 375 px.
> **Never** merge code you cannot explain. Mentors will ask at random, in review.
> **Always** disclose AI-generated sections in the PR description, with a screenshot.

### 8.3 Graded artefact

`docs/ai-journal.md` — maintained from D6 to D20, reviewed at Gates 2 and 3 and at Demo Day. **Minimum 12 entries.** An entry that only says "used AI to build a component" scores zero; each entry must name the defect the intern caught in the generated output.

---

## 9. Evaluation

### 9.1 Scorecard (100 points)

| # | Criterion | Weight | What "excellent" looks like |
|---|---|---:|---|
| 1 | **Core language & framework fundamentals** (Week 1 labs) | 10 | All five labs complete; can explain re-render or change-detection causes and justify a state-management choice unaided |
| 2 | **Design fidelity & responsiveness** | 15 | Matches Figma at every breakpoint; no spacing or type drift |
| 3 | **Component architecture & reuse** | 15 | Typed, variant-driven, genuinely reusable; no copy-paste screens |
| 4 | **Data, state & forms** | 15 | Correct caching and invalidation, complete loading/error/empty states, robust forms |
| 5 | **Performance & accessibility** | 15 | Lighthouse ≥ 90 (React Native: mobile budget met); fully keyboard- or screen-reader-operable; can explain each fix |
| 6 | **Testing & CI** | 10 | Behaviour-focused component tests + a working E2E; green pipeline |
| 7 | **Realtime & real-API integration** | 10 | Live order updates reconcile without duplicates; switching to the real API needs only a config change; contract differences found, fixed and documented |
| 8 | **AI adoption & critical thinking** | 5 | Strong journal; can name concrete AI failures they caught |
| 9 | **Communication, docs & demo** | 5 | Clear documentation; confident, structured demo |

### 9.2 Decision bands

| Score | Outcome |
|---|---|
| **≥ 85** | **Strong pass.** Assign to a client project as a contributor; open the Fresher conversation early. |
| **70 – 84** | **Pass.** Continue to month 2 on a real project with normal supervision. |
| **55 – 69** | **Conditional.** A two-week targeted remediation plan on the weakest two criteria, then re-assess. |
| **< 55** | **Not passing.** Structured feedback and closure of the internship. |

**Non-negotiable minimums, regardless of total score:** CI green on `main` · zero TypeScript errors · production build deployed · the intern can explain any randomly selected component they wrote.

---

## 10. Mentor Playbook

| Cadence | Duration | Purpose |
|---|---|---|
| Daily standup | 15 min | Unblock — not status theatre |
| PR review | ≤ 4h SLA | Review the screenshot **and** the code; ask for a rewrite rather than fixing it yourself |
| Pairing (Tue, Thu) | 60 min | Intern drives, mentor navigates |
| UI critique (Wed) | 30 min | The whole team critiques one screen — trains the eye, not just the hands |
| Gate review (Fri) | 45 min | Demo + verbal defence + written feedback |
| 1:1 (Fri) | 15 min | Wellbeing, motivation, career direction |

**Mentor preparation before Day 1:** nothing for the Week 1 data — it is provided in [`datasets/`](../datasets/README.md) (`db.json`, served by `json-server`; `order-events.json` seeds the D16 realtime mock) · a complete Figma file for all 8 order screens with real tokens · the OrderHub API contract · MSW handler seeds · a mock order-event stream (SSE or WebSocket) for D16 · access to a running OrderHub API Gateway for D17 · a GitHub repo template with CI and a hosting project · the track decision made from the interview notes.

**Escalate to the team lead the same day if:**

- The intern is more than 2 days behind the plan at any gate.
- The intern cannot explain their own component twice in one week.
- Any hard AI rule in §8.2 is violated.

**The mentor's most important habit:** review on a real phone, not a browser resized to phone width. Half of all responsive defects only appear on the device.

---

## 11. Shared Reference Library

> Every link points to primary/official documentation (vendor docs, MDN, W3C). Documentation sites reorganize: if a URL 404s, search the same official domain rather than following a third-party mirror or a dated blog post.

Language- and framework-specific references are in each track file, next to the day they are used.

### 11.1 Language fundamentals

[MDN JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) · [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) · [MDN Web APIs](https://developer.mozilla.org/en-US/docs/Web/API) · [MDN HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) · [MDN CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [TypeScript utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

### 11.2 Quality — testing, a11y, performance

[Vitest](https://vitest.dev/guide/) · [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) · [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles) · [Playwright](https://playwright.dev/docs/intro) · [MSW](https://mswjs.io/docs/) · [web.dev Core Web Vitals](https://web.dev/articles/vitals) · [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) · [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) · [WCAG 2.2 quick reference](https://www.w3.org/WAI/WCAG22/quickref/) · [axe DevTools](https://www.deque.com/axe/devtools/) · [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)

### 11.3 Tooling & practice

[Git documentation](https://git-scm.com/doc) · [GitHub Actions](https://docs.github.com/en/actions) · [Conventional Commits](https://www.conventionalcommits.org/) · [ESLint](https://eslint.org/docs/latest/) · [Prettier](https://prettier.io/docs/) · [Vite](https://vite.dev/guide/) · [Vercel](https://vercel.com/docs) · [Figma Dev Mode](https://help.figma.com/hc/en-us/articles/15023124644247-Guide-to-Dev-Mode) · [Google Engineering Practices — Code Review](https://google.github.io/eng-practices/review/)

---

## 12. Appendix A — Definition of Done (every PR)

- Branch named `feat/…`, `fix/…` or `chore/…`; commits follow Conventional Commits
- PR description states: what, why, how to test, **which parts were AI-generated**, plus a screenshot or recording
- Checked at 375 px, 768 px and 1280 px (React Native: small phone, large phone, tablet)
- Keyboard-navigable; no new axe critical or serious violations
- Loading, error and empty states handled
- Zero TypeScript errors, zero ESLint errors; tests added or updated; CI green
- No hardcoded UI string (i18n), no magic spacing number (tokens)

---

## 13. Appendix B — Programme Calendar

| Week | Days | Theme | Checkpoint |
|---|---|---|---|
| 1 | D1–D5 | Core language & framework fundamentals | Gate 1 |
| 2 | D6–D10 | Capstone starts — design system & app shell | Gate 2 |
| 3 | D11–D15 | Order data, state, forms & testing | Gate 3 |
| 4 | D16–D20 | Realtime, real API & ship | **Demo Day** |

---

## 14. Appendix C — Risks, Remediation & the Scope-Cut Ladder

### 14.1 Risks

| Risk | Early signal | Mentor action |
|---|---|---|
| Weak CSS / layout fundamentals | D7 struggle, heavy AI reliance for layout | Insert a half-day of flexbox/grid drills; reduce the D8 catalogue to 4 components |
| Weak TypeScript | Frequent `any` in L1; fights the compiler on D10 | Pair on typing the API client; ban `any` from D11 via a lint rule |
| Week 1 feels too easy for a strong intern | L1–L3 finished by lunch | Skip to L4/L5 and add a stretch lab: undo/redo in the order builder |
| Real-API integration blocked | D17: no OrderHub API reachable | Stay on MSW, diff the mock against the published OpenAPI spec, record the gaps in `docs/contract-diff.md`, and integrate early in month 2 |
| Design fidelity ignored in favour of speed | Screens "work" but drift from Figma | Run the Figma diff at Gate 2, not D20 |
| Over-reliance on AI, shallow understanding | Cannot explain a lab at Gate 1 | One mandatory AI-free day; the intern rebuilds a screen unaided |
| Performance left to the last week | Bundle grows silently through Week 3 | Add a bundle-size check to CI on D15 |

### 14.2 Scope-cut ladder — drop in this order if the intern falls behind

1. CSV export and the activity timeline (D14).
2. The product-picker search in Create Order (D13) — a plain select is fine; keep the line items, validation and optimistic insert.
3. The i18n switcher (D12) — keep dark mode and the extracted strings.
4. Bulk order actions and the 5,000-row list (D18) — keep CSV export of the filtered orders.
5. Cross-browser checking (D19) — keep the axe audit and Lighthouse.

**Never cut:** the design-token discipline · the component library · loading/error/empty states · testing and CI · the Demo Day retrospective. Those carry 55 of the 100 scorecard points.

---

*Prepared for the VATEK Internship Program 2026 · Frontend Developer Intern (Fullstack Frontend) · Month 1 of 3–6.*

---

*VATEK Internship Program 2026 · Frontend Developer Intern (Fullstack Frontend) · Programme Guide · Month 1 of 3–6.*
