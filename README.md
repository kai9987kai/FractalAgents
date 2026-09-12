# Hybrid Fractal RL Observatory v3

A real-time, GPU-accelerated **3D fractal observatory** that combines signed-distance-field ray marching, hybrid procedural geometry, HDR post-processing, audio reactivity, adaptive GPU performance management, and contextual machine-learning control — all inside a single dependency-free HTML file.

Hybrid Fractal RL Observatory v3 turns the browser into an interactive generative graphics laboratory. Mandelbulb structures, Julia-inspired fields, gyroid shells, recursive space folding, procedural nebulae, audio analysis, adaptive rendering and autonomous camera/fractal control are combined into one portable WebGL application.

> **v3 is a major architectural upgrade over the original Observatory.**
>
> It replaces the earlier heuristic `SimpleGNN`/pseudo-Q-learning controller with contextual LinUCB bandits, adds WebGL2 and HDR support, introduces GPU-aware adaptive rendering, expands the fractal field system, improves audio analysis, and adds benchmarking, presets, diagnostics, state import/export and considerably stronger fault handling.

---

## Highlights

- Real-time GPU ray-marched 3D fractals
- WebGL 2 renderer with WebGL 1 fallback
- HDR `RGBA16F` rendering where supported
- Four procedural fractal/SDF field configurations
- Mandelbulb distance estimation
- Julia-inspired hybrid fields
- Gyroid and lattice structures
- Recursive spatial folding
- Adaptive sphere tracing
- Contextual LinUCB autonomous agents
- GPU-time-aware dynamic quality
- Dynamic ray-march budgets
- FFT microphone analysis
- Bass, mid and treble extraction
- Spectral-transient detection
- Procedural audio fallback
- HDR bloom
- Multiple tone-mapping operators
- Edge-aware post-process anti-aliasing
- Chromatic lens distortion
- Procedural film grain
- Seven colour palettes
- Five rendering presets
- Interactive orbit camera
- Touch and pinch controls
- Autonomous camera movement
- Performance benchmarking
- Real-time telemetry
- WebGL capability diagnostics
- JSON state import/export
- Persistent local state
- PNG screenshot capture
- Fullscreen mode
- Reduced-motion awareness
- WebGL context-loss recovery
- No libraries
- No frameworks
- No external assets
- No build process
- One HTML file

---

# Demo Architecture

The application uses a multi-stage real-time graphics pipeline:

```text
Audio / Procedural Input
          │
          ▼
FFT + Spectral Analysis
          │
          ├── Bass
          ├── Mid
          ├── Treble
          ├── Overall Energy
          └── Transient / Beat Detection
          │
          ▼
Context Vector
          │
          ▼
LinUCB Contextual Controllers
          │
          ├── Fractal Controller
          └── Camera Controller
          │
          ▼
Fractal / Camera Parameters
          │
          ▼
Hybrid SDF Scene
          │
          ▼
Adaptive Sphere Tracing
          │
          ▼
HDR Scene Buffer
          │
          ├───────────────┐
          ▼               ▼
     Scene Colour    Bright-Pass
                          │
                          ▼
                  Gaussian Bloom
                          │
          ┌───────────────┘
          ▼
Post Processing
          │
          ├── Bloom
          ├── Edge AA
          ├── Lens distortion
          ├── Chromatic aberration
          ├── Film grain
          └── Tone mapping
          │
          ▼
        Display
```

---

# What's New in v3

## WebGL2-First Renderer

v3 attempts to create a **WebGL 2** rendering context first.

If WebGL2 is unavailable, it can fall back to WebGL1 with capability-dependent rendering paths.

The diagnostics panel reports the active renderer.

```text
WebGL 2
```

or:

```text
WebGL 1
```

This provides substantially better portability than assuming a single graphics configuration.

---

## HDR Rendering

On compatible hardware, the main scene is rendered into an:

```text
RGBA16F
```

floating-point framebuffer.

This preserves values greater than `1.0` before tone mapping, allowing bright fractal surfaces to generate physically more useful bloom.

If floating-point colour attachments are unavailable, the renderer automatically falls back to a conventional LDR render target.

The diagnostics panel displays:

