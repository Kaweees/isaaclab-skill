# isaaclab-skill

A collection of [Agent Skills](https://agentskills.io/) for NVIDIA Isaac Lab which covers building, training, debugging, and migrating NVIDIA Isaac Lab projects across PhysX and Newton.

The skills mirror the `skills/user/` directory of the [Isaac Lab](https://github.com/isaac-sim/IsaacLab) repository. This version tracks Isaac Lab v3.0.0.

## Install

Install with the [`skills` CLI](https://github.com/vercel-labs/skills).

Global install:

```sh
npx skills add kaweees/isaaclab-skill -g        # prompt to choose skills and agents
npx skills add kaweees/isaaclab-skill --all -g  # install every skill for every agent
```

Project install:

```sh
npx skills add kaweees/isaaclab-skill           # prompt to choose skills and agents
npx skills add kaweees/isaaclab-skill --all     # install every skill for every agent
```

## After install

1. Set the env vars used by the skills (see the [environment contract](skills/SKILLS.md#environment-contract)):

   | Variable | Purpose | Example |
   | --- | --- | --- |
   | `$ISAAC_LAB_DIR` | Isaac Lab checkout root | `/path/to/IsaacLab` |
   | `$ISAAC_SIM_DIR` | Isaac Sim install or source-build root, when required | `/path/to/IsaacSim` |
   | `$WORKSPACE_DIR` | Project outputs, experiments, logs, and scratch space | `/path/to/project/.workspace` |

2. Start with the catalog: [skills/SKILLS.md](skills/SKILLS.md). Its [suggested read order](skills/SKILLS.md#suggested-read-order) maps each kind of request to an entry skill.

## Layout

```
.
├── README.md
├── LICENSE
├── skills/
│   ├── SKILLS.md            # Catalog and how skills compose
│   ├── install-isaac-lab/
│   ├── create-environments/
│   ├── train-rl-agents/
│   ├── select-backends/
│   └── ...                  # One directory per skill (each has SKILL.md)
```

Each skill is a folder under `skills/` with a `SKILL.md` plus optional `scripts/` and `references/`. Format details: [agentskills.io specification](https://agentskills.io/specification).

## License

MIT. See [LICENSE](LICENSE).
