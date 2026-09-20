# Experiment Workflow Stages

Use the same workflow for every user. Human roles do not create separate experiment paths. In this document, **teacher** and **student** name the two model roles only.

The authoritative stage numbers are 0 through 14. Keep these numbers when explaining progress or diagnosing a failure. The 60-minute class selects a subset of the same stages; it is not a second state machine.

Commands below use overridable variables instead of literal placeholder syntax. Before running them, inspect the checked-out repository and adapt paths to the current host rather than copying remembered values.

Each new non-interactive SSH command starts a new shell: a PATH export in an earlier SSH command does not configure the next one. Use the discovered absolute interpreter or repeat a verified environment prefix for every remote call. For multiline Python from Windows, prefer a PowerShell single-quoted here-string piped to the remote interpreter over deeply nested shell quotes. Check critical steps individually; the last command's success cannot clear an earlier failure.

```bash
export COURSE_REPO="${COURSE_REPO:-/root/distill-course}"
export COURSE_DATA_ROOT="${COURSE_DATA_ROOT:-/root/distill-work}"
cd "$COURSE_REPO"
```

For every executed stage, report:

1. what the command reads;
2. what it writes or starts;
3. the observed output and exit state;
4. the smallest artifact that proves completion;
5. what the evidence still cannot prove.

## Evidence boundaries that apply to every route

- A smoke run uses eight retained demonstrations, dev5, and two training steps. It verifies the engineering chain and does not establish capability improvement.
- A prepared or archived artifact must be identified as prepared or archived. Never describe it as an artifact produced by the current user or host.
- A dual-service conversation is qualitative evidence. Record formatting and numeric correctness separately; do not replace fixed-protocol evaluation with a hand-picked prompt.
- Final test results are valid only after the formal run is frozen. Never use the test split during the classroom smoke route.
- The reference pair is Qwen2.5-7B-Instruct and Qwen2.5-0.5B-Instruct. Another larger-teacher and smaller-student pair may be used as an adapted run after the model IDs, revisions, template, precision, resource checks, and code changes are reviewed and recorded.
- The stock preflight encodes the unquantized Qwen reference assumptions, including one visible GPU, BF16, and at least 22 GiB. A different verified model or precision route needs an equivalent gate, not a GPU model whitelist.

## The 60-minute classroom selection

Prepare the repository, environments, and access before class where possible. Recommend passwordless SSH for remote GPU work unless the user chooses otherwise; reuse a verified alias, and read [ssh-onboarding.md](ssh-onboarding.md) only if setup is needed. Read [environments.md](environments.md) for the two Python environments. The table below is the models-ready route; a prepared formal merged model is needed only for that particular qualitative demo.

| Time | Authoritative stages used | Live outcome |
|---|---|---|
| 0 to 5 minutes | 0 and 1 | Confirm connection, repository identity, GPU, and reference or adapted gate |
| 5 to 9 minutes | 5 | Establish the teacher-text-distillation and LoRA mental model |
| 9 to 16 minutes | 6 | Generate and inspect eight smoke demonstrations |
| 16 to 21 minutes | 7 | Record the original student's dev5 baseline |
| 21 to 35 minutes | 8 | Inspect configuration, train for two steps, and export the merged model |
| 35 to 41 minutes | 8 | Evaluate the smoke student on the same dev5 questions |
| 41 to 51 minutes | 12 | Compare the base student with a pre-verified formal-500 merged student |
| 51 to 56 minutes | 11, read-only evidence only | Interpret verified formal dev100 evidence without running the formal stage |
| 56 to 60 minutes | 12 cleanup and the cleanup checks from 14 | Stop classroom services, check GPU state, and assign the formal post-class route |

If weights are missing, default to the course's official downloader (stage 3), start it early, and overlap stages 4 and 5 plus the [single-sample file walkthrough](single-sample-walkthrough.md). Parallel acceleration is a backup for persistent trouble or an explicit request, not a prerequisite. A candidate schedule is 0–5 connection/start download; 5–25 video and Jupyter artifacts; 25–30 verify/register if complete; 30–48 live smoke chain; 48–56 qualitative or saved comparisons; 56–60 recap and cleanup. This is not a measured 60-minute guarantee. At minute 30, if required weights are still unavailable, offer prepared evidence/shared host and keep unfinished work explicit. Do not delete/restart a healthy transfer merely for pacing.

