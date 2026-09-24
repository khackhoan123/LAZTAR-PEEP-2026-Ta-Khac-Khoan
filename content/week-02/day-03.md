+++
title = "Day 03 - 23/09/2026 (On-Site)"
weight = 3
+++

## PROGRESS REPORT: MULTI-PAGE ARCHITECTURAL INTEGRATION, CROSS-PLATFORM TUNING & RESUME PRINT ENGINE

---

### 1. Multi-Page Routing Architecture & Strategic Resource Allocation

#### Monorepo Planning & Endpoint Segregation
- **Architectural Mindset:** Rather than maintaining fragmented codebases across separate repositories, leveraged Next.js App Router conventions to unify two distinct functional endpoints within a single deployment pipeline:
  - **Brand Spatial Landing Page (`/`):** Serves as an interactive 3D spatial experience tailored for architectural pitches and stakeholder presentations.
  - **Engineer Portfolio System (`/portfolio`):** Delivers a comprehensive overview of engineering qualifications, academic milestones, and production software case studies.
- **CI/CD Efficiency:** Unified the build pipeline onto the Vercel Edge Network, eliminating redundant pipeline runs and halving deployment configuration overhead.

#### Conditional Graphics Resource Management
- **Identified Bottleneck:** Retaining the primary architectural model (`building.glb`) along with heavy mesh buffers while navigating to the documentation-centric `/portfolio` route wasted significant GPU VRAM and introduced thermal penalties.
- **Technical Solution:** Refactored `LaztarCanvas.tsx` to support conditional runtime instantiation via a `loadModel={false}` control flag:
  - Inherited visual brand continuity by preserving ambient background shaders (Molten Bronze Liquid Shader) and particle emitters (Forge Sparks).
  - Automatically unmounts high-density geometry and textures upon entering `/portfolio`, immediately reclaiming memory and allocating 100% of compute performance to document readability.

#### Digitalization of Professional Engineering Case Studies
- Structured a specialized profile within the tactile Skeuomorphic interface: positioned credentials as a Full-Stack Software Engineer & Korean BrSE (TOPIK 3, FPT University, FPT Software Academy).
- Documented in-depth technical case studies across 3 flagship systems: **CosMate** (asynchronous pipelines, in-memory vector calculations), **CineManage System** (transaction boundaries, domain architecture), and **KoiCareHome**.

---

### 2. UI/UX Refinement & In-Depth Typography Normalization

#### Information Architecture & Focus Hierarchy
- Replaced the initial placeholder monogram with an authentic engineering portrait card accented with chamfered metallic borders and multi-layered depth drop-shadows.
- Cleaned up redundant status badges from the persistent Header, consolidating attention around a single glowing availability badge (*Availability Status*) in the Hero Section driven by an ambient `animate-pulse` effect.
- Integrated an internal smooth-scroll navigation bar (*Anchor Smooth Scrolling*) centered with a frosted glass treatment (`backdrop-blur`), directing viewers smoothly to core sections: About, Skills, Projects, and Education.

#### Elimination of Baseline Misalignment in Numerical Figures (Oldstyle Figures Resolution)
- **Problem Statement:** Classical serif typography (*Cormorant Garamond*) introduced proportional oldstyle numerals exhibiting inconsistent baseline descenders (e.g., numbers 3, 4, 7, and 9 dipping below the baseline), causing noticeable visual misalignment across technical metric displays.
- **Engineering Fix:**
  - Standardized typographical hierarchy: reserved serif accents for personal branding and standardized on a contemporary geometric sans-serif (*Plus Jakarta Sans*) for quantitative technical metrics.
  - Activated advanced OpenType CSS features:
    ```css
    font-variant-numeric: lining-nums tabular-nums;