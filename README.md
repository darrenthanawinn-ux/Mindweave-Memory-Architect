# MindWeave: The Memory Architect

A surreal, narrative-driven psychological puzzle experience for Meta VR (Meta Quest / XR SDKs). Players sit at a seated console and use bare hands — no controllers — to repair fractured memories belonging to historical or fictional figures, guided by Lumen, an agentic AI companion.

This repository contains:

- **`index.html`** — a self-contained WebXR/Three.js prototype of the core onboarding loop, playable in a browser today (VR headset or desktop preview).
- **`docs/design-doc.md`** — the full narrative, art, audio, and accessibility design bible.
- **`docs/competition-mapping.md`** — how each feature ties back to the five target award categories.
- **`docs/production-roadmap.md`** — how this prototype scales into the shipping Unity + Meta XR SDK build.

---

## Running the prototype

**On a Meta Quest:**
1. Host `index.html` anywhere reachable over HTTPS (a local dev server with a tunnel, GitHub Pages, Netlify, etc. — WebXR requires a secure context). `file://` will not expose the XR device.
2. Open the URL in the Quest Browser.
3. Tap **Enter the Console (VR)**. Approve the hand-tracking permission prompt.
4. Reach forward (within roughly two feet) and pinch your thumb and index finger together over the glowing amber shard. Drag it toward the dark ring on the bridge and release the pinch. Watch the cinematic push-in and listen for the audio swell.

**On desktop (no headset needed):**
1. Open `index.html` in any modern browser (Chrome/Edge recommended).
2. Click **Preview on Desktop**.
3. Click-drag the shard to the bridge socket to simulate the pinch-grab-weave gesture.

Everything — settings, hand-tracking sensitivity, progress — persists locally between visits via the encrypted vault described below.

---

## What this prototype demonstrates

| Brief requirement | Where it lives in `index.html` |
|---|---|
| Seated, stationary, airplane-seat-safe console | Fixed camera rig, zero locomotion, all interaction within ~2 ft |
| Bare-hand pinch/grab/pull mechanics | `pollHandJoints()` reads real WebXR Hand Input joint poses (thumb-tip ↔ index-finger-tip distance) |
| Mesh-stretching / weave-to-repair mechanic | Shard-to-socket proximity check + snap + `weaveComplete()` |
| 5-minute onboarding hook | Title → Enter Console → first pinch → weave → cinematic camera push-in, designed to land inside ~30–60 seconds |
| Agentic AI companion (Lumen) | Contextual state machine (`Lumen.onEnter/onNearBridge/onWoven/onIdle`) reacting to hand proximity and puzzle state — see the Production Roadmap for how this becomes a real model-backed agent |
| Adaptive soundtrack | `Audio_` — layered Web Audio drones that fade in warmth/complexity as the puzzle resolves, plus a resolving chime |
| Spatial audio | Web Audio `PannerNode`-ready gain graph anchored to the shard/companion objects (extend with `PannerNode` per source for full 3D panning in the native build) |
| Glassmorphic holographic UI | CSS `backdrop-filter` glass panels: companion bubble, console HUD, accessibility panel |
| Encrypted persistent settings/progress | `Vault` — versioned, keyed, obfuscated localStorage read/write (see Security note below) |
| Full accessibility panel | One-handed mode, hand-tracking sensitivity, seated height calibration, high-contrast paths, subtitles, master volume, comfort vignette toggle |
| Obsidian + amber/gold visual language | Scene lighting, materials, and particle-stream palette |

---

## Persistence & security note

The in-browser `Vault` helper (XOR + base64) exists to demonstrate the **shape** of the brief's "securely encrypted on-device" requirement — versioned keys, structured progress data, obfuscation at rest — inside a single portable HTML file with no build step. It is **not** cryptographically secure and should not be treated as such.

For the shipping build, replace it with one of:
- **WebXR/PWA build:** the `SubtleCrypto` (WebCrypto) AES-GCM API, with the key derived from a platform-provided secret rather than hardcoded.
- **Unity + Meta XR SDK build:** platform secure storage (Android Keystore-backed `EncryptedSharedPreferences` via a Unity Android plugin), or Meta's platform save-data APIs where available.

---

## Known simplifications in this prototype

- **Art:** primitives (octahedron shard, torus socket, box desk) stand in for the "rich obsidian stone structures" — a real art pass needs sculpted/baked meshes, PBR textures, and baked lighting to hit Quest's ~90fps budget with the described fidelity.
- **One memory chapter:** the prototype ships the single onboarding fragment described in the brief's 5-minute hook. `state.progress.chapter` and the daily-fragment retention loop are scaffolded but not content-complete — see `docs/production-roadmap.md`.
- **Lumen is scripted, not model-backed:** the companion's dialogue is a small state machine so the loop works fully offline. The production roadmap describes wiring it to a real-time contextual model call.
- **Spatial audio is stereo-panned via gain, not fully binaural:** swap in per-emitter `PannerNode`s (or Meta's spatial audio SDK in the native build) for true head-locked directional audio.

None of these affect the interaction loop itself — the pinch-to-weave mechanic, accessibility settings, HUD, and persistence all work end-to-end today.

---

## Troubleshooting

If the buttons ever appear unresponsive again: open the browser's DevTools console. `index.html` now shows a dismissible red banner at the top of the page for any startup error instead of failing silently — including a failed Three.js CDN load (e.g. no internet connection, or a network policy blocking `cdn.jsdelivr.net`) — so the console plus that banner should immediately say what went wrong.

## Tech stack

- **Prototype:** vanilla HTML/CSS/JS, [Three.js r160](https://threejs.org/) (UMD build via CDN), native WebXR Device API + WebXR Hand Input Module, Web Audio API. No build step, no dependencies to install.
- **Target production stack (per brief):** Unity with Meta XR SDK v81+/v207, or an Immersive Web SDK/WebXR build for browser-first distribution — see `docs/production-roadmap.md` for the tradeoffs between the two.