Executing stages 9–11, 13, and formal archiving belongs to post-class work, although a fast group may start stage 9. Reading formal results does not mean executing that formal run.

Use three soft learning checks during the classroom route:

1. after stage 6, ask which audit fields show whether a teacher answer entered training;
2. before training in stage 8, ask whether frozen base weights still participate in the forward pass;
3. after stage 8 or 12, ask why a completed smoke chain or one conversation does not prove capability improvement.

Proactively offer the relevant artifact explanation even when no question was asked. Accept a field pointer, a binary choice, or one plain sentence. In guided mode give the user room to answer or choose a skip; provide a hint when needed. In delegated continuous mode explain briefly and continue, recording Agent explanation rather than learner understanding. These checks are feedback, not grades, and skipping them is not a technical failure.

## Stage 0: Confirm repository and run identity

Purpose: bind code, configuration, and later evidence to one checked-out version.

```bash
cd "$COURSE_REPO"
if test -d .git; then
  git remote -v
  git rev-parse HEAD
  git status --short
else
  sha256sum bundle-manifest.json
fi
python scripts/verify_bundle.py
python -m json.tool configs/course.json
```

`git rev-parse HEAD` records the version when `.git` exists. An image may omit `.git`; record its `bundle-manifest.json` hash and successful bundle verification instead of declaring it broken. Non-interactive SSH may need the discovered absolute Python path (the rehearsal used `/root/miniconda3/bin/python`). Bundle verification does not prove dependencies, models, GPU, or data ready.

Inspect existing runs before choosing a new ID:

```bash
find "$COURSE_DATA_ROOT/runs" -mindepth 1 -maxdepth 1 -type d -printf '%f\n' 2>/dev/null | sort
```

Do not delete or reuse a run merely to obtain a cleaner demonstration.

## Stage 1: Connect and complete host preflight

Purpose: establish that the terminal is on the intended machine and that the selected route has usable GPU, disk, environment, and process state.

```bash
hostname
nvidia-smi
df -h
cd "$COURSE_REPO"
bash scripts/check_host.sh
python scripts/course.py preflight
```

Keep the generated preflight record. Interpret every warning or failure against the selected route. An online-source failure does not invalidate an already verified local model or prepared dataset. A CUDA or model-load failure does block the corresponding GPU stage.

If connection is not working within two minutes, switch to a working group, a prepared shared host, or the platform terminal. Do not ask for a private key, password, or unverified host-key acceptance.

## Stage 2: Prepare storage and the two software environments

Purpose: make model, cache, data, logs, training, and export locations explicit without rebuilding a working image during class.

First inspect, then plan:

```bash
ls -ld "$COURSE_DATA_ROOT" "$COURSE_DATA_ROOT/models" "$COURSE_DATA_ROOT/cache" 2>/dev/null || true
python scripts/hello.py
```

Resolve symlink targets, not just link existence. A fresh instance may retain model/cache links while the data-disk target directories are absent. Confirm the intended mounted target and create only the missing task directories before downloading; do not replace the links, migrate data, or reinstall software by default.

For a working fixed image, do not run dependency planning/installers merely because course code moved. Check actual imports and stage capability as in [environments.md](environments.md). Only when a missing capability warrants repair, review an installation plan and the intended interpreter before `--apply`. If a separate training environment is used, set it only after verifying the executable:

```bash
export COURSE_TRAIN_ENV="${COURSE_TRAIN_ENV:-/root/train-env}"
test -x "$COURSE_TRAIN_ENV/bin/llamafactory-cli"
```

For a validated single-environment route, leave `COURSE_TRAIN_ENV` unset and verify that `llamafactory-cli` resolves in the active environment. Read [environments.md](environments.md) for the verified reference profiles and the distinction between candidate requirements and archived freezes.

## Stage 3: Obtain and verify the two models

Purpose: obtain one larger teacher and one smaller student with traceable source and integrity evidence. Follow [download planning](optional-download-acceleration.md): reuse verified complete files; otherwise default to the course's official ModelScope route. Parallel transfer is optional, reserved for persistent download problems or a user request. Record timing and source. Inspect `--help` before using version-dependent commands; after confirming weights are genuinely missing, use:

```bash
cd "$COURSE_REPO"
python scripts/course.py download-models
```