```text
HDR target: RGBA16F
```

or the appropriate fallback.

---

# Hybrid Fractal Engine

The Observatory is not limited to one static Mandelbulb.

v3 provides multiple procedural field configurations.

## Field 0 — Mandelbulb

A more traditional Mandelbulb-based distance field.

Useful for:

- clean fractal structures
- lower rendering cost
- performance testing
- calm scenes

---

## Field 1 — Hybrid

Combines several implicit structures including:

- Mandelbulb distance estimation
- Julia-inspired transformations
- recursive folding
- procedural modulation

This is the default balanced field.

---

## Field 2 — Crystal / Gyroid

Introduces stronger periodic and gyroid-like structures.

This creates:

- crystalline cavities
- repeating shells
- intricate tunnels
- folded architectural structures

It is particularly effective with the **Hyperfold** preset.

---

## Field 3 — Void / Lattice

Produces more spatially fragmented fields suitable for abstract tunnels, voids and lattice-like environments.

---

# Improved Ray Marching

v3 contains a more sophisticated marching strategy than the original implementation.

The renderer includes:

- analytic scene-bound intersection
- dynamic hit precision
- distance-sensitive stepping
- conservative relaxation for hybrid fields
- configurable march budgets
- closest-approach recovery
- adaptive quality integration

Instead of blindly executing the maximum number of steps for every pixel, the renderer can adjust its effective workload according to the performance controller.

---

# Surface Rendering

Once a ray intersects the distance field, the shader computes a procedural surface using:

- tetrahedral normal estimation
- diffuse lighting
- specular highlights
- rim illumination
- ambient occlusion
- soft shadows
- environment reflections
- iteration-derived colour
- audio modulation
- agent modulation

The scene remains entirely procedural.

No mesh file or texture asset is required.

---

# Contextual Machine Learning

The original Observatory contained a lightweight custom `SimpleGNN` and heuristic Q-value update system.

v3 replaces this with a more mathematically defined **LinUCB contextual bandit system**.

Two separate adaptive controllers are used:

```text
Fractal Bandit
Camera Bandit
```

The system receives a context containing runtime information such as:

```text
Audio intensity
Bass energy
Mid-frequency energy
Treble energy
Transient activity
Frame/performance pressure
```

The controller then chooses actions appropriate to the current context.

This allows autonomous behaviour without requiring a large neural network, external model, TensorFlow.js, WebGPU inference engine or server.

### Why LinUCB?

For this application, contextual bandits are useful because the problem is largely:

> Given the current visual/audio/performance state, which action appears most useful now?

This is more appropriate than pretending the renderer contains a fully trained deep reinforcement-learning model.

It is:

- lightweight
- interpretable
- fast
- incremental
- browser friendly
- capable of learning online

---

# Autonomous Camera

The Observatory includes an autonomous camera system.

When **Auto** is enabled, the camera slowly explores the generated structure.

Camera motion can react to:

- time
- audio activity
- current visual state
- contextual-bandit actions

Manual interaction temporarily takes priority so the autonomous system does not immediately fight the user.

Disable **Auto** for completely manual navigation.

---

# Audio-Reactive Rendering

Press:

```text
Mic
```

to enable microphone input.

When microphone access is available, the Observatory performs real-time FFT analysis.

The audio signal is divided into approximate spectral regions:

```text
Bass
Mid
Treble
```

The renderer also calculates overall audio energy and spectral change.

This allows much richer reactions than simply measuring microphone volume.

Audio can affect:

- fractal power
- lighting
- colour
- field morphing
- camera movement
- bloom
- agent context
- animation intensity

If microphone access is unavailable or denied, the Observatory continues using a procedural synthetic signal.

### Microphone requirements

Browsers generally require microphone APIs to run from a secure context.

Recommended environments are:

```text
https://
```

or:

```text
localhost
```

Opening the file directly may still render correctly, but microphone access depends on browser security policy.

---

# Spectral Transient Detection

v3 tracks changes between FFT frames to estimate sudden increases in spectral energy.

This creates a lightweight transient detector capable of reacting to:

- percussion
- beats
- sudden sounds
- attacks
- energetic musical changes

