# Artifact Map

For video-plus-Jupyter explanation, use [single-sample-walkthrough.md](single-sample-walkthrough.md): trace `source_id` through a real audit/train pair and distinguish current retention from an archived rejection.

When explaining an artifact, first identify its authoritative stage and run ID. Then state what it supports and what it cannot prove by itself. A file from an archive, prepared bundle, or another host must remain labeled as such.

## Contract, environment, and data

| Stage | File or directory | Meaning | Supports | Does not prove by itself |
|---|---|---|---|---|
| 0 | `bundle-manifest.json` | Inventory and hashes for managed course files | Which bundled files belong to the verified package | That dependencies, GPU, models, or data are ready |
| 0 | `configs/course.json` | Reference experiment contract | Intended model roles, data, splits, seed, prompts, and defaults | That the remote model files are complete or that an adaptation is valid |
| 1 | `environment/preflight-*.json` | Host and runtime snapshot written by preflight | The recorded Python, Torch, CUDA, GPU, and disk state | That every later stage used the same unchanged environment |
| 3 | Teacher and student model manifests | Source, revision, inventory, and integrity evidence for both model roles | Which model files were verified and where they came from | That the models generate correct answers or fit simultaneously in memory |
| 4 | `data/selection_report.json` | Data selection and filtering funnel | How source rows became candidate, dev, and test splits | That every row is pedagogically suitable |
| 4 | `data/manifest.json` | Data source, counts, split identities, and hashes | Which exact data version a run should use | That final-test isolation was respected later |

## Generation and training data

| Stage | File or directory | Meaning | Supports | Does not prove by itself |
|---|---|---|---|---|
| 6 or 9 | `request.json` | Request captured for the run | Run name, mode, and requested operation | That generation or any later stage completed |
| 6 or 9 | `generation_audit.jsonl` | Teacher-generation attempts, parse results, termination state, and rejection reasons | Why a candidate was retained or rejected | That every retained reasoning trace is correct |
| 6 or 9 | `dataset/train.json` | Demonstrations that actually enter training | Training rows and their count | That the data is unbiased, leak-free, or sufficient for capability gain |
| 6 or 9 | `dataset/dataset_info.json` | Training dataset registration and provenance summary | How the training file is exposed to the training framework | That the upstream license or cleaning claim is correct without its source record |
| 6 or 9 | `generation_complete.json` | Generation-stage completion record | That the supervised generation stage reached its recorded completion state with identified inputs and outputs | That every example is good enough for training or that the current host generated an imported dataset |

## Training and export

| Stage | File or directory | Meaning | Supports | Does not prove by itself |
|---|---|---|---|---|
| 8 or 10 | `train_config.yaml` | Rendered LoRA training configuration | Actual base model, data path, template, rank, alpha, targets, precision, steps, and output path | That the configuration is appropriate or training succeeded |
| 8 or 10 | `train.log` | Training-process log | Progress, losses, warnings, errors, and duration | That decreasing loss means higher answer accuracy |
| 8 or 10 | `train_complete.json` | Training-stage completion record | Completion state, configuration identity, and duration | That an adapter matches an uninspected base or that export can load it |
| 8 or 10 | `adapter/` or `adapter-lite/` | LoRA incremental parameters and configuration | That the incremental artifact exists for the recorded run | That it works without the matching student base or is a standalone model |
| 8 or 10 | `export_config.yaml` | Adapter-and-base merge configuration | Which base, adapter, template, and output are intended for export | That the merge completed |
| 8 or 10 | `export.log` | Export-process log | Export progress, warnings, errors, and duration | That the merged directory is complete and reloadable |
| 8 or 10 | `export_complete.json` | Export-stage completion record | That export reached its recorded completion state | That every merged file has the expected identity or that inference quality improved |
| 8 or 10 | `merged_manifest.json` | Inventory and hashes for the merged model | Which files belong to the recorded export | That all weights are present in a lightweight archive or that the model answers correctly |
| 8 or 10 | `merged/` | Reloadable combined student model | That the base and adapter were materialized as a model directory | That evaluation or serving completed successfully |

