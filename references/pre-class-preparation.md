# Pre-class preparation

Use this route when the user has not prepared a device, Agent, or compute environment. Do not assume a preinstalled skill, a Windows client, a GPU, an SSH alias, or a paid instance. Ask only the next material choice, not a long mandatory questionnaire.

## Control computer versus compute environment

Default classroom route: a Windows/macOS/Linux control computer and the verified remote GPU image. The control computer needs a browser and usable terminal/SSH client, not CUDA, Conda, or local model weights. A text-only Agent can still explain while the user executes commands; terminal access is needed for delegation, not for all participation.

Optional local route: inference, training and export run on the user's own computer. Read the appropriate Windows, macOS or Linux section of [local-compute.md](local-compute.md), plus the two-role profiles in [environments.md](environments.md). No loopback SSH or local SSH server is required. Local GPU setup is pre-class/post-class exploration until verified; do not promise a clean installation and the lesson fit in 60 minutes.

The Agent's conversational model is separate from the experimental teacher/student models. Local computation does not establish that the Agent itself is offline or free of usage limits.

## A. Agent and course access

1. Obtain the complete Coach from its actual distribution source; no preinstalled local copy is assumed. The current source is [Beirana/distillation-lab-coach](https://github.com/Beirana/distillation-lab-coach), a private repository requiring authorization. Do not claim public access or solicit shared credentials. If inaccessible, ask the instructor for access or an approved complete package, not repeated login attempts.
2. Select a new task and user-chosen experiment directory. Verify that the host can read `SKILL.md` and one linked reference. For WorkBuddy, use [workbuddy-onboarding.md](workbuddy-onboarding.md). Read [compatibility.md](compatibility.md) for other hosts.
3. Record the downloaded commit or package digest, actual instruction path and host version. A directory on disk is not proof of automatic discovery; explicit file loading is distinct from installed-skill discovery.
4. Inspect the actual course package. The course repository is [Beirana/distill-course](https://github.com/Beirana/distill-course); the skill is not the GPU runtime and does not contain model weights. Follow [repository-sync.md](repository-sync.md) only when code capabilities require a compatible refresh, preserving the image's environment.

When doing a fresh-host test, do not restore previous logs, memory or experimental artifacts into the new directory. Historical files that the user provides remain historical evidence, not current readiness.

## B. Remote preparation

- Confirm the selected image identifier, current host, port, user, trust-verification channel, disk persistence and budget. GPU names are not an ordered compatibility test; check actual resources against the selected model/profile. Do not create/start paid instances or change billing settings without authorization.
- Prefer passwordless SSH unless the user chooses another route. Use [ssh-onboarding.md](ssh-onboarding.md) for the actual client OS and current host. Human terminal success and Agent terminal success are separate observations.
- Check generation/evaluation and training/export capabilities separately using [environments.md](environments.md) and [validation-philosophy.md](validation-philosophy.md). Do not reinstall a working image to match table entries.
- Verify/reuse complete models and available data. Missing models follow [download planning](optional-download-acceleration.md): official downloader first, acceleration optional. State download time separately; preparation-only requests do not automatically authorize large downloads or a smoke run.

## C. Local preparation

Agree whether this is read-only feasibility, an isolated installation, or a full adaptation test. Inspect the actual OS/architecture, backend, memory, storage, existing environments and permitted changes. Present a small plan identifying the generation and training interpreters/backends, assets, code adaptations, verification and fallback. Choose environment managers freely within that plan; record package/source identities.

Use real code capabilities, not just package imports. Windows/WSL, macOS MPS/MLX and alternate Linux backends remain course adaptations until tested. Changing device checks without changing incompatible execution code is not a repair. Hardware too small for the reference profile does not preclude a separately agreed adapted experiment.

## D. Student readiness and classroom handoff

Report scoped statuses such as `ready`, `not checked`, `needs human action`, and `blocked for this stage`. Minimum useful handoff:

- chosen remote/local/hybrid route, real control terminal and compute environment;
- host/skill/course identity and actual paths, with no credentials;
- user-operated or delegated mode, connection result if relevant;
- generation and training capabilities checked separately; model/data state;
- outstanding setup, timing/budget and fallback;
- links to files/videos that actually exist, not assumed private materials.

Then ask whether to begin the classroom workflow in [stages.md](stages.md), continue preparation, or stop. Preserve proactive teaching during downloads and smoke transitions; deeper explanation is optional, not a prerequisite exam. Never infer comprehension or completion from an Agent's own explanation.

For managed/shared computers, respect installation and network policies, do not share long-lived private keys, and do not require administrator rights by default. Browser/Jupyter or approved group work can be a fallback. Save evidence and arrange credential revocation/logout before releasing temporary accounts or machines.
