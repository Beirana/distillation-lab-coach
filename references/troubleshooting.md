# Troubleshooting and Classroom Fallbacks

Identify the current SSOT-aligned stage, the last command actually run, its exit code, and artifacts already present. Preserve the first failure log before proposing a rerun.

## Triage table

| Symptom | Inspect first | Smallest safe action | Live-class cutoff |
|---|---|---|---|
| SSH still asks for a password | Host alias, selected identity, `BatchMode=yes` result, unlocked key | Let the human repair authentication or use an already verified shared terminal; never request the secret | 2 minutes |
| Host key is new or changed | Platform hostname, port, and trusted SHA256 fingerprint | Stop for human verification; never disable host checking or erase `known_hosts` broadly | Immediate |
| `verify_bundle.py` fails | Expected package version, Git diff, missing file, source | Hold selection of the affected new code, not the whole runtime; separate known documentation edits from unknown executable changes. Never auto-rehash to hide drift or reinstall the image | Before using affected code |
| Teacher/student differ from the reference pair | `configs/course.json`, model roles, revisions, template, tokenizer logic | Decide whether this is reference reproduction or an explicitly accepted adapted run; stop only when roles or compatibility are unresolved | Immediate |
| stock `preflight` rejects a different but planned profile | Visible GPUs, BF16 or chosen precision, memory, actual models and stage | Preserve the result, evaluate the actual stage capability as in validation-philosophy.md; do not convert reference-profile drift into a whole-image failure | Before affected GPU work |
| Non-interactive SSH reports `python: command not found` or false CUDA failures | `command -v python`, `/root/miniconda3/bin/python`, login-shell PATH | Use the verified absolute interpreter or explicitly set the intended PATH; rerun the failed check before diagnosing CUDA | Immediate |
| Model/cache link exists but download raises FileNotFoundError | Resolved symlink target and actual data-disk mount | Confirm the intended destination, create only missing task directories, preserve the link and existing files | Before download |
| Merged tokenizer warns about regex or template behavior | Actual base/merged tokenizer outputs for the current prompts, warning text and configs | Report the tested scope: matching dev5 input token IDs does not prove all tokenization/decoding unaffected; preserve warning and investigate before broader claims | Before affected comparison claims |
| Models have not been downloaded yet | Existing verified cache, resolved destination, official course downloader | Default to official download, record timing, overlap with video/file teaching; assess parallel backup only for persistent problems or user request | Candidate route checkpoint at minute 30 |
| A supposedly complete model is damaged or unexpectedly incomplete | Manifest, tokenizer/config load, cache links, free space, download log | Preserve evidence; resume/repair only the affected task, or use prepared evidence while investigating | Triage within 1 minute, then offer fallback |
| Model files exist but course registration is missing | `course.py --help`, role manifest, trusted source manifest, hashes | If available, use local `register-models`; otherwise use an available reviewed compatible bundle or prepared host. Do not rerun an official downloader merely to manufacture registration | Before GPU work |
| Official client starts processing externally downloaded files again | `local_dir`, hidden client index/temp files, source manifest and verification receipt | Stop before duplicate transfer grows; preserve logs and use course registration after byte/semantic verification | Before class |
| ModelScope source manifest contains different `Revision` values per file | Top-level source revision, every download URL, `git ls-remote` result | Regenerate with one repository commit; never promote the first file's history revision to snapshot identity | Immediate |
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
- Leave the class waiting idly for downloads or run unrequested performance sweeps/reinstallations. Planned background transfer with video/file teaching is allowed; do not stop it merely because it is class time.