Verify model directories, source/revision, inventory, configuration, and tokenizer loading. The official route downloads teacher and student sequentially; its client may parallelize files internally. Only if `--help` lists it, run `python scripts/course.py model-status`; the old image does not have this new local interface. Otherwise inspect the existing course manifests and perform local config/tokenizer/load checks, without repeating downloads. Hashing is not a once-per-second progress monitor.

When a trusted source supplied files, verify locally instead of calling `modelscope download` as a supposed validator. On a tested course version containing `register-models`, use actual source-manifest paths (set the variables first):

```bash
python scripts/course.py register-models \
  --teacher-source-manifest "$TEACHER_SOURCE_MANIFEST" \
  --student-source-manifest "$STUDENT_SOURCE_MANIFEST"
python scripts/course.py model-status
```

The provisional ModelScope route completed one transfer and later pinned-snapshot revalidation. The old image lacks this registration command; use [repository-sync.md](repository-sync.md), not fabricated manifests or repeated official downloads. HF non-LFS metadata may lack source SHA256 required by strict registration; disclose and resolve that provenance gap as described in download planning.

If verified models already exist in an image, cache, offline package, or persistent disk, reuse them and record their origin. Do not start a large unverified download merely because the default online check failed.

## Stage 4: Prepare data and verify split isolation

Purpose: create or import candidates, dev, and final test data while preserving source, counts, hashes, and split boundaries.

Use one of these routes after inspecting the current data root.

Prepared course material:

```bash
test -e "$COURSE_DATA_ROOT/data/manifest.json" || python scripts/use_materials.py prepared
```

Online preparation, only when the selected source and network route are intentional:

```bash
export HF_ENDPOINT="https://hf-mirror.com"
python scripts/course.py prepare-data
unset HF_ENDPOINT
```

Verify the selection report and manifest. Candidates feed teacher generation, dev supports development comparison, and test remains unopened until stage 13. Importing prepared material is valid, but it proves reuse of verified data rather than local re-download and re-selection.

## Stage 5: Establish the conceptual map

Purpose: connect the directory artifacts to the learning story before GPU execution.

Use one retained example, one rejected example when available, the generation audit, and the actual LoRA configuration to explain:

```text
candidate question and gold answer
        -> teacher-generated text
        -> parse, termination, length, format, and answer checks
        -> retained supervised example
        -> student LoRA SFT
        -> adapter plus matching base
        -> merged reloadable student
        -> same-protocol before, student, and teacher evaluation
```

The reference LoRA branch can be summarized as `y = Wx + (alpha / r) BAx`. The frozen base matrix `W` still participates in the forward pass; the low-rank `A` and `B` branch contains the trainable increment. LoRA describes how parameters are updated. Distillation describes where the supervision came from.

## Stage 6: Generate and audit smoke demonstrations

Purpose: verify teacher loading, prompting, generation, filtering, and data persistence with a new smoke run.

Choose a unique ID and keep it for the whole smoke chain:

```bash
export RUN_ID="smoke-group01-0920"
export RUN_DIR="$COURSE_DATA_ROOT/runs/$RUN_ID"
if test -e "$RUN_DIR"; then
  printf 'Run already exists; choose a new RUN_ID: %s\n' "$RUN_DIR" >&2
else
  python scripts/run_stage.py generate --run "$RUN_ID" --mode smoke
fi
```

The first two lines define the run and artifact directory. The conditional is a no-overwrite gate and cannot fall through to generation when the directory exists. `generate --mode smoke` asks the teacher model to retain eight accepted demonstrations.

Verify completion and inspect real data:

```bash
test -f "$RUN_DIR/generation_complete.json"
test -f "$RUN_DIR/generation_audit.jsonl"
test -f "$RUN_DIR/dataset/train.json"
python -m json.tool "$RUN_DIR/generation_complete.json"
python -m json.tool "$RUN_DIR/request.json"
```

Confirm that the training file contains eight retained demonstrations. Inspect all eight in a smoke class when time permits. If the current run contains a rejection record, inspect one. If it contains none, use a rejection record from a clearly identified archive and say that it is archived evidence. `rejection_reasons` being empty is necessary evidence that a row was retained; it does not prove that every reasoning step is correct.

If generation exceeds three minutes without explainable progress, preserve the log and current run, then switch to identified smoke archive evidence. Do not claim the archived generation occurred on the current host.

## Stage 7: Record the smoke before baseline

Purpose: capture the original student's behavior on dev5 before training.

