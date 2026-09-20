# Teacher Control Mode

Teacher control mode gives the instructor an explanation that can be spoken immediately and one clear next action. It should not require the instructor to search through the whole project during class.

## Response format

When the teacher asks what something is or what to do next, answer in four parts:

1. **Current position:** identify the active workflow stage.
2. **What to say:** provide a natural 20 to 40 second Chinese classroom script.
3. **What to show:** select one file or visual entry point.
4. **Next action and cutoff:** state when to continue and when to switch to prepared evidence.

## One-minute storyline

When asked for the main story, produce a concise Chinese explanation covering these points:

- the 7B teacher first solves the questions;
- reference answers, formatting, length, and termination state filter its output;
- the 0.5B student is measured before training;
- LoRA trains the student on the retained teacher demonstrations;
- the same questions and settings are used after training;
- the classroom smoke run verifies the pipeline, while the archived 500-example experiment is used to discuss effectiveness.

## Short answers to common questions

### Does this count as knowledge distillation?

Yes, it is teacher-demonstration text distillation. The student learns from filtered answer text produced by the teacher. The course does not use teacher logits or a KL-temperature loss, so do not describe it as a complete reproduction of classic online logit distillation.

### Why can the larger teacher still be wrong?

More parameters do not guarantee a correct answer to every question. The teacher can make calculation errors, violate the format, produce excessive output, or terminate abnormally. Its output therefore requires automated checks and human sampling.

### Is LoRA the distillation method?

LoRA is the parameter-efficient method used to train the student on the demonstrations. Distillation describes where the student's training knowledge comes from; LoRA describes how the student's parameters are updated.

### Why measure `before` first?

Without a pre-training baseline, the class cannot tell whether post-training behavior changed because of the current experiment or was already present in the original student. `before` is the reference point for comparison.

### Why might the smoke result stay unchanged or get worse?

It uses only eight demonstrations and two training steps. Its purpose is to expose engineering problems, not to obtain a statistically reliable capability gain.

### Does decreasing loss mean the student learned the task?

Loss only shows that the model is fitting the current training text more closely. Correctness, output format, and clean termination require independent evaluation.

### What do students learn if the Agent runs the commands?

The Agent handles repetitive commands, log location, and file discovery. Students still decide whether an example is suitable for training, what the configuration means, and what the results can support. Those judgments are the core laboratory skills.

## Classroom control rules

- If an explanation remains unresolved after two minutes, return to the seven-stage workflow, give one immediate conclusion, and move the deeper question to post-class discussion.
- If one student cannot connect after two minutes, pair the student with a working group. If most students cannot connect, switch to the teacher's prepared host.
- If a runtime stage exceeds the lesson cutoff, preserve the live log and switch to archived artifacts. Do not turn the class into environment maintenance.
- For an out-of-scope question, first classify it as data, training, evaluation, or model internals. Give one sentence and record it for later.
- When uncertain, say in Chinese that the detail should be checked in the current configuration or log rather than answered from memory. Verifying live evidence is part of the demonstration.
- Use only three soft checks during classroom guided mode. Keep each within 20 to 45 seconds, give a hint when needed, and continue.
- If machine progress is slow, the teacher may switch to fast execution. This removes later waiting but keeps every hard gate.

## Required classroom outputs

Each group submits a host preflight result, one retained/rejected example judgment, a LoRA configuration summary, the run artifact path, and a five-sentence exit note. Do not rank groups by smoke-run accuracy.
