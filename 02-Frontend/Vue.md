# Frontend Track — Vue 3 + Nuxt
### VATEK Internship Program 2026 · Frontend Developer Intern (Fullstack Frontend)

| | |
|---|---|
| **Framework** | Vue 3 + Nuxt — the whole month: labs, every screen, every test |
| **Capstone** | OrderHub Admin Console — every screen is an order task |
| **Duration** | 20 working days · Week 1 fundamentals → Weeks 2–4 capstone → Demo Day |
| **Programme guide** | [README.md](README.md) — capstone spec, gates, grading, AI rules |

> **Read-only reference.** This document describes what to learn and build, week by week. Do all of the work in your own repository — nothing here needs to be copied or edited. Each week ends with the deliverables your mentor reviews at that Friday's gate; the rules you are graded against are in the [programme guide](README.md).

---

## Your Stack

| Layer | Technology |
|---|---|
| **Language** | TypeScript (strict) |
| **Framework** | Vue 3 (Composition API, `<script setup lang="ts">`) · Nuxt 4 |
| **Styling & UI** | TailwindCSS · Reka UI primitives |
| **State & data** | Pinia · `useFetch` / `$fetch` (or TanStack Query for Vue) · MSW |
| **Forms** | VeeValidate + Zod |
| **Realtime** | VueUse `useEventSource` / `useWebSocket` |
| **Quality** | Vitest + Vue Test Utils · Playwright · axe · Lighthouse · GitHub Actions |
| **Deploy** | Vercel or Netlify |

---

## Before Day 1 — Environment Checklist

- Node.js 22 LTS + pnpm (the JavaScript toolchain)
- VS Code with ESLint, Prettier and Tailwind CSS IntelliSense extensions
- Vue DevTools browser extension
- VS Code "Vue - Official" extension
- A Vercel or Netlify account linked to your GitHub
- Access to the OrderHub Figma file (view + Dev Mode) — ask your mentor
- The Week 1 lab dataset from [`datasets/`](../datasets/README.md), served with `json-server` (see its README)
- Git configured with your company identity; SSH key added to GitHub
- Your AI coding assistant installed and signed in (Claude Code / Cursor / Copilot)
- Read the programme guide [README.md](README.md) §1–§6 once, end to end

---

## Week 1 — Core Language & Framework Fundamentals (D1–D5)

**Theme:** Understand the machine before decorating it. Mornings are concepts and reading; afternoons are graded micro-exercises (**labs**) in a `/labs` folder, kept separate from the capstone repository.

| Day | Concept block | Lab (afternoon) | Done when |
|---|---|---|---|
| **D1** | **JavaScript core:** scope & closures, `this`, prototypes, ES modules, destructuring, spread/rest, optional chaining, immutability. **TypeScript core:** primitives, unions, interfaces vs types, generics, narrowing, utility types (`Partial`, `Pick`, `Omit`, `Record`), `unknown` vs `any` | **L1 — Typed order utilities.** Define `Order`, `OrderLine` and `OrderStatus` types, then write 8 pure functions over them — group by status, sort by date or total, paginate, compute an order total from its lines, format currency (VND/USD) and dates — with unit tests | `tsc --strict` is clean; no `any`; every function has a test |
| **D2** | **DOM, events & async:** DOM API, event delegation and bubbling, the **event loop**, microtasks vs macrotasks, Promises, `async`/`await`, `Promise.all` vs `allSettled`, `fetch`, `AbortController`, error handling | **L2 — Vanilla JS order list.** No framework: fetch orders from a mock endpoint (`json-server` serving `datasets/db.json`), render a list searchable by order code or customer with debounce, handle loading/error/empty, cancel in-flight requests | Works with no framework; rapid typing cancels stale requests; failures show a message, not a blank page |
| **D3** | **Vue core:** single-file components, template syntax, directives (`v-if`, `v-for`, `v-model`, `v-bind`, `v-on`), props & emits, the reactivity system (`ref`, `reactive`), `computed`, lists and keys, slots | **L3 — Order list in Vue.** Rebuild L2 component by component (`OrderList`, `OrderRow`, `OrderSearch`, `StatusBadge`), keyed by order ID | The intern can explain what Vue tracks and re-renders; no index-as-key on a mutable list |
| **D4** | **Composition API in depth:** `<script setup>`, `computed` vs `watch` vs `watchEffect` (and **when not to watch**), lifecycle hooks, template refs, `provide` / `inject`, **composables** | **L4 — Order composables.** Extract `useDebounce`, `useOrders` (fetch with abort), `useLocalStorage` (remember the last status filter), `useDisclosure` (order detail drawer); refactor L3 to use them | No `watch` that could have been a `computed`; composables are tested |
| **D5** | **State management:** local vs shared vs global; `provide` / `inject` and its limits; **Pinia** — stores, getters, actions, setup vs option stores, DevTools; when each is the right call | **L5 — Order builder, two ways.** A draft-order editor (add / remove lines, quantity, discount, live total) with `provide` / `inject` and with Pinia; `labs/state-comparison.md` | Both work; the comparison names a concrete scenario where each wins |

