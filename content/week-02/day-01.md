+++
title = "Day 01 - 21/09/2026 (Remote)"
weight = 1
+++

## PROGRESS REPORT: 60 FPS PERFORMANCE TUNING & MODERN SKEUOMORPHIC UI ARCHITECTURE

---

### 1. Runtime Graphics Performance Tuning (Locked 60 FPS)

#### Main Thread Offloading
- **Bottleneck Identified:** The mouse listener computing orbital camera rotation matrices (Mouse Parallax) inside the `useFrame` hook conflicted with GSAP ScrollTrigger interpolation, causing frame pacing drops and cursor stutter during rapid pointer movements[cite: 5].
- **Resolution:** Stripped matrix rotational computations entirely from the render loop, anchoring camera trajectories exclusively to smooth scroll interpolation and freeing compute cycles across the CPU and GPU[cite: 5].

#### WebGL Renderer Parameter Optimization
- **Pixel Density Throttling (DPR):** Clamped the canvas resolution to `dpr={[1, 1.25]}`[cite: 5]. This prevents browsers on high-DPI Retina/4K displays from attempting unthrottled multi-million pixel renders, preventing VRAM overflow and thermal throttling[cite: 5].
- **Deactivating Real-Time Shadows:** Disabled the dynamic shadow mapping pass (`shadows={false}`), eliminating expensive shadow-map matrix computations unnecessary for a stylized landing page[cite: 5].
- **Forced High-Performance Graphics Context:** Initialized the canvas with performance-optimized flags: `powerPreference: "high-performance"`, `depth: true`, `antialias: false`, securing a stable 60 FPS baseline across target device tiers[cite: 5].

---

### 2. Design System Establishment & Tactile Skeuomorphic UI

#### Brand Color Tokens & Procedural Shader Layers
- **Core Visual Palette:** Absolute Black (`#000000`), Metallic Gold (`#D4AF37`), and Molten Bronze (`#C88A35`)[cite: 5].
- **Artistic Shading Elements:** Retained the procedural molten metal background shader (*Molten Bronze Shader*) along with ambient ember floating particles (*Forge Sparks*) to maintain an industrial architectural presence[cite: 5].

#### Glassmorphic Card Components & Tactile Buttons
- **Floating Glass Surfaces:** Encapsulated typography, descriptions, and feature badges within beveled glass panels (`rounded-2xl`, `bg-[#161412]/85`, `backdrop-blur-xl`, multi-layered drop shadows), resolving legibility issues against the animated 3D background[cite: 5].
- **Tactile CTAs:** Engineered pill-shaped tactile buttons (`rounded-full`) finished with directional metallic highlights to simulate physical mechanical buttons[cite: 5].

#### Typography Standardization & Vietnamese Diacritic Fixes
- **Glyph Bug Resolution:** Replaced faulty font configurations that broke Vietnamese compound diacritics (`ế`, `ứ`, `ộ`, `ẫ`)[cite: 5].
- **Next.js Font Optimization Integration:**
  - *Cormorant Garamond / Playfair Display:* Configured for display headers to evoke luxury editorial architecture publications[cite: 5].
  - *Plus Jakarta Sans:* Applied across all body paragraphs and descriptions, optimizing legibility on digital screens[cite: 5].

#### Micro-Badges & Lead Form Validation
- Replaced oversized tags (`[ 01 / SHOWCASE KIẾN TRÚC ]`) with compact pill micro-badges featuring refined amber indicator dots[cite: 5].
- Enforced input validation across phone and email fields, providing real-time visual feedback states prior to submission confirmation[cite: 5].

---

### 3. WebGL Interaction: OGL Glow Cursor Trail

#### Luminescent Pointer Integration
- Stripped away default CSS/SVG circle cursor elements in favor of a customized WebGL component[cite: 5].
- Integrated an ultra-lightweight glow cursor driven by the standalone OGL WebGL engine, running independently without degrading the Three.js canvas frame rate[cite: 5].
- Mapped luminescence along primary brand spectrums: Amber Glow (`#C88A35`) blended into Soft Warm Cream (`#FFF6ED`)[cite: 5].

#### Fragment Shader Falloff Restructuring
- Restructured optical decay functions (`taper/life`) within the custom fragment shader, concentrating peak intensity at the pointer tip (*hotspot*) while leaving a trailing organic luminescent wake that dissipates smoothly with mouse velocity[cite: 5].

---

### 4. Current Milestone Status & Upcoming Execution

#### Accomplishments Delivered
- Completed all structural layout interfaces, 4-stage scroll-driven 3D camera transitions, WebGL pointer glow trails, Glassmorphic HUDs, and the interior 3-project showcase[cite: 5].
- The application executes smoothly at a locked 60 FPS, with full Vietnamese diacritic support and strict alignment to the Laztar Construction design language[cite: 5].

#### Next Steps
- Implement backend API endpoints to route inquiry and estimation form submissions to CRM / Database repositories[cite: 5].
- Replace mock image assets with production-grade architectural renderings from the design division[cite: 5].