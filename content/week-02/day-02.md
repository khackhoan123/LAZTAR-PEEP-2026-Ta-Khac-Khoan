+++
title = "Day 02 - 22/09/2026 (Remote)"
weight = 2
+++

## SYSTEM ARCHITECTURE & PROJECT KICKOFF REPORT: MINI-WMS (FRESHLINK PRODUCE)

---

### 1. Project Scoping & Tech Stack Alignment

#### Strategic Mission & Operational Context
- **Project Scope:** Selected to develop the **FreshLink Produce** agricultural warehouse management system over a 5-week competitive milestone cycle between two trainee teams. Performance metrics will evaluate core proficiencies (Backend, Frontend, BA, QA) for official commercial client assignments.
- **Standardized Production Stack:**
  - **Backend:** NestJS (TypeScript) with domain-driven modular architecture.
  - **Database & ORM:** PostgreSQL 16 accompanied by Prisma ORM for type-safe schema and migration management.
  - **Frontend:** Next.js (React + TypeScript) architected with a strict **Mobile-first** operational mindset for floor workers.
  - **Infrastructure & Automation:** Docker Compose for local environments and GitHub Actions for continuous integration (CI).

---

### 2. Domain Rule Analysis & Core System Architecture

#### Architectural Invariants
- **Ledger-based / Append-only Engine:** Established a non-negotiable accounting principle—strictly prohibiting direct `UPDATE` or `DELETE` queries on inventory records. All stock physical deltas are appended as immutable records within `stock_ledger` (analogous to banking balance ledgers). Errors must be balanced out using offsetting reversal entries to preserve a 100% auditable trail.
- **Single Gatekeeper Architecture:** Both Inbound and Outbound workflows are prohibited from directly manipulating balances, delegating balance state modifications exclusively through the centralized `InventoryLedgerService`.

#### 8 Agricultural Produce Warehouse Domain Rules
1. **Stock State Segregation:** Rigorous boundary separation between physical stock on shelves (*On-hand*) and allocatable volume (*Available = On-hand - Committed*).
2. **Standard Base UoM Normalization:** Automating the conversion of packaging containers (crates, cartons, baskets) into normalized base units (`kg`) on ledger entries.
3. **FEFO (*First Expired, First Out*) Allocation:** Enforcing expiration-based picking allocation to safeguard against organic decay, replacing conventional FIFO.
4. **Catch Weight Integration:** Handling variable-weight agricultural items at packaging stations to deduct inventory and invoice clients on actual physical weight rather than nominal weight.
5. **Short-Pick Handling:** Defining three branch paths when picking stock is insufficient: partial fulfillment, SKU substitution, or automatic backorder generation.
6. **Shrinkage Control via Approval Gates:** Mandating that all stock adjustment tickets remain pending until verified and approved by the Warehouse Manager before ledger commit.
7. **Idempotent Balance Initialization:** Securing opening balance imports with Idempotency Keys to prevent duplicate ledger transactions during initial batch loads.
8. **Concurrency Control (Row-Level Locking):** Enforcing pessimistic concurrency locks (`SELECT ... FOR UPDATE`) in PostgreSQL to eliminate race conditions when multiple operators pick identical SKUs simultaneously.

#### Authorization & Security Model (Lightweight RBAC)
- Implemented a fine-grained **Permission-based Access Control** framework decoupling business capabilities from arbitrary role names.
- Configured custom NestJS Decorators and Guards to evaluate atomic operational privileges, adhering to the Principle of Least Privilege and Open/Closed Principle.

---

### 3. Team Organization & Resource Distribution (5-Person Team)

#### Leadership & Core Code Ownership
- Stepped into the **Team Lead** position, spearheading sprint execution, Git conventions, code reviews, and serving as primary **Code Owner** of the `InventoryLedgerService`.
- **Leadership Boundary Delineation:** Clarified responsibilities between delivery management (Lead) and functional use-case specifications/rule matrices (BA).

#### Member Allocation Matrix:
- **Lead / Backend 1:** Prisma schema engineering, core ledger service, Auth/RBAC, Master Data, Inbound pipelines, and Stock Adjustment queues.
- **Backend 2:** Outbound pipelines, FEFO routing algorithms, short-pick branches, catch-weight station processing, and dispatch fulfillment.
- **Frontend 1 (Mobile-first):** Floor operational UI: barcode scanning interfaces, receiving docks, mobile pick-lists, and packaging verification screens.
- **Frontend 2 (Desktop):** Web administration console: Master Data configurations, granular ledger audit reports, and managerial approval dashboards.
- **BA / QA:** End-to-end business flowcharts, test matrices targeting edge boundaries (negative stock limits, catch weight variance, near-expiry allocations).

---

### 4. Week 1 Gate-Check Deliverables

- Instituted daily 15-minute standup cadences and formalized technical communication channels.
- Scaffolded repository environments, Docker Compose dev setups, and baseline CI actions.
- Finalized end-to-end warehouse workflow documentation for mentor gate-check evaluation ahead of Week 2 implementation.