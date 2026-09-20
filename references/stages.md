# Student Execution Stages

The commands below assume `/root/distill-course` as the remote repository and `/root/distill-work` as the work root. If the checked-out documentation or environment variables differ, report the difference. Do not replace current configuration with a remembered path.

Classroom guided mode uses only three soft checks, in stages 1, 3, and 5. Accept one sentence, one field pointer, or one binary choice. If the student does not know, give one hint, explain, and continue. Fast execution mode shows each question together with its answer and does not wait.

## 0. Connection and contract

Connect through an existing SSH host alias, then inspect:

```bash
cd /root/distill-course
git status --short
git rev-parse HEAD
python scripts/verify_bundle.py
python scripts/course.py preflight
python -m json.tool configs/course.json
nvidia-smi
```

Verify that the repository is complete, the GPU is available, the teacher and student match the course contract, and no conflicting job is using the assigned resources.

Point out that teacher, student, and dataset settings live in `configs/course.json`. If the class cannot connect within two minutes, switch to the teacher's prepared host or pair the student with a working group.

## 1. Create smoke demonstrations

Confirm that the new `<run-id>` does not exist before starting:

```bash
export COURSE_DATA_ROOT=/root/distill-work
test ! -e "/root/distill-work/runs/<run-id>"
python scripts/run_stage.py generate --run <run-id> --mode smoke
```

Verify that generation completed, `dataset/train.json` contains eight retained demonstrations, and `generation_audit.jsonl` records attempts and rejection evidence.

Soft check 1: show one audit record and ask which field helps determine whether the teacher answer entered training. Accept an answer that checks whether `rejection_reasons` is empty together with parse and termination status. If the student does not know, highlight the fields in the current record and explain them.

If generation exceeds the classroom cutoff, preserve the log and switch to the prepared `smoke01` evidence.

## 2. Measure the pre-training baseline

```bash
python scripts/run_stage.py eval --run <run-id> --model before --split dev
```

Verify that `eval_dev/before_summary.json` and `before_predictions.jsonl` exist and belong to the same run. A smoke run evaluates only a few development examples.

Explain that `before` is the pre-training reference point. Five questions support pipeline verification and qualitative observation, not a stable accuracy estimate. Do not pause for an additional quiz.

## 3. Build and inspect the training configuration

```bash
python scripts/course.py make-config --run <run-id>
```

Show `train_config.yaml` before training. Point out at least the student model path, data path, `finetuning_type`, LoRA rank, alpha, target modules, training steps, and output path.

Hard gate: inspect the model, data path, output path, and smoke step count in the actual `train_config.yaml`. Stop on an incorrect path or mode; classroom pacing never overrides a configuration error.

Soft check 2: ask the student to choose between these statements: the base model does not participate in computation, or it participates in the forward pass while its original weights remain frozen. Explain after the choice. A missing answer does not block training; record it as `hinted`.

## 4. Train and export

If the host requires a separate training environment, follow the checked-out repository documentation and verified environment when setting `COURSE_TRAIN_ENV`. Do not reuse an uncertain historical path.

```bash
python scripts/course.py train --run <run-id>
python scripts/course.py export --run <run-id>
```

Verify that `train_complete.json` records completion, an adapter directory and `adapter_config.json` exist, export completed, and `merged_manifest.json` and the merged model belong to the current run.

Explain that an adapter contains incremental parameters and depends on its matching base model, while the merged directory contains the combined model. Decreasing loss shows a change in the training objective on the current data; it does not prove improved answer accuracy. Do not add another quiz here.

## 5. Compare after training

```bash
python scripts/run_stage.py eval --run <run-id> --model student --split dev
```

Read `before_summary.json`, `student_summary.json`, and their corresponding prediction files side by side. Confirm the same run, split, and evaluation protocol.

Soft check 3: ask whether this smoke run demonstrates a completed pipeline or improved capability, and request one reason. If the student does not know, explain that it proves only the completed pipeline because eight examples, two training steps, and a few development questions do not provide stable effectiveness evidence.

## 6. Explain archived full-run results

Stop the classroom smoke run here and do not run the test split. The teacher may show the frozen 500-example archive: the archived dev100 results are 4% for `before`, 62% for `student`, and 90% for `teacher`; test300 results are 2%, 48%, and 84%, respectively. State explicitly that these are verified archived results, not results produced by the current group's smoke run.

For a post-class 500-example run, reread the current repository README and student guide, use a new run ID, and inspect teacher demonstrations before training. Do not automatically continue from the classroom smoke run into the full workflow.
