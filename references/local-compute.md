# Local computation on Windows, macOS and Linux

Use only when the user wants inference and training on their own computer. The default class remains the verified remote image. These are adaptation plans, not tested installations. Read only the relevant OS branch and common contracts. No SSH-to-self, local SSH server, paid remote host, or mandatory environment manager is needed.

## Common contract and autonomy

Ask whether the user wants feasibility only, an isolated setup, or implementation/testing of an adapted experiment. Inspect OS/CPU architecture, actual GPU/backend, free memory/storage, permitted resource use, existing interpreters and working libraries. Inspect relevant paths only, not secrets or unrelated personal files. On shared machines, obey administrator and scheduler rules.

Generation/evaluation and training/export are two functional roles. One proven-compatible environment is acceptable; separate prefixes often help isolate dependencies. uv, venv, Conda and authorized existing containers are options, not requirements. Preserve existing working projects. The reference image's `venv --system-site-packages` arrangement is not a universal recipe.

Before installation, give one small plan: generation backend/interpreter, training backend/interpreter, assets/storage, required code adaptation, minimum test, time/cost boundary and fallback. Within agreed scope, decide ordinary checks, compatible candidate versions and isolated repairs without asking about every package. Ask again for driver/system changes, administrator rights, reboot, new expense, lengthy compilation, large extra downloads, shared-environment changes, code adaptation, or model/backend/precision/protocol changes. Do not use `sudo pip`, disable security tools, or silently install system services.

## Windows: computation on the same Windows machine

Inspect the real OS and CPU architecture, GPU vendor, memory, drivers and existing environments. When applicable, `Get-Command python`, `wsl --list --verbose`, and `nvidia-smi` are initial checks; missing commands mean unknown/missing capability, not permission to install.

### Option A: Windows Agent, same-machine WSL2 compute

- First reuse an available suitable distribution and confirm its GPU access. If WSL2 is absent, explain setup/reboot/disk implications and get agreement. `wsl --install` is not a read-only probe.
- Generation: choose a Linux Python environment with a compatible Torch/runtime/vLLM combination, then the tokenizer/data/course dependencies. Windows Python packages are not automatically installed in WSL.
- Training: use a compatible environment or separate prefix in the same distribution for LLaMA-Factory/PEFT. Check its device capability separately. Set `COURSE_TRAIN_ENV` to an actual Linux prefix, not a Windows path or Conda name.
- Verify the Agent's actual WSL command context, selected distribution, working directory and interpreter. A human's interactive WSL success does not prove Agent access. Explicitly set interpreter/environment for each separate invocation.
- Plan Windows/WSL path mapping and asset ownership. Consider a Linux filesystem for heavy I/O, with deliberate evidence export; do not move large existing assets or duplicate weights without agreement. Ensure generation and training see the same data and outputs.