```bash
python scripts/run_stage.py eval --run "$RUN_ID" --model before --split dev
test -f "$RUN_DIR/eval_dev/before_predictions.jsonl"
python -m json.tool "$RUN_DIR/eval_dev/before_summary.json"
```

The predictions explain individual outcomes; the summary aggregates them. Five examples are enough to verify the path and compare the same questions later, but not to estimate general capability.

## Stage 8: Inspect configuration, train, export, and compare smoke

Purpose: complete the minimal LoRA and export chain, then reload the current run's merged student for dev5.

Create and inspect the actual configurations:

```bash
python scripts/course.py make-config --run "$RUN_ID"
python -m json.tool "$RUN_DIR/train_config.yaml"
python -m json.tool "$RUN_DIR/export_config.yaml"
```

The tested course generator writes JSON syntax into these `.yaml` files, and JSON is valid YAML, so `json.tool` both formats and validates the reference output. If a later checkout emits ordinary YAML syntax, inspect it with a YAML-aware parser or a read-only text viewer instead of treating the `json.tool` failure as a training failure.

Before training, inspect the student model, dataset path, output path, template, LoRA rank and alpha, target modules, cutoff length, precision, and step count. The reference smoke configuration uses rank 8, alpha 16, target `all`, and two training steps. Stop if a path or mode is wrong. An intentional adaptation must be recorded rather than silently normalized back to the reference.

Train and verify the incremental artifact:

```bash
python scripts/course.py train --run "$RUN_ID"
test -f "$RUN_DIR/train_complete.json"
test -f "$RUN_DIR/adapter/adapter_config.json" || test -f "$RUN_DIR/adapter-lite/adapter_config.json"
python -m json.tool "$RUN_DIR/train_complete.json"
```

Export and verify the reloadable artifact:

```bash
python scripts/course.py export --run "$RUN_ID"
test -f "$RUN_DIR/export_complete.json"
test -f "$RUN_DIR/merged_manifest.json"
python -m json.tool "$RUN_DIR/export_complete.json"
```

Evaluate the same dev5 slice after training:

```bash
python scripts/run_stage.py eval --run "$RUN_ID" --model student --split dev
test -f "$RUN_DIR/eval_dev/student_predictions.jsonl"
python -m json.tool "$RUN_DIR/eval_dev/student_summary.json"
```

Compare `before` and `student` predictions under the same run, split, prompts, and decoding rules. The adapter is a small increment that depends on its matching student base. The merged directory is intended to be independently reloadable. Decreasing loss or a completed export does not prove better answers.

The archived `smoke01` student scored 0 accuracy and 0 format rate on dev5 while completing the engineering chain. Use that fact to reinforce the evidence boundary, not as a target for current groups.

## Stage 9: Generate or import the formal 500 demonstrations

Purpose: establish a reviewed demonstration set suitable for a formal run. Use a new run rather than extending the classroom smoke run.

```bash
export FORMAL_RUN="formal-group01-0920"
export FORMAL_DIR="$COURSE_DATA_ROOT/runs/$FORMAL_RUN"
if test -e "$FORMAL_DIR"; then
  printf 'Run already exists; choose a new FORMAL_RUN: %s\n' "$FORMAL_DIR" >&2
else
  printf 'New formal run is available: %s\n' "$FORMAL_DIR"
fi
```

Choose exactly one entry route.

Generate locally:

```bash
if test -e "$FORMAL_DIR"; then
  printf 'Refusing to overwrite existing run: %s\n' "$FORMAL_DIR" >&2
else
  python scripts/run_stage.py generate --run "$FORMAL_RUN" --mode pilot500
fi
```

Or import the verified course material:

```bash
if test -e "$FORMAL_DIR"; then
  printf 'Refusing to overwrite existing run: %s\n' "$FORMAL_DIR" >&2
else
  test -e "$COURSE_DATA_ROOT/data/manifest.json" || python scripts/use_materials.py prepared
  python scripts/use_materials.py teacher500 --run "$FORMAL_RUN"
fi
```

Offer to inspect the audit and retained demonstrations (ten is a suggestion, not a requirement). Ask whether to inspect together or continue; the user may delegate. Importing `teacher500` proves reuse, not generation on this host.

## Stage 10: Complete formal before, configuration, training, and export

Purpose: establish the dev100 baseline and produce the formal adapter and merged student.

