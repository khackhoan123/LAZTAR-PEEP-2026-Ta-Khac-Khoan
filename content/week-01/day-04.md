+++
title = "Day 04 - 18/09/2026"
weight = 4
+++

## Work completed

### 1. Added and updated diagrams for the PEEP2026 project_2

- Use Case Diagram: Added the "View Internal Statistics" use case for the Admin actor

- Class Diagram: drew class diagrams for
  - Web Admin
  - Mobile Application

- Drew State Machine Diagrams for:
  - OrderStatus
  - PaymentStatus

### 2. Learned about the SKILLS integrated into the project

#### Goal

Understand what workflows this repo has standardized, where they are located, and when to use them.

#### What I found

This project has 2 folders that contain skills: `.agents/skills` and `.claude/skills`. Both contain the same main group of skills, so it can be understood that the project is maintaining a guidance set for different agents/contexts.

These skills were grouped by task type:

1. `add-backend-endpoint`: used when adding or extending backend APIs based on the repo's NextJS standard. It includes module, controller, service, DTO, constants, auth, and test.
2. `add-env-var`: used when adding environment variables for the backend, making sure there is validation when the app boots and updating `.env.example`
3. `add-mobile-screen`: used when adding a screen on mobile or adding an API call in Expo, following the existing conventions for navigation, theme, i18n, and TanStack Query
4. `prisma-migration`: used when changing the Prisma schema or changing the database. This skill emphasizes creating incremental migrations and not editing old migrations that have already been committed
5. `production-readiness`: used to review the checklist before deployment such as real env values, migrations, push notifications, security, CI, and the verify gate
6. `troubleshoot`: used when encountering environment issues, command errors, commit hook errors, TypeScript errors, Auth/IAP issues, or FCM issues
7. `write-tests`: used when tests need to be added for backend or mobile

**Summary**: this skill set covers the important groups of work quite fully, from backend, mobile, database migration, env config, testing, troubleshooting, and readiness before production.
