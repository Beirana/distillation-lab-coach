# Environment and Adaptation

Read this reference when checking a host, explaining the two environments, or adapting the reference experiment to different models, GPUs, paths, or precision.

## Establish actual paths first

Use the checked-out course documentation and the current shell. The tested defaults are:

```bash
export COURSE_REPO=/root/distill-course
export COURSE_DATA_ROOT=/root/distill-work
export COURSE_TRAIN_ENV=/root/train-env
cd "$COURSE_REPO"
```

`COURSE_DATA_ROOT` is the logical course work root. In the tested AutoDL layout, `models/` and `cache/` may be symbolic links to a larger persistent disk while `data/`, `runs/`, and `environment/` stay under the work root. Do not call the entire work root a data disk.

Keep `COURSE_TRAIN_ENV` unset when the current Python environment is the verified training environment. Set it only when a real separate environment contains `llamafactory-cli`.

## Tested generation and evaluation profile

| Component | Tested value |
|---|---|
| Environment prefix | `/root/miniconda3` |
| Python | 3.12.3 |
| PyTorch | 2.8.0+cu128 |
| CUDA | 12.8 |
| vLLM | 0.11.0 |
| transformers | 4.57.6 |
| tokenizers | 0.22.2 |
| datasets | 4.0.0 |
| ModelScope | 1.26.0 |
| huggingface-hub | 0.36.2 |
| numpy | 2.2.6 |
| pandas | 3.0.5 |
| sentencepiece | 0.2.2 |
| safetensors | 0.8.0 |
| Triton | 3.4.0 |
| PyYAML | 6.0.2 |

The public `configs/requirements-gen.txt` is a candidate installation profile, not a complete environment lock.

## Tested training and export profile

| Component | Tested value |
|---|---|
| Environment prefix | `/root/train-env` |
| Construction | `venv --system-site-packages` using the base PyTorch |
| Python | 3.12.3 |
| LLaMA-Factory | 0.9.5 |
| Tested source commit | `7af909522a951e3ad9f022ea6f88b6755257eaa5` |
| transformers | 4.57.6 |
| datasets | 4.0.0 |
| accelerate | 1.11.0 |
| peft | 0.18.1 |
| trl | 0.24.0 |
| gradio | 5.50.0 |
| pandas | 2.3.3 |
| PyYAML | 6.0.2 |
| Tested compatibility pair | Starlette 0.52.1 and prometheus-fastapi-instrumentator 7.1.0 |

The public `configs/requirements-train.txt` is also a candidate profile. A full `pip freeze` from a verified run is stronger environment evidence. Do not assume a freeze file or `/root/train-env` is distributed with the public course repository.

## Readiness checks

For the fixed teaching image, apply [validation-philosophy.md](validation-philosophy.md). Check current capabilities, not exact agreement with the reference version table. A course-code refresh does not require dependency resolution or installation.

```bash
cd "$COURSE_REPO"
if test -d .git; then git rev-parse HEAD; else sha256sum bundle-manifest.json; fi
python scripts/verify_bundle.py
python -c 'import sys, torch, vllm; print(sys.executable, torch.__version__, vllm.__version__)'
python scripts/course.py preflight
```

Interpret these checks separately:

- `verify_bundle.py` checks managed course files against the bundle manifest.
- `setup_env.py gen` without `--apply` produces an installation plan, not a health check. Use it only when planning an actual repair, not after each course refresh.
- stock `preflight` records Python, PyTorch, CUDA, GPU, and disk information and checks one visible GPU, BF16, and at least 22 GiB for the unquantized Qwen reference profile.
- stock `preflight` does not prove that every model file, Python dependency, or training CLI is complete.

For a separate training environment, also check:

```bash
test -n "${COURSE_TRAIN_ENV:-}"
test -x "$COURSE_TRAIN_ENV/bin/python"
test -x "$COURSE_TRAIN_ENV/bin/llamafactory-cli"
"$COURSE_TRAIN_ENV/bin/python" -m pip check
```

If `COURSE_TRAIN_ENV` is intentionally unset, verify `llamafactory-cli` in the current environment instead.

Check `llamafactory`/`peft` imports with the training interpreter. A `pip check` warning about an unused UI package calls for scoped investigation, not an automatic reinstall. The training source may legitimately remain under the old image-bundled course's `vendor/` directory; keep it in place.

## Adapted models and hardware

The semantic minimum is one larger teacher and one smaller student. A different GPU model, vGPU, disk layout, or compatible software version is allowed after evidence-based validation.

Before accepting an adapted run, record:

1. teacher and student IDs, immutable revisions, and file manifests;
2. the prompt or chat template and tokenizer special tokens;
3. context length, precision or quantization, and actual load checks;
4. inference and training commands;
5. rendered LoRA configuration and resource parameters;
6. an equivalent host gate for the chosen combination;
7. a new data root or run identity when the result is not directly comparable to the reference profile.

The current course implementation contains `template: qwen` and Qwen-specific chat markers in its length calculation. A non-Qwen pair requires code and template adaptation, followed by a fresh smoke run. Never imply that editing only the model names is sufficient.

## Two-service resource check

Before class, verify that the base student and prepared formal merged model can coexist with the selected `gpu-memory-utilization` values and ports. Also verify that they share the base architecture, tokenizer, and chat template required for a genuine before/after comparison. If they do not share that lineage, label the result as a cross-model demonstration. If they cannot coexist:

- serve the two models sequentially;
- use one verified shared demonstration host; or
- compare verified static prediction files.

The qualitative demonstration is optional evidence. Do not weaken the formal evaluation protocol to make both services fit.
