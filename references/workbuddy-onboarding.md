# WorkBuddy host preparation

Read only when WorkBuddy is selected. The core skill remains host-neutral. This is a documentation-informed onboarding candidate, not evidence that the current WorkBuddy version, model or OS passed this course. Check actual behavior and record gaps.

## Install and identify the host

Use [WorkBuddy's official site](https://www.workbuddy.cn/) and the applicable current installer. Official documentation provides [Windows guidance](https://www.workbuddy.cn/docs/workbuddy/From-Beginner-to-Expert-Guide/Installation-Win-Guide) and a Mac installation entry. Linux desktop availability is not confirmed by this course; do not invent an installer or equate Linux compute support with WorkBuddy desktop support. Offer another authorized terminal-capable host or human execution when needed.

The human signs in and selects an actually available conversational model; do not prescribe a model name or promise registration credits cover the lab. Account access and credentials stay with the human. WorkBuddy, its model provider, and the experimental models have different roles and costs.

## Start from no local skill

The authoritative development source is the private [Beirana/distillation-lab-coach](https://github.com/Beirana/distillation-lab-coach) repository. Use the user's existing authorized browser/Git connection, or request instructor-provided access; do not reauthorize unnecessarily, borrow the instructor's token, or change repository visibility.

The user can obtain a complete repository archive from GitHub, or authorize a clone into a new, nonexisting skill directory. Select and record the course-designated commit once obtained. No universal local path, preloaded skill or specific old experiment directory is assumed. Do not overwrite an existing skill; first compare identities and preserve user modifications.

The bootstrapping request may say:

```text
Please obtain the complete distillation-lab-coach skill from
https://github.com/Beirana/distillation-lab-coach using my existing authorization.
Preserve existing files; if access fails, tell me what permission is missing.
Then read its SKILL.md and guide me through lab preparation and a 60-minute session.
Confirm my compute route and connection details with me. Do not modify the skill.
```

This is a user instruction given before the skill is loaded, not a claim that the skill can install itself without access.

## Load the complete package

The [official skill UI](https://www.workbuddy.cn/docs/workbuddy/From-Beginner-to-Expert-Guide/Function-Description/Skills-Market) supports importing a local skill package and enabling installed skills. Downloading from GitHub necessarily creates local files; the test is no pre-provisioned copy, not forbidding the user's requested download.

Keep the full `distillation-lab-coach` folder with `SKILL.md`, `references/`, and `examples/`. `SKILL.zh-CN.md` is a human translation, not the discovery entry. Use the package layout accepted by the current UI; record import failures instead of silently dropping references. A `.workbuddy/skills/distillation-lab-coach/` path was used in a prior local setup but is not a guarantee of discovery on every version or platform. Prefer documented import when discovery fails, or explicitly read the entry file and references while labeling that as manual loading.

## Work directory, permissions and verification

Select a user-chosen local experiment directory for a new ordinary task; do not assume an automatically generated directory is the desired evidence location. WorkBuddy's [task UI documentation](https://www.workbuddy.ai/docs/zh/workbuddy/From-Beginner-to-Expert-Guide/Function-Description/Task-Bar) describes task directories. Desktop, cloud and mobile-triggered tasks must not be conflated: none proves access to the intended local terminal/GPU.

Keep [default permissions](https://www.workbuddy.cn/docs/workbuddy/From-Beginner-to-Expert-Guide/Function-Description/Permission-Modes) initially. Request only needed terminal/SSH/path access; do not require fully permissive mode or disabled security tools to attend class.

Validate with small scoped actions:

1. Read the actual English entry and a linked reference; report paths and relevant instructions. Quoting a skill name alone is not loading evidence.
2. With authorization, run a read-only check of the actual directory, OS, shell and interpreter. No full environment dumps or secret scans.
3. For remote computation, verify noninteractive SSH in this host's execution context. For local computation, verify the chosen native/WSL/container command path instead. Do not set up unnecessary SSH.
4. If saving reports is authorized, create one uniquely named small file inside the work directory and verify its content, preserving existing files.

Record host version, actual model label, skill identity, loading method, execution context, approvals, and untested capabilities. Do not automate UI interaction that the current host cannot perform; give human instructions instead. Before classroom release, validate Windows and Mac host behavior separately; Linux needs its own host decision.