The reference [vLLM 0.11.0 documentation](https://docs.vllm.ai/en/v0.11.0/getting_started/installation/gpu.html) points Windows users toward WSL rather than native support. Check the selected version, not an unrelated latest-version claim. For NVIDIA WSL, use the supported Windows driver arrangement; do not install a Linux display driver inside WSL. See [NVIDIA's WSL guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html).

### Option B: native Windows

Preserve an existing native Torch environment. If the user chooses native execution, investigate generation engine, training/export tools, subprocess handling and paths independently. The reference vLLM route is not promised to work natively. An alternative inference backend requires agreed course-interface adaptation; one library importing or generating text does not prove the entire course works. For AMD/Intel GPUs, inspect the actual supported backend instead of installing NVIDIA packages.

### Option C: an existing authorized container setup

Verify GPU access, actual available image, mounts, permissions and persistence. A container does not automatically solve host compatibility; do not assume an AutoDL community image is a publicly pullable Docker image. Installing Docker or changing virtualization is a separate decision.

Acceptance: exercise generation, training, export and reload in the selected compute context, not just inspect the Windows display adapter. Record the Windows-to-WSL/container/native invocation chain.

## macOS: computation on the Mac, not merely remote control

Identify Apple Silicon versus Intel, macOS version, Python architecture and memory pressure. `uname -m` and `sw_vers` are useful; do not collect serial numbers. Unified memory also serves the OS and other apps; reserve space for weights, KV cache, training activations, optimizer and merge/export. Total RAM is not all available model memory. Intel Mac and Apple Silicon package/device support must be checked separately.

### Option A: PyTorch/MPS and compatible Hugging Face tools

- Generation: inspect the selected Torch build and MPS availability, then evaluate Transformers or another supported engine for the actual models. Existing course vLLM/BF16 calls require adaptation, not just an installation change.
- Training: check the selected LLaMA-Factory/PEFT/trainer versions, device support, optimizer and required operators. If the existing CLI cannot be reused, propose an authorized adapted trainer. Prove real forward/backward execution, parameter update and adapter save before scaling the smoke run.
- Select a supported dtype; retain comparable before/after decoding and precision. Do not automatically enable CPU fallback to hide missing GPU operators. If hybrid execution is accepted, record it and its cost.
- Merge and reload on the intended backend. An exported file is not proof of compatibility with the original CUDA/vLLM evaluation path.

`torch.backends.mps.is_built()` and `torch.backends.mps.is_available()` distinguish build support and device availability; they are initial checks, not course acceptance. See [PyTorch MPS documentation](https://docs.pytorch.org/docs/main/notes/mps.html). Do not infer support from a CUDA-style check or import alone.

### Option B: MLX/MLX-LM on Apple Silicon

[MLX-LM](https://github.com/ml-explore/mlx-lm) offers local generation and fine-tuning. Check the exact model family/version rather than running its default example model. A compatible MLX environment may serve both functional roles; a CUDA/Torch installation is not automatically required for this route.

- Preserve teacher/student role mapping, original model revision, converted-weight identity, quantization and conversion provenance.
- Translate course data into the chosen trainer's format with traceable IDs and filtering. Check prompt masking, target layers, rank/scaling and optimization semantics rather than mechanically copying LLaMA-Factory YAML.
- Treat MLX adapters and fused weights as their actual format, not automatically PEFT adapters or Hugging Face merged models. Validate any export conversion separately.
- Keep course numerical/format evaluation. A framework's perplexity test is not the course's dev accuracy or format score.

Consult [MLX-LM LoRA/fusion/data documentation](https://github.com/ml-explore/mlx-lm/blob/main/mlx_lm/LORA.md) for current commands, schema and masking behavior. Do not publish/upload weights as a side effect of fusion.

### Course integration and acceptance for either option

List reusable dataset/filter/scoring code separately from engine, trainer, device checks and artifact validation requiring change. Implement only with authorization in an isolated adaptation checkout; preserve new run identity and real logs. Never manufacture the stock completion markers. A chat application that has not demonstrated training/export is at most an inference component.

First verify single-sample generation, then short training, adapter, merge/fusion and reload; compare before/student with one consistent protocol/backend. A later conversion to CUDA evaluation is separately validated. Intel/CPU fallback may be explored if supported, but do not promise classroom speed. Do not force a remote route merely because local adaptation is untested; offer its cost and fallback honestly.

## Linux: computation on the user's workstation

Use the local terminal; no root account or SSH service is required. Check distribution, CPU architecture, GPU vendor/runtime, actual free resources, selected interpreters and package availability. Linux alone is not a compatibility guarantee.

### NVIDIA

- Generation: retain working Torch/runtime where compatible; assess the vLLM binary/API combination and model support before installing. If changing working Torch is necessary, prefer a new isolated environment.
- Training: reuse or prepare a separate actual LLaMA-Factory/PEFT prefix, recording source commit, Python and CLI paths. Test training-device capability independently of generation.
- On multi-GPU hosts select an authorized suitable card for the reference run; do not automatically occupy all GPUs, introduce distributed training or stop unrelated work. Check x86_64/aarch64 wheels separately.
- Missing `nvcc` does not alone disprove use of prebuilt packages; `nvidia-smi` success does not alone prove Torch/operators work. Modify drivers only after scoped diagnosis and agreement.

### AMD, Intel and other accelerators

Check the actual hardware/OS/backend matrix for selected PyTorch, inference and training versions, including attention and quantization kernels when used. Some ROCm Torch interfaces retain `torch.cuda` names; inspect build/backend information and actual device instead of inferring NVIDIA from that API name. See [PyTorch installation guidance](https://pytorch.org/get-started/locally/).

The course's `nvidia-smi`, BF16 checks and fixed inference/training configuration still need review. Installing ROCm/XPU packages alone is not complete adaptation. Verify both generation and training; inference support does not establish export compatibility.

Containers and environment managers remain optional. Record container identity and persistent mounts if used. Follow shared-server allocation rules. Linux WorkBuddy desktop availability is a separate host concern; another approved host or human terminal execution can still use the course.

## Common code constraints and verification

At paired course commit `d5ae90bab7c59da1b6055dee838dafa91056bfbc`:

- `course.py preflight` checks Python, CUDA, one visible GPU, BF16 and at least 22 GiB, and calls `nvidia-smi`.
- `engine()` uses vLLM with `dtype="bfloat16"`; training/export prechecks also assert CUDA/BF16.
- `common.py` renders `template: qwen`, `bf16: true`, and `train_on_prompt: false`; course length handling includes Qwen-specific logic.
- `setup_env.py` assumes existing Torch/CUDA, does not create an environment, and writes reports even without `--apply`; a training plan may fetch vendor source. It is not a side-effect-free generic bootstrap/health probe.

Inspect the actual checkout because these constraints may evolve. Alternate precision, model or backend can require real code changes; never just delete checks or rewrite manifests to pass. Preserve differences in an isolated adaptation and give it a new run/data identity when not comparable.

Use [environments.md](environments.md) for reference dependency tables; candidate requirements files are not complete locks or system-driver installers. Check actual `sys.executable`/`sys.prefix`. `COURSE_TRAIN_ENV` must resolve the CLI and Python layout expected by the actual script. Separate containers/machines need additional invocation/mount/artifact-transfer adaptation, not merely this variable.

Verify only the stages in scope: imports/device/operators → model integrity and load → generation/filter audit → before dev → short LoRA → export/reload → student dev. Small step count does not eliminate teacher weight memory. Preserve exits and artifacts; neither lower loss nor one answer proves completion. Download only missing assets through the default official route; time downloads separately. Large source builds or unavailable drivers are reasons to discuss post-class continuation, not repeated blind reinstalls.

Keep two-service demonstration optional and resources scoped to this task. Missing prepared formal weights means sequential/static/omitted demonstration, not relabeling a smoke model as formal evidence. Local generation moved to cloud, or reuse of externally generated data, must be labeled hybrid/partial-local rather than full-local completion.

Record OS/architecture/backend, actual interpreters and dependencies, course/skill identities and code diff, model/conversion identities, protocol, stage exits/artifacts/timing, user choices, and remaining untested capabilities. Redact credentials in logs/environment exports. End with the next safe step; do not expand preparation to formal 500/10,000-example work.
