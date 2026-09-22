# Frontend Developer Intern — 1-Month Training Plan
### VATEK Internship Program 2026 · Software Development Department

| | |
|---|---|
| **Position** | Frontend Developer Intern (Fullstack Frontend) |
| **Plan scope** | Month 1 — Ramp-up & Foundation phase of the 3–6 month internship |
| **Format** | Full-time, onsite · **20 working days (4 weeks)** · ~8h/day |
| **Structure** | Week 1 fundamentals → Weeks 2–4 capstone project → Demo Day |
| **Exit state** | Intern is deployable onto a real client project as a supervised contributor |

---

## 1. Programme at a Glance

| Week | Days | Theme | What the intern produces | Checkpoint |
|---|---|---|---|---|
| **1** | D1–D5 | **Core language & React fundamentals** | 5 graded labs (L1–L5), ending in one feature built three ways | **Gate 1** |
| **2** | D6–D10 | **Capstone starts — design system & app shell** | Deployed Next.js app: tokens, component library, app shell, first data screen | **Gate 2** |
| **3** | D11–D15 | **Data, state, forms & testing** | All 8 screens working against a mock API, with tests in CI | **Gate 3** |
| **4** | D16–D20 | **Second framework, quality & ship** | Extension-framework app + performance/a11y pass + production deploy | **Demo Day** |

**The capstone project begins on Day 6 and runs to Day 20.** Week 1 is fundamentals and labs only — it is not project time, and its labs live in a separate folder.

---

## 2. Why This Plan Exists

The JD asks for a frontend engineer who can **turn a Figma file into a fast, accessible, production UI in React/Next.js**, and who is not locked into a single framework. This plan builds exactly that: one week on the language and React's real mental model, two weeks deep in React + Next.js, then a deliberate crossing into a second framework.

From Week 2 the plan is **project-first**. Every day produces a commit against one real application.

### 2.1 Outcomes — what the intern can do on Day 20

1. Use **modern JavaScript and TypeScript** confidently: closures, the event loop, async/await, generics, narrowing, utility types.
2. Explain **how React actually renders** — state, reconciliation, keys, re-render triggers — and use hooks correctly rather than by imitation.
3. Choose and implement the right **state solution**: local, lifted, Context, Redux Toolkit or Zustand — and justify the choice.
4. Read a Figma file and reproduce it **pixel-accurately and responsively** with TailwindCSS.
5. Build a **Next.js App Router** application with layouts, protected routes, and correct server/client component boundaries.
6. Build a **reusable component library** with variants, typed props, and accessibility built in.
7. Integrate a REST API properly: typed client, caching, pagination, optimistic updates, complete loading / error / empty states.
8. Build **complex forms** with React Hook Form + Zod, including file upload with progress.
9. Write **component tests and one E2E flow**, running in CI on every PR.
10. Rebuild a working feature in a **second framework** and explain the trade-offs.
11. Hit **Lighthouse ≥ 90** on performance and accessibility, and explain every fix that got them there.
12. Use an **AI coding assistant** daily — and verify every generated UI for responsiveness, a11y, design fidelity and render cost.

---

## 3. Track Selection (Day 1 decision)

**The main stack is the same for everyone: React + Next.js + TypeScript.** It is the company's primary frontend stack. Only the Week 4 extension differs, chosen by the mentor with the intern on Day 1.

| Track | Main (Weeks 1–3, ~80%) | Extension (Week 4, ~20%) | Choose this when |
|---|---|---|---|
| **F1** | React 19 · Next.js 15 (App Router) · TypeScript | **Vue 3 + Nuxt 4** (Composition API, Pinia) | The intern is heading toward broad web project work; the cleanest paradigm contrast for reinforcing concepts |
| **F2** | React 19 · Next.js 15 (App Router) · TypeScript | **Angular 18+** (standalone components, signals, RxJS) | The intern will join an enterprise client project, or came from a strongly OOP background |
| **F3** | React 19 · Next.js 15 (App Router) · TypeScript | **React Native + Expo (basic)** | The intern shows mobile interest and already has solid React fundamentals by Gate 3 |

> **Guardrail:** Track F3 is only offered to interns who pass **Gate 3 at ≥ 80%**. React Native rewards strong React fundamentals and punishes weak ones.

