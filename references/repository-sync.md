# Course Repository Refresh on a Frozen Image

Availability note (2026-09-20): `register-models`, `verify-models`, and `model-status` are included in local candidate commit `d5ae90bab7c59da1b6055dee838dafa91056bfbc`, absent from the rehearsal image. The candidate passed 30 offline CPU tests and bundle verification; its new registration path has not completed a new-image GPU end-to-end run. At this handoff the commit has not been pushed. Use a supplied candidate bundle or confirm remote availability before promising retrieval; cloning the old published commit does not supply these commands. Ordinary official-client downloads on the image do not require a code refresh. No automatic commit/push is implied by this procedure.

Keep the existing AutoDL community image as the runtime base. It provides CUDA, PyTorch, vLLM, the training environment, and an offline course fallback. Ordinary course-code, documentation, or Skill changes do not require republishing that image.

The image is not scheduled for refresh. The Coach guides retrieval of a compatible course package; it does not automatically update the runtime. Apply [validation-philosophy.md](validation-philosophy.md) when interpreting differences. Keep the old code directory: `/root/train-env` may still import its editable-installed `vendor/LLaMA-Factory` sources. A new course script using this old, working training CLI is expected, not a path mismatch to repair.

Use two separate paths:

```text
/root/distill-course          image-bundled offline fallback
/root/distill-course-live     Git checkout selected for the current class
/root/distill-work            models, data, and runs; never replaced by a code refresh
```

## Select a tested version

The instructor or course release must provide a tag or commit. Do not invent a ref and do not automatically follow `main` during class.

The paired local course candidate and optional downloader commits are recorded in [compatibility.md](compatibility.md). Until they are pushed, use an explicitly supplied local bundle; do not execute the network checkout example below assuming the candidate is already on GitHub. Verify the supplied archive digest, then copy/extract it into a new directory, not over the image-bundled course or an existing dirty checkout. Run the code's own bundle verification before selecting it. Once a fixed ref is actually available remotely, the following checkout pattern applies.

```bash
export COURSE_BUNDLED=/root/distill-course
export COURSE_LIVE=/root/distill-course-live
export COURSE_REF='<tested tag or commit>'

if ! test -d "$COURSE_LIVE/.git"; then
  git clone --filter=blob:none https://github.com/Beirana/distill-course.git "$COURSE_LIVE"
fi

test -z "$(git -C "$COURSE_LIVE" status --porcelain)" || {
  printf 'Live course checkout has local changes; preserve it and stop.\n' >&2
  return 1 2>/dev/null || exit 1
}

git -C "$COURSE_LIVE" fetch --tags origin
git -C "$COURSE_LIVE" checkout --detach "$COURSE_REF"
git -C "$COURSE_LIVE" rev-parse HEAD
git -C "$COURSE_LIVE" status --short
```

`git pull` is not the classroom update mechanism: it silently combines branch movement with the local checkout state. A detached tested ref makes the selected code identity explicit.

## Verify before selecting it

Use the Python interpreter intended for the course command. On the reference two-environment image, generation and evaluation use the base interpreter:

```bash
/root/miniconda3/bin/python "$COURSE_LIVE/scripts/verify_bundle.py"
cd "$COURSE_LIVE"
/root/miniconda3/bin/python -m unittest discover -s tests -v
export COURSE_REPO="$COURSE_LIVE"
```

Record the existing generation interpreter and training CLI; check required imports without installation. Compare old and new `configs/course.json` before reusing prepared data. Unchanged configuration and asset schemas should reuse the same work root. Data/config fingerprint conflicts are investigated separately, not fixed by deleting data or rewriting old manifests. Run status/registration checks required by the selected stage. A new package release needs a pre-class smoke acceptance, but ordinary class startup does not repeat all experiments. Bundle identity is not runtime readiness.

If fetching fails, keep the working image and prepared teaching route. If bundle identity or a functional CPU test fails, do not select the affected unverified new code; inspect that failure without reinstalling the image. If a GPU stage fails, diagnose that stage rather than assuming the code refresh corrupted every environment. The old bundle is a fallback only for capabilities it actually provides: it cannot register external files merely because it was selected again. Preserve downloaded assets, use an available compatible package, or continue file teaching. Do not modify `/root/distill-work` merely to change course code.

## Compatibility record

For each published course ref, record:

- full course commit;
- tested image/runtime profile;
- whether training uses the current environment or `COURSE_TRAIN_ENV`;
- compatible `distillation-lab-coach` and optional downloader versions;
- smoke run identity and result;
- any migration needed for existing model or run manifests.

This record fixes interface compatibility without turning a GPU model, exact disk layout, or one dependency snapshot into a universal requirement.
