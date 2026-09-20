# Troubleshooting and Classroom Fallbacks

Identify the current SSOT-aligned stage, the last command actually run, its exit code, and artifacts already present. Preserve the first failure log before proposing a rerun.

## Triage table

| Symptom | Inspect first | Smallest safe action | Live-class cutoff |
|---|---|---|---|
| SSH still asks for a password | Host alias, selected identity, `BatchMode=yes` result, unlocked key | Let the human repair authentication or use an already verified shared terminal; never request the secret | 2 minutes |
| Host key is new or changed | Platform hostname, port, and trusted SHA256 fingerprint | Stop for human verification; never disable host checking or erase `known_hosts` broadly | Immediate |
| `verify_bundle.py` fails | Current commit, Git status, missing managed file, repository source | Stop using the unknown checkout; obtain a clean verified checkout or prepared host | Immediate |
| Teacher/student differ from the reference pair | `configs/course.json`, model roles, revisions, template, tokenizer logic | Decide whether this is reference reproduction or an explicitly accepted adapted run; stop only when roles or compatibility are unresolved | Immediate |
| stock `preflight` rejects a different but planned profile | Visible GPUs, BF16 or chosen precision, memory, actual models and stage | Keep the failure as evidence, define an equivalent gate for the adapted profile, and record why the stock 22 GiB Qwen gate does not apply | Before GPU work |
| Model directory is absent or incomplete | Model manifest, tokenizer/config load, cache links, free space | Use a verified preloaded model or prepared host in class; repair or resume the model outside class | 1 minute |
| Run ID already exists | Request file, run directory, current user intent | Preserve the old run and choose a new simple ID | Immediate |
| Generation or evaluation exits with code 2 | Supervisor record, prediction count, finish reasons, target artifact | Report the cleanup protocol failure; do not call it clean success or reuse the same run ID | 3 minutes |
| GPU memory is occupied | `nvidia-smi`, current-task supervisor, launch command | Identify only processes owned by the current task; use a prepared host or sequential service plan | 2 minutes |
| `make-config` fails | Generation completion record, data hash, run request | Locate the missing prerequisite; never fabricate a completion file | 2 minutes |
| `llamafactory-cli` is missing | `COURSE_TRAIN_ENV`, current PATH, selected single/separate environment route | Correct the environment pointer only after verifying the intended environment; do not reinstall during class | 2 minutes |
| Training fails | `train.log`, environment prefix, rendered config, disk space | Preserve the log and run; switch the lesson to verified artifacts while repairing later | 3 minutes |
| Training exits but adapter evidence is absent | Exit code, `train_complete.json`, `adapter_config.json` and weight files | Treat training as incomplete; do not start export | Immediate |
| Export fails | `export.log`, base model identity, adapter identity, disk space | Preserve the failure; never present the adapter as a standalone merged model | 2 minutes |
| Two services cannot coexist | Ports, base and merged model paths, memory budget, stale task-owned services | Serve sequentially, use a verified shared host, or compare static predictions | 2 minutes |
| Port is occupied | `ss -ltnp` and the owning process | Confirm ownership before selecting another verified port; never kill an ambiguous process | 1 minute |
| One qualitative answer shows no difference | Same prompt, system message, served model names, full outputs | Record the observation and move to verified prediction files and batch metrics; do not search for a flattering test example | 1 minute |
| Current smoke metrics do not improve | Sample count, two-step config, per-question predictions | Report the result honestly and use the formal run only as separately identified archived evidence | No retry for score |
| Final test is requested before freeze | Formal dev completion and `frozen.json` | Stop, complete the human freeze decision, then run test once | Immediate |
| Result differs from a displayed reference | Run, split, role, commit, config, decoding and fingerprint | Decide whether the results are comparable; preserve real differences instead of forcing the reference number | 3 minutes |

## Diagnostic response

Respond in the user's language with:

```text
Current stage:
Last command and exit code:
Valid artifacts already present:
Direct evidence:
Current uncertainty:
Smallest recommended repair:
Live-class fallback:
Claim boundary:
```

Do not bury the usable artifacts beneath the error. State whether the current run may safely continue, must switch to prepared evidence, or requires a new run ID.

## Prohibited shortcuts

- Delete an existing run and rerun under the same name.
- Manually create `*_complete.json` files.
- Change a model, dataset, template, seed, prompt, decoding option, score rule, or split without disclosure and acceptance.
- Tune after viewing final-test results and then retest the same formal run.
- Treat a few predictions, lower loss, or target-format compliance as a capability estimate.
- Call a process cleanly completed when its supervisor records forced cleanup.
- Use `killall`, broad `pkill`, or an ambiguous process name.
- Reinstall an environment, download a large model, or benchmark concurrency while the live class waits unless the user explicitly changes the session plan.