### 3.1 Standard toolchain (all tracks)

TypeScript (strict) · TailwindCSS · shadcn/ui *(or MUI / Ant Design where the client project mandates it)* · Redux Toolkit **and** Zustand (both taught in Week 1; the project uses one) · TanStack Query · React Hook Form + Zod · Vitest + React Testing Library · Playwright · MSW · ESLint + Prettier · Figma · Git + GitHub PR flow · GitHub Actions · Vercel · an AI coding assistant.

---

## 4. Week 1 — Core Language & React Fundamentals

**Theme:** Understand the machine before decorating it. Mornings are concepts and reading; afternoons are graded micro-exercises (**labs**) in a `/labs` folder, kept separate from the capstone repository.

| Day | Concept block | Lab (afternoon) | Done when |
|---|---|---|---|
| **D1** | **JavaScript core:** scope & closures, `this`, prototypes, ES modules, destructuring, spread/rest, optional chaining, immutability. **TypeScript core:** primitives, unions, interfaces vs types, generics, narrowing, utility types (`Partial`, `Pick`, `Omit`, `Record`), `unknown` vs `any` | **L1 — Typed utilities.** Write 8 pure functions (group, sort, paginate, format currency/date) with full type signatures and unit tests | `tsc --strict` is clean; no `any`; every function has a test |
| **D2** | **DOM, events & async:** DOM API, event delegation and bubbling, the **event loop**, microtasks vs macrotasks, Promises, `async`/`await`, `Promise.all` vs `allSettled`, `fetch`, `AbortController`, error handling | **L2 — Vanilla JS mini-app.** No framework: fetch a public API, render a searchable list with debounce, handle loading/error/empty, cancel in-flight requests | Works with no framework; rapid typing cancels stale requests; failures show a message, not a blank page |
| **D3** | **React core:** JSX, components & props, **state**, the render cycle and reconciliation, lists and keys, conditional rendering, controlled forms, lifting state up, composition vs prop-drilling | **L3 — Rebuild L2 in React.** Same behaviour, component-based, with a correctly-keyed list | The intern can explain exactly what triggers each re-render; no index-as-key on a mutable list |
| **D4** | **Hooks in depth:** `useState`, `useEffect` (and **when not to use it**), `useRef`, `useMemo`/`useCallback` (and when they are noise), `useContext`, `useReducer`, the Rules of Hooks, **custom hooks** | **L4 — Custom hooks.** Extract `useDebounce`, `useFetch` (with abort), `useLocalStorage`, `useDisclosure`; refactor L3 to use them | No `useEffect` that could have been derived state or an event handler; hooks are tested |
| **D5** | **State management:** local vs lifted vs global; Context and its re-render cost; **Redux Toolkit** — store, slices, reducers, `createAsyncThunk`, RTK Query, DevTools & time-travel; **Zustand** as the lightweight alternative; when each is the right call | **L5 — Same feature, three ways.** Implement a cart (add / remove / quantity / total) in Context, in Redux Toolkit, and in Zustand; write `labs/state-comparison.md` | All three work; the comparison names a concrete scenario where each wins |

### 4.1 Week 1 reference documentation

