# Compatibility and Host Adapters

## Portable core

The host-neutral core consists of:

- `SKILL.md`: mode selection, experiment contract, checkpoints, operational boundaries, and completion criteria;
- `references/`: stage commands, artifact meanings, teacher guidance, and troubleshooting;
- `examples/`: prompts that users can copy or adapt.

The core does not call a Codex, OpenAI, Claude, or other agent-host API. Experiment commands belong to the public `distill-course` repository. The host agent explains, orchestrates, and verifies evidence.

## Optional host adapters

`agents/openai.yaml` provides optional Codex/OpenAI presentation metadata: display name, summary, and default prompt. Removing or ignoring it does not change the core workflow. Other hosts may add their own metadata, but an adapter must not weaken the experiment contract, hard gates, or credential boundaries.

## Installation model

Copy or clone the whole repository into a directory discovered by the host. Keep the repository name, directory name, and the `name` field in `SKILL.md` aligned as `distillation-lab-coach`.

- Codex user scope: `~/.codex/skills/distillation-lab-coach/`
- Claude Code user scope: `~/.claude/skills/distillation-lab-coach/`
- Claude Code project scope: `.claude/skills/distillation-lab-coach/`
- Other hosts: use the skill directory documented by that host.

Claim compatibility with a host only after testing skill discovery, instruction loading, terminal and SSH permissions, and approval behavior. A compatible directory layout does not imply identical sandboxing, network access, or unattended execution.

## Modes are not authorization

Teacher control and student execution change explanation and pacing only. They do not grant additional system permissions. The agent remains subject to the current host's permission and approval model.
