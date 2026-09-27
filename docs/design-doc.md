# MindWeave: The Memory Architect — Design Document

## 1. Premise

The player is a Memory Architect: someone who can enter the fractured, dreamlike recollections of historical and fictional figures and physically repair them with their bare hands. Each memory is rendered as a small, contained diorama — a bridge, a room, a shoreline — built from obsidian stone shot through with glowing amber-gold pathways where the memory has come loose. Fixing a memory means finding its broken seams and weaving them back together, gesture by gesture.

The tone sits between cozy and dark-fantasy/sci-fi: melancholy but never bleak, ritualistic rather than horror-adjacent. Repairing a memory should feel like a small, private act of care.

## 2. Player fantasy & core loop

**Fantasy:** "I am gently, physically mending someone's past with my own two hands."

**Core loop (per memory fragment):**
1. Sit at the console. A memory sphere boots up, projecting its diorama into the 2-ft reach zone.
2. Lumen frames the fragment: whose memory this is, and what feels wrong about it.
3. The player pinches, pulls, and aligns memory shards until the structure resolves — a bridge reconnects, a doorway unwarps, a melody re-threads.
4. On resolution: a cinematic camera beat, an audio swell, and a short narrative payoff (a line of dialogue, a visual flourish revealing more of the person's story).
5. The console offers the next fragment, or — if the daily allotment is spent — a warm sign-off from Lumen and a preview of tomorrow's fragment.

## 3. Seated console architecture

Everything happens from one stationary seated position:
- **Reach zone:** all interactive elements live within a 2-foot radius arc in front of the player, roughly chest-to-eye height.
- **No locomotion, no artificial movement of the player's own viewpoint** beyond small, player-triggered cinematic pushes (see below) that are short, motivated, and skippable.
- **Cinematic camera beats** (e.g., the push-in on a successful weave) are capped at small translations over 2+ seconds with ease-out curves — no rotation, no acceleration spikes — to stay inside comfort guidelines even for motion-sensitive players.

This is the "airplane seat test": if you can comfortably play this holding a tablet-sized console on a tray table, seated, without turning your body, the seated-console constraint is satisfied.

## 4. Interaction design (hands-first)

Gestures, in order of introduction:

| Gesture | Motion | Used for |
|---|---|---|
| Pinch | Thumb + index tip together | Grab a memory shard |
| Pull | Pinch + draw hand back | Detach a shard from a warped structure |
| Weave | Pinch + guide shard along a glowing path to its socket | The core repair action |
| Palm-stop | Open palm held still | Pause/halt a moving element (introduced in later chapters) |

Design rules:
- Every core gesture must be performable one-handed (accessibility requirement — see §7).
- No gesture requires sustained fine motor precision for longer than ~2 seconds; the pinch-tolerance radius is generous and tunable via the sensitivity setting.
- Failure states are soft: a mis-released shard drifts back toward its last valid position rather than falling, breaking, or penalizing the player.

## 5. Visual style

- **Palette:** obsidian black/charcoal (`#0a0b0f`, `#14161d`, `#1d2029`) structure, amber/gold (`#f2a94e`, `#ffcf8a`) memory pathways, a cool violet (`#9d8bf0`) reserved exclusively for Lumen so the companion always reads as distinct from the environment.
- **Materials:** polished obsidian with subtle specular highlights; memory pathways are emissive, faintly animated (slow pulse, not strobe) to suggest they are "alive" without being distracting.
- **UI:** translucent glassmorphic panels, floating at a fixed offset within the reach zone, never occluding the diorama itself. Typography is calm and legible at VR reading distances — no small caps, no low-contrast grey-on-grey.
- **Particles:** slow-moving amber motes drifting upward through the scene, denser near unresolved fractures, thinning as a memory resolves — a passive visual signal of progress that doesn't require reading a UI number.

## 6. Audio design

- **Ambient bed:** a low resonant bass drone, always present, establishing the "inside a memory" register.
- **Adaptive layers:** as fragments resolve, higher-register pad and chime layers fade in — the soundtrack literally gets warmer and more complex as the player succeeds, reinforcing the loop without any explicit reward jingle.
- **Spatial cues:** whispers and directional chimes are anchored to their source object in 3D space, so turning your head or hand toward a fracture makes its audio cue localize and brighten — encouraging players to physically investigate rather than stare at a HUD.
- **Silence is used deliberately** at chapter transitions, before Lumen speaks, to give dialogue weight.

## 7. Accessibility framework

Designed as first-class, not an afterthought:

- **One-handed mode:** every gesture has a single-hand equivalent; toggling this doesn't remove content, only removes the requirement that both hands participate.
- **Adjustable hand-tracking sensitivity:** widens or narrows the pinch-detection tolerance for players with limited finger dexterity or hand tremor.
- **High-contrast memory paths:** boosts path glow intensity and adds solid outline strokes to interactive shards for low-vision players.
- **Seated height / offset calibration:** recenters the console to the player's actual eye height and reach, important for wheelchair users, shorter/taller players, and anyone not standing.
- **Subtitles:** full text for all companion dialogue and narrative audio cues, on by default.
- **Master volume + individual layer awareness:** players who are hard of hearing still get the full narrative through subtitles; players sensitive to low-frequency drones can reduce the ambient bed independently in a future pass.
- **Comfort vignette toggle:** present but expected to be rarely needed, since the experience never uses artificial locomotion.

## 8. Agentic AI companion — Lumen

Lumen is not a menu narrator; it's meant to read as *present in the room with you*. Its jobs:
- **Frame** each memory fragment before the player touches anything.
- **Notice** — comment when a hand lingers near the wrong shard, when a player is close to a solution, when they've been idle too long (gentle nudge, never a timer-pressure mechanic).
- **React environmentally** — dim or warm the lighting, thicken or thin the particle stream, in response to player progress, so the world itself feels responsive, not just the dialogue.
- **Carry the throughline** — Lumen's own arc (why it exists, what it wants) unfolds gradually across chapters as a secondary narrative hook for "Best Reason to Come Back."

See `production-roadmap.md` for how the prototype's scripted state machine maps onto a real contextual model integration.

## 9. Retention structure — "Best Reason to Come Back"

- **Daily memory fragment:** one new fragment unlocks per real-world day, keeping sessions short and habitual rather than grindy.
- **Chapter structure:** fragments belong to a specific person's memory-chapter; completing a chapter reveals a short vignette recontextualizing everything repaired so far.
- **No punishing systems:** no energy meters, no monetized speed-ups — the daily gate exists for pacing and anticipation, not restriction-for-its-own-sake.

## 10. First five minutes (cold-start spec)

0:00 – Title layer, one clear choice: enter the console.
0:00–0:10 – Player is seated at the console; Lumen greets them in one short line.
0:10–0:25 – A single fractured shard is presented; Lumen explains the pinch gesture in-context, not via a tutorial popup.
0:25–0:45 – Player pinches and weaves the first shard. Soft-fail is forgiving; success is inevitable within a couple of attempts.
0:45–0:55 – Cinematic push-in + audio swell + Lumen's payoff line.
0:55–1:00 – Console reveals "one more fragment today" or the chapter close, establishing the daily-return rhythm immediately.

Everything above is index-mapped to `index.html`'s `startExperience()` → first pinch → `weaveComplete()` flow.
