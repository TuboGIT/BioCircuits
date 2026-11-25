# BioCircuits ⚡

**Human physiology as electronics.** An interactive, offline-first website that explains how neurons, the heart, nerves, and pacemakers work by modelling them as electrical circuits.

Drag a slider, change a component value, and watch the biology respond on a live graph.

> ⚠️ **Educational model only.** BioCircuits uses simplified physics and electronics analogies. It is not a medical device or diagnostic tool and must not be used for diagnosis or treatment.

| Neuron as an RC circuit | Heart as an oscillator |
|---|---|
| ![Neuron module](docs/screenshots/1-neuron.png) | ![Heart module](docs/screenshots/2-heart.png) |
| **Nerve as a transmission line** | **Pacemaker & defibrillator sandbox** |
| ![Nerve module](docs/screenshots/3-axon.png) | ![Pacemaker module](docs/screenshots/4-pacemaker.png) |

---

## Modules

### 01 · Neuron — RC circuit
A Hodgkin–Huxley membrane patch wired as a parallel RC circuit, with a live oscilloscope and an equivalent-circuit diagram that updates as you tune it.

- Sliders: membrane resistance `Rm` (10–200 kΩ), capacitance `Cm` (0.05–1.00 µF), Na⁺ conductance `gNa` (0–16 mS), K⁺ conductance `gK` (0.2–8 mS), stimulus current `Istim` (0.1–2.5 µA)
- **Stimulate** button (6 ms current pulse), **Reset**, and a **continuous stimulus** mode to see tonic firing and the refractory limit
- Live readouts: membrane voltage, time constant τ = Rm·Cm, and action potential phase

### 02 · Heart — oscillator
The SA node as a relaxation oscillator, the AV node as an RC delay stage, and the His–Purkinje system as a driver that produces the P wave, QRS complex, and T wave.

- Sliders: heart rate (35–170 BPM), pacemaker pulse width (60–160 ms), AV conduction delay (100–380 ms)
- Presets: **Normal**, **Tachycardia**, **Bradycardia**, **2:1 Heart Block** (triggered when the AV delay exceeds ~290 ms)
- A scrolling 4-second ECG monitor and a conduction signal-chain diagram

### 03 · Nerve — transmission line
Cable theory in slow motion: a 100 mm axon segment where a myelinated and an unmyelinated fibre race each other.

- Sliders: axon core diameter (1–20 µm) and myelin thickness (0–5 µm)
- Saltatory conduction between nodes of Ranvier vs. continuous conduction, with a spatial voltage profile along the axon
- Live metrics: conduction speeds, speed-up factor, effective membrane capacitance, length constant, and 100 mm travel time

### 04 · Pacemaker — defibrillator sandbox
A 150 µF capacitor discharging into a 50 Ω thoracic load (τ = 7.5 ms), with `E = ½·C·V²` computed live.

- Rhythms: **VFib**, **VTach**, **Sinus Pause**
- Sliders: pulse energy (5–360 J) and pulse timing / escape interval (400–1400 ms)
- **Deliver Pulse** to defibrillate or cardiovert, or enable the **demand auto-pacemaker** to pace a slow rhythm
- Animated capacitor discharge circuit

Every module also includes a plain-language **How it works** section and a side-by-side **Biology ↔ Electronics** mapping (e.g. cell membrane = capacitor, ion channel = variable resistor, AV node = RC delay stage).

---

## Highlights

- **11 languages:** English, Türkçe, Русский, Deutsch, Español, Français, Português, Italiano, 日本語, 中文, العربية (with right-to-left layout). The language is auto-detected from the browser and remembered between visits.
- **Fully offline:** no external requests, no CDN, no framework, no build step.
- **HiDPI-aware canvas rendering** with a single shared 60 fps animation loop.
- **Responsive layout** that works on desktop and mobile.

---

## Tech stack

- HTML5, CSS3, and vanilla JavaScript (no dependencies)
- `<canvas>` 2D for all graphs and circuit diagrams, animated with `requestAnimationFrame`
- Scripts are plain `<script defer>` tags, so the site also works when `index.html` is opened directly from disk

---

## Getting started

```bash
git clone https://github.com/TuBoGIT/biocircuits.git
cd biocircuits
```

Then either open `index.html` directly in your browser, or serve it locally:

```bash
# Python
python -m http.server 8000

# or Node
npx serve .
```

Open `http://localhost:8000` and pick a module from the top navigation bar.

### Deploy on GitHub Pages

Because the site is fully static, you can publish it for free: **Settings → Pages → Deploy from a branch → `main` / root**.

---

## Project structure

```
biocircuits/
├── index.html      # Page layout, the four module sections, and navigation
├── style.css       # Dark theme and responsive layout
├── app.js          # App shell: tab switching, shared animation loop, canvas helpers
├── i18n.js         # Translation engine and dictionaries for 11 languages
├── neuron.js       # Module 1: Neuron (Hodgkin–Huxley / RC circuit)
├── heart.js        # Module 2: Heart oscillator and ECG synthesis
├── axon.js         # Module 3: Myelinated vs. unmyelinated conduction
├── pacemaker.js    # Module 4: Pacemaker and defibrillator sandbox
└── docs/
    └── screenshots/
```

Each module registers itself with the shell via `BioCircuits.registerModule(id, { init, update, draw })`, so adding a new module means adding one script, one `<section>`, and one nav tab.

---

## Model notes and limitations

- **Neuron:** a Hodgkin–Huxley-style membrane patch integrated with a small fixed time step. Playback is slowed down so spikes are easy to watch.
- **Heart and pacemaker:** ECG waveforms are synthesised from scheduled P, QRS, and T events, not computed from a tissue model. Energy thresholds for defibrillation (≥150 J for VFib, ≥100 J for VTach) are simplified teaching values.
- **Nerve:** conduction speeds come from simplified empirical scaling with fibre diameter and myelin thickness, not a full cable-equation solver.

---

## Roadmap

- [x] Four interactive 2D simulation modules with live graphs
- [x] Multilingual interface (11 languages, including right-to-left)
- [ ] 3D models and animations with Three.js (neuron, heart, axon, defibrillator), driven by the same simulation state as the 2D graphs
- [ ] Hover tooltips with biological names and electronics analogies
- [ ] Guided tour mode with step-by-step captions
- [ ] Quiz mode
- [ ] "Build your own circuit" sandbox with the biological equivalent shown live
- [ ] Respect `prefers-reduced-motion` and improve keyboard accessibility

---

## Contributing

Ideas, bug reports, and pull requests are welcome. Please open an issue first to discuss larger changes. Translation fixes are especially appreciated: all strings live in `i18n.js`.

---

## License

MIT. See [LICENSE](LICENSE) for details.
