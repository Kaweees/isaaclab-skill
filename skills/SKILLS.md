# Isaac Lab Skills Index

Platform-neutral inventory of the agent skills in this repository. The
top-level [`README.md`](../README.md) points here for the catalog and the
recommended workflow entry points.

All skills live under `skills/`. A skill is a directory containing a
`SKILL.md` with YAML frontmatter for discovery and auto-loading, plus optional
references, examples, evaluations, and scripts.

These skills are workflow routers and execution checklists for an Isaac Lab
checkout. They intentionally defer current commands, APIs, and configuration
details to the checkout's official documentation, source, and maintained
examples.

---

## Quick index

- [Setup and troubleshooting](#setup-and-troubleshooting) — install, verify, diagnose launch failures
- [Environment authoring](#environment-authoring) — create tasks, choose workflows, convert direct environments
- [Sensors, actuators, and randomization](#sensors-actuators-and-randomization) — task data, control models, event terms
- [Physics backends and policy transfer](#physics-backends-and-policy-transfer) — PhysX, Newton, OvPhysX, presets, asset readiness, sim-to-sim
- [Reinforcement learning](#reinforcement-learning) — train, debug, and scale across GPUs or nodes
- [Manipulation and pose diagnostics](#manipulation-and-pose-diagnostics) — phase-gated manipulation tasks and measured joint-pose fixes
- [Migration](#migration) — Isaac Gym to Isaac Lab and Isaac Lab 2.x to 3.x
- [Environment contract](#environment-contract) — paths shared across workflows
- [How the library composes](#how-the-library-composes) — common skill pipelines
- [Suggested read order](#suggested-read-order) — where to start for each kind of request

---

## Setup and troubleshooting

Install Isaac Lab, verify the selected installation path, and diagnose setup or
launch failures before starting task work.

| Skill | Status | What it does |
|---|---|---|
| [`isaaclab-installing-isaac-lab`](install-isaac-lab/SKILL.md) | Experimental | Auto-detects the host, selects the documented install route (automatic uv, downloaded Isaac Sim, source build, Isaac Lab wheel, legacy `isaaclab.sh`, managed Python env, or Docker), applies the documented China Asset Region Profile when requested, presents one consolidated confirmation, executes the official commands, and verifies the result. |
| [`isaaclab-setup-troubleshooting`](setup-troubleshooting/SKILL.md) | Experimental | Routes installation, import, verification, launch, known-issue (such as RTX camera buffer limits), and simulation-performance problems to the matching official troubleshooting guidance and smallest useful check. |

---

## Environment authoring

Create and structure Isaac Lab tasks. Manager-based environments are the
default for new reusable tasks; direct environments remain useful for parity
migrations, bespoke control flow, and validated prototypes.

| Skill | Status | What it does |
|---|---|---|
| [`isaaclab-building-environments`](create-environments/SKILL.md) | Experimental | Scaffolds template-generated external projects or internal tasks with `uv run isaaclab --new`, chooses manager-based versus direct workflow, implements the task, and validates registration, reset, random stepping, and short training. |
| [`isaaclab-converting-direct-to-manager`](convert-direct-to-manager/SKILL.md) | Experimental | Converts a validated direct environment into reusable scene, action, observation, reward, command, event, curriculum, and termination configurations while retaining the direct baseline for parity checks. |
| [`isaaclab-using-presets`](use-presets/SKILL.md) | Experimental | Defines discoverable `PresetCfg` variants for physics, rendering, sensors, events, and task-specific configuration without scattering runtime conditionals through task logic. |

---

## Sensors, actuators, and randomization

Add task inputs and control models after the base environment can construct,
reset, and step.

| Skill | Status | What it does |
|---|---|---|
| [`isaaclab-using-sensors-actuators`](use-sensors-actuators/SKILL.md) | Experimental | Adds cameras, contact and joint-wrench sensors, ray casters, IMUs, sensor-derived observations or rewards, and actuator models; validates binding, shape, device, and update timing. |
| [`isaaclab-randomizing-with-events`](domain-randomization-events/SKILL.md) | Stable | Implements domain randomization through `EventManager` terms in direct and manager-based tasks, including backend-aware prestartup, startup, reset, and interval modes, plus success-driven Automatic/Adaptive Domain Randomization (ADR) curricula. |

---

## Physics backends and policy transfer

Choose a backend, isolate backend-specific configuration, prepare assets for
Newton with MJWarp, and validate policy transfer in both directions.

| Skill | Status | What it does |
|---|---|---|
| [`isaaclab-selecting-backends`](select-backends/SKILL.md) | Experimental | Selects and validates PhysX, Newton, OvPhysX, and renderer behavior, including the opt-in asynchronous render path; maps settings through public schema configs and checks backend support before training. |
| [`isaaclab-using-presets`](use-presets/SKILL.md) | Experimental | Encapsulates explicit backend and renderer alternatives behind named `PresetCfg` variants. |
| [`isaaclab-preparing-assets-for-newton`](prepare-assets-for-newton/SKILL.md) | Experimental | Makes PhysX-compatible robots, objects, and scenes ready for Newton MJWarp by auditing imported schemas, mechanics, contact, actuator response, solver profiles, and reset stability. |
| [`isaaclab-transferring-policies-sim-to-sim`](isaaclab-transferring-policies-sim-to-sim/SKILL.md) | Experimental | Transfers policies between PhysX and Newton MJWarp after task and asset parity, preserving the checkpoint contract, pinning joint and body ordering to one convention for training and every replay, and evaluating the full PhysX/PhysX, PhysX/Newton, Newton/Newton, and Newton/PhysX matrix; validated on `Isaac-Lift-Franka`, `Isaac-Velocity-Rough-G1`, and `Isaac-Velocity-Rough-AnymalD`. |

---

## Reinforcement learning

Train only after the environment passes import, reset, and random-action smoke
tests. Scale to distributed execution only after the same task succeeds on one
GPU.

| Skill | Status | What it does |
|---|---|---|
| [`isaaclab-training-rl-agents`](train-rl-agents/SKILL.md) | Experimental | Selects an RL framework, wires the matching agent config, launches or resumes training, evaluates checkpoints, and records a reproducible run contract. |
| [`isaaclab-debugging-rl-training`](debug-rl-training/SKILL.md) | Experimental | Diagnoses reward behavior, metrics, resets, physics, runners, and checkpoint compatibility through focused reproductions, ablations, and deterministic rollouts. |
| [`isaaclab-training-multi-gpu`](train-multi-gpu/SKILL.md) | Experimental | Launches multi-GPU and multi-node jobs, distinguishes launcher failures from NCCL stalls, captures per-rank evidence, and validates actual distributed scaling. |

---

## Manipulation and pose diagnostics

Use phase gates and measured articulation state for contact-rich manipulation
instead of trying to infer task readiness or joint corrections from training
reward or screenshots alone.

| Skill | Status | What it does |
|---|---|---|
| [`isaaclab-planning-manipulation-tasks`](plan-manipulation-tasks/SKILL.md) | Experimental | Stages reach, grasp, lift, place, insertion, and tool-use tasks through scene, reset, action, observation, reward, and deterministic behavior gates. |
| [`isaaclab-diagnosing-joint-poses`](diagnose-joint-poses/SKILL.md) | Experimental | Converts a semantic pose request into a body-axis condition, sweeps candidate joints within limits, and reports measured before/after alignment. |

---

## Migration

Preserve a working baseline and migrate one compatibility layer at a time.

| Skill | Status | What it does |
|---|---|---|
| [`isaaclab-migrating-from-isaac-gym`](migrate-from-isaac-gym/SKILL.md) | Experimental | Ports Isaac Gym tasks, tensors, assets, rewards, resets, and training directly to the current Isaac Lab APIs as a direct environment first (no intermediate older-release port), scaffolds new external projects with the template generator, then validates actual policy behavior. |
| [`isaaclab-converting-direct-to-manager`](convert-direct-to-manager/SKILL.md) | Experimental | Provides the follow-up path from a validated direct parity port to a reusable manager-based task. |
| [`isaaclab-migrating-2x-to-3x`](migrate-2x-to-3x/SKILL.md) | Experimental | Migrates Isaac Lab 2.x projects to 3.x across installation, backends and presets, asset and actuator APIs, sensors, quaternion conventions, `ProxyArray`, RL entry points, simulation launch (`launch_simulation`), visualizers and recording, importers, and extension loading. |

---

## Environment contract

The workflows assume these variables are set in the agent configuration or
shell profile. Skills should reference variables rather than hardcoded local
machine paths.

| Variable | Purpose | Example |
|---|---|---|
| `$ISAAC_LAB_DIR` | Isaac Lab checkout root whose docs, source, examples, and entry points are authoritative | `/path/to/IsaacLab` |
| `$ISAAC_SIM_DIR` | Isaac Sim installation or source-build root when the selected Isaac Lab route uses Isaac Sim | `/path/to/IsaacSim` |
| `$WORKSPACE_DIR` | Project-local outputs, experiments, logs, checkpoints, and scratch files | `/path/to/project/.workspace` |

Run checkout-relative commands from `$ISAAC_LAB_DIR`. Do not assume that a
similarly named installed wheel or another checkout provides the same APIs,
task registrations, or documentation.

---

## How the library composes

Use these diagrams during orientation to select the smallest relevant skill
chain.

### New task pipeline

```text
isaaclab-installing-isaac-lab
              │
              ▼
isaaclab-selecting-backends ─────► isaaclab-using-presets
              │
              ▼
isaaclab-building-environments
              │
              ├────────► isaaclab-using-sensors-actuators
              │
              └────────► isaaclab-randomizing-with-events
                              │
                              ▼
                 isaaclab-training-rl-agents
                              │
                 ┌────────────┴────────────┐
                 ▼                         ▼
isaaclab-debugging-rl-training   isaaclab-training-multi-gpu
```

### Manipulation task pipeline

```text
isaaclab-building-environments
              │
              ▼
isaaclab-planning-manipulation-tasks
              │
              ├────────► isaaclab-using-sensors-actuators
              │
              ├────────► isaaclab-diagnosing-joint-poses
              │
              ▼
isaaclab-training-rl-agents
              │
              ▼
isaaclab-debugging-rl-training
```

### Isaac Gym migration pipeline

```text
Isaac Gym task
      │
      ▼
isaaclab-migrating-from-isaac-gym
      │
      ▼
validated DirectRLEnv / DirectMARLEnv baseline
      │
      ▼
isaaclab-converting-direct-to-manager
      │
      ▼
reusable manager-based task
```

### PhysX and Newton policy-transfer pipeline

```text
isaaclab-selecting-backends
              │
              ▼
isaaclab-using-presets
              │
              ▼
isaaclab-preparing-assets-for-newton
              │
              ├────────► validate task in PhysX
              └────────► validate task in Newton MJWarp
                              │
                              ▼
                 train same task contract in both
                              │
                              ▼
       isaaclab-transferring-policies-sim-to-sim
                  (PP, PN, NN, and NP)
```

---

## Suggested read order

There is no single mandatory order for all 17 skills. Start with the entry
point matching the request, then follow its cross-skill hand-offs:

| Goal | Start here | Continue with |
|---|---|---|
| Install Isaac Lab | [`isaaclab-installing-isaac-lab`](install-isaac-lab/SKILL.md) | [`isaaclab-setup-troubleshooting`](setup-troubleshooting/SKILL.md) if verification fails |
| Create a new task | [`isaaclab-building-environments`](create-environments/SKILL.md) | Sensors/actuators, events, presets, then RL training |
| Add contacts, cameras, IMUs, or actuators | [`isaaclab-using-sensors-actuators`](use-sensors-actuators/SKILL.md) | Backend selection if support differs |
| Add domain randomization or ADR | [`isaaclab-randomizing-with-events`](domain-randomization-events/SKILL.md) | Presets when PhysX and Newton need different terms |
| Build a manipulation task | [`isaaclab-planning-manipulation-tasks`](plan-manipulation-tasks/SKILL.md) | Environment authoring, sensor setup, and pose diagnostics |
| Train a task | [`isaaclab-training-rl-agents`](train-rl-agents/SKILL.md) | RL debugging, then multi-GPU scaling |
| Debug learned behavior | [`isaaclab-debugging-rl-training`](debug-rl-training/SKILL.md) | Manipulation planning for contact-rich task failures |
| Run on multiple GPUs or nodes | [`isaaclab-training-multi-gpu`](train-multi-gpu/SKILL.md) | Start only after single-GPU training succeeds |
| Port an Isaac Gym task | [`isaaclab-migrating-from-isaac-gym`](migrate-from-isaac-gym/SKILL.md) | Direct-to-manager conversion after parity succeeds |
| Upgrade an Isaac Lab 2.x project | [`isaaclab-migrating-2x-to-3x`](migrate-2x-to-3x/SKILL.md) | Targeted smoke tests for each migrated API area |
| Add Newton support | [`isaaclab-selecting-backends`](select-backends/SKILL.md) | Presets, Newton asset preparation, then sim-to-sim transfer |
| Transfer policies between PhysX and Newton | [`isaaclab-preparing-assets-for-newton`](prepare-assets-for-newton/SKILL.md) | [`isaaclab-transferring-policies-sim-to-sim`](isaaclab-transferring-policies-sim-to-sim/SKILL.md) after both backends pass |

---

## When stuck

- Installation, import, or launch failure: [`isaaclab-setup-troubleshooting`](setup-troubleshooting/SKILL.md)
- Simulation or training is unexpectedly slow: [`isaaclab-setup-troubleshooting`](setup-troubleshooting/SKILL.md)
- Environment does not import, reset, or step: [`isaaclab-building-environments`](create-environments/SKILL.md)
- Reward improves but behavior does not: [`isaaclab-debugging-rl-training`](debug-rl-training/SKILL.md)
- Distributed job hangs or aborts in NCCL: [`isaaclab-training-multi-gpu`](train-multi-gpu/SKILL.md)
- PhysX and Newton behave differently: [`isaaclab-selecting-backends`](select-backends/SKILL.md)
- A transferred checkpoint drives the wrong joints in the other backend: [`isaaclab-transferring-policies-sim-to-sim`](isaaclab-transferring-policies-sim-to-sim/SKILL.md)
- An asset fails or becomes unstable only in Newton MJWarp: [`isaaclab-preparing-assets-for-newton`](prepare-assets-for-newton/SKILL.md)
- Robot tool, wrist, camera, or gripper starts in the wrong orientation: [`isaaclab-diagnosing-joint-poses`](diagnose-joint-poses/SKILL.md)

---

*Library size: 17 skills (1 stable, 16 experimental). Synced from the `skills/user/` directory of Isaac Lab v3.0.0.*
