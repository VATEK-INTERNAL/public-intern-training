# VATEK Internship Program 2026 — Training Plans

One-month (20 working days) ramp-up programme for the three intern positions in [`JD_Intern_VATEK_09_2026.docx`](JD_Intern_VATEK_09_2026.docx). Every role follows the same shape: **one week of core language and framework fundamentals, then three weeks building one real capstone project, ending in a Demo Day.** Every exercise, in every role, is built around one business domain: **Orders**.

---

## Folder Structure

```
Training2026/
├── README.md                     ← you are here
├── JD_Intern_VATEK_09_2026.docx  ← source job descriptions
│
├── 01-Backend/
│   ├── README.md                 ← programme guide (mentors)
│   ├── DotNet.md
│   ├── Java.md
│   ├── NodeJS.md
│   └── Python.md
│
├── 02-Frontend/
│   ├── README.md                 ← programme guide (mentors)
│   ├── React.md
│   ├── Vue.md
│   ├── Angular.md
│   └── ReactNative.md
│
├── 03-AI-Engineer/
│   ├── README.md                 ← programme guide (mentors)
│   └── Python.md
│
├── datasets/                     ← shared OrderHub data: orders, products, customers, shipments…
│
└── _archive/                     ← earlier single-file versions, for reference only
```

**One file per main language.** Each track file is independent: it covers only its own language or framework, from the Week 1 fundamentals to Demo Day. An intern needs exactly two documents — their role's programme guide and their track file.

**Read-only.** These documents are the reference for requirements, exercises, stack and reading. Interns do all of their work in their own repositories; nothing in this folder is copied, forked or edited.

---

## Roles & Tracks

| Role | Programme guide | Capstone project | Track files |
|---|---|---|---|
| **Backend Developer Intern** (Fullstack-oriented) | [01-Backend/README.md](01-Backend/README.md) | **OrderHub** — two order microservices behind an API Gateway with aggregated Swagger, plus an AI-assisted admin console | [.NET](01-Backend/DotNet.md) · [Java](01-Backend/Java.md) · [Node.js](01-Backend/NodeJS.md) · [Python](01-Backend/Python.md) |
| **Frontend Developer Intern** (Fullstack Frontend) | [02-Frontend/README.md](02-Frontend/README.md) | **OrderHub Admin Console** — orders list, order detail, create order, sales report, realtime updates, real-API integration | [React](02-Frontend/React.md) · [Vue](02-Frontend/Vue.md) · [Angular](02-Frontend/Angular.md) · [React Native](02-Frontend/ReactNative.md) |
| **AI Engineer Intern** | [03-AI-Engineer/README.md](03-AI-Engineer/README.md) | **OrderHub AI Assistant** — purchase-order extraction, cited answers on order policies, an order agent with tools, MCP server, evaluation harness | [Python](03-AI-Engineer/Python.md) |

---

## Programme Calendar (all roles)

| Week | Days | Phase | Checkpoint |
|---|---|---|---|
| **1** | D1–D5 | Core language & framework fundamentals — five graded labs | **Gate 1** |
| **2** | D6–D10 | Capstone starts — foundations of the product | **Gate 2** |
| **3** | D11–D15 | Capstone core — integration, the hardest week | **Gate 3** |
| **4** | D16–D20 | Ship — quality, documentation, demo | **Demo Day** |

---

## Who Reads What

| Reader | Reads | When |
|---|---|---|
| **Intern** | Their role's programme guide §1–§6 | Once, on Day 1 |
| **Intern** | Their track file — the reference for what to build in their own repository | Every day |
| **Mentor** | The whole programme guide + each mentee's track file; reviews the intern's own repository against the week's deliverables | Before Day 1, then at every Friday gate |
| **Team lead** | This file + each programme guide's §9 Evaluation + gate outcomes from the mentors | Weekly |

---

## Common to Every Role

- **Same programme shape** — Week 1 fundamentals, capstone from Day 6, three Friday gates, Demo Day on Day 20.
- **One domain: Orders** — every exercise in every role, from the Week 1 labs to the capstone, works on orders: order data, order APIs, order screens, purchase orders and order policies.
- **One dataset** — [`datasets/`](datasets/README.md) holds the shared order data (200 orders, products, customers, shipments, events), a `json-server` mock API and the answer key for the Week 1 report labs.
- **Same AI-adoption standard** — mandatory per the JD; merge-blocking rules and a graded AI usage journal (each programme guide §8).
- **Same decision bands** — ≥ 85 strong pass · 70–84 pass · 55–69 conditional (two-week remediation) · < 55 not passing (each programme guide §9).
- **Cross-role integration** — all three roles work on the same OrderHub platform: every frontend track switches to the Backend's real order API on D17, and the AI assistant's `lookup_order` reads the Backend's order schema.

---

*VATEK Internship Program 2026 · Training Plans · Month 1 of a 3–6 month internship.*
