# Frontend Track — React Native + Expo
### VATEK Internship Program 2026 · Frontend Developer Intern (Fullstack Frontend)

| | |
|---|---|
| **Framework** | React Native + Expo — the whole month: labs, every screen, every test |
| **Capstone** | OrderHub Mobile — the eight order screens as a phone app |
| **Duration** | 20 working days · Week 1 fundamentals → Weeks 2–4 capstone → Demo Day |
| **Programme guide** | [README.md](README.md) — capstone spec, gates, grading, AI rules |

> **Read-only reference.** This document describes what to learn and build, week by week. Do all of the work in your own repository — nothing here needs to be copied or edited. Each week ends with the deliverables your mentor reviews at that Friday's gate; the rules you are graded against are in the [programme guide](README.md).

---

## Your Stack

| Layer | Technology |
|---|---|
| **Language** | TypeScript (strict) |
| **Framework** | React Native · Expo · Expo Router |
| **Styling & UI** | `StyleSheet` (or NativeWind) · safe-area context |
| **State & data** | Zustand or Redux Toolkit (chosen in L5) · TanStack Query · `msw/native` |
| **Forms** | React Hook Form + Zod |
| **Realtime** | WebSocket or SSE client · `expo-notifications` |
| **Quality** | Jest + React Native Testing Library · Maestro · GitHub Actions |
| **Deploy** | EAS Update / EAS Build |

---

## Before Day 1 — Environment Checklist

- Node.js 22 LTS + pnpm (the JavaScript toolchain)
- VS Code with ESLint and Prettier extensions
- Expo Go installed on your own phone (iOS or Android)
- An Expo account (for EAS)
- Optional: Android Studio emulator or Xcode simulator
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
| **D2** | **Async JavaScript:** the event loop, microtasks vs macrotasks, Promises, `async`/`await`, `Promise.all` vs `allSettled`, `fetch`, `AbortController`, error handling | **L2 — Order fetch script.** A typed script (run with `npx tsx`) that fetches orders from `json-server`, simulates debounced searches by order code, cancels stale requests with `AbortController`, and reports failures | Stale requests are cancelled; network failures are reported, not swallowed |
| **D3** | **React core + hooks:** components & props, state, the render cycle, lists and keys, `useState`, `useEffect` (and **when not to use it**), `useMemo` / `useCallback`, custom hooks | **L3 — Order list screen.** A `FlatList` of orders (`OrderRow`, `StatusBadge`, a search `TextInput`), with `keyExtractor` on the order ID | The intern can explain what triggers each re-render; no index keys |
| **D4** | **React Native core:** `View` / `Text` / `Pressable` / `TextInput` / `Image`, `StyleSheet` and flexbox (column-first), `FlatList` vs `ScrollView`, iOS vs Android differences, Expo and Expo Go, safe areas | **L4 — Order hooks & detail sheet.** Extract `useDebounce`, `useOrders` (fetch with abort), `useStoredFilter` (AsyncStorage), and an order detail bottom sheet; refactor L3 to use them | No `useEffect` that could have been derived state; runs on a real phone in Expo Go |
| **D5** | **Navigation & state:** Expo Router (stacks, tabs, params); Context vs **Zustand** vs **Redux Toolkit**; persisting state | **L5 — Order builder, three ways.** A draft-order screen (add / remove lines, quantity, discount, live total) in Context, in Zustand and in Redux Toolkit; `labs/state-comparison.md` | All three work; the comparison names a concrete scenario where each wins |

### 📚 Week 1 reading — official documentation