| Topic | Official documentation |
|---|---|
| JavaScript language | [MDN JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) · [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) · [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) |
| TypeScript | [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) · [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) · [Utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) |
| DOM & events | [MDN DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) · [MDN Events](https://developer.mozilla.org/en-US/docs/Web/API/Event) · [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) · [AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) |
| Async & event loop | [Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) · [Event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model) |
| React core | [react.dev — Learn](https://react.dev/learn) · [Describing the UI](https://react.dev/learn/describing-the-ui) · [Adding interactivity](https://react.dev/learn/adding-interactivity) · [Managing state](https://react.dev/learn/managing-state) · [Rendering lists](https://react.dev/learn/rendering-lists) |
| React hooks | [Hooks reference](https://react.dev/reference/react/hooks) · [useEffect](https://react.dev/reference/react/useEffect) · [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect) · [Custom hooks](https://react.dev/learn/reusing-logic-with-custom-hooks) · [Rules of Hooks](https://react.dev/reference/rules/rules-of-hooks) |
| Redux | [Redux Toolkit](https://redux-toolkit.js.org/) · [Redux Essentials](https://redux.js.org/tutorials/essentials/part-1-overview-concepts) · [RTK Query](https://redux-toolkit.js.org/rtk-query/overview) · [Redux style guide](https://redux.js.org/style-guide/) |
| Zustand | [Zustand documentation](https://zustand.docs.pmnd.rs/) · [Zustand repository](https://github.com/pmndrs/zustand) |
| Testing (used from L1) | [Vitest](https://vitest.dev/) · [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) |

**AI drill (Week 1).** Set up the assistant with a project rules file (`CLAUDE.md` / `.cursorrules`) naming the stack and conventions. Then, on every lab: **write it yourself first, then ask AI to review it**, recording which suggestions you accepted and rejected. On D4, additionally ask AI to write a component that uses `useEffect` for something that does not need one — and explain why it is wrong. Learning to recognise that anti-pattern is worth more than any generated component.

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L4 and L5. The mentor asks: *"This component re-renders on every keystroke. Show me why, and give me two different fixes."* and *"When would you reach for Redux Toolkit over Context here, and what does it cost you?"* Passing requires answering without reading the code.

---

## 5. The Capstone Project — "OrderHub Admin Console" (starts Day 6)

Every intern builds the same application against the same API contract. This keeps mentoring, grading and peer review consistent — and it is the **same product the Backend interns build**, so a joint integration session in Week 4 is possible and encouraged.

> **OrderHub Admin Console** — the internal web console for a B2B order-management platform. Staff sign in, browse and filter orders, open an order and change its status, upload an invoice, manage products, and read a daily sales report.

### 5.1 Screens to build

| # | Screen | Built on | Key requirements |
|---|---|---|---|
| 1 | **Login** | D7 | Form validation, error states, redirect after login, "remember me" |
| 2 | **App shell** | D9 | Sidebar + topbar, active-route highlighting, user menu, responsive drawer on mobile |
| 3 | **Orders list** | D10–D11 | Data table, server-side pagination, filter by status and date range, search, sortable columns, skeleton loading |
| 4 | **Order detail** | D14 | Summary card, line-item table, status change with confirmation, invoice upload with progress, activity timeline |
| 5 | **Products** | D13 | CRUD with modal forms, image upload, inline status toggle, optimistic update |
| 6 | **Daily report** | D14 | Date-range picker, KPI tiles, one chart (Recharts), CSV export |
| 7 | **Settings** | D12 | Profile form, theme toggle (light/dark), language toggle (vi/en) |
| 8 | **Not-found / error** | D9 | Proper 404 and error boundary pages |

### 5.2 API

A mock API is provided via **MSW** from Day 10, so frontend work never blocks on backend availability. The contract is fixed and published in `docs/api-contract.md`. In Week 4 the console is optionally pointed at a Backend intern's real OrderHub API — same contract, no code change beyond the base URL.

### 5.3 Non-functional requirements (graded on Day 20)

- Responsive and usable at **375 px, 768 px, 1280 px, 1920 px**.
- **Lighthouse ≥ 90** on Performance, Accessibility, Best Practices, SEO (production build).
- Fully keyboard-navigable; visible focus states; no axe-core critical violations.
- No layout shift on data load — skeletons reserve space (CLS < 0.1).
- Zero TypeScript errors, zero ESLint errors on `main`.
- Deployed and reachable on a Vercel URL.

### 5.4 Stretch backlog (only after the required scope is green)

Order detail rebuilt in the extension framework · virtualized table for 10k rows · Storybook · visual regression tests · offline/optimistic queue · advanced animation choreography · CSV import.

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

## 7. Week 2 — Design System & App Shell *(capstone begins)*

**Theme:** From an empty repo to a deployed app shell built from a real design system.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D6** | Repository setup · Next.js + TypeScript + Tailwind + ESLint/Prettier · Git/PR flow · Vercel deploy · **Figma handoff** (reading spacing, typography, colour tokens, auto-layout, exporting assets) | Scaffold committed, **first PR merged**, preview deployed, `tailwind.config.ts` carrying the project's design tokens | Vercel preview URL live; `tsc --noEmit` clean; no magic numbers — tokens only |
| **D7** | Semantic HTML · CSS layout (flexbox, grid) · responsive strategy · Tailwind fundamentals | **Login screen** built from the Figma file, responsive at all four breakpoints | Side-by-side comparison with Figma shows no spacing or type-scale drift |
| **D8** | Component composition · variants with `cva` · typed props · accessibility basics (labels, roles, focus order, keyboard) | 6 base components — Button, Input, Select, Badge, Card, Modal — plus a `/dev/components` catalogue page | Every component supports variants and sizes; every interactive one is operable by keyboard alone |
| **D9** | Next.js App Router: file routing, layouts, **server vs client components**, `metadata`, `loading.tsx`, `error.tsx` | **App shell** — sidebar + topbar + route group for authenticated pages, skeleton loading, 404 and error pages | Navigating between routes does not remount the shell; the mobile drawer works |
| **D10** | API integration · typed API client · env configuration · **MSW** mock server · loading / error / empty states | **Orders list** rendering mock data with all three states handled | Turning the mock off shows a proper error state, never a blank screen or a crash |

**Reading:** [Next.js App Router](https://nextjs.org/docs/app) · [Server & Client Components](https://nextjs.org/docs/app/building-your-application/rendering/server-components) · [TailwindCSS](https://tailwindcss.com/docs) · [MDN CSS layout](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout) · [MDN HTML elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element) · [shadcn/ui](https://ui.shadcn.com/docs) · [class-variance-authority](https://cva.style/docs) · [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) · [MSW](https://mswjs.io/docs/) · [Figma Dev Mode](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-Dev-Mode)

**AI drill (Week 2).** Use AI to convert Figma frames into components — and **write down every correction you had to make**. A rules file that names your design tokens and component conventions is what turns generic Tailwind output into mergeable code; iterate on that file all week. Start the **AI Usage Journal** (`docs/ai-journal.md`) on D6.

**🚩 Gate 2 — Friday, 45 min.** Demo the app shell, component catalogue and orders list on desktop and mobile, then navigate the whole UI with the keyboard only. The mentor asks: *"Why is this a server component and that one a client component?"* and *"Where does this spacing value come from?"* (the answer must be a token, not a number).

---

## 8. Week 3 — Data, State, Forms & Testing

**Theme:** The part that separates a page from an application — real data, real failure modes.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D11** | TanStack Query: caching, staleness, invalidation, mutations, server-side pagination · URL as state | Orders list with server pagination, status filter, date-range filter and debounced search | Filters are reflected in the URL; refreshing the page restores the same view |
| **D12** | Auth flow · token storage and refresh · route guards · role-based UI · global state (the store chosen in L5) · theming & i18n | Login → protected routes → auto-refresh on 401 → logout; **Settings screen** with dark mode and vi/en switcher | An expired token silently refreshes once, then redirects to login if the refresh fails; no hardcoded UI string remains |
| **D13** | Forms with React Hook Form + Zod · dependent fields · modal forms · optimistic updates · toasts and confirmation dialogs | **Products screen**: CRUD with modal forms, inline status toggle, optimistic update | A failed mutation rolls back the optimistic update and shows a toast |
| **D14** | File upload with progress · data visualization · date-range input · export | **Order detail** (status change + invoice upload + timeline) and **Daily report** (KPI tiles, Recharts chart, CSV export) | Upload shows real progress and handles failure; the chart is readable in both light and dark mode |
| **D15** | Testing: Vitest + React Testing Library · Playwright E2E · GitHub Actions CI | ≥8 component tests + 1 Playwright E2E (login → filter → detail → status change); CI green | CI runs on every PR; tests assert user-visible behaviour, not implementation details |

**Reading:** [TanStack Query](https://tanstack.com/query/latest) · [React Hook Form](https://react-hook-form.com/get-started) · [Zod](https://zod.dev/) · [Next.js internationalization](https://nextjs.org/docs/app/building-your-application/routing/internationalization) · [Recharts](https://recharts.org/en-US/) · [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles) · [Playwright](https://playwright.dev/docs/intro) · [Vitest](https://vitest.dev/guide/) · [GitHub Actions](https://docs.github.com/en/actions)

**AI drill (Week 3).** Use AI to generate mock data sets, Zod schemas from the API contract, repetitive table markup and component tests. Journal each one: task, prompt approach, what AI got wrong, what you fixed.

> **Week 3 is the densest week in the plan.** If the intern is behind by Wednesday, cut in this order: CSV export → the activity timeline → the Products image upload. **Never cut testing and CI** — they are 10 scorecard points and the whole of D15.

**🚩 Gate 3 — Friday, 45 min.** Demo the full authenticated flow with the network throttled to Slow 3G, then break the mock API live and show the failure handling. The mentor asks: *"What is cached right now, for how long, and what invalidates it?"*
**Track F3 eligibility is decided at this gate.**

---

## 9. Week 4 — Second Framework, Quality & Ship

**Theme:** Prove the concepts transfer, then make it fast, accessible and presentable.

The intern rebuilds **two screens** — Login and Orders list — in the extension framework, against the same MSW mock API, as a separate app in the same repo (`/apps/extension`). Order detail is a stretch item, not a requirement.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D16** | Extension crash course: reactivity model, component and template syntax, tooling, TypeScript integration, routing, state management | Scaffolded extension app + routing + app shell + a global store/service, using the shared design tokens | Builds and runs; the intern can explain how its reactivity differs from React's re-render model |
| **D17** | Data fetching, lists and forms in the new paradigm | **Login + Orders list** rebuilt against the same mock API, with loading and error states | Feature parity on those two screens with the React version |
| **D18** | Build, deploy and reflect · framework trade-offs | Extension app deployed (Vercel / Netlify / Expo Go) + `docs/framework-comparison.md` | The comparison covers reactivity, state management, DX, ecosystem, and *when you would choose each* |
| **D19** | Performance: code splitting, dynamic import, `next/image`, font strategy, bundle analysis · accessibility audit (axe), keyboard navigation, screen-reader pass · cross-browser check | Bundle analyzed and reduced; heavy components lazily loaded; all critical and serious axe violations fixed · **code freeze at 17:00** | Lighthouse ≥ 90 on Performance **and** Accessibility on a production build; first-load JS documented before/after |
| **D20** | Visual QA against Figma · production deploy · rehearsal · **Demo Day** · evaluation | Screen-by-screen Figma diff closed; production deploy live; the demo (§9.2); signed scorecard + Individual Development Plan for months 2–6 | No open visual defect above "minor"; demo delivered; scorecard completed; IDP agreed |

**Reading — Vue (F1):** [Vue 3 guide](https://vuejs.org/guide/introduction.html) · [Reactivity fundamentals](https://vuejs.org/guide/essentials/reactivity-fundamentals.html) · [Composition API FAQ](https://vuejs.org/guide/extras/composition-api-faq.html) · [Nuxt](https://nuxt.com/docs) · [Pinia](https://pinia.vuejs.org/) · [Vue Router](https://router.vuejs.org/)
**Reading — Angular (F2):** [Angular docs](https://angular.dev/) · [Components](https://angular.dev/guide/components) · [Signals](https://angular.dev/guide/signals) · [Dependency injection](https://angular.dev/guide/di) · [Routing](https://angular.dev/guide/routing) · [RxJS](https://rxjs.dev/guide/overview)
**Reading — React Native (F3):** [React Native docs](https://reactnative.dev/docs/getting-started) · [Expo](https://docs.expo.dev/) · [React Navigation](https://reactnavigation.org/docs/getting-started)
**Reading — quality:** [web.dev Core Web Vitals](https://web.dev/articles/vitals) · [Next.js optimizing](https://nextjs.org/docs/app/building-your-application/optimizing) · [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) · [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility) · [WCAG 2.2 quick reference](https://www.w3.org/WAI/WCAG22/quickref/) · [axe DevTools](https://www.deque.com/axe/devtools/) · [Vercel](https://vercel.com/docs)

**AI drill (Week 4).** Use AI to translate React components into the extension framework — then **hunt down the idiomatic gaps**. `docs/framework-comparison.md` must name at least three cases where the AI's output was technically valid but not idiomatic (a `useEffect`-shaped pattern where Vue wants a `computed`; an imperative flow where Angular wants an RxJS stream; a web `div` layout where React Native needs `View`/`StyleSheet`), and how the intern rewrote it. This is the month's sharpest test of the JD's "critical thinking about AI output" requirement.

### 9.1 Note on ordering

Performance and accessibility land on D19 rather than being spread through the month, but they are **not** a last-minute bolt-on: the a11y rules are taught on D8 and enforced in every PR from then on (see Appendix A), and a bundle-size check runs in CI from D15. D19 is the audit, not the first attempt.

### 9.2 Demo Day agenda (20 minutes + 10 minutes questions)

| Minutes | Segment |
|---:|---|
| 0–2 | The product and the design system: tokens, components, why they are reusable |
| 2–8 | Live walkthrough of all 8 screens — desktop, then the same flow on a phone |
| 8–11 | Failure modes: slow network, API down, expired session, empty results |
| 11–14 | Quality evidence: Lighthouse scores, axe report, CI run with tests |
| 14–17 | The second framework: side-by-side demo and the trade-off argument |
| 17–20 | **AI usage retrospective:** where AI saved the most time, where it produced UI that looked right but failed on responsiveness or a11y, what you now check by reflex |
| +10 | Mentor and team questions |

---

## 10. AI Adoption — Mandatory Standards

The JD makes AI adoption a hiring requirement, not a bonus. These rules are enforced in code review from Day 1.

### 10.1 Required practice

- Use one assistant daily (Claude Code / Cursor / GitHub Copilot or equivalent).
- Give AI the **project context**, not a generic request. "Make a card component" produces generic output. "Create a `Card` component using our `cva` variant pattern and the `surface`/`border` tokens in `tailwind.config.ts`, matching the `OrderSummary` frame in Figma, with `header`/`body`/`footer` slots" produces mergeable output.
- Use AI where it pays: Figma-to-component conversion, repetitive markup, complex Tailwind and grid layouts, mock data, Zod schemas, component tests, refactors, TypeScript error diagnosis.

### 10.2 Hard rules — a violation blocks the merge

> **Never** paste credentials, customer data, or NDA-scope design assets or source into a public AI tool.
> **Never** merge UI you have not personally checked for **responsiveness, accessibility, design fidelity and render cost**. AI-generated UI looks convincing and fails quietly at 375 px.
> **Never** merge code you cannot explain. Mentors will ask at random, in review.
> **Always** disclose AI-generated sections in the PR description, with a screenshot.

### 10.3 Graded artefact

`docs/ai-journal.md` — maintained from D6 to D20, reviewed at Gates 2 and 3 and at Demo Day. **Minimum 12 entries.** An entry that only says "used AI to build a component" scores zero; each entry must name the defect the intern caught in the generated output.

---

## 11. Evaluation

### 11.1 Scorecard (100 points)

| # | Criterion | Weight | What "excellent" looks like |
|---|---|---:|---|
| 1 | **Core language & React fundamentals** (Week 1 labs) | 10 | All five labs complete; can explain re-render causes and justify a state-management choice unaided |
| 2 | **Design fidelity & responsiveness** | 15 | Matches Figma at every breakpoint; no spacing or type drift |
| 3 | **Component architecture & reuse** | 15 | Typed, variant-driven, genuinely reusable; no copy-paste screens |
| 4 | **Data, state & forms** | 15 | Correct caching and invalidation, complete loading/error/empty states, robust forms |
| 5 | **Performance & accessibility** | 15 | Lighthouse ≥ 90; fully keyboard-operable; can explain each fix |
| 6 | **Testing & CI** | 10 | Behaviour-focused component tests + a working E2E; green pipeline |
| 7 | **Extension framework app** | 10 | Idiomatic in the second framework; the comparison shows real understanding |
| 8 | **AI adoption & critical thinking** | 5 | Strong journal; can name concrete AI failures they caught |
| 9 | **Communication, docs & demo** | 5 | Clear documentation; confident, structured demo |

### 11.2 Decision bands

| Score | Outcome |
|---|---|
| **≥ 85** | **Strong pass.** Assign to a client project as a contributor; open the Fresher conversation early. |
| **70 – 84** | **Pass.** Continue to month 2 on a real project with normal supervision. |
| **55 – 69** | **Conditional.** A two-week targeted remediation plan on the weakest two criteria, then re-assess. |
| **< 55** | **Not passing.** Structured feedback and closure of the internship. |

**Non-negotiable minimums, regardless of total score:** CI green on `main` · zero TypeScript errors · production build deployed · the intern can explain any randomly selected component they wrote.

---

## 12. Mentor Playbook

| Cadence | Duration | Purpose |
|---|---|---|
| Daily standup | 15 min | Unblock — not status theatre |
| PR review | ≤ 4h SLA | Review the screenshot **and** the code; ask for a rewrite rather than fixing it yourself |
| Pairing (Tue, Thu) | 60 min | Intern drives, mentor navigates |
| UI critique (Wed) | 30 min | The whole team critiques one screen — trains the eye, not just the hands |
| Gate review (Fri) | 45 min | Demo + verbal defence + written feedback |
| 1:1 (Fri) | 15 min | Wellbeing, motivation, career direction |

**Mentor preparation before Day 1:** a complete Figma file for all 8 screens with real tokens · the OrderHub API contract · MSW handler seeds · a GitHub repo template with CI and a Vercel project · the track decision made from the interview notes.

**Escalate to the team lead the same day if:**

- The intern is more than 2 days behind the plan at any gate.
- The intern cannot explain their own component twice in one week.
- Any hard AI rule in §10.2 is violated.

**The mentor's most important habit:** review on a real phone, not a browser resized to phone width. Half of all responsive defects only appear on the device.

---

## 13. Reference Library

> Every link points to primary/official documentation (vendor docs, MDN, W3C). Documentation sites reorganize: if a URL 404s, search the same official domain rather than following a third-party mirror or a dated blog post.

### 13.1 Language fundamentals
[MDN JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) · [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) · [MDN Web APIs](https://developer.mozilla.org/en-US/docs/Web/API) · [MDN HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) · [MDN CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [TypeScript utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

### 13.2 React & Next.js
[react.dev — Learn](https://react.dev/learn) · [React reference](https://react.dev/reference/react) · [Hooks](https://react.dev/reference/react/hooks) · [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect) · [Rules of Hooks](https://react.dev/reference/rules/rules-of-hooks) · [Next.js docs](https://nextjs.org/docs) · [App Router](https://nextjs.org/docs/app) · [Next.js optimizing](https://nextjs.org/docs/app/building-your-application/optimizing)

### 13.3 State & data
[Redux Toolkit](https://redux-toolkit.js.org/) · [Redux Essentials](https://redux.js.org/tutorials/essentials/part-1-overview-concepts) · [RTK Query](https://redux-toolkit.js.org/rtk-query/overview) · [Redux style guide](https://redux.js.org/style-guide/) · [Zustand](https://zustand.docs.pmnd.rs/) · [TanStack Query](https://tanstack.com/query/latest) · [React Hook Form](https://react-hook-form.com/get-started) · [Zod](https://zod.dev/) · [Axios](https://axios-http.com/docs/intro)

### 13.4 Styling & UI
[TailwindCSS](https://tailwindcss.com/docs) · [shadcn/ui](https://ui.shadcn.com/docs) · [MUI](https://mui.com/material-ui/getting-started/) · [Ant Design](https://ant.design/docs/react/introduce) · [class-variance-authority](https://cva.style/docs) · [Motion](https://motion.dev/docs) · [Recharts](https://recharts.org/en-US/) · [Chart.js](https://www.chartjs.org/docs/latest/)

### 13.5 Quality — testing, a11y, performance
[Vitest](https://vitest.dev/guide/) · [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) · [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles) · [Playwright](https://playwright.dev/docs/intro) · [MSW](https://mswjs.io/docs/) · [web.dev Core Web Vitals](https://web.dev/articles/vitals) · [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) · [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) · [WCAG 2.2 quick reference](https://www.w3.org/WAI/WCAG22/quickref/) · [axe DevTools](https://www.deque.com/axe/devtools/) · [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)

### 13.6 Extension frameworks
**Vue:** [Vue 3 guide](https://vuejs.org/guide/introduction.html) · [Nuxt](https://nuxt.com/docs) · [Pinia](https://pinia.vuejs.org/) · [Vue Router](https://router.vuejs.org/)
**Angular:** [Angular docs](https://angular.dev/) · [Signals](https://angular.dev/guide/signals) · [RxJS](https://rxjs.dev/guide/overview)
**React Native:** [React Native](https://reactnative.dev/docs/getting-started) · [Expo](https://docs.expo.dev/) · [React Navigation](https://reactnavigation.org/docs/getting-started)

### 13.7 Tooling & practice
[Git documentation](https://git-scm.com/doc) · [GitHub Actions](https://docs.github.com/en/actions) · [Conventional Commits](https://www.conventionalcommits.org/) · [ESLint](https://eslint.org/docs/latest/) · [Prettier](https://prettier.io/docs/en/) · [Vite](https://vite.dev/guide/) · [Vercel](https://vercel.com/docs) · [Figma Dev Mode](https://help.figma.com/hc/en-us/articles/32132100833559-Guide-to-Dev-Mode) · [Google Engineering Practices — Code Review](https://google.github.io/eng-practices/review/)

---

## 14. Appendix A — Definition of Done (every PR)

- [ ] Branch named `feat/…`, `fix/…` or `chore/…`; commits follow Conventional Commits
- [ ] PR description states: what, why, how to test, **which parts were AI-generated**, plus a screenshot or recording
- [ ] Checked at 375 px, 768 px and 1280 px
- [ ] Keyboard-navigable; no new axe critical or serious violations
- [ ] Loading, error and empty states handled
- [ ] Zero TypeScript errors, zero ESLint errors; tests added or updated; CI green
- [ ] No hardcoded UI string (i18n), no magic spacing number (tokens)

## 15. Appendix B — Week-to-Date Planner

| Week | Days | Theme | Checkpoint | Dates (fill in) |
|---|---|---|---|---|
| 1 | D1–D5 | Core language & React fundamentals | Gate 1 | ______ – ______ |
| 2 | D6–D10 | Capstone starts — design system & app shell | Gate 2 | ______ – ______ |
| 3 | D11–D15 | Data, state, forms & testing | Gate 3 | ______ – ______ |
| 4 | D16–D20 | Second framework, quality & ship | **Demo Day** | ______ – ______ |

## 16. Appendix C — Risks, Remediation & the Scope-Cut Ladder

### 16.1 Risks

| Risk | Early signal | Mentor action |
|---|---|---|
| Weak CSS / layout fundamentals | D7 struggle, heavy AI reliance for layout | Insert a half-day of flexbox/grid drills; reduce the D8 catalogue to 4 components |
| Weak TypeScript | Frequent `any` in L1; fights the compiler on D10 | Pair on typing the API client; ban `any` from D11 via a lint rule |
| Week 1 feels too easy for a strong intern | L1–L3 finished by lunch | Skip to L4/L5 and add a stretch lab: `useReducer`-based undo/redo in the cart |
| Extension framework overwhelms the intern | D16 scaffold incomplete | Cut to Login only; keep the comparison document — it carries most of the marks |
| Design fidelity ignored in favour of speed | Screens "work" but drift from Figma | Run the Figma diff at Gate 2, not D20 |
| Over-reliance on AI, shallow understanding | Cannot explain a lab at Gate 1 | One mandatory AI-free day; the intern rebuilds a screen unaided |
| Performance left to the last week | Bundle grows silently through Week 3 | Add a bundle-size check to CI on D15 |

### 16.2 Scope-cut ladder — drop in this order if the intern falls behind

1. CSV export and the activity timeline (D14).
2. The Products image upload (D13) — keep the CRUD and optimistic update.
3. The i18n switcher (D12) — keep dark mode and the extracted strings.
4. The extension app's Orders list (D17) — keep Login and the comparison document.
5. Cross-browser checking (D19) — keep the axe audit and Lighthouse.

**Never cut:** the design-token discipline · the component library · loading/error/empty states · testing and CI · the Demo Day retrospective. Those carry 55 of the 100 scorecard points.

---

*Prepared for the VATEK Internship Program 2026 · Frontend Developer Intern (Fullstack Frontend) · Month 1 of 3–6.*
