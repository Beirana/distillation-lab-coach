# Optional Model-Download Acceleration

The lab may optionally use [model-download-accelerator](https://github.com/Beirana/model-download-accelerator). It is not a required dependency and does not automatically replace the `distill-course` downloader.

Use the companion skill only when it is actually installed or otherwise available, the user has put model download work in scope, and the selected provider has passed its qualification contract. If any condition is missing, keep the course downloader or a verified preloaded image as the plan.

## Decision sequence

1. **Is the model already complete?** Verify against the course manifest or a load check. Reuse a complete model and skip downloading.
2. **Is class about to start?** Complete large transfers and performance experiments before class. If a model is missing during class, switch to a prepared host.
3. **Is the provider supported?** The public download skill currently targets public Hugging Face-compatible sources. It does not claim ModelScope support.
4. **Can the source satisfy the provider qualification contract?** Require an immutable revision, complete file inventory, stable direct URLs, probed Range behavior, integrity evidence, and known rate-limit behavior.
5. **Are local prerequisites available?** Check free disk space, filesystem behavior, network egress, `aria2c`, and host execution permissions.
6. **Can the benchmark be fair?** Compare the normal and parallel paths with the same immutable revision and cold files. Record sustained throughput, wall time, failures, retries, HTTP errors, and final integrity.

Fall back to the official course client or a prepared classroom host whenever a key condition is missing. Never weaken integrity checks merely to claim acceleration.

## Boundary with the current course

`distill-course` currently obtains its teacher and student models through ModelScope. The public `model-download-accelerator` does not claim ModelScope support. Therefore:

- list it in the README as an optional companion skill;
- assess applicability before any transfer begins;
- generate a parallel download plan only after the provider contract has been implemented and tested;
- keep the course `download-models` path as the default and fallback.

The current course command calls the teacher and student downloads in sequence. A provider client may still download files concurrently inside one model, but that is different from starting both model repositories in parallel. Do not describe the current course command as a two-model parallel downloader.

## Future benchmark record

```text
Provider or mirror:
Repository and immutable revision:
Files and total bytes:
Machine, region, disk, and network:
Official-client wall time:
Accelerated wall time:
Sustained throughput, excluding warm-cache peaks:
HTTP 403, 429, and reset counts:
Retry count and unfinished files:
Byte-level verification:
Semantic or index verification:
Measured improvement and limitations:
```

Do not publish a numerical speedup in this skill until this record has been completed with measured evidence.

After a successful ModelScope experiment, update this reference only from the saved benchmark and integrity evidence. Record whether the implementation belongs in the course script, the optional companion skill, or a pre-class operator workflow; do not infer that placement from throughput alone.