```bash
python scripts/course.py make-config --run "$FORMAL_RUN"
python scripts/run_stage.py eval --run "$FORMAL_RUN" --model before --split dev
python -m json.tool "$FORMAL_DIR/train_config.yaml"
python -m json.tool "$FORMAL_DIR/export_config.yaml"
```

Ask whether the user has seen the configuration and before summary, and offer an explanation. Check paths and compatibility; an actual mismatch needs correction, while skipping a learning question does not block training. The reference uses 500 demonstrations, rank 8, alpha 16, target `all`, three epochs, effective batch 16. Preserve adapted configurations and label adapted results.

When the user chooses to continue and technical prerequisites hold, train and verify:

```bash
python scripts/course.py train --run "$FORMAL_RUN"
test -f "$FORMAL_DIR/train_complete.json"
test -f "$FORMAL_DIR/adapter/adapter_config.json" || test -f "$FORMAL_DIR/adapter-lite/adapter_config.json"
python -m json.tool "$FORMAL_DIR/train_complete.json"
```

Then export and verify the reloadable model in a separate step:

```bash
python scripts/course.py export --run "$FORMAL_RUN"
test -f "$FORMAL_DIR/export_complete.json"
test -f "$FORMAL_DIR/merged_manifest.json"
test -f "$FORMAL_DIR/merged/config.json"
python -m json.tool "$FORMAL_DIR/export_complete.json"
```

## Stage 11: Complete formal dev comparison and report

Purpose: compare the original student, formal merged student, and teacher model under the same dev100 protocol.

```bash
python scripts/run_stage.py eval --run "$FORMAL_RUN" --model student --split dev
python scripts/run_stage.py eval --run "$FORMAL_RUN" --model teacher --split dev
python scripts/course.py report --run "$FORMAL_RUN"
```

Verify each summary's model role, split, sample count, correct count, accuracy, format rate, normal-stop count, fingerprint, and decoding settings. Never fill a missing side with numbers from another run.

When the user or course provider supplies the verified `instruct500-new-01` evidence package, it provides the following classroom evidence:

| Model role | Accuracy | Format rate | Normal stops |
|---|---|---|---|
| before | 4% | 4% | 99 of 100 |
| student | 62% | 96% | 99 of 100 |
| teacher | 90% | 93% | 100 of 100 |

Identify these as archived formal results. They show that the formal student followed the target format much more often under that protocol. They do not make formatting equivalent to correctness, and they do not make the teacher an infallible gold source. A standalone public Skill installation does not include this evidence package; when the summaries or their provenance are unavailable, report the missing evidence and do not quote the table as locally verified fact.

## Stage 12: Run the optional dual-service qualitative comparison

Purpose: let the user compare the original student and a pre-verified formal merged student on the exact same non-test prompt.

For the 60-minute class, use a formal-500 merged model that was verified before class. The just-exported smoke merged model may be reloaded as an engineering check, but it must not carry the promise of a visible improvement.

Before calling this a before/after comparison, confirm that the formal merged model was produced from the same student base and a compatible tokenizer and chat template. If it was not, label the activity as a cross-model demonstration rather than evidence of the course intervention.

For a run-based prepared model, inspect its `request.json`, `export_config.yaml`, and `merged_manifest.json` and compare the recorded student identity and revision with the current base. For a directly supplied model directory, require the course provider or user to identify an equivalent provenance directory. If that provenance is missing, the services may still be shown as two named models, but not as a verified before/after pair.

Run the following preliminary configuration/provenance check from any free terminal. It compares labels, not historical base bytes: matching `master` or another mutable branch is insufficient to claim a verified before/after pair. A direct-directory handoff must set `DEMO_PROVENANCE_DIR`; a run-based handoff can derive it from `DEMO_RUN`.

