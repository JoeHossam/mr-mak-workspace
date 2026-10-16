# Combat prototype plan

**Outcome:** a small playable Unreal arena where you can judge whether the core
combat feels great (the parry, Momentum, Resolve, two Styles, Style weaving and
Momentum Surge) against a group of soldiers and one champion. It also answers
three of the technical risks with measured evidence.

This plan covers roadmap item 2 of the [foundation design](foundation.md). It
is a feel prototype: graybox art, placeholder animations and no story content.

## What exists

- **Unreal project:** `C:\Users\Joe\source\ue\dream`, an empty UE 5.8 C++
  project (only a GameModeBase). Not yet under version control.
- **Editor tooling:** Epic's experimental ModelContextProtocol plugin serves MCP
  at `127.0.0.1:8000/mcp` while the editor is open. Sessions started in the
  project folder get `unreal-mcp` and the `unreal-engine-skills` plugin.
  PCGToolset and ModelingToolsEditorMode are enabled.
- **Engine and toolchain:** UE 5.8 at `C:\Games\UE_5.8` (with the PoseSearch,
  MotionWarping and AnimationLocomotionLibrary plugins available) and Visual
  Studio Community 2026.
- **Hardware:** Ryzen 5 2600 (6 cores), 32 GB RAM, Radeon RX 6600 XT. Keep the
  arena small and the scalability at medium. C++ builds are slow on 6 cores, so
  put gameplay rules in a few focused C++ classes and keep tuning values in data
  assets.

## Constraints

- **Free assets only.** Characters are the UE5 mannequins, which are faceless
  and suit the hard rules. Locomotion comes from Epic's Game Animation Sample if
  it supports 5.8, otherwise from the mannequin defaults. Sword clips come from
  free Fab packs. Fill gaps with retargeting, mirroring and montage time
  scaling, and record what is missing instead of buying it.
- **The foundation's hard rules apply,** even to placeholders: no faces, no
  humans, no NSFW.
- **Implementation runs in a Claude chat started in the Unreal project folder,**
  with this workspace added through `/add-dir`. That chat owns the Unreal work,
  and this card holds the plan and the results.
- **Never close the editor or restart a running chat without asking.**

## Technical direction

- **Gameplay Ability System** for actions (attack, parry, dodge, Style switch,
  Technique, Surge) and attributes (the hero's Momentum, enemy Resolve pips,
  Health).
- **Combat timing comes from animation notify states:** attack-active, parry
  and cancel windows. A debug overlay shows these windows, the input buffer,
  Momentum and Resolve live.
- **Motion Matching drives locomotion only.** Combat actions are montages with
  explicit cancel windows, so fluid movement never delays a parry.
- **Each Style is a data asset:** its moveset, its parry follow-up, the defense
  tags it breaks, and its Techniques.
- **Enemies use StateTree,** with clear telegraphs and an attack-token system
  that limits how many soldiers attack at once.
- **Camera:** a free camera plus lock-on.

## Stages

Each stage names its output and the observation that shows it works. Agent
verification (tests, captures, measurements) is separate from your acceptance of
the feel after playing it.

| # | Stage | Output | Shows it works |
|---|---|---|---|
| 0 | **Safety and setup** | Git with an Unreal `.gitignore` and Git LFS for `.uasset`/`.umap`; a clean C++ build from the command line; MCP reachable | First commit; the build log succeeds; the MCP tool list responds |
| 1 | **Arena and hero controller** | Graybox arena, a mannequin hero with a sword, locomotion, combat dash, free camera and lock-on | Capture of movement, dashing and lock-on; a dash can be cancelled into an attack |
| 2 | **Combat core and impact** | Light and heavy attacks with an input buffer and cancel windows; block; perfect parry; dodge; hit-stop, camera shake and knockback from the first hit | Automated tests for the parry window and buffer timing; the overlay shows the windows; hits feel heavy in a capture |
| 3 | **Soldiers and groups** | A soldier with readable normal, heavy and flurry attacks, attack tokens, 0–1 Resolve | A recorded skilled run kills five soldiers in under 2 seconds, and a slow, safe run also wins |
| 4 | **Momentum and Resolve** | Momentum gained through aggression, perfect parries and perfect dodges; Momentum drained by blocking and a guard break at zero; Resolve pips that crack visibly; the decisive blow | Automated tests for the Momentum and Resolve rules; a capture of cracking and the decisive blow |
| 5 | **Two Styles and weaving** | Instant switching between two Styles, each with its own parry follow-up (for example a riposte and a sweeping cut) and a defense it breaks (shield or heavy) | Switch latency measured in frames; weaving grants its bonus in the overlay; a capture of mid-fight switching |
| 6 | **Champion duel and Surge** | One champion with several Resolve pips, varied telegraphs (including a grab and a poison strike), one Technique per Style for the hero, and Momentum Surge (a time-slowed chain of decisive cuts) | The duel plays from start to finish; a capture of a Surge chain through soldiers |
| 7 | **Technical spikes** (time-boxed; the output is an answer, not production code) | (a) **Terrain deformation:** one slash cuts a terrain section, using dynamic-mesh terrain or authored destruction; (b) **Physics at scale:** a clash throws 50–100 bodies and debris; (c) **Motion Matching against responsiveness:** input-to-action latency with Motion Matching on and off | A findings note on this card with frame times and latencies on this hardware, and a recommendation for each risk |
| 8 | **Review** | Native captures and a short evidence record (`gameplay-visual-review`); you play the build | You accept or reject the feel, with concrete notes for the next iteration |

## Mandatory and optional

**Mandatory:** stages 0–8. This is the smallest build that answers "does this
combat feel great?" and the early technical questions the foundation lists.

**Optional extensions,** only after you accept stage 8: an earth-shaking slash
(the end-of-early-game impact level), aerial combat from a launching parry,
stealth takedowns, a second champion, controller vibration.

## Not in this prototype

Story, the Trust system, the mercy setting, the open world, PCG land, final
characters and art, audio beyond placeholder hit sounds, and the battles of
hundreds (risk 3 waits for a later spike).

## Skills for the implementing chat

- `unreal-engine-skills` (enabled in the Unreal project)
- `.agents/skills/game-animation-integration/SKILL.md`: clip timing, events and
  interruptions
- `.agents/skills/game-vfx-workflow/SKILL.md`: hit and clash effects
- `.agents/skills/gameplay-visual-review/SKILL.md`: stage 8 evidence
- `.agents/skills/workspace-authoring/SKILL.md`: reporting results on this card