## Evaluation and reports

The paths below use `eval_dev/`; the final-test equivalents live in `eval_test/` and are valid only after stage 13 freeze.

| Stage | File or directory | Meaning | Supports | Does not prove by itself |
|---|---|---|---|---|
| 7 or 10 | `eval_dev/before_predictions.jsonl` | Per-question output from the original student | Concrete pre-training behavior under the recorded protocol | Full benchmark capability or post-training improvement |
| 7 or 10 | `eval_dev/before_summary.json` | Aggregate pre-training metrics and settings | A same-run `before` reference point | Effectiveness without the matched student result |
| 8 or 11 | `eval_dev/student_predictions.jsonl` | Per-question output from the run's merged student | Matched qualitative comparison with `before` | That a few examples represent overall performance |
| 8 or 11 | `eval_dev/student_summary.json` | Aggregate post-training student metrics | Same-protocol accuracy, format, termination, and sample count | The teacher reference, final-test result, or causality beyond this run |
| 11 | `eval_dev/teacher_predictions.jsonl` | Per-question output from the teacher model | Teacher behavior on the same development questions | That teacher output is a gold answer or always suitable for training |
| 11 | `eval_dev/teacher_summary.json` | Aggregate teacher development metrics | A same-protocol teacher reference | A theoretical upper bound or proof that every demonstration is correct |
| 7, 8, 10, 11, or 13 | `eval_dev/*_memory.jsonl` or `eval_test/*_memory.jsonl` | Sampled memory telemetry for one evaluation process | Resource behavior under the recorded run conditions | A portable minimum-memory requirement for every GPU or model |
| 11 or 13 | `results-*.csv` and `results-*.md` | Generated comparison report | A convenient view of summaries already present in the run | New evidence beyond the underlying predictions, summaries, and fingerprints |
| 13 | `frozen.json` | Configuration and file fingerprints locked before final test | The recorded pre-test contract | That the test set was never inspected earlier without corroborating process evidence |

For a comparison claim, check the run ID, model role, split, sample count, prompt and decoding settings, code version, and fingerprints. Do not combine sides from different runs to complete a table.

## Process and qualitative demonstration evidence

| Stage | File or record | Meaning | Supports | Does not prove by itself |
|---|---|---|---|---|
| Any GPU stage | `environment/process-*/supervisor.json` | Managed subprocess exit and cleanup state | Whether the supervised process exited and cleanup completed as recorded | That a generated answer is correct or every target artifact is complete |
| 12 | `/v1/models` responses from both services | Served model names and reachable endpoints | Which named services responded on the recorded ports | That the underlying paths are the intended weights without launch records |
| 12 | Same prompt plus both complete outputs | Qualitative base-versus-distilled case | How both served models responded to the same input | Aggregate effectiveness or a representative accuracy rate |
| 12 | Dual-service observation record | Separate judgments for format, parseability, correctness, and normal termination | A reproducible interpretation of the displayed case | A substitute for dev100 or test metrics |
| 14 | Final `nvidia-smi` and task-process check | End-of-session resource state | Whether the task's visible GPU work has been released | That unrelated hidden services do not exist on the host |

## Commonly confused terms

- **gold:** the dataset's reference final answer used to check model output; it is not the teacher's generated text.
- **teacher_text:** the complete text generated by the teacher model; it can still be wrong or unsuitable.
- **before:** the original student model evaluated before the current run's training.
- **student:** the current run's exported student evaluated after training.
- **teacher:** the larger demonstration model evaluated as a same-protocol reference.
- **adapter:** the incremental weights learned by LoRA; small, but dependent on the matching base model.
- **merged:** the reloadable model produced by combining the adapter with its matching student base.
- **prepared evidence:** verified material reused from the course bundle or archive; valid when labeled, but not proof that the current host produced it.
