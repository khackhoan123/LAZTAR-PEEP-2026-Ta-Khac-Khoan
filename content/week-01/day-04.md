+++
title = "Day 04 - 18/09/2026 (Remote)"
weight = 4
+++

## TECHNICAL REPORT: 3D ASSET PIPELINE REALIZATION & RESOLUTION OF WEBGL RENDERING DEFECTS

---

### 1. Asset Pipeline Streamlining & 3D Model Optimization

#### Empirical Benchmarking & Asset Source Consolidation
- Following preliminary evaluations from Day 03, importing auxiliary models (`sea_keep_lonely_watcher.glb`) alongside runtime model-switching toggles was found to fragment the architectural narrative and double initial page-load latency[cite: 7].
- Purged all redundant model assets, establishing a single standardized production model file located at: `/public/models/building.glb`[cite: 7].

#### DRACO Loader Decompression Configuration
- Resolved runtime failures decoding compressed GLB files by integrating the `DRACOLoader` module from the `@react-three/drei` library[cite: 7].
- Offloaded geometric parsing tasks to background Web Workers, enabling the decompression of heavy polygon meshes without freezing the main browser thread[cite: 7].

#### Geometric Normalization & Scale Alignment Algorithm (Box3 Normalization)
- **Problem:** Raw exports from 3D design software exhibited severe pivot-offset discrepancies, causing the camera to orbit an arbitrary origin point and pushing geometry outside the viewport frustum[cite: 7].
- **Engineering Solution:** Applied Three.js `Box3` bounding volume algorithms:
  - Scanned the entire vertex buffer to compute the true geometric center of the architectural structure[cite: 7].
  - Translated the mesh to align its bounding center strictly with the global origin `(0, 0, 0)`[cite: 7].
  - Computed the bounding sphere radius to automatically calculate scaling ratios proportional to the camera field of view (FOV), guaranteeing proper framing across all screen dimensions[cite: 7].

---

### 2. Spatial Restructuring & Showroom Interior Visual Logic

#### Resolving Spatial Layout Discrepancies
- **Defect Identified:** The prototype layout rendered the exhibition partition walls and display frames suspended in open air behind the structure, breaking real-world architectural coherence[cite: 7].
- **Spatial Redesign:** Extracted and relocated the full exhibition structure along with the 3 flagship project frames (`/public/images/projects`) entirely **INSIDE** the building's glass gallery room, ensuring seamless continuity during Stage 3's glass pass-through camera motion[cite: 7].

---

### 3. In-Depth WebGL Debugging & Defect Elimination

#### Elimination of Coplanar Z-Fighting Artifacts
- **Technical Cause:** The showroom ceiling plane and the structural support beams were positioned on parallel planes with near-zero Z-axis separation[cite: 7]. At glancing camera angles and distant ranges, finite Z-buffer resolution prevented the GPU from resolving surface depth, causing severe flickering[cite: 7].
- **Three-Tier Resolution Strategy:**
  1. Enabled `logarithmicDepthBuffer: true` on the WebGL Canvas to distribute depth precision logarithmically across viewing distances[cite: 7].
  2. Calibrated camera clipping planes (`near = 0.1`, `far = 1000`) to maximize effective Z-buffer bit depth[cite: 7].
  3. Assigned `polygonOffset: true` with a factor of `polygonOffsetFactor: -1` to the showroom ceiling material, enforcing GPU rendering priority over underlying structural elements[cite: 7].

#### Eradicating Backface Culling & Surface Clipping
- Resolved mesh disappearance issues during camera orbital changes by enforcing `side: THREE.DoubleSide` across exterior wall and glass shader materials[cite: 6].
- Implemented `Bounds` and `Center` helpers to wrap bounding boundaries, preventing the camera from clipping inside exterior meshes[cite: 6].

#### Depth Preservation & Multi-Source Lighting Balance
- Removed opaque black bounding walls in favor of transparent Floating HUD Cards to preserve 3D depth[cite: 6].
- Balanced multi-source environmental lighting by pairing an `Environment HDR Preset` with calibrated Directional and Ambient lights, ensuring glass reflections and concrete roughness render authentically from any viewing angle[cite: 6].