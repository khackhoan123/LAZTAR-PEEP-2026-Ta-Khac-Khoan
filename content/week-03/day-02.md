+++
title = "Day 02 - 29/09/2026 (Remote)"
weight = 2
+++

## DAILY PROGRESS REPORT: STOCK LEDGER ARCHITECTURE VERIFICATION, ERD SCHEMA AUDITING & MERMAID AUTOMATION

---

### 1. Completed Tasks (Tasks Accomplished Today)

#### Core Architecture Verification & Stock Ledger Invariants (Domain E)
- **Grouping Identifier Integrity (`transaction_group_id` UUID):**
  - Clarified and preserved the operational role of `transaction_group_id` as a double-entry balance grouping mechanism rather than an arbitrary foreign key.
  - Formulated the execution logic: Each internal stock relocation (Putaway, Bin Transfer) generates balanced paired records (one decrement at origin, one increment at destination) sharing an identical `transaction_group_id`, ensuring net inventory change across the warehouse sums to zero.
- **Relational Optimization & Query Overhead Elimination:**
  - Evaluated and eliminated the need for an intermediate `transactions` table, preventing unnecessary `JOIN` overhead during high-frequency ledger insertion pipelines.
  - Coupled `reference_type_id` and `reference_id` to bind original source documents directly to ledger lines.
- **Self-Referential Reversal Engine (`reversal_of_id`):**
  - Enforced the append-only constraint by modeling an offsetting reversal mechanism (`REVERSAL`) to correct operational mistakes without using `UPDATE` or `DELETE`.
  - Established a two-way audit trail that prevents duplicate reversal operations on the same historical entry.
- **Relational Integrity Audit on 8 Core Foreign Keys:**
  - Standardized relational integrity checks for all 8 foreign keys on `stock_ledger`: `warehouse_id`, `location_id`, `sku_id`, `lot_id`, `movement_type_id`, `reference_type_id`, `reversal_of_id`, and `created_by`.

#### ERD Entity-Relationship Auditing & Bug Resolution
- **Cardinality & Syntax Bug Fixing:**
  - Audited entity connections on Draw.io, correcting relationship cardinality between `skus` and `lots` from an erroneous 1-to-1 mapping back to a proper 1-to-Many (1-N) structure.
  - Enforced strict table-naming conventions across entity headers (`skus`, `lots`) and disambiguated them from singular foreign key attributes (`sku_id`).
- **Missing Entity Recovery & Schema Completeness:**
  - Re-introduced the missing `lots` entity to the primary visual diagram to safeguard expiration-based routing (FEFO) and produce shelf-life tracking.
  - Standardized foreign key attribute syntax by stripping plural suffixes (refactored `warehouses_id`, `reference_types_id`, and `movement_types_id` into `warehouse_id`, `reference_type_id`, and `movement_type_id`).
  - Expanded the visual schema of `stock_ledger` from an incomplete 6-field draft to its full 18-attribute specification (incorporating `sku_id`, `lot_id`, `qty_before`, `qty_after`, `reference_id`, `created_at`, `created_by`, etc.).

#### Automated Mermaid Pipeline & Diagram Layout Optimization
- **Automated Mermaid ERD Compilation:**
  - Engineered an automated script to extract and compile all 20 database entities, data types, and 32 relational links from PlantUML and data dictionary definitions into valid Mermaid ERD code.
  - Enabled direct single-step imports into diagramming tools, eliminating error-prone manual entity drawing.
- **Diagram Readability & Force-Directed Layout:**
  - Standardized all connecting connectors (Edges) from curved paths into orthogonal lines with rounded corners.
  - Configured organic layout parameters (Node Spacing: 100, Repulsive Power: 150) to prevent edge intersections and overlapping table blocks.
- **Cross-Artifact Consistency Validation:**
  - Cross-checked relational definitions across the exported ERD diagram, the PlantUML definition file (`ERD PlantUML_2.puml`), and the Data Dictionary spreadsheet (`WMS_Data_Dictionary_v1_audit_updated_2.xlsx`) to confirm 100% attribute parity.

---

### 2. Current Status, Challenges & Resolutions

- **Current Progress:** Sprint 0 database architecture, entity relationships, and ledger invariants are completely standardized and validated across all documentation artifacts.
- **Challenges Faced:** Manual modifications on visual diagramming tools often led to attribute typos and broken cardinalities.
- **Resolution Implemented:** Eliminated manual drawing by establishing an automated text-to-diagram pipeline via Mermaid scripts, ensuring changes in the schema automatically propagate without human error.

---

### 3. Next Steps (Upcoming Planned Tasks)

- Initialize the NestJS backend project repository and configure the PostgreSQL connection via Prisma ORM.
- Translate the audited 20-table ERD into production-ready `schema.prisma` files.
- Scaffold baseline database migrations and verify foreign key constraint integrity inside Docker containers.