It is intentionally lightweight enough to run continuously alongside GPU rendering.

---

# Adaptive Performance System

Real-time fractal ray marching is computationally expensive.

Rather than assuming every GPU can sustain the same workload, v3 dynamically adapts.

The controller can alter:

```text
Render resolution
Ray-march step budget
```

according to the requested frame-rate target.

---

## GPU Timing

Where supported, v3 uses asynchronous GPU timer queries.

This allows the application to estimate how long the GPU actually spends rendering a frame rather than relying exclusively on JavaScript frame timing.

The diagnostic panel reports whether the application is using:

```text
GPU query
```

or:

```text
Frame-time fallback
```

This is particularly important because CPU frame time and GPU rendering cost are not always equivalent.

---

# Target FPS

The user can choose the desired performance target.

Typical values include:

```text
30 FPS
45 FPS
60 FPS
90 FPS
```

The adaptive renderer attempts to balance fidelity against this target.

---

# Dynamic Resolution

The renderer can internally render at a lower resolution than the display canvas and upscale the result during the final composition stage.

For example:

```text
Display: 2560 × 1440
Render scale: 0.70

Internal render:
1792 × 1008
```

This can dramatically reduce ray-marching cost.

---

# Dynamic Step Budget

Resolution is not the only expensive part of an SDF renderer.

The number of distance-field evaluations per ray also matters.

v3 can therefore adjust its effective march-step budget independently.

This provides a second performance dimension:

```text
Pixel count × ray complexity
```

instead of relying exclusively on resolution scaling.

---

# Rendering Presets

Five presets provide useful starting configurations.

## Balanced

Designed as the general-purpose default.

Balances:

- image quality
- frame rate
- fractal complexity
- bloom
- movement

---

## Cinematic HDR

Prioritises visual fidelity.

Uses:

- full quality target
- higher march budget
- stronger HDR presentation
- richer bloom
- increased geometric detail

Best suited to more powerful GPUs or screenshot generation.

---

## Performance

Reduces GPU workload.

Uses:

- lower internal resolution
- smaller step budget
- simpler fractal field
- reduced post-processing

Useful for:

- integrated graphics
- mobile devices
- older laptops

---

## Hyperfold

Pushes the procedural field toward much more complex recursive structures.

Features:

- deep folding
- high detail
- crystal/gyroid field
- stronger bloom
- stronger lens processing
- increased motion

This is one of the most visually aggressive modes.

---

## Calm / Reduced Motion

Reduces motion and visual intensity.

Useful when:

- autonomous motion is distracting
- recording stable footage
- studying fractal geometry
- reduced-motion behaviour is preferred

---

# Tone Mapping

HDR colours need to be converted into displayable output.

v3 includes three tone-mapping operators.

```text
Neutral / Exponential
ACES-like
Reinhard
```

Each produces a different highlight response.

### ACES-like

Produces a cinematic highlight roll-off and stronger contrast.

### Neutral

Uses a cleaner exponential response.

### Reinhard

Provides a classic simple HDR compression curve.

---

# Bloom

Bloom uses a multi-pass pipeline:

```text
HDR Scene
   ↓
Soft Bright-Pass
   ↓
Horizontal Blur
   ↓
Vertical Blur
   ↓
Additional Blur
   ↓
Composite
```

When bloom is disabled, those rendering passes are skipped rather than needlessly executing hidden work.

---

# Post Processing

The final composition stage can include:

- HDR exposure
- bloom
- tone mapping
- gamma conversion
- edge-aware anti-aliasing
- radial lens distortion
- chromatic aberration
- film grain

All effects are executed directly on the GPU.

---

# Palettes

v3 contains seven procedural colour palettes.

Colour is generated mathematically inside the shader rather than sampled from image textures.

Use:

```text
P
```

to cycle through palettes.

The active palette can also be selected from the interface.

---

# Controls

## Mouse

| Input | Action |
|---|---|
| Left drag | Orbit camera |
| Mouse wheel | Zoom |
| Double click | Reset camera |

## Touch

| Input | Action |
|---|---|
| One-finger drag | Orbit |
| Two-finger pinch | Zoom |

## Keyboard

