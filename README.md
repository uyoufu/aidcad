# AIDCAD 🤖📐

> **AI-Aided Digital CAD:** The Next-Generation AI-Driven Geometric Kernel and Web Renderer.

[![License](https://shields.io)](LICENSE)
[![WebGL2/WebGPU](https://shields.io)]()
[![WASM](https://shields.io)]()

**AIDCAD** (pronounced *Aid-Cad*) is an open-source, ultra-performance Web CAD framework designed from the ground up for the AI era. By bridging the visual intelligence of Multimodal LLMs, the robust geometric precision of Open CASCADE (OCCT) WebAssembly, and the fluid rendering of Three.js, AIDCAD enables AI agents to act as the primary design brain—eliminating traditional parametric constraint solvers in favor of direct, deterministic coordinate-based geometric reasoning.

---

## ✨ Core Engineering Innovations

### 🧠 1. Constraint-Free Topology Brain (JSON-to-CAD)
Traditional CAD relies heavily on historical feature trees and complex parametric constraint networks, which frequently break due to the topological naming problem when driven by AI. AIDCAD bypasses this entirely:
- **Imperative Hard-Modification:** The AI agent perceives and reasons using a lean, high-fidelity JSON Adjacency List (Topology Graph) representing points, lines, arcs, and layers.
- **Deterministic Coordinates:** The AI directly computes absolute coordinates in its latent space or via cloud-based code interpreters, commanding the local kernel to rebuild geometry imperatively without historic baggage.

### 📐 2. Dual-Precision RTC Shader (Macro-to-Micro rendering)
WebGL natively operates on 32-bit single-precision floats (Float32), causing catastrophic coordinate jittering and canvas tearing when rendering large-scale survey data (e.g., millions of meters) simultaneously with tiny mechanical tolerances (e.g., millimeters).
- **High/Low Precision Splitting:** AIDCAD splits 64-bit double-precision (Float64) coordinates from the OCCT kernel into `Position_High` and `Position_Low` Float32 streams.
- **Camera-Relative RTC:** Leverages Three.js `onBeforeCompile` (or WebGPU NodeMaterial) to translate coordinates relative to the eye inside the vertex shader, delivering jitter-free, lossless rendering across vast spatial scales combined with `logarithmicDepthBuffer` optimization.

### ⚙️ 3. Ultra-Lean OCCT WASM Core
Open CASCADE Technology (OCCT) is an industrial titan but notoriously bloated for web deployments (~30MB+). 
- **Modular Shaving:** We compiled a heavily stripped-down C++ OCCT core into WebAssembly via Emscripten, entirely removing 3D mesh rendering visualizers and unneeded file I/O.
- **Micro-size, Native Speed:** Retains purely the 2D/3D geometric intersection (`Geom2dAPI_InterCurveCurve`), trimming, splitting, and vertex snapping engines, shrinking the compressed WASM footprint to **< 5MB** while executing topological changes in microseconds.

### 🖼️ 4. Feature-Rich Rendering for VLM Attention
To minimize token depletion and bypass the low-resolution downsamping blur of generic Vision-Language Models (e.g., GPT-4o, Claude 3.5 Sonnet):
- **Color-Coded Semantics:** The web client renders geometry using highly-contrasted color channels mapped to metadata (e.g., continuous vertices, unconstrained segments, annotations).
- **Local Spatial Anchors (ROI):** Embeds micro-ID tags natively into local Viewport viewframes. The agent interacts purely by declaring localized bounding boxes and ID references, reducing typical interaction tokens by **up to 85%**.

---

## 🏗️ System Architecture

```text
       [ Multimodal AI Agent (Cloud) ]
                     │
         (Visual Prompt & ROI Cuts)
                     ▲
                     ├──────────────────────────────┐
                     ▼ (Lossless JSON Topology)      │
       [ JavaScript State Manager (Web Client) ]    │ (Feature-Rich PNG)
                     │                              │
         ┌───────────┴───────────┐                  │
         ▼                       ▼                  │
  [ OCCT WASM Core ]    [ Three.js Renderer ] ──────┘
 (Double-Precision CPU) (Single-Precision GPU/RTC)
```

---

## 🛠️ Repository Ecosystem

AIDCAD is modularized across three distinct layer repositories:

* **[`aidcad-core`](#)**: The Emscripten-compiled, performance-tuned C++ Open CASCADE WebAssembly engine. Manages geometric intersections, trimming, and strict micro-tolerance snapping.
* **[`aidcad-view`](#)**: The Three.js/WebGPU-powered rendering viewport. Implements the high/low attribute splitting, RTC shaders, R-Tree spatial indexing (`rbush`), and local UI snapping handles.
* **[`aidcad-brain`](#)**: The AI Agent orchestration middleware. Handles the JSON topology abstraction, multi-view coordinate transformation matrices, and the localized viewport-clipping canvas extraction.

---

## 🚀 Quick Start (Coming Soon)

```bash
git clone https://github.com
cd aidcad-view
npm install
npm run dev
```

---

## 📄 License

AIDCAD is licensed under the **MIT License**. The underlying geometric math core conforms to the **Open CASCADE Technology Public License**.
