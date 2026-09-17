+++
title = "Day 03 - 17/09/2026 (On-site)"
weight = 3
+++

## PROGRESS REPORT: R&D & 3D LANDING PAGE DEVELOPMENT (LAZTAR CONSTRUCTION)

### 1. Creative Strategy & System Architecture

* **Graphics Rendering Strategy Evaluation:** Conducted comparative benchmarking between an *Apple-style Pre-rendered Image Sequence (Canvas Stream)* and a *Real-time WebGL Engine* to evaluate runtime performance against visual fidelity.
* **Core Technology Stack:** Architected on Next.js (App Router) combined with Three.js, React Three Fiber (R3F), and GSAP ScrollTrigger, integrated with Lenis Smooth Scroll to normalize viewport momentum and scrolling physics.
* **Scroll-driven 3D Storytelling Workflow (4 Stages):**
  * **Stage 1 (Brand Hook):** Establishes the architectural identity and brand positioning through an expansive, wide-angle panoramic perspective.
  * **Stage 2 (Infrastructure & Landscape):** Transitions the camera into a close-up tracking view highlighting sustainable landscape integration and exterior structural engineering.
  * **Stage 3 (Capability Gallery):** Guides the viewport into a deep spatial exhibition that showcases verified technical capabilities and flagship developments.
  * **Stage 4 (Conversion Trigger):** Re-orientates the camera trajectory to engage the interactive quotation estimator and project inquiry form.

### 2. 3D Asset Pipeline & Prototyping

* **Asset Sourcing & Feasibility Testing:** Evaluated 3D licensing and generation workflows across Sketchfab, Spline, Meshy AI, and open architectural repositories.
* **CAD/BIM Asset Conversion Pipeline:** Established an asset optimization pipeline converting technical `.skp` (SketchUp) models into web-ready `.glb` / `.gltf` formats to reduce polygon density and minimize network delivery overhead.
* **Integrated Test Scenes:**
  * Procedurally generated geometric corporate headquarters (Procedural Mesh).
  * Real-world commercial landmark prototype (Sea Keep Landmark).

### 3. UI/UX Implementation & WebGL Optimization

* **Glassmorphic UI Design System:** Crafted a dark architectural aesthetic anchored by Titanium Black and Champagne Gold accents, featuring persistent global navigation, floating capability cards, and an inquiry status modal.
* **Technical WebGL Problem Resolution:**
  * **Clipping & Backface Culling Elimination:** Resolved surface transparency and mesh clipping defects by standardizing `DoubleSide` material properties and automating spatial normalization via `Bounds` and `Center`.
  * **HUD Floating Architecture:** Replaced opaque interface boundaries with floating glassmorphic HUD overlays, preserving spatial depth across the 3D canvas.
  * **Lighting Balance:** Balanced multi-source illumination using an `Environment HDR Preset` alongside calibrated Directional and Ambient lights to realistically render glass reflections and concrete textures.
  * **Hardware Performance Tuning:** Clamped the Device Pixel Ratio (DPR) and eliminated compute-heavy dynamic shadow passes to maintain a consistent 60 FPS across varied client device profiles.

### 4. Next Steps

* Ingest official architectural CAD/BIM assets from the design team to replace current mock assets.
* Refine camera keyframe interpolation to mirror marketing narratives and conversion milestones.
* Wire the real-time project cost estimation form with backend CRM and customer lead capture APIs.