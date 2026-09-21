# Compatibility and Host Adapters

## Portable core

The host-neutral core consists of:

- `SKILL.md`, which defines routing by task and pace, experiment contracts, checkpoints, operational boundaries, and completion criteria;
- `references/`, which contains stage commands, artifact meanings, presentation guidance, and troubleshooting;
- `examples/`, which contains prompts that users can copy or adapt.

The core does not call a Codex, OpenAI, Claude, or other agent-host API. Experiment commands belong to the public `distill-course` repository. The host agent explains, orchestrates, and verifies evidence within the permissions granted by the current user and host.

## Current course baseline

This version has been checked against `distill-course` commit `d7761de6a58b3d0566cdde57797bd70ecd3f9c3b`. Treat that commit as the current tested baseline, not as a permanent lock.

Candidate pairing at the 2026-09-20 handoff (pushed to GitHub):

| Component | Commit | Verified scope |
|---|---|---|
| Course candidate | `d5ae90bab7c59da1b6055dee838dafa91056bfbc` | 30 offline CPU tests and bundle verification; adds local registration and stage-aware checks |
| Optional downloader | `8db77af25096bb5564957732166c132efa4ce051` | 9 offline tests; ModelScope snapshot adapter remains provisional |

These commits are available in their GitHub repositories; they are not tagged releases or newly GPU-accepted versions. A supplied local archive is also a valid delivery option; verify its digest and the course's bundle manifest. These candidates are not prerequisites for the default official-client route. The 2026-09-20 37-minute SOL rehearsal used the old image bundle, not the candidate's new registration API; it does not certify the new path end to end or measure learner classroom time. See [repository-sync.md](repository-sync.md).

Before execution, inspect the checked-out course README, `configs/course.json`, Git commit, and existing run directories. A reference reproduction should use the verified course contract. An adapted run may use different models, paths, precision, or resource parameters when the differences are explicit and the resulting evidence remains traceable.

## Public evidence boundary

The public `distill-course` repository distributes only the files committed there. Local formal-run archives, lesson-preparation notes, interactive animations, recordings, and video assets are not currently distributed with that repository.

Do not assume those local materials are present. If a task refers to an archived result or prepared fallback, first locate a public evidence package or user-provided artifact with a run identity, course commit, configuration, summaries, predictions, and manifest. Without that evidence, continue from the current run or public course materials and state the limitation.

## Optional host adapters

WorkBuddy is a candidate classroom host, with source retrieval, package import and scoped checks in [workbuddy-onboarding.md](workbuddy-onboarding.md). Linux compute support does not establish a Linux WorkBuddy desktop client. Cross-platform SSH and native Windows/WSL, macOS MPS/MLX, and alternate Linux compute plans are documentation-informed; they have not inherited GPU end-to-end acceptance from the Windows-to-AutoDL rehearsal. Verify each actual host/backend separately.

`agents/openai.yaml` provides optional Codex or OpenAI presentation metadata. Removing or ignoring it does not change the core workflow. Other hosts may add their own metadata, but an adapter must not weaken the experiment contract, safety gates, evidence requirements, or credential boundaries.

## Installation model

Install the complete repository in a directory discovered by the host. Keep the repository directory, Skill directory, and the `name` field in `SKILL.md` aligned as `distillation-lab-coach`.

- Codex user scope uses `~/.codex/skills/distillation-lab-coach/`.
- Codex project scope follows the current Codex project configuration and official documentation.
- Claude Code user scope uses `~/.claude/skills/distillation-lab-coach/`.
- Claude Code project scope uses `.claude/skills/distillation-lab-coach/`.
- Other hosts use the Skill directory documented by that host.

The current development remote is the private repository [Beirana/distillation-lab-coach](https://github.com/Beirana/distillation-lab-coach). Authorized accounts may clone it with `git clone https://github.com/Beirana/distillation-lab-coach.git`. Do not describe it as a public release or promise access to unapproved users. When the repository becomes public, pin course-facing installation instructions to a stable release or tag.

Claim compatibility with a host only after testing Skill discovery, instruction loading, terminal and SSH permissions, and approval behavior. A compatible directory layout does not imply identical sandboxing, network access, or unattended execution.

## Project references

Keep the public Skill repository as the single publication source. Course projects should link to its stable release or tag after publication instead of maintaining a copied second source. Use a Git submodule only when the parent project genuinely needs a pinned offline copy and can maintain submodule metadata correctly.

## Modes are not authorization

Classroom guidance, fast execution, presentation, question answering, diagnosis, and post-class work change pacing and response shape only. They do not grant additional system permissions. Every user remains subject to the current host's permission and approval model.