| Key | Action |
|---|---|
| `W` | Move camera forward |
| `S` | Move camera backward |
| `A` | Steer left |
| `D` | Steer right |
| `B` | Toggle bloom |
| `P` | Cycle colour palette |
| `H` | Toggle clean HUD |
| `R` | Reset camera |
| `Space` | Pause / resume |

---

# Interface Controls

The HUD exposes runtime controls for:

- preset
- target FPS
- quality
- ray-march budget
- exposure
- tone mapping
- fractal field
- fractal power
- fold depth
- geometric detail
- motion
- bloom intensity
- bloom threshold
- lens strength
- film grain
- palette

Important systems can also be toggled directly:

```text
Mic
Bloom
LinUCB
Auto
Fold
AA
Pause
Shot
Bench
Full
Export
Import
Reset
Clean
```

---

# Benchmark Mode

Press:

```text
Bench
```

to run a short performance benchmark.

The benchmark samples approximately 180 rendered frames and reports values including:

```text
Average FPS
1% low FPS
Average GPU time
Render scale
Ray-march step budget
```

Example:

```text
Benchmark:
61.4 FPS average
55.2 FPS 1% low
11.82 ms GPU
scale 0.88
184 steps
```

GPU timing is shown only when the browser exposes the required timer-query extension.

---

# Diagnostics

v3 includes a live capability panel.

It reports information such as:

```text
Renderer     WebGL 2
HDR target   RGBA16F
GPU timer    GPU query
WebGPU       Available
Bandit       LinUCB contextual
Storage      Available
```

These diagnostics make performance and compatibility problems considerably easier to investigate.

---

# Telemetry

The application includes live telemetry for observing runtime behaviour.

Metrics include information related to:

- frame performance
- audio activity
- adaptive controller state
- rendering quality
- autonomous behaviour

This makes the Observatory useful as both an artwork and an experimental graphics environment.

---

# State Persistence

Renderer state is periodically stored in:

```text
localStorage
```

This allows many settings and learned controller parameters to survive page reloads.

Storage failure is handled gracefully for privacy modes and restricted browser environments.

---

# Export / Import

Complete Observatory states can be exported as JSON.

Press:

```text
Export
```

to create:

```text
fractal-observatory-v3-state.json
```

The exported state contains compatible renderer settings and adaptive-controller state.

Use:

```text
Import
```

to restore a previously exported configuration.

This allows interesting visual states to be preserved or transferred between browsers.

---

# Screenshots

Press:

```text
Shot
```

to capture the next completed rendered frame as a PNG.

This captures the final composed canvas rather than only the raw fractal render target.

---

# Reduced Motion

The application checks:

```css
prefers-reduced-motion: reduce
```

When requested by the operating system/browser, default motion and autonomous behaviour are reduced.

The **Calm** preset can reduce movement further.

---

# Running the Observatory

No installation or build system is required.

Clone or download the project and open:

```text
hybrid_fractal_observatory_v3.html
```

in a modern browser.

For the best compatibility, run it through a local HTTP server.

Using Python:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/hybrid_fractal_observatory_v3.html
```

This is especially recommended if microphone functionality is required.

---

# Browser Support

## Recommended

- Google Chrome
- Microsoft Edge
- Firefox
- Safari with modern WebGL support

WebGL2-capable browsers provide the preferred rendering path.

WebGL1 is used as a fallback where possible.

---

# Hardware

The renderer is entirely GPU driven and performance therefore varies considerably between devices.

### Recommended

- WebGL2-capable GPU
- hardware-accelerated browser
- recent integrated or discrete GPU
- 4 GB+ system RAM
- modern desktop/mobile processor

A discrete GPU is not required.

The **Performance** preset is recommended for weaker integrated graphics.

---

# No Dependencies

One of the core design goals is portability.

The application does **not** require:

```text
Three.js
Babylon.js
React
Vue
TensorFlow.js
ONNX Runtime
npm
Node.js
Webpack
Vite
external shaders
external textures
external models
CDNs
```

Everything required to render the Observatory is contained inside a single HTML document.

---

# Project Structure

The entire application is intentionally self-contained:

```text
hybrid_fractal_observatory_v3.html
```

Internally it contains:

```text
HTML
 ├── Observatory HUD
 ├── Controls
 ├── Diagnostics
 └── Telemetry

