# Handoff: The Vigil combat prototype

**Receiving agent:** a Claude chat started in `C:\Users\Joe\source\ue\dream`,
with this workspace added through
`/add-dir C:\Users\Joe\source\repos\mr-mak-workspace`.

**Task:** build the combat prototype, stage 0 onward, from the plan. Confirm the
plan with the owner before starting stage 0.

## Read first

1. `workspace/2026-10-17_the-vigil/combat-prototype-plan.md`: the plan, with
   stages, outputs, checks, constraints and technical direction.
2. `workspace/2026-10-17_the-vigil/foundation.md`: the agreed design. Its hard
   rules apply to every placeholder. The Combat section defines the rules being
   prototyped.
3. `AGENTS.md` in the workspace: the repository rules for anything written there.

## Baseline (2026-10-17)

- **Workspace:** `main` at `9d071da` (foundation and card). The plan, this
  handoff and the card's second tab were written after that commit; read them
  from disk.
- **Unreal project:** UE 5.8 C++, containing only `dreamGameModeBase` and
  module boilerplate in `Source/dream/`. Content has no gameplay assets. Not a
  Git repository. `workflow.json` (0 bytes) is of unknown purpose; leave it.
- **Plugins enabled in `dream.uproject`:** ModelContextProtocol,
  MCPClientToolset, AllToolsets, PCGToolset, ModelingToolsEditorMode.
- **Editor:** open. Its MCP endpoint at `127.0.0.1:8000/mcp` answered. Never
  close the editor or restart a chat without asking the owner.
- **Engine:** `C:\Games\UE_5.8`. PoseSearch, MotionWarping and
  AnimationLocomotionLibrary are available as engine plugins.
- **Tools:** Visual Studio Community 2026 with the C++ desktop workload (the
  "Game development with C++" workload is not installed), Git LFS 3.4.0.
- **Hardware:** Ryzen 5 2600, 32 GB RAM, Radeon RX 6600 XT.

## Accepted choices

- Unreal Engine 5.8 in C++, with the Gameplay Ability System, StateTree, Motion
  Matching for locomotion only, and combat montages with explicit notify-state
  windows.
- **Free assets only:** UE5 mannequins, Epic's Game Animation Sample if it
  supports 5.8, and free Fab sword packs. Record missing clips; don't buy any.
- The combat model: Read → Answer → Punish, Momentum (the hero), Resolve pips
  (enemies), instant Style switching with distinct parry follow-ups, Style
  weaving and Momentum Surge.

## Ownership

- **This chat owns** everything in `C:\Users\Joe\source\ue\dream`.
- **In the workspace, write only:**
  - `workspace/2026-10-17_the-vigil/prototype-log.md` (new): a chronological
    log of stages, checks and findings. Register it as a "Prototype log" tab on
    the `the-vigil` card in `workspace/workspace.json`, and update that card's
    `updated` date.
  - `workspace/2026-10-17_the-vigil/assets/prototype/`: captures and evidence.
  - `combat-prototype-plan.md`: change it only when evidence changes the
    approach, and give the reason in the log.
- **Do not edit `foundation.md`.** Propose design changes to the owner instead.
- Re-read `workspace.json` immediately before editing it; other chats may have
  changed it.

## Starting points for stage 0

- Git: run `git init` in the project, add an Unreal `.gitignore` (excluding
  `Binaries/`, `Intermediate/`, `Saved/`, `DerivedDataCache/` and `.vs/`), run
  `git lfs track "*.uasset" "*.umap"`, then make the first commit.
- Command-line build:
  `C:\Games\UE_5.8\Engine\Build\BatchFiles\Build.bat dreamEditor Win64 Development -Project="C:\Users\Joe\source\ue\dream\dream.uproject" -WaitMutex`.
  Live Coding in an open editor can block command-line builds. If it does, use
  Live Coding from the editor or ask the owner before closing the editor.
- Confirm the MCP tools respond from this chat before relying on them.

## Skills

- `unreal-engine-skills` (enabled in the Unreal project)
- `C:\Users\Joe\source\repos\mr-mak-workspace\.agents\skills\game-animation-integration\SKILL.md`
- `C:\Users\Joe\source\repos\mr-mak-workspace\.agents\skills\game-vfx-workflow\SKILL.md`
- `C:\Users\Joe\source\repos\mr-mak-workspace\.agents\skills\gameplay-visual-review\SKILL.md`
- `C:\Users\Joe\source\repos\mr-mak-workspace\.agents\skills\workspace-authoring\SKILL.md`

## Acceptance

Each stage passes by the observation listed in the plan. Keep agent
verification (tests, captures, measured frames and latencies) separate from
owner acceptance. Only the owner, after playing the build at normal speed in
stage 8, decides whether the combat feels great. Do not call a stage done
without its evidence.

## Unresolved

- Whether the Game Animation Sample supports UE 5.8.
- How much attack, parry, hit-reaction and death coverage free sword packs
  provide.
- Whether Visual Studio 2026 builds UE 5.8 cleanly (stage 0 answers this).
- The terrain-deformation approach (the stage 7 spike answers this).
