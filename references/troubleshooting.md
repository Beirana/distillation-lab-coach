# Troubleshooting and Classroom Fallbacks

First identify the current stage, the last command actually run, its exit code, and existing artifacts. Preserve the original log; do not overwrite evidence by immediately rerunning the same operation.

| Symptom | Inspect first | Action | Classroom cutoff |
|---|---|---|---|
| SSH connection fails | Host alias, instance power state, platform address/port, first-connection fingerprint | Never request a private key or password; let the student repair the connection or pair with another group | 2 minutes |
| `verify_bundle` fails | Current Git state, missing files, repository source | Stop; do not continue in an unknown workspace; switch to the prepared teacher host | Immediate |
| Teacher or student configuration differs | `configs/course.json` and current Git commit | Report the difference and stop; do not silently change models | Immediate |
| Model directory is missing | `/root/distill-work/models/{teacher,student}` and data-disk links | Do not wait for a large download during class; use a prepared host and repair assets later | 1 minute |
| `Run ID exists` | Matching run directory and request | Preserve the old run and choose a new simple run ID | Immediate |
| Generation or evaluation exits with code 2 | Stage supervisor, log, and target artifact | Explain that process cleanup did not pass; do not call it fully successful merely because an artifact exists; do not reuse the same run ID | 3 minutes |
| GPU memory is occupied | `nvidia-smi`, current-task supervisor, launch command | Identify only processes created by the current task; do not broadly terminate other jobs | 2 minutes |
| `make-config` preflight fails | Generation completion record and data hash for the current run | Locate the missing prerequisite; never fabricate a completion file | 2 minutes |
| Training fails | `train.log`, environment prefix, configuration path, disk space | Preserve the log; do not reinstall the environment during class; switch to archived adapter/results | 3 minutes |
| Export fails | `export.log`, base model, adapter, disk space | Never present an adapter as a complete model; switch to an archived merged model | 2 minutes |
| Result differs from the teacher's display | Run, split, model role, commit, configuration, decoding settings | Determine whether results are comparable; record real differences instead of forcing the same number | 3 minutes |

## Diagnostic report format

Respond in the user's language using these fields:

```text
Stage:
Last command and exit code:
Valid artifacts already present:
Direct evidence:
Current uncertainty:
Smallest recommended repair:
Switch to prepared classroom material:
```

## Prohibited shortcuts

- Delete an existing run and rerun under the same name.
- Manually create `*_complete.json` files to bypass prerequisites.
- Treat a few predictions as an accuracy estimate.
- Tune the training configuration after looking at final-test results and then retest.
- Silently change models, data, or scoring rules to save time.
- Use `killall`, broad `pkill`, or an ambiguous process name to clean the whole host.
