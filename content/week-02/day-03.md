+++
title = "Day 03 - 23/09/2026 (On-Site)"
weight = 3
+++

## PROGRESS REPORT: MULTI-PAGE ARCHITECTURAL INTEGRATION, CROSS-PLATFORM TUNING & RESUME PRINT ENGINE

---

### 1. Multi-Page Routing Architecture & Strategic Resource Allocation

#### Monorepo Planning & Endpoint Segregation
- **Architectural Mindset:** Rather than splitting the application into two separate repositories, which introduces fragmentation in dependency management and doubles operational maintenance, leveraged the Next.js 14/15 App Router conventions to unify two distinct functional endpoints within a single codebase:
  - **Brand Spatial Landing Page (`/`):** Functions as an interactive 3D spatial presentation designed for architectural pitching and stakeholder demonstrations.
  - **Engineer Portfolio System (`/portfolio`):** Delivers a comprehensive overview of engineering qualifications, academic milestones, and production software case studies.
- **CI/CD Operational Efficiency:** Unified the deployment pipeline onto the Vercel Edge Network. This monorepo architecture streamlines version control, ensures that both presentation endpoints share a cohesive design language, and eliminates duplicate configuration overhead across continuous deployment cycles.

#### Context-Aware Conditional Graphics Resource Management
- **Identified Bottleneck:** Retaining the primary architectural model (`building.glb`), together with high-resolution texture maps and complex mesh buffers, while navigating to the documentation-centric `/portfolio` route consumed excessive GPU VRAM and led to device thermal throttling.
- **Technical Solution:** Refactored the core `LaztarCanvas.tsx` component to support conditional runtime instantiation via a `loadModel={false}` control flag:
  - Preserved brand aesthetic continuity across both endpoints by inheriting the ambient procedural background shaders (Molten Bronze Liquid Shader) and particle emitters (Forge Sparks).
  - Automatically unmounts the structural building geometry, vertex buffers, and heavy material shaders upon route transition to `/portfolio`.
  - Reclaims system memory immediately and allocates 100% of compute performance to document readability and text layout rendering.

#### Digitalization of Professional Engineering Case Studies
- Structured a specialized profile within the tactile Skeuomorphic interface: positioned credentials as a Full-Stack Software Engineer & Korean BrSE (TOPIK 3, FPT University, FPT Software Academy).
- Documented in-depth technical case studies across 3 flagship systems:
  - **CosMate:** Asynchronous request processing, in-memory vector calculations, and database concurrency handling.
  - **CineManage System:** Transaction boundaries, domain-driven architecture, and relational database integrity.
  - **KoiCareHome:** Comprehensive full-stack application lifecycle and containerized deployment.

---

### 2. UI/UX Refinement & In-Depth Typography Normalization

#### Information Architecture & Focus Hierarchy
- Replaced the initial placeholder monogram with an authentic engineering portrait card accented with chamfered metallic borders, subtle linear gradients, and multi-layered depth drop-shadows to match the design system.
- Cleaned up redundant status badges from the persistent Header, consolidating attention around a single glowing availability badge (*Availability Status*) in the Hero Section driven by an ambient `animate-pulse` effect.
- Integrated an internal smooth-scroll navigation bar (*Anchor Smooth Scrolling*) centered with a frosted glass treatment (`backdrop-blur`), directing viewers smoothly to core sections: About, Skills, Projects, and Education.

#### Elimination of Baseline Misalignment in Numerical Figures (Oldstyle Figures Resolution)
- **Problem Statement:** Classical serif typography (*Cormorant Garamond*) applied proportional oldstyle figures with varying vertical alignments (e.g., numbers 3, 4, 7, and 9 dipping below the baseline), causing noticeable visual misalignment across technical metric displays.
- **Engineering Fix:**
  - Standardized typographical hierarchy: reserved serif typography strictly for personal branding identity, while standardizing on a contemporary geometric sans-serif (*Plus Jakarta Sans*) for all quantitative metrics and technical data.
  - Activated advanced OpenType CSS features on quantitative metric elements:
    ```css
    font-variant-numeric: lining-nums tabular-nums;
    ```
  - **Result:** Ensured 100% of numerical data points (80+ APIs, 3 Roles, 100% Dockerized) sit flush on a uniform baseline with equalized tabular column widths.

---

### 3. Dedicated Print Publishing Engine (@media print Engine)

#### High-Fidelity Static Asset Delivery
- Provisioned a centralized static asset delivery endpoint at `/public/Ta-Khac-Khoan-CV.pdf`, enabling instantaneous one-click verification and offline distribution of the official engineering resume.

#### Complete Layout Restructuring for Print Media
- **Technical Challenge:** Native browser print commands (or "Save as PDF") on dark-mode glassmorphic layouts caused illegible color banding, broken margins, and an unusable 8-page document footprint.
- **Engine Configuration:** Isolated print styling completely via `@media print` rules:
  - Stripped out all dynamic UI overhead: WebGL Canvas elements, global navigation bars, interactive CTA buttons, and background bloom shaders are completely hidden during printing.
  - Restructured the multi-column glassmorphic cards into an international standard **Minimalist Tech Resume**: 100% crisp white canvas, maximum-contrast pure black typography (`#000000`), and subtle geometric section dividers.
  - Calibrated print scaling ratios and container margins to guarantee that the full professional history, technical stack, and education fit inside **exactly 1 standard A4 page**.

---

### 4. Cross-Platform Responsive Engineering (Mobile-First Optimization)

#### Vertical Viewport 3D Adaptation (Mobile Port)
- **Aspect Ratio Compensation:** On vertical viewports (Aspect Ratio < 1), dynamically recalculated camera projection parameters (FOV and focal distances), shifting the architectural center upward into the top 40% of the screen to eliminate overlap with typography.
- **Thermal and Battery Conservation:** Throttled spark particle density (*Forge Sparks*) by 50% and disabled touch-screen WebGL cursor calculations (*GlowCursor*) to prioritize frame rate and reduce power consumption during mobile browsing.

#### Mobile Portfolio Layout Refactoring
- Restructured responsive CSS grids: fluidly collapsed 4-column skill matrices into balanced 1-to-2-column views on tablet and mobile viewports without clipping card edges.
- Enforced fluid typography and verified that touch interactions satisfy mobile accessibility standards with minimum tap targets of **44x44px**.

---

### 5. Automated Operational Pipelines & Production Deployment (CI/CD)

- **Version Control Discipline:** Structured repository history on GitHub following strict feature branch separation and standardized Conventional Commit messages.
- **Automated Verification & Edge Deployment:** Integrated the repository with the Vercel Edge Network, establishing automated pipeline gates that enforce strict TypeScript compilation verification (`tsc --noEmit`) and bundle optimization (`npm run build`) prior to promotion.
- **Delivery Verification:** Production build verified with continuous zero-downtime deployments triggered on each authenticated merge event.