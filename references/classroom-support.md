# Classroom Support

Use this reference proactively during a guided classroom run, download wait, or smoke-stage transition, and when the user asks for explanation. The learner need not know which question to ask before teaching is offered. Do not choose a workflow based on whether the human user is an instructor or a learner. Everyone follows the same stages in [stages.md](stages.md).

In this reference, **teacher** and **student** refer only to model roles.

Default to the user operating their connected terminal/Jupyter with the Agent explaining. Recommend existing passwordless SSH for remote GPU work unless the user chooses otherwise. During downloads use [single-sample-walkthrough.md](single-sample-walkthrough.md), including its artifact map, video suggestions, and monitoring commands. Start a qualified download first so explanation overlaps it.

## Proactive offers, optional depth

At a natural checkpoint, provide a short purpose/command/output explanation and one real artifact location. Offer “inspect together / explain more / continue” rather than waiting for a learner question or automatically delivering the whole artifact map. Download waits are a natural time to offer the single-sample video and trace. After generation offer audit → train.json; before training offer the rendered LoRA config; after export offer adapter/base/merged; after dev offer one paired prediction. Do not repeat an explanation already covered unless requested or the current artifact changes the conclusion.

In guided mode, allow a reply. If the user skips, continue without a quiz requirement. If continuous execution is delegated, explain briefly and continue within that scope; do not count the Agent's own answer as user understanding. Authorized background downloads can continue while the learner decides. These are teaching choices, not new integrity gates.

## Response shape

For a time-sensitive classroom request, answer in four parts:

1. **Current position:** identify the authoritative stage number and the live or prepared evidence in use.
2. **What to say:** give a natural 20 to 40 second Chinese explanation suitable for speaking aloud.
3. **What to show:** name one file, terminal view, prediction pair, or visual entry point.
4. **Next action and cutoff:** give the next command or question and state when to switch to prepared evidence.

For an execution request, replace the spoken script with the exact command, what it reads, what it writes, and the completion evidence. For a conceptual question, answer directly without forcing the user through a checkpoint.

## One-minute storyline

When asked for the main story, cover these points:

- a larger teacher model generates candidate solutions;
- gold answers, parsing, formatting, length, and termination checks filter those solutions;
- the smaller student model is evaluated before training;
- LoRA trains the student on retained teacher demonstrations while the frozen base still participates in the forward pass;
- export combines the adapter with the matching student base into a reloadable merged model;
- the same questions and decoding protocol are used for before and after comparison;
- the smoke run proves that the engineering chain works, while the reviewed formal-500 run supports effectiveness discussion;
- a dual-service prompt illustrates output behavior, but fixed-protocol evaluation supports quantitative claims.

The reference implementation uses Qwen2.5-7B-Instruct and Qwen2.5-0.5B-Instruct. Present them as the verified reference pair, not the only possible teacher and student models.

## The three soft checks

Use no more than these three checks during the 60-minute route. Allow a short answer, give one concrete hint when needed, explain, and continue.

### After smoke generation

Question: Which fields help determine whether a teacher response entered training?

Accept answers that point to `rejection_reasons` together with parse status, final-answer value or format, and termination status. Empty rejection reasons support retention; they do not prove perfect reasoning.

### Before LoRA training

Question: Does the frozen base model stop participating in computation?

Expected idea: no. The base matrix still participates in the forward pass, while the low-rank branch carries the trainable increment. A concise formula is `y = Wx + (alpha / r) BAx`.

### After smoke comparison or dual service

Question: Why does a completed smoke chain or one good conversation not prove capability improvement?

Expected idea: eight demonstrations, two training steps, dev5, or one selected prompt are too small and selective. Capability claims require the reviewed formal run and a fixed evaluation protocol.

Record actual interaction: `offered`, `user_answered`, `hinted`, `skipped`, `delegated`, or `no_response`. `hinted` means a hint was provided, not that the learner understood. A self-supplied explanation is `agent_explained`, never a learner confirmation. These records are teaching feedback, not grades.

## Short answers to common questions

### Does this count as knowledge distillation?

Yes, it is teacher-demonstration text distillation. The student learns from filtered text generated by the teacher model. The course does not read teacher logits or optimize a teacher-student KL loss, so do not present it as a reproduction of classic online logit distillation.

### Why can the larger teacher model still be wrong?

More parameters do not guarantee a correct answer to every question. The teacher model can calculate incorrectly, violate the required format, generate excessive text, or terminate abnormally. Automated checks and human sampling are therefore part of the experiment rather than optional decoration.

### Is LoRA the distillation method?

Distillation describes where the supervision comes from. LoRA describes how the student is updated with a small trainable low-rank branch. They answer different questions.

### Why measure `before` first?

Without a pre-training baseline, a later student answer cannot show what changed during this run. `before` and `student` must use the same split and evaluation protocol.

