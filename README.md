# Bonfire

> A third-person **soulslike prototype** built in **Unity (C#)** — my **first-ever 3D game and my first real codebase**.

This repository is where my game-programming journey started. It is the **only Unity / C# project** in my portfolio (everything since has been Unreal Engine / C++).

I share it openly and honestly: parts of it are clearly **beginner code**, but it already contains a few **surprisingly advanced systems** for that stage — and, more importantly, **the origin of almost every architectural pattern I still use today**.

> **Status — early learning project, unfinished.** The value here is the **learning curve and the early instincts**, not polish or feature-completeness.

---

## Where this fits in my development

This is **project #1**. Reading it next to my later work shows a concrete, honest progression:

| Project | When | Engine | What it shows |
|---|---|---|---|
| **➡ Bonfire (this repo)** | university | earliest (~1.5 yrs in) | Unity / C# | First 3D & codebase: hand-built FSM, interfaces, ScriptableObject events & data, procedural generation |
| **Eternal Grace Arena** | university | UE5 / C++ | First Unreal project — a solo soulslike |
| **EternalGrace Prototype** | after graduating | UE5 / C++ | The deliberate clean rebuild: data payloads, SOLID, save subsystem, Behavior-Tree AI |
| **Project Mirror / Glass Purgatory** | current indie project | UE5 / C++ | Decoupled Pub/Sub event subsystem, CommonUI + MVVM, framework-for-reuse |

Several threads in my later projects can be traced directly back to here:

- **AI:** the hand-built **FSM in this project** → manual sensing (*Eternal Grace Arena*) → Behavior Trees (*EternalGrace Prototype*) → State Trees (later, professional work).
- **Architecture:** **interfaces & ScriptableObject events/data here** → interfaces + data payloads + subsystems (*EternalGrace Prototype*) → decoupled Pub/Sub + reusable framework (*Project Mirror*).
- **Genre:** even the name *Bonfire* marks the start of the **soulslike line** (HP, stamina, lock-on, souls, bonfire respawn) that runs through every project since.

Everything under `Assets/Scripts/` is my own work, with two clearly-marked exceptions noted below.

---

## Tech stack

| Area | Details |
|---|---|
| Engine | **Unity 2022.3 LTS** |
| Language | **C#** |
| Patterns | Custom finite-state machine, C# interfaces, ScriptableObject events & data, `[ExecuteInEditMode]` editor tooling |
| Rendering | GPU instancing via `Graphics.DrawMeshInstanced` for procedural vegetation |

---

## Engineering highlights

These are the systems that make this more than a tutorial project — and the seeds of my later work.

### Hand-built finite-state machine for enemy AI
Enemy AI runs on a **state machine I wrote from scratch**: each state is a class with `Enter` / `Update` / `Exit`, and transitions are driven by a **table of delegate conditions** (`Dictionary<State, Dictionary<Delegate, State>>`). It covers detection/aggro and Idle / Chase / Attack / Strafe / Return states, plus boss and "Agonized" variants (`EnemyStateMachineBase`, `Boss01_StateMachine`, …).
→ This is the **origin of my AI line**: FSM → Behavior Trees → State Trees.

### Interfaces from day one
Even in my very first codebase I was thinking in terms of contracts: `IDamageable`, `IAttackable`, `IAggroable`, `IWetable`, `IElectrilizable`, … 
→ The **start of the interface-driven design** that appears in *every* later project.

### Decoupled event system via ScriptableObjects
A self-built **Pub/Sub event system** (`GameEvent` / `EventListener`) using ScriptableObject assets as event channels, so systems communicate without referencing each other directly.
→ The **origin of the Pub/Sub event architecture** I later built as a full subsystem in *Project Mirror*.

### Data-driven design via ScriptableObjects
Gameplay data (e.g. `SO_Spell`) lives in **ScriptableObject assets rather than hardcoded in scripts**.
→ The **start of data-driven design** (later DataTables / DataAssets in Unreal).

### Emergent elemental system
A small **systemic interaction**: *wet* + *lightning* → *electrified*, modelled through `StatusEffect`, `WetableEnemy`, `ElectrifyableSurface` and `ElectrifiedWaterSplash`.
→ The **origin of mechanic-driven, systemic design**.

### Procedural environment generation with GPU instancing *(advanced for the stage — see honest note)*
An area-based `EnvironmentManager` (`[ExecuteInEditMode]`) generates procedural plane/terrain meshes (`CustomPlane`, `PlaneGenerator`, `FalloffGenerator`) and scatters **noise-driven vegetation** — spawn positions sampled from the mesh, oriented to vertex normals, filtered by flatness/height thresholds, with randomized rotation/scale. Rendering uses **`Graphics.DrawMeshInstanced`**, deliberately batched into lists of 1000 matrices to respect the instancing limit.
→ Evidence that I **taught myself an advanced topic early on** (mesh generation, GPU instancing).

---

## Honest maturity note

I'm keeping this honest because the **learning curve is the point**:

- **Beginner patterns are present:** `GameObject.Find("Player")`, public fields used as inspector wiring, a few `NotImplementedException` stubs, frame-polling, and a transition loop that can mutate state mid-iteration.
- **Alongside them sit genuinely advanced systems:** the procedural GPU-instanced environment, the hand-built FSM, and interface-driven design.

For **~1.5 years of self-taught experience with no formal background**, that mix is what makes the jump to my later, much cleaner projects credible. The procedural-generation work in particular I treat as **proof of learning ability and breadth, not a current core competency** — I haven't used Unity/C# or procedural generation actively since.

---

## Building

### Third-party code

Almost all gameplay code under `Assets/Scripts/` is my own. The exceptions are clearly isolated:

- `Assets/Scripts/Game World Generation/Noise/Noise.cs` — a third-party Simplex-noise primitive (libnoise / Sebastian Lague). The procedural-generation system built *around* it is my own.
- `Assets/NavMeshComponents/` — Unity's official NavMeshComponents package, not my code.

---

## License

© 2026 Leonard Kemenani. All rights reserved.
Source is published for **portfolio and review purposes**. Not licensed for redistribution or reuse without permission.