### 📚 Week 1 reading — official documentation

| Day | Documentation |
|---|---|
| **D1** | [MDN JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) · [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) · [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) · [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) · [Utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) |
| **D2** | [MDN DOM](https://developer.mozilla.org/en-US/docs/Web/API/Document_Object_Model) · [MDN Events](https://developer.mozilla.org/en-US/docs/Web/API/Event) · [Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) · [Event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model) · [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) · [AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) |
| **D3** | [Vue guide](https://vuejs.org/guide/introduction.html) · [Template syntax](https://vuejs.org/guide/essentials/template-syntax.html) · [Reactivity fundamentals](https://vuejs.org/guide/essentials/reactivity-fundamentals.html) · [Computed properties](https://vuejs.org/guide/essentials/computed.html) · [List rendering](https://vuejs.org/guide/essentials/list.html) · [Components basics](https://vuejs.org/guide/essentials/component-basics.html) |
| **D4** | [Composition API FAQ](https://vuejs.org/guide/extras/composition-api-faq.html) · [Watchers](https://vuejs.org/guide/essentials/watchers.html) · [Lifecycle hooks](https://vuejs.org/guide/essentials/lifecycle.html) · [Composables](https://vuejs.org/guide/reusability/composables.html) · [Provide / inject](https://vuejs.org/guide/components/provide-inject.html) |
| **D5** | [Pinia](https://pinia.vuejs.org/) · [Pinia core concepts](https://pinia.vuejs.org/core-concepts/) · [State management in Vue](https://vuejs.org/guide/scaling-up/state-management.html) · [Vitest](https://vitest.dev/) · [Vue Test Utils](https://test-utils.vuejs.org/) |

**AI drill (Week 1).** Set up the assistant with a project rules file (`CLAUDE.md` / `.cursorrules`) naming the stack and conventions. Then, on every lab: **write it yourself first, then ask AI to review it**, recording which suggestions you accepted and rejected. On D4, additionally ask AI for an order component that uses `watch` for something that should be derived state — and explain why it is wrong. Learning to recognise that anti-pattern is worth more than any generated component.

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L4 and L5. The mentor asks: *"This order list recomputes on every keystroke. Show me why, and give me two different fixes."* and *"When would you reach for Pinia over `provide` / `inject` for the order builder, and what does it cost you?"* Passing requires answering without reading the code.

### ✅ Week 1 deliverables

What must exist in your own repository by Gate 1 (end of Week 1):

- **D1** — L1 — typed order utilities
- **D2** — L2 — vanilla JS order list
- **D3** — L3 — order list in Vue
- **D4** — L4 — order composables
- **D5** — L5 — order builder × 2 (provide/inject / Pinia)

---

## Week 2 — Design System & App Shell *(capstone begins)* (D6–D10)

**Theme:** From an empty repo to a deployed app shell built from a real design system.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D6** | Repository setup · Git/PR flow · preview deploy · **Figma handoff** (spacing, typography, colour tokens, auto-layout, asset export) | Scaffold committed, **first PR merged**, preview deployed, design tokens in the Tailwind config | Preview URL live; type-check clean; no magic numbers — tokens only |
| **D7** | Semantic HTML · CSS layout (flexbox, grid) · responsive strategy · Tailwind fundamentals | **Login screen** from the Figma file, responsive at 375 / 768 / 1280 / 1920 px | Side-by-side with Figma shows no spacing or type-scale drift |
| **D8** | Component composition · variants · typed props · accessibility basics (labels, roles, focus order, keyboard) | 6 base components — Button, Input, Select, Badge, Card, Modal — plus a component catalogue page | Every component supports variants and sizes; every interactive one works by keyboard alone |
| **D9** | Routing · layouts · route guards · lazy-loaded routes · loading and error states | **App shell** — sidebar + topbar + authenticated route group, skeleton loading, 404 and error pages | Moving between order pages does not remount the shell; the mobile drawer works |
| **D10** | API integration · typed API client · env configuration · **MSW** mock server · loading / error / empty states | **Orders list** rendering mock orders with all three states handled | Turning the mock off shows a proper error state, never a blank screen or a crash |

### 🔧 Your stack this week — Vue

| Day | Tools & commands |
|---|---|
| D6 | `nuxi init` (TypeScript) · `@nuxtjs/tailwindcss` · `@nuxt/eslint` · Prettier · Vercel / Netlify preview deployments |
| D7 | Tailwind utilities and design tokens · `@nuxt/fonts` |
| D8 | SFC components with `defineProps` / `defineEmits` / slots · variant maps · Reka UI primitives |
| D9 | Nuxt `pages/` + `layouts/` · route middleware for auth · `error.vue` · lazy components |
| D10 | A typed `$fetch` client · MSW browser worker |

**Reading:** [Nuxt](https://nuxt.com/docs) · [Nuxt routing](https://nuxt.com/docs/4.x/getting-started/routing) · [Nuxt layouts](https://nuxt.com/docs/guide/directory-structure/layouts) · [Reka UI](https://reka-ui.com/) · [MDN CSS layout](https://developer.mozilla.org/en-US/docs/Learn_web_development/Core/CSS_layout) · [MDN HTML elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements) · [TailwindCSS](https://tailwindcss.com/docs) · [WAI-ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) · [MSW](https://mswjs.io/docs/) · [Figma Dev Mode](https://help.figma.com/hc/en-us/articles/15023124644247-Guide-to-Dev-Mode)

**AI drill (Week 2).** Use AI to convert Figma frames into components — and **write down every correction you had to make**. A rules file that names your design tokens and component conventions is what turns generic output into mergeable code; iterate on that file all week. Start the **AI Usage Journal** (`docs/ai-journal.md`) on D6.

**🚩 Gate 2 — Friday, 45 min.** Demo the app shell, component catalogue and orders list on desktop and a phone, then navigate the whole UI with the keyboard only. The mentor asks: *"Why is this data fetched with `useFetch` during server rendering and that with `$fetch` on the client?"* and *"Where does this spacing value come from?"* (the answer must be a token, not a number).

### ✅ Week 2 deliverables

What must exist in your own repository by Gate 2 (end of Week 2):

- **D6** — Scaffold · design tokens · preview · first PR merged
- **D7** — Login screen
- **D8** — 6 base components + catalogue
- **D9** — App shell · not-found / error
- **D10** — MSW mock API · Orders list with 3 states

---

## Week 3 — Order Data, State, Forms & Testing (D11–D15)

**Theme:** The part that separates a page from an application — real order data, real failure modes.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D11** | Server-state caching · invalidation · server-side pagination · URL as state | Orders list with server pagination, status filter, date-range filter and debounced order-code search | Filters live in the URL; refreshing the page restores the same view |
| **D12** | Auth flow · token storage and refresh · route guards · role-based UI · global state · theming & i18n | Login → protected routes → auto-refresh on 401 → logout; **Order preferences** screen with default filters, dark mode and vi/en | An expired token silently refreshes once, then redirects to login; no hardcoded UI string remains |
| **D13** | Multi-step forms · schema validation · dynamic field arrays · searchable picker · optimistic updates · toasts | **Create Order**: customer → line items (product picker, quantity, discount) → review → submit, with optimistic insert into the orders list | Invalid steps cannot advance; a failed submit rolls back the optimistic order and shows a toast |
| **D14** | File upload with progress · charts · date-range input · export | **Order detail** (status change + invoice upload + timeline) and **Daily sales report** (KPI tiles, chart, CSV export) | Upload shows real progress and handles failure; the chart reads well in light and dark mode |
| **D15** | Component tests · end-to-end test · CI | ≥8 component tests + 1 E2E (login → filter → order detail → status change); CI green | CI runs on every PR; tests assert user-visible behaviour, not implementation details |

### 🔧 Your stack this week — Vue

| Day | Tools & commands |
|---|---|
| D11 | `useFetch` / `useAsyncData` with `watch` on the query (or TanStack Query for Vue) · `useRoute().query` for URL state |
| D12 | Pinia (auth + preferences stores) · `@nuxtjs/i18n` · `@nuxtjs/color-mode` |
| D13 | VeeValidate + Zod (`toTypedSchema`) · `useFieldArray` · optimistic update in a Pinia action |
| D14 | Upload progress via `XMLHttpRequest` · vue-echarts · date-fns |
| D15 | Vitest + Vue Test Utils (`@nuxt/test-utils`) · Playwright · GitHub Actions |

**Reading:** [Nuxt data fetching](https://nuxt.com/docs/getting-started/data-fetching) · [Pinia](https://pinia.vuejs.org/) · [VeeValidate](https://vee-validate.logaretm.com/v4/) · [Zod](https://zod.dev/) · [Nuxt i18n](https://i18n.nuxtjs.org/) · [vue-echarts](https://github.com/ecomfe/vue-echarts) · [Vue Test Utils](https://test-utils.vuejs.org/) · [Nuxt testing](https://nuxt.com/docs/4.x/getting-started/testing) · [Playwright](https://playwright.dev/docs/intro) · [Testing Library guiding principles](https://testing-library.com/docs/guiding-principles)

**AI drill (Week 3).** Use AI to generate mock order data, validation schemas from the API contract, repetitive table or list markup and component tests. Journal each one: task, prompt approach, what AI got wrong, what you fixed.

> **Week 3 is the densest week in the plan.** If the intern is behind by Wednesday, cut in this order: CSV export → the activity timeline → the product-picker search in Create Order. **Never cut testing and CI** — they are 10 scorecard points and the whole of D15.

**🚩 Gate 3 — Friday, 45 min.** Demo the full authenticated flow with the network throttled to Slow 3G, then break the mock API live and show the failure handling. The mentor asks: *"What is cached right now, for how long, and what invalidates it?"*

### ✅ Week 3 deliverables

What must exist in your own repository by Gate 3 (end of Week 3):

- **D11** — Cached orders list · pagination · filters
- **D12** — Auth flow · Order preferences (dark mode, vi/en)
- **D13** — Create Order (multi-step) · optimistic insert
- **D14** — Order detail · Daily sales report
- **D15** — Component tests · E2E · CI green

---

## Week 4 — Realtime, Real API & Ship (D16–D20)

**Theme:** Live order updates, the real backend, then make it fast, accessible and presentable.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D16** | **Realtime order updates:** Server-Sent Events or WebSocket · reconnect · reconciling pushed updates with cached data | Orders list and Order detail update live when an order's status changes; a "new order" toast; automatic reconnect after network loss | A status pushed by the mock server appears within 1 s without a refresh; no duplicate rows after a reconnect |
| **D17** | **Real backend integration:** API Gateway base URL · real authentication · CORS · contract drift | The app pointed at the real OrderHub API through the gateway; `docs/contract-diff.md` listing every mismatch found and fixed; E2E re-run against the real API | Switching mock ↔ real needs only an environment change; the E2E passes against the real API |
| **D18** | Bulk order actions · large lists · export | Multi-select bulk status change with confirmation; CSV export of the filtered orders; the orders table stays smooth with 5,000 rows (virtualization) | Bulk change reports partial failures per order; scrolling 5,000 rows stays smooth |
| **D19** | Performance (code splitting, lazy loading, image & font strategy, bundle analysis) · accessibility audit (axe, keyboard, screen reader) · cross-browser check | Bundle reduced; all critical and serious axe violations fixed · **code freeze at 17:00** | Lighthouse ≥ 90 on Performance and Accessibility (production build); first-load JS documented before/after |
| **D20** | Visual QA against Figma · production deploy · rehearsal · **Demo Day** · evaluation | Figma diff closed; production deploy live; the demo (README §7.5); signed scorecard + Individual Development Plan for months 2–6 | No open visual defect above "minor"; demo delivered; scorecard completed |

### 🔧 Your stack this week — Vue

| Day | Tools & commands |
|---|---|
| D16 | VueUse `useEventSource` or `useWebSocket` · patch the Pinia store / cached data on each message |
| D17 | `runtimeConfig.public.apiBase` = the API Gateway · `$fetch` interceptors for token refresh |
| D18 | Row selection · `vue-virtual-scroller` · CSV export |
| D19 | `Lazy`-prefixed components · `@nuxt/image` · `nuxi analyze` · axe DevTools · Lighthouse |
| D20 | Vercel or Netlify production deployment |

**Reading:** [MDN — Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) · [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) · [VueUse useEventSource](https://vueuse.org/core/useeventsource/) · [VueUse useWebSocket](https://vueuse.org/core/usewebsocket/) · [vue-virtual-scroller](https://github.com/Akryum/vue-virtual-scroller) · [Nuxt Image](https://image.nuxt.com/) · [web.dev Core Web Vitals](https://web.dev/articles/vitals) · [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) · [axe DevTools](https://www.deque.com/axe/devtools/) · [WCAG 2.2 quick reference](https://www.w3.org/WAI/WCAG22/quickref/)

**AI drill (Week 4).** Use AI to draft the realtime reconnect logic and a contract-diff checklist from the OpenAPI spec — then test the reconnect by killing the mock server mid-stream. AI-written reconnect code commonly duplicates rows or leaks listeners; journal what you found. Present a 5-minute **AI Usage Retrospective** at Demo Day.

### Note on ordering

Performance and accessibility land on D19 rather than being spread through the month, but they are **not** a last-minute bolt-on: the a11y rules are taught on D8 and enforced in every PR from then on (see Appendix A), and a bundle-size check runs in CI from D15. D19 is the audit, not the first attempt.

### Demo Day agenda (20 minutes + 10 minutes questions)

| Minutes | Segment |
|---:|---|
| 0–2 | The product and the design system: tokens, components, why they are reusable |
| 2–8 | Live walkthrough of all 8 screens — desktop, then the same flow on a phone |
| 8–11 | Failure modes: slow network, API down, expired session, empty results |
| 11–14 | Quality evidence: Lighthouse scores, accessibility audit, CI run with tests |
| 14–17 | Realtime and real API: push an order status change live, then show the app running against the real OrderHub API and the contract differences you fixed |
| 17–20 | **AI usage retrospective:** where AI saved the most time, where it produced UI that looked right but failed on responsiveness or a11y, what you now check by reflex |
| +10 | Mentor and team questions |

### ✅ Week 4 deliverables

What must exist in your own repository by Demo Day:

- **D16** — Realtime order updates
- **D17** — Real OrderHub API via the gateway · `docs/contract-diff.md`
- **D18** — Bulk order actions · CSV export · 5,000-row table
- **D19** — Performance + accessibility audit · code freeze
- **D20** — Visual QA vs Figma · production deploy · Demo Day

---

## Track Reference Library

> Every link points to official documentation; if one moves, search the same official domain.

### Vue & Nuxt

[Vue guide](https://vuejs.org/guide/introduction.html) · [Vue API](https://vuejs.org/api/) · [Nuxt](https://nuxt.com/docs) · [Vue Router](https://router.vuejs.org/) · [VueUse](https://vueuse.org/) · [Vue DevTools](https://devtools.vuejs.org/)

### State, data & forms

[Pinia](https://pinia.vuejs.org/) · [TanStack Query for Vue](https://tanstack.com/query/latest/docs/framework/vue/overview) · [VeeValidate](https://vee-validate.logaretm.com/v4/) · [Zod](https://zod.dev/) · [Nuxt i18n](https://i18n.nuxtjs.org/)

### Styling & UI

[TailwindCSS](https://tailwindcss.com/docs) · [Reka UI](https://reka-ui.com/) · [vue-echarts](https://github.com/ecomfe/vue-echarts)

### Testing

[Vitest](https://vitest.dev/guide/) · [Vue Test Utils](https://test-utils.vuejs.org/) · [Nuxt testing](https://nuxt.com/docs/4.x/getting-started/testing) · [Playwright](https://playwright.dev/docs/intro) · [MSW](https://mswjs.io/docs/)

### Language & web platform

[MDN JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) · [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) · [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) · [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) · [Utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) · [MDN — Server-sent events](https://developer.mozilla.org/en-US/docs/Web/API/Server-sent_events) · [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) · [web.dev Core Web Vitals](https://web.dev/articles/vitals) · [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview) · [axe DevTools](https://www.deque.com/axe/devtools/) · [WCAG 2.2 quick reference](https://www.w3.org/WAI/WCAG22/quickref/)

---

*VATEK Internship Program 2026 · Frontend Developer Intern · Track — Vue 3 + Nuxt*