CSS
 ├── Responsive layout
 ├── HUD styling
 └── Reduced-motion behaviour

JavaScript
 ├── WebGL capability detection
 ├── Shader compilation
 ├── Framebuffer management
 ├── HDR pipeline
 ├── Audio analyser
 ├── GPU timer
 ├── Adaptive quality controller
 ├── LinUCB bandits
 ├── Camera controller
 ├── Input handling
 ├── State persistence
 ├── Benchmarking
 └── Render loop

GLSL
 ├── Mandelbulb DE
 ├── Julia field
 ├── Gyroid field
 ├── Space folding
 ├── Hybrid SDF
 ├── Ray marcher
 ├── Surface normals
 ├── Ambient occlusion
 ├── Shadows
 ├── Lighting
 ├── Procedural environment
 ├── Bright pass
 ├── Gaussian blur
 └── Final composite
```

---

# Original vs v3

| Capability | Original | v3 |
|---|---|---|
| WebGL1 | Yes | Yes |
| WebGL2 | — | **Yes** |
| HDR floating-point render target | — | **Yes** |
| Adaptive render resolution | Yes | **Improved** |
| Adaptive march complexity | — | **Yes** |
| GPU timer queries | — | **Yes** |
| Mandelbulb | Yes | Yes |
| Julia hybridisation | Yes | **Expanded** |
| Gyroid geometry | Yes | **Expanded** |
| Multiple field modes | — | **4** |
| Colour palettes | 6 | **7** |
| Overall audio level | Yes | Yes |
| FFT frequency bands | — | **Yes** |
| Transient detection | — | **Yes** |
| Heuristic `SimpleGNN` | Yes | Replaced |
| Contextual LinUCB | — | **Yes** |
| Autonomous camera | Yes | **Improved** |
| Presets | — | **5** |
| Tone-map selection | — | **3 modes** |
| Benchmark | — | **Yes** |
| GPU diagnostics | — | **Yes** |
| State export/import | — | **Yes** |
| Fullscreen | — | **Yes** |
| Touch pinch zoom | — | **Yes** |
| Reduced-motion support | — | **Yes** |
| Context-loss recovery | Limited | **Improved** |
| WebXR session request | Yes | Not currently included |

The original WebXR button only requested an immersive session and did not implement a complete stereoscopic XR render path. A future version should restore VR as a proper WebXR rendering backend rather than simply restoring the old session button.

---

# Why It Is Still Called “RL Observatory”

The project originated with a reinforcement-learning-inspired autonomous control system.

v3 retains that identity but uses a **contextual-bandit learning model** rather than pretending that the browser contains a large deep-RL system.

The distinction is intentional.

LinUCB learns which actions perform well under different runtime contexts while remaining lightweight enough to execute continuously inside the browser.

This makes the adaptive component more technically meaningful while retaining the experimental spirit of the original project.

---

# Current Limitations

The renderer is deliberately ambitious, so several limitations remain.

### GPU dependent

Ray-marched fractals can be extremely expensive.

Complex fields, high resolution and large ray budgets can push integrated GPUs hard.

### Browser shader variation

Different GPU drivers and browser implementations can produce slightly different precision and performance characteristics.

### LinUCB is not deep RL

The contextual agents learn online action preferences.

They are not neural policy networks and do not perform long-horizon deep reinforcement learning.

### No temporal reconstruction

Each frame is currently rendered largely independently.

There is no temporal accumulation or reprojection system yet.

### WebGPU is diagnostic only

The interface can report WebGPU availability, but v3 does not yet use WebGPU for rendering.

### No complete WebXR renderer

The original experimental WebXR session entry has not been carried forward because a correct VR implementation requires per-eye rendering, XR reference spaces and XR-specific camera transforms.

---

# Future Research / v4

The v3 architecture creates a strong foundation for a considerably more advanced renderer.

Potential v4 work includes:

## WebGPU / WGSL Renderer

Create a second rendering backend using:

```text
WebGPU
WGSL
Compute shaders
Storage buffers
Timestamp queries
```

while retaining WebGL as a compatibility path.

---

## Temporal Accumulation

Combine information from previous frames to improve:

- anti-aliasing
- subpixel detail
- noise stability
- reflections
- volumetric effects

---

## Temporal Reprojection

Reproject previous samples using camera movement to avoid recalculating every pixel from scratch.

---

## Blue-Noise Sampling

Use spatially distributed blue-noise jitter for:

- ray origin variation
- soft shadows
- ambient occlusion
- temporal supersampling
- volumetric integration

---

## Adaptive Ray Budgets

Estimate image complexity per region and concentrate ray-marching work around:

- silhouettes
- high-curvature regions
- fine fractal detail
- high-contrast areas

while reducing work in empty space.

---

## Hierarchical Distance Fields

Introduce coarse spatial representations capable of accelerating empty-space traversal before evaluating expensive high-detail fractal distance estimators.

---

## Compute-Based Exposure

Use GPU compute reductions to calculate:

```text
Luminance histogram
Average luminance
Exposure adaptation
Highlight statistics
```

for automatic cinematic exposure.

---

## Temporal Bloom

Accumulate and stabilise highlights over multiple frames for more sophisticated emissive behaviour.

---

## Proper WebXR

Implement full immersive rendering with:

- `XRWebGLLayer`
- XR reference spaces
- headset orientation
- per-eye matrices
- per-eye viewports
- controller input
- stereoscopic fractal rendering

---

## Advanced Adaptive Agents

Future versions could experiment with:

- neural contextual bandits
- actor-critic methods
- lightweight policy networks
- ONNX Runtime Web
- WebNN
- WebGPU inference
- offline-trained aesthetic models

Any future agent should be judged against a simple question:

> Does it make a measurable improvement over a simpler controller?

Complexity should only be added where it produces useful behaviour.

---

# Design Philosophy

Hybrid Fractal RL Observatory is intended to sit somewhere between:

```text
Generative artwork
Real-time graphics experiment
Fractal explorer
Shader laboratory
Audio visualizer
Adaptive-system experiment
Machine-learning playground
```

The project deliberately avoids hiding the interesting parts behind a large rendering engine.

The distance estimators, post-processing pipeline, performance controller and adaptive agents remain directly inspectable in the source.

That makes the project useful not only for producing visuals, but also for experimenting with the underlying ideas.

---

# Performance Tips

For maximum visual quality:

```text
Preset: Cinematic HDR
Quality: 1.00
High march budget
WebGL2
HDR enabled
Bloom enabled
```

For everyday use:

```text
Preset: Balanced
Target: 60 FPS
Adaptive quality: enabled
```

For integrated graphics:

```text
Preset: Performance
Target: 60 FPS
Lower quality scale
Reduced march budget
```

For screenshots:

```text
Preset: Cinematic HDR
Pause or reduce motion
Adjust camera
Select palette
Tune exposure
Capture Shot
```

For extreme procedural geometry:

```text
Preset: Hyperfold
Field: Crystal/Gyroid
Increase Fold
Increase Detail
```

---

# Technical Goals

The project prioritises:

1. **Real-time performance**
2. **Mathematically generated imagery**
3. **No external runtime dependencies**
4. **Graceful hardware degradation**
5. **Observable adaptive behaviour**
6. **Interactive experimentation**
7. **GPU-oriented architecture**
8. **Portable single-file deployment**
9. **Technically defensible machine-learning terminology**
10. **A clear path toward WebGPU**

---

# Version

Current major version:

```text
Hybrid Fractal RL Observatory v3
```

Renderer architecture:

```text
WebGL2-first / WebGL1 fallback
Hybrid SDF ray marcher
HDR post-processing pipeline
Contextual LinUCB control
Adaptive GPU workload management
FFT audio-reactive system
```

---

## Final Note

Hybrid Fractal RL Observatory v3 is more than a visual update to the original.

It restructures the project around a stronger rendering and adaptive-control architecture while retaining the original idea:

> **A procedural universe that observes its inputs, adapts its behaviour and continuously generates new visual structures in real time.**

The entire system still runs from one HTML file.