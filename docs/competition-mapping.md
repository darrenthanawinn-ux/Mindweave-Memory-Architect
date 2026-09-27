# Competition Category Mapping

How MindWeave's design decisions target each award category.

## Best New Entertainment Experience ($100k)
The overall pitch: a hands-first, seated psychological puzzle experience with a distinct visual identity (obsidian + amber memory-weaving) and a narrative hook (repairing real/fictional figures' memories) that doesn't map onto an existing VR genre template. Judges should come away able to describe the concept in one sentence — that's the test the design doc's premise section is written to pass.

## Best Agentic Interaction ($25k)
Carried by Lumen. The differentiator to emphasize in the submission: Lumen doesn't just deliver hint text, it **reads the player's hands and environment** (proximity, dwell time, near-miss attempts) and **acts back on the world** (lighting, particle density, pacing of dialogue) rather than only reacting in a chat-style bubble. The prototype's scripted state machine demonstrates the interaction *shape*; the production roadmap's model-backed version is what should be built out before submission for this category specifically, since judges will likely probe how dynamic the responses really are.

## Best First Five Minutes ($25k)
Directly scoped in `docs/design-doc.md` §10 and implemented in the prototype's boot flow: title → seated console → first gesture taught in-context (no tutorial popup) → guaranteed early success → cinematic payoff, landing inside roughly a minute, well under any 5-minute ceiling. Keep this sequence tightly timed and rehearsed for the submission video — first-five-minutes categories are usually judged largely on a recorded playthrough.

## Best Reason to Come Back ($25k)
Carried by the daily-fragment structure and chapter-completion vignettes (design doc §9), not by punitive retention mechanics. The pitch to lead with: progress persists meaningfully (a person's memory gets more whole every day you return) rather than a countdown timer or energy system — this is the more defensible story for a judged category that's wary of dark-pattern retention design.

## Best Accessibility Forward Experience ($25k)
The strongest, most concretely demonstrated category in this prototype — the accessibility panel is fully functional today: one-handed mode, tunable hand-tracking sensitivity, seated height calibration, high-contrast paths, subtitles, and volume control all work end-to-end and persist between sessions. For submission, pair this with a short accessibility-focused demo clip showing the same puzzle solved one-handed and with high-contrast mode on, since this category tends to reward *shown*, not just *listed*, accessibility.

## Cross-category notes
- The zero-locomotion, seated-only design (comfort/airplane-seat test) isn't itself a category, but removing motion-sickness risk protects every other category's judged playthrough — a judge who gets uncomfortable won't finish evaluating the other four.
- Keep the submission video under the format each category typically expects (short, first-person, headset POV where possible) — confirm current submission requirements against the event's official rules page before final cut, since those specifics aren't fixed by this design doc.