| Day | Documentation |
|---|---|
| **D1** | [MDN JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) · [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) · [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) · [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) · [Utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html) |
| **D2** | [Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) · [Event loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Execution_model) · [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) · [AbortController](https://developer.mozilla.org/en-US/docs/Web/API/AbortController) |
| **D3** | [react.dev — Learn](https://react.dev/learn) · [Managing state](https://react.dev/learn/managing-state) · [Hooks reference](https://react.dev/reference/react/hooks) · [You Might Not Need an Effect](https://react.dev/learn/you-might-not-need-an-effect) · [Custom hooks](https://react.dev/learn/reusing-logic-with-custom-hooks) |
| **D4** | [React Native docs](https://reactnative.dev/docs/getting-started) · [Core components](https://reactnative.dev/docs/components-and-apis) · [Style](https://reactnative.dev/docs/style) · [Flexbox](https://reactnative.dev/docs/flexbox) · [FlatList](https://reactnative.dev/docs/flatlist) · [Expo](https://docs.expo.dev/) |
| **D5** | [Expo Router](https://docs.expo.dev/router/introduction/) · [Zustand](https://zustand.docs.pmnd.rs/) · [Redux Toolkit](https://redux-toolkit.js.org/) · [React Native Testing Library](https://oss.callstack.com/react-native-testing-library/) |

**AI drill (Week 1).** Set up the assistant with a project rules file (`CLAUDE.md` / `.cursorrules`) naming the stack and conventions. Then, on every lab: **write it yourself first, then ask AI to review it**, recording which suggestions you accepted and rejected. On D4, additionally ask AI for an order component that uses `useEffect` for something that should be derived state — and explain why it is wrong. Learning to recognise that anti-pattern is worth more than any generated component.

**🚩 Gate 1 — Friday, 45 min.** The intern walks the mentor through L4 and L5. The mentor asks: *"This `FlatList` row re-renders on every keystroke. Show me why, and give me two different fixes."* and *"When would you reach for Redux Toolkit over Zustand for the order builder, and what does it cost you?"* Passing requires answering without reading the code.

### ✅ Week 1 deliverables

What must exist in your own repository by Gate 1 (end of Week 1):

- **D1** — L1 — typed order utilities
- **D2** — L2 — order fetch script
- **D3** — L3 — order list screen
- **D4** — L4 — order hooks & detail sheet
- **D5** — L5 — order builder × 3 (Context / Zustand / Redux Toolkit)

---

## Week 2 — Design System & App Shell *(capstone begins)* (D6–D10)

**Theme:** From an empty repo to an app shell running on a real phone, built from a real design system.

| Day | Focus | Build deliverable | Done when |
|---|---|---|---|
| **D6** | Repository setup · Git/PR flow · Expo project · **Figma handoff** for mobile (spacing, type scale, touch targets ≥ 44 pt) | Expo app committed, **first PR merged**, running in Expo Go, design tokens in `theme.ts` | Runs on a real phone; type-check clean; no magic numbers — tokens only |
| **D7** | Flexbox layout (column-first) · `StyleSheet` · safe areas · keyboard avoidance · phone vs tablet | **Login screen** from the Figma file, correct on a small phone, a large phone and a tablet | Side-by-side with Figma shows no spacing or type drift; the keyboard never covers the input |
| **D8** | Component composition · variants · typed props · accessibility basics (`accessibilityRole`, `accessibilityLabel`, focus order) | 6 base components — Button, TextField, Select, Badge, Card, BottomSheet — plus a catalogue screen | Every component supports variants; TalkBack and VoiceOver announce each one correctly |
| **D9** | Navigation: stacks, tabs, auth flow · deep links · loading and error states | **App shell** — tab bar + Orders stack + auth flow, skeleton loading, not-found screen | Back navigation behaves natively on iOS and Android; a deep link opens a specific order |
| **D10** | API integration · typed client · env configuration · **MSW** (`msw/native`) · loading / error / empty states | **Orders list** (`FlatList`) with mock orders and all three states handled | Airplane mode shows a proper error state, never a blank screen or a crash |

### 🔧 Your stack this week — React Native

| Day | Tools & commands |
|---|---|
| D6 | `create-expo-app` (TypeScript) · ESLint + Prettier · Expo Go · EAS Update preview |
| D7 | `StyleSheet` or NativeWind · `react-native-safe-area-context` · `KeyboardAvoidingView` |
| D8 | `Pressable` with `accessibilityRole` / `accessibilityLabel` · variant styles · `@gorhom/bottom-sheet` |
| D9 | Expo Router — stacks, tabs, an `(auth)` group, `+not-found`, deep links |
| D10 | A typed `fetch` client · `msw/native` · `FlatList` |

**Reading:** [Expo Router](https://docs.expo.dev/router/introduction/) · [Style](https://reactnative.dev/docs/style) · [Flexbox](https://reactnative.dev/docs/flexbox) · [Accessibility](https://reactnative.dev/docs/accessibility) · [Safe area context](https://docs.expo.dev/versions/latest/sdk/safe-area-context/) · [MSW in React Native](https://mswjs.io/docs/integrations/react-native) · [Figma Dev Mode](https://help.figma.com/hc/en-us/articles/15023124644247-Guide-to-Dev-Mode)

**AI drill (Week 2).** Use AI to convert Figma frames into components — and **write down every correction you had to make**. A rules file that names your design tokens and component conventions is what turns generic output into mergeable code; iterate on that file all week. Start the **AI Usage Journal** (`docs/ai-journal.md`) on D6.

**🚩 Gate 2 — Friday, 45 min.** Demo the app shell, component catalogue and orders list on a phone and a tablet, then navigate with TalkBack or VoiceOver. The mentor asks: *"Why does opening an order push onto a stack while Reports switches a tab — and what happens on Android back?"* and *"Where does this spacing value come from?"* (the answer must be a token, not a number).

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
| **D11** | Server-state caching · invalidation · infinite scroll · pull-to-refresh | Orders list with infinite scroll, status filter chips and debounced order-code search | Pull-to-refresh refetches; the filter survives leaving and returning to the tab |
| **D12** | Auth flow · secure token storage · refresh · role-based UI · global state · theming & i18n | Login → protected tabs → auto-refresh on 401 → logout; **Order preferences** screen with default filters, dark mode and vi/en | The token lives in secure storage; the system dark mode is respected |
| **D13** | Multi-step forms · schema validation · field arrays · searchable picker · optimistic updates | **Create Order**: customer → line items → review → submit, with optimistic insert into the orders list | Invalid steps cannot advance; a failed submit rolls back and shows a toast |
| **D14** | Camera / file-picker upload with progress · charts · date picker | **Order detail** (status change + invoice photo upload + timeline) and **Daily sales report** (KPI tiles, chart) | Upload shows real progress and handles failure; the chart fits a phone screen |
| **D15** | Component tests · end-to-end test · CI | ≥8 component tests + 1 E2E (login → filter → order detail → status change); CI green | CI runs on every PR; tests assert user-visible behaviour |

### 🔧 Your stack this week — React Native

| Day | Tools & commands |
|---|---|
| D11 | TanStack Query `useInfiniteQuery` · `RefreshControl` · `onEndReached` |
| D12 | `expo-secure-store` · Zustand or Redux Toolkit (from L5) · `expo-localization` + i18next · `useColorScheme` |
| D13 | React Hook Form + Zod · `useFieldArray` · optimistic `useMutation` |
| D14 | `expo-image-picker` / `expo-document-picker` · upload with progress · a charting library such as `react-native-gifted-charts` |
| D15 | Jest + React Native Testing Library · Maestro · GitHub Actions |

**Reading:** [Infinite queries](https://tanstack.com/query/latest/docs/framework/react/guides/infinite-queries) · [expo-secure-store](https://docs.expo.dev/versions/latest/sdk/securestore/) · [React Hook Form](https://react-hook-form.com/get-started) · [Zod](https://zod.dev/) · [expo-image-picker](https://docs.expo.dev/versions/latest/sdk/imagepicker/) · [expo-localization](https://docs.expo.dev/versions/latest/sdk/localization/) · [React Native Testing Library](https://oss.callstack.com/react-native-testing-library/) · [Maestro](https://docs.maestro.dev/)

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
| **D16** | **Realtime order updates + push notifications** | Orders list and detail update live; a push notification for a new order opens that order | A pushed status appears within 1 s in the foreground; tapping the notification deep-links correctly |
| **D17** | **Real backend integration:** API Gateway base URL · real authentication · CORS · contract drift | The app pointed at the real OrderHub API through the gateway; `docs/contract-diff.md` listing every mismatch found and fixed; E2E re-run against the real API | Switching mock ↔ real needs only an environment change; the E2E passes against the real API |
| **D18** | **Offline & large lists:** cached orders offline, queued status changes, 5,000-row list tuning | App opens offline with the last orders; a status change made offline syncs when back online; 5,000 orders scroll smoothly | The offline queue survives an app restart; no dropped frames on a mid-range Android |
| **D19** | Performance (startup time, re-renders, list tuning, bundle size) · accessibility audit (TalkBack, VoiceOver, dynamic type) | Performance and accessibility fixes · **code freeze at 17:00** | Cold start < 3 s on a mid-range Android; list scroll at 60 fps; screen-reader pass with no blockers |
| **D20** | Visual QA against Figma · EAS preview build · rehearsal · **Demo Day** · evaluation | EAS Update / internal build shared; the demo (README §7.5); signed scorecard + Individual Development Plan for months 2–6 | No open visual defect above "minor"; demo delivered; scorecard completed |

### 🔧 Your stack this week — React Native

| Day | Tools & commands |
|---|---|
| D16 | WebSocket (or an SSE client) · `expo-notifications` · deep link from the notification |
| D17 | `EXPO_PUBLIC_API_BASE_URL` = the API Gateway · token refresh in the fetch client |
| D18 | TanStack Query persistence (async-storage persister) · an offline mutation queue · FlashList or a tuned `FlatList` |
| D19 | React DevTools Profiler · `React.memo` on rows · Hermes · TalkBack / VoiceOver · dynamic type |
| D20 | EAS Update or an EAS internal build |

**Reading:** [Push notifications](https://docs.expo.dev/push-notifications/overview/) · [MDN — WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSocket) · [Query persistence](https://tanstack.com/query/latest/docs/framework/react/plugins/persistQueryClient) · [FlashList](https://shopify.github.io/flash-list/) · [Performance](https://reactnative.dev/docs/performance) · [Accessibility](https://reactnative.dev/docs/accessibility) · [EAS Update](https://docs.expo.dev/eas-update/introduction/)

**AI drill (Week 4).** Use AI to draft the realtime reconnect logic and a contract-diff checklist from the OpenAPI spec — then test the reconnect by killing the mock server mid-stream. AI-written reconnect code commonly duplicates rows or leaks listeners; journal what you found. Present a 5-minute **AI Usage Retrospective** at Demo Day.

### Demo Day agenda (20 minutes + 10 minutes questions)

| Minutes | Segment |
|---:|---|
| 0–2 | The product and the design system: tokens, components, why they are reusable |
| 2–8 | Live walkthrough of all 8 screens — on a phone, then the same flow on a tablet |
| 8–11 | Failure modes: slow network, API down, expired session, empty results |
| 11–14 | Quality evidence: startup-time and scroll measurements, accessibility audit, CI run with tests |
| 14–17 | Realtime and real API: push an order status change live, then show the app running against the real OrderHub API and the contract differences you fixed |
| 17–20 | **AI usage retrospective:** where AI saved the most time, where it produced UI that looked right but failed on responsiveness or a11y, what you now check by reflex |
| +10 | Mentor and team questions |

### ✅ Week 4 deliverables

What must exist in your own repository by Demo Day:

- **D16** — Realtime order updates + push notifications
- **D17** — Real OrderHub API via the gateway · `docs/contract-diff.md`
- **D18** — Offline orders + 5,000-row list
- **D19** — Performance + accessibility audit · code freeze
- **D20** — Visual QA · EAS preview build · Demo Day

---

## Track Reference Library

> Every link points to official documentation; if one moves, search the same official domain.

### React Native & Expo

[React Native docs](https://reactnative.dev/docs/getting-started) · [Core components](https://reactnative.dev/docs/components-and-apis) · [Performance](https://reactnative.dev/docs/performance) · [Accessibility](https://reactnative.dev/docs/accessibility) · [Expo](https://docs.expo.dev/) · [Expo Router](https://docs.expo.dev/router/introduction/) · [React Navigation](https://reactnavigation.org/docs/getting-started) · [EAS](https://docs.expo.dev/eas/) · [react.dev](https://react.dev/learn)

### State, data & forms

[Zustand](https://zustand.docs.pmnd.rs/) · [Zustand repository](https://github.com/pmndrs/zustand) · [Redux Toolkit](https://redux-toolkit.js.org/) · [TanStack Query](https://tanstack.com/query/latest) · [React Hook Form](https://react-hook-form.com/get-started) · [Zod](https://zod.dev/) · [expo-secure-store](https://docs.expo.dev/versions/latest/sdk/securestore/)

### UI & device

[NativeWind](https://www.nativewind.dev/) · [FlashList](https://shopify.github.io/flash-list/) · [expo-image-picker](https://docs.expo.dev/versions/latest/sdk/imagepicker/) · [Push notifications](https://docs.expo.dev/push-notifications/overview/)

### Testing

[React Native Testing Library](https://oss.callstack.com/react-native-testing-library/) · [Jest](https://jestjs.io/docs/getting-started) · [Maestro](https://docs.maestro.dev/) · [MSW in React Native](https://mswjs.io/docs/integrations/react-native)

### Language

[MDN JavaScript guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) · [MDN JavaScript reference](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference) · [Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Closures) · [TypeScript handbook](https://www.typescriptlang.org/docs/handbook/intro.html) · [Everyday types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html) · [Generics](https://www.typescriptlang.org/docs/handbook/2/generics.html) · [Utility types](https://www.typescriptlang.org/docs/handbook/utility-types.html)

---

*VATEK Internship Program 2026 · Frontend Developer Intern · Track — React Native + Expo*
