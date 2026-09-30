+++
title = "Day 04 - 24/09/2026 (Remote)"
weight = 4
+++

## SPRINT 0 PROGRESS REPORT: TEAM CONVENTIONS ALIGNMENT & IMMUTABLE LEDGER ARCHITECTURE DESIGN (DOMAIN E)

---

### 1. Technical Standardization & Domain Allocation

- **Naming Conventions:**
  - Standardized all database table identifiers to plural `snake_case` (`users`, `warehouses`, `locations`... with the specialized exception of `stock_ledger`).
  - Standardized identifier constraints: Primary keys defaulted strictly to `id`, while foreign keys adhere to `<singular_table_name>_id`.
- **System-Wide Data Type Standardization:**
  - Set primary keys to auto-incrementing `BIGINT` to ensure long-term scalability.
  - Standardized inventory quantities to `DECIMAL(18,4)` to accommodate fractional amounts and agricultural catch-weight variance.
  - Synchronized temporal fields across all tables to `TIMESTAMPTZ` (UTC).
- **Data Integrity via Soft Deletes:** Prohibited hard `DELETE` operations on master data entities with historical transactional links; lifecycle tracking is handled via `is_active` flags or `status` columns.
- **Cross-Domain Dependency Matrix:** Partitioned the 5 core domains across team members and mapped execution dependencies: Domain B (Master Data) and Domain C (Locations/Partners) serve as foundational registries required before Domain D (Inventory) and Domain E (Stock Ledger) link foreign keys.

---

### 2. Immutable Ledger Core Architecture (Domain E - Stock Ledger)

- **Append-Only Ledger Principle:**
  - Designed the centralized `stock_ledger` entity as the single source of truth for physical inventory movements.
  - Restricted operations on this table strictly to `INSERT`, prohibiting `UPDATE` and `DELETE` at both application and database constraint levels.
- **Reversal Entry Mechanism:**
  - When operational mistakes occur (e.g., incorrect SKU receipt or quantity errors), the system avoids modifying historical records; instead, it appends an offsetting counter-entry referenced via `reversal_of_id`.
- **Specialized Audit Trail Design:**
  - Unlike master data tables, `stock_ledger` maintains strictly two audit columns: `created_at` and `created_by` (foreign key referencing `users.id`), deliberately omitting `updated_at` and `updated_by` to maintain historical immutability.
- **Double-Entry Movement Grouping:**
  - Introduced `transaction_group_id` (`UUID`): Internal relocation tasks (Putaway, Bin Transfer, Picking) generate paired debit/credit ledger entries (one negative decrement at the source and one positive increment at the destination) sharing identical group IDs, ensuring net system-wide variance remains exactly zero.
- **Master Reference Classifications:**
  - Defined explicit relational constraints for `movement_types` (`RECEIPT`, `PUTAWAY_OUT`, `PUTAWAY_IN`, `PICK`, `SHIP`, `TRANSFER_OUT`, `TRANSFER_IN`, `ADJUST_IN`, `ADJUST_OUT`, `REVERSAL`) and `reference_types` to safeguard transactional data integrity.

  ---

### 3. Current Status, Challenges & Resolutions (Status & Blockers)
- **Current Status:** Finalized team-wide technical conventions and designed the Domain E immutable ledger architecture strictly on schedule.
- **Challenges & Resolutions:** A potential risk of inconsistent data types across domains emerged; mitigated this by establishing and enforcing a unified data dictionary baseline (BIGINT, DECIMAL(18,4), TIMESTAMPTZ).

### 4. Next Steps (Upcoming Planned Tasks)
- Cross-audit the Data Dictionary covering all 20 domain tables.
- Review, refine, and finalize the system-wide catalog of 42 Business Rules.