```bash
if test -z "${DEMO_PROVENANCE_DIR:-}" && test -n "${DEMO_RUN:-}"; then
  export DEMO_PROVENANCE_DIR="$COURSE_DATA_ROOT/runs/$DEMO_RUN"
fi
if test -n "${DEMO_PROVENANCE_DIR:-}" \
  && test -f "$DEMO_PROVENANCE_DIR/request.json" \
  && test -f "$DEMO_PROVENANCE_DIR/export_config.yaml" \
  && test -f "$DEMO_PROVENANCE_DIR/merged_manifest.json"; then
  if python -c 'import json,sys; c=json.load(open(sys.argv[1],encoding="utf-8")); r=json.load(open(sys.argv[2],encoding="utf-8"))["config"]; keys=("student","modelscope_revision"); a=tuple(c.get(k) for k in keys); b=tuple(r.get(k) for k in keys); print("current student:",a); print("prepared student:",b); raise SystemExit(a != b)' "$COURSE_REPO/configs/course.json" "$DEMO_PROVENANCE_DIR/request.json"; then
    python -m json.tool "$DEMO_PROVENANCE_DIR/export_config.yaml" \
      && python -m json.tool "$DEMO_PROVENANCE_DIR/merged_manifest.json"
  else
    printf 'Prepared model identity or revision does not match the current course configuration.\n' >&2
    false
  fi
else
  printf 'Verified before/after provenance is incomplete: %s\n' "${DEMO_PROVENANCE_DIR:-<unset>}" >&2
  false
fi
```

The preliminary comparison returns nonzero when the current course `student` or `modelscope_revision` differs from the prepared request. Also inspect the export base path, template and merged manifest. A strict before/after claim additionally needs a matching immutable base snapshot or historical base-file hashes tied to the formal run and checked against the current base. A matching path, `master` label or merged manifest alone cannot establish that. If the extra evidence is unavailable, explain the limitation and offer the two identified models as a qualitative demonstration or use saved predictions; do not block unrelated smoke work or fabricate provenance.

The `0.25` memory fractions, `2048` context length, `bfloat16` precision, and example ports below are reference values for the tested Qwen setup. Replace them only with values already validated for the current models and host; do not discover serving limits during the timed class.

Terminal A, original student:

```bash
export COURSE_DATA_ROOT="${COURSE_DATA_ROOT:-/root/distill-work}"
export BASE_MODEL_PATH="${BASE_MODEL_PATH:-$COURSE_DATA_ROOT/models/student}"
export BASE_PORT="${BASE_PORT:-8001}"
export BASE_GPU_UTIL="${BASE_GPU_UTIL:-0.25}"
export SERVE_MAX_LEN="${SERVE_MAX_LEN:-2048}"
export SERVE_DTYPE="${SERVE_DTYPE:-bfloat16}"
if test -f "$BASE_MODEL_PATH/config.json"; then
  vllm serve "$BASE_MODEL_PATH" \
    --host 127.0.0.1 \
    --port "$BASE_PORT" \
    --served-model-name base \
    --gpu-memory-utilization "$BASE_GPU_UTIL" \
    --max-model-len "$SERVE_MAX_LEN" \
    --dtype "$SERVE_DTYPE" \
    --enforce-eager
else
  printf 'Base model config is missing: %s\n' "$BASE_MODEL_PATH/config.json" >&2
  false
fi
```

Terminal B, formal merged student:

```bash
export COURSE_DATA_ROOT="${COURSE_DATA_ROOT:-/root/distill-work}"
if test -z "${DEMO_MODEL_PATH:-}"; then
  if test -n "${DEMO_RUN:-}"; then
    export DEMO_MODEL_PATH="$COURSE_DATA_ROOT/runs/$DEMO_RUN/merged"
  fi
fi
export DISTILLED_PORT="${DISTILLED_PORT:-8002}"
export DISTILLED_GPU_UTIL="${DISTILLED_GPU_UTIL:-0.25}"
export SERVE_MAX_LEN="${SERVE_MAX_LEN:-2048}"
export SERVE_DTYPE="${SERVE_DTYPE:-bfloat16}"
if test -n "${DEMO_MODEL_PATH:-}" && test -f "$DEMO_MODEL_PATH/config.json"; then
  vllm serve "$DEMO_MODEL_PATH" \
    --host 127.0.0.1 \
    --port "$DISTILLED_PORT" \
    --served-model-name distilled \
    --gpu-memory-utilization "$DISTILLED_GPU_UTIL" \
    --max-model-len "$SERVE_MAX_LEN" \
    --dtype "$SERVE_DTYPE" \
    --enforce-eager
else
  printf 'Set DEMO_MODEL_PATH or DEMO_RUN to a verified formal merged model with config.json; resolved path: %s\n' "${DEMO_MODEL_PATH:-<unset>}" >&2
  false
fi
```

If two services do not fit, use a prepared shared host, load the models sequentially, or compare saved predictions.

From any free terminal, verify both endpoints before starting the clients:

