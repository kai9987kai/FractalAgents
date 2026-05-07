# Hybrid Fractal RL Observatory

A single-file WebGL fractal visualizer with audio-reactive shading, dual lightweight RL-style agents, multi-pass bloom, environment reflection, volumetric lighting, adaptive quality, and cinematic post-processing.

The app lives in `index.html` and does not require a build step.

## Run

Start a local server from this folder:

```powershell
python -m http.server 8765 --bind 127.0.0.1
```

Open:

```text
http://127.0.0.1:8765/index.html
```

Using localhost is recommended because microphone input and WebXR APIs are restricted by browser security rules.

## Features

- Hybrid Mandelbulb / Julia-like distance-field fractal.
- Deep fold field mode for more aggressive geometric mutation.
- Dynamic procedural starfield and nebula background.
- Multi-pass bright-pass bloom with threshold and intensity controls.
- Volumetric lighting, soft shadows, ambient occlusion, and environment reflection.
- Audio reactivity with microphone input or procedural fallback.
- Dual lightweight RL-style agents for fractal and camera behavior.
- Replay buffers and persisted agent state via `localStorage`.
- Adaptive render scale for smoother performance.
- Lens/chromatic post-processing.
- Screenshot capture.
- WebXR VR session attempt where supported.
- Responsive HUD with compact mode on small screens.

## Controls

### HUD

- `Mic`: request microphone input.
- `Bloom`: toggle bloom compositing.
- `Agent`: toggle RL-style agent updates.
- `Auto`: toggle cinematic autopilot camera motion.
- `Fold`: toggle deep fold shader mode.
- `Shot`: capture the next rendered frame as PNG.
- `VR`: request an immersive VR session if the browser/device supports it.

### Sliders

- `Exposure`: final scene brightness.
- `Bloom`: bloom intensity.
- `Cutoff`: bloom bright-pass threshold.
- `Quality`: render scale target.
- `Fold`: deep fold strength.
- `Lens`: chromatic/lens post-processing strength.

### Keyboard

- `W` / `S`: zoom forward/back.
- `A` / `D`: rotate camera left/right.
- `B`: toggle bloom.
- `F`: toggle agents.
- `C`: toggle autopilot.
- `X`: toggle deep fold.
- `P`: cycle palette.
- `H`: hide/show HUD.
- `R`: reset agent state.

### Pointer

- Drag on the canvas to orbit the camera.
- Mouse wheel zooms in/out.

## Notes

- If the scene becomes too slow, lower `Quality` or disable `Fold`.
- If microphone permission is denied, the visualizer continues with procedural audio movement.
- Agent state is stored under `fractalAgent.v2` and `cameraAgent.v2` in browser `localStorage`.
- This is an experimental visual system, not a production RL implementation. The agents are deliberately lightweight and run entirely in the browser.
