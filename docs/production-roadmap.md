# Production Roadmap: Prototype → Shipping Build

`index.html` proves the core loop end-to-end in a browser. This document maps each piece onto the two viable production paths named in the brief — **Unity + Meta XR SDK** (native, best perf/fidelity ceiling) or **WebXR/Immersive Web SDK** (browser-first, faster iteration, easier distribution) — and calls out what's genuinely new work versus a direct port.

## Choosing the stack

| | Unity + Meta XR SDK | WebXR / Immersive Web SDK |
|---|---|---|
| Perf ceiling for the "obsidian + baked lighting" art bar | Higher — full control over shader pipeline, baked GI, occlusion culling | Lower — constrained by browser WebGL/WebGPU support on Quest |
| Distribution | Meta Horizon Store, app review process | Instant link, no store review, easiest for judges to try pre-event |
| Hand tracking fidelity/APIs | Meta XR SDK's native hand-tracking + gesture recognition, more robust | WebXR Hand Input Module — this prototype's approach — solid but fewer built-in gesture helpers |
| Iteration speed | Slower build/deploy cycle | This prototype *is* the iteration environment already |

Recommendation for a hackathon timeline: keep iterating in WebXR (as this prototype does) through most of development for speed, and only port to Unity late if the perf/fidelity ceiling becomes the limiting factor for the "Best New Entertainment Experience" bar. Most judged demos run fine at WebXR fidelity if the art direction (see below) is executed well.

## Art pipeline (biggest gap from prototype to submission-ready)

The prototype uses primitive geometry as placeholders. Priority order for the art pass:
1. **The memory shard + bridge socket** — these are on-screen for the entire cold-start, so they get first attention: sculpted/baked mesh, proper PBR material, a subtler idle animation than the current placeholder spin.
2. **The console desk + immediate obsidian surfaces** — anything within the 2-ft reach zone is seen up close and scrutinized; this is where "rich obsidian stone" needs to actually read as stone, not a flat-shaded box.
3. **Background/environment dressing** — lowest priority for a hackathon timeline; can stay abstract/foggy since the fog + particle system already does a lot of atmospheric work cheaply.

Target a 90fps frame budget on Quest hardware: bake lighting where the scene is static, keep the shard/socket as the only real-time-lit dynamic objects, and profile particle count (the prototype's 220 particles are cheap; a full build has headroom for more if needed).

## Lumen: from scripted stub to real agentic companion

The prototype's `Lumen` object is a small state machine keyed on `onEnter/onNearBridge/onWoven/onIdle`. To make this a genuinely agentic companion for the "Best Agentic Interaction" category:

1. **Context assembly:** on each significant event (hand proximity change, dwell time crossing a threshold, puzzle state change), assemble a compact context object — current fragment, player's recent gesture history, time since last line — instead of a hardcoded string.
2. **Model call:** send that context to a real-time completion call (this project already has a documented pattern for calling the Claude API from a web/WebXR context available in this environment — reuse that request shape, with a short system prompt establishing Lumen's voice from `docs/design-doc.md` §8) to generate the next line contextually, rather than picking from four fixed strings.
3. **Latency budget:** cache/pre-fetch likely next lines where possible (e.g., start the "near bridge" call as soon as the player's hand crosses a proximity threshold, not after they've already released) so dialogue never visibly lags the action that triggered it.
4. **Guardrails:** keep a small set of scripted fallback lines (the current stub) for offline/network-failure cases, so the companion never goes silent.
5. **Environmental reactivity:** extend beyond dialogue — have the same context object drive lighting warmth and particle density, which the prototype already does mechanically (`Audio_.setWarmth`) but currently keys off puzzle state rather than the richer context object.

## Audio: from stereo gain to true spatial

Swap each `Audio_` layer's plain `GainNode` routing for a `PannerNode` (HRTF panning model) anchored to its source object's world position, updated per-frame from the Three.js scene graph (or the native audio spatializer in a Unity build, where Meta's XR Audio SDK handles this natively). The adaptive-warmth logic (`setWarmth`) and chime trigger carry over unchanged — only the spatial routing needs replacing.

## Persistence: from XOR stub to real secure storage

Replace `Vault`'s XOR/base64 obfuscation with:
- **WebXR path:** WebCrypto `SubtleCrypto.encrypt`/`decrypt` (AES-GCM), key derived from a per-install secret rather than a hardcoded string.
- **Unity path:** platform secure storage (`EncryptedSharedPreferences` via an Android plugin, or Meta's platform save-data APIs).

The data shape (`defaultSettings` object: accessibility flags + `progress`) can port over unchanged — only the encryption layer changes.

## Content pipeline: from one fragment to a full chapter structure

The prototype hardcodes a single fragment and a `state.progress.chapter` field that isn't yet content-backed. Before submission, prioritize:
1. A lightweight fragment-definition schema (shard start position, socket position, success dialogue lines, chapter id) so new memories can be authored as data rather than new code.
2. Enough authored fragments to demonstrate the daily-return loop convincingly in a submission video (a working "tomorrow's fragment" teaser is more convincing than a fully built week of content).
3. The chapter-completion vignette described in the design doc, even as a simple text/visual reveal, since it's the payoff the "Best Reason to Come Back" pitch leans on.

## Suggested build order for a hackathon timeline

1. Lock the fragment-definition schema + author 3–4 real fragments (unblocks everyone else).
2. Art pass on shard/socket/desk (highest visual-scrutiny items).
3. Wire Lumen to a real model call with scripted fallback.
4. Spatial audio pass.
5. Real encrypted storage swap-in (low risk, do last — same data shape).
6. Record the submission video against the first-five-minutes and accessibility beats specifically, since those two categories are most likely to be judged directly from footage.