```bash
curl -fsS "http://127.0.0.1:${BASE_PORT:-8001}/v1/models"
curl -fsS "http://127.0.0.1:${DISTILLED_PORT:-8002}/v1/models"
```

Confirm that the first response lists `base` and the second lists `distilled`; an HTTP success from the wrong endpoint is not sufficient.

Use two client terminals because the public `chat.py` connects to one service at a time.

Terminal C, original student client:

```bash
export COURSE_REPO="${COURSE_REPO:-/root/distill-course}"
export BASE_PORT="${BASE_PORT:-8001}"
python "$COURSE_REPO/chat.py" "${BASE_PORT:-8001}" base
```

Terminal D, formal merged student client:

```bash
export COURSE_REPO="${COURSE_REPO:-/root/distill-course}"
export DISTILLED_PORT="${DISTILLED_PORT:-8002}"
python "$COURSE_REPO/chat.py" "${DISTILLED_PORT:-8002}" distilled
```

The two clients receive the same prompt. The public repository does not contain a general `ask_both.py`; do not depend on an archived helper with hard-coded paths.

Record for each side:

- whether the reasoning is readable;
- whether the final line follows `Answer: <number>`;
- whether the final line parses;
- whether the number matches the gold answer;
- whether generation ends normally.

Stop the clients and both servers with `Ctrl+C`, then run `nvidia-smi`. Never use broad `killall` or name-wide `pkill` on a shared host.

## Stage 13: Freeze and run the formal final test

Purpose: lock the formal run before one-time final evaluation.

```bash
if python scripts/course.py freeze --run "$FORMAL_RUN"; then
  test -f "$FORMAL_DIR/frozen.json" && python -m json.tool "$FORMAL_DIR/frozen.json"
else
  printf 'Freeze failed; final test remains blocked for run %s\n' "$FORMAL_RUN" >&2
  false
fi
```

Verify the frozen record's run identity, code, data, configuration fingerprints, and formal dev/report prerequisites. Only then run the one-time final evaluation:

```bash
python scripts/run_stage.py eval --run "$FORMAL_RUN" --model before --split test
python scripts/run_stage.py eval --run "$FORMAL_RUN" --model student --split test
python scripts/run_stage.py eval --run "$FORMAL_RUN" --model teacher --split test
python scripts/course.py report --run "$FORMAL_RUN"
```

Verify `frozen.json`, all three test prediction and summary pairs, and the final report. Do not tune the same run after seeing test results. If one side is missing, disclose the gap rather than combining different runs.

## Stage 14: Archive, clean up, and define extensions

Purpose: leave a reviewable run and a clean host.

At minimum, preserve the commit, request, data manifest, audit, rendered configurations, logs, completion records, adapter or reconstruction record, merged manifest, predictions, summaries, report, environment record, and any protocol anomaly. Check only processes created by the current task and verify final GPU state:

```bash
nvidia-smi
git -C "$COURSE_REPO" rev-parse HEAD
printf '%s\n' "$RUN_DIR"
```

For a formal run, print `FORMAL_DIR` instead of `RUN_DIR`. Large weights may follow the course storage policy, but their manifest and reconstruction requirements must remain traceable.

The `distill-exercises` 10,000-example source is a research input, not a verified drop-in course chain. A post-class extension must create a new data version and run, review licensing and deduplication, define frozen train/dev/test splits, validate on a small subset, and only then connect the larger source to generation and training.

## Classroom cutoffs and fallbacks

| Condition | Action |
|---|---|
| Connection exceeds 2 minutes | Move to a working group, prepared shared host, or platform terminal |
| A required model is missing | Default to the official course downloader; use parallel acceleration only as a backup; overlap with file/video teaching and use prepared evidence if time runs out |
| Data preparation exceeds 3 minutes | Use verified prepared material and record its source |
| Smoke generation exceeds 3 minutes without explainable progress | Preserve logs and switch to identified smoke archive evidence |
| Training or export fails | Preserve the run and logs; explain the failure with archived artifacts without relabeling them |
| Less than 8 minutes remain | Start no new GPU stage; interpret evidence, clean up, and record unfinished work |
| Dual services do not fit | Use a shared demonstration, sequential loading, or static predictions |
| The Agent is unavailable | Follow the public handout and inspect the same artifacts manually |

After any fallback, state exactly what the current host completed, what prepared material was shown, and what remains for post-class work.