### Why might the smoke result stay unchanged or get worse?

The reference smoke run retains only eight demonstrations and trains for two steps. It is designed to expose broken paths, incompatible environments, export problems, and evaluation mistakes, not to deliver a statistically reliable gain.

### Does decreasing loss mean the student learned the task?

It is a training signal, not a capability result. Losses on different step/batch inputs alone do not prove that every training example is fitted better. Correct answers, required formatting, and normal termination still need independent evaluation.

### What remains for the user when the Agent runs commands?

The Agent explains and locates evidence; the user can execute short checks and smoke or request execution. Ask whether they have seen a sample and the configuration, offer to inspect together, then respect "continue" or delegated inspection. Do not demand a quiz answer or mandatory manual signature. Model/data changes, final-test protocol, and concrete technical inconsistencies still need explicit treatment.

### Why separate formatting from correctness?

A response can end with a perfectly formatted `Answer: <number>` line and still contain the wrong number. The formal archive demonstrates a strong format shift, but accuracy remains a separate metric.

## The dual-service teaching moment

Use the original student base and a formal-500 merged student verified before class. Give both services the exact same non-test prompt and record:

- readable reasoning;
- required final-line format;
- parse success;
- numeric correctness against gold;
- normal termination.

Do not promise that an arbitrary prompt will produce an obvious difference. If the live case is ambiguous, show the corresponding saved predictions and the verified formal dev100 summary. When the user or course provider supplies the verified `instruct500-new-01` evidence package, that archive reports:

| Model role | Accuracy | Format rate | Normal stops |
|---|---|---|---|
| before | 4% | 4% | 99 of 100 |
| student | 62% | 96% | 99 of 100 |
| teacher | 90% | 93% | 100 of 100 |

Always identify those numbers as archived formal evidence, not the result of the current smoke run. A standalone public Skill installation does not contain this package. If the summaries or provenance are unavailable, state the evidence gap and do not quote the table as a locally verified result.

## Classroom control rules

- If a connection is not working after two minutes, move to a working group, prepared shared host, or platform terminal.
- If most hosts are affected, demonstrate from one verified host and let users retain their own observation and explanation records.
- If an explanation remains unresolved after two minutes, give the immediate course conclusion and move the deeper question to post-class discussion.
- If a GPU stage exceeds its cutoff, preserve the live log and switch to identified prepared artifacts. Never relabel them as live output.
- If less than eight minutes remain, start no new GPU stage. Interpret evidence, stop services, check `nvidia-smi`, and record unfinished work.
- If two vLLM services do not fit, use a verified shared host, load the models sequentially, or compare saved predictions.
- If a result differs from a displayed archive, compare run ID, commit, model roles, split, prompts, decoding, and configuration. Record the real difference instead of forcing the archived number.
- When uncertain, inspect the current configuration, log, or artifact rather than answering from memory. The verification itself is part of the lesson.
- Preserve real constraints: credentials, run collisions, corrupt/incomplete models, incompatible configuration, final-test isolation, and unrelated processes. Not having read a file is a conversational checkpoint, not a hard gate.

## Required classroom outputs

Each group or individual record should contain:

1. the smoke run ID and full Git commit;
2. the `generation_complete.json` path;
3. the retained-row count and an explanation of one audit record;
4. the `before_summary.json` and `student_summary.json` paths;
5. the purpose of the adapter and merged artifacts;
6. a dual-service or saved-prediction observation that separates format from correctness;
7. one sentence explaining why smoke does not prove capability improvement;
8. the selected formal-500 post-class route, generated or imported.

Do not request private keys, passwords, access tokens, or full model weights as classroom submissions. Do not rank users by smoke-run accuracy.

## What to show at each key moment

| Moment | Best single evidence view | Boundary to state aloud |
|---|---|---|
| Teacher generation completes | `generation_audit.jsonl` beside `dataset/train.json` | Retention does not prove perfect reasoning |
| Before evaluation completes | One before prediction plus `before_summary.json` | dev5 is a pipeline sample, not a benchmark estimate |
| Before training | Rendered `train_config.yaml` | Check paths, mode and steps against the requested run; offer explanation without requiring manual sign-off |
| Training completes | `train_complete.json` plus adapter configuration | Loss change does not prove answer improvement |
| Export completes | `export_complete.json` plus `merged_manifest.json` | A manifest proves artifact identity, not model quality |
| Smoke student evaluation completes | Matched before/student predictions | Same protocol makes comparison meaningful; n=5 remains small |
| Dual service runs | Same prompt and the five-row observation record | A conversation is qualitative evidence |
| Formal archive is discussed | Three dev100 summaries or the verified table | Archived results are not current-host smoke results |

Read [files.md](files.md) when a user asks what a specific artifact proves or cannot prove.
