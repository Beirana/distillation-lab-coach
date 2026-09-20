# Download Planning: Official Client by Default, Parallel Backup

Reuse verified complete models. For missing weights, default to the course's official ModelScope downloader; it creates the course records understood by the old image. Do not require installing, discovering, or probing [model-download-accelerator](https://github.com/Beirana/model-download-accelerator) before this normal route. Keep parallel acceleration as an optional backup for repeated failures, stalls, sustained throughput inadequate for the remaining time, or an explicit user request. A slow first few seconds or a different version number alone is not a reason to switch.

Do not interrupt a healthy transfer just to try another tool. If a backup is warranted, explain the problem, preserve downloaded files and logs, and establish a safe resume/import plan before changing tools. Use the companion skill only when it is available and the selected provider and downstream course interface are compatible. No backup skill or new course registration interface is required for ordinary official-client downloads.

## Normal route

1. **Already complete?** Verify existing manifests and local config/tokenizer/load evidence and reuse the files. If external files exist but registration is missing, follow the local registration section below; do not redownload them with the official client as a supposed verification step.
2. **Destination ready?** Inspect actual paths, symlink targets, mounted disk, free capacity and write access. A symlink can exist while its target is absent on a fresh data disk. After confirming the intended target, create only the missing task directories; do not rebuild the environment or move unrelated data.
3. **Use the official course command.** Inspect `course.py --help`, then use `download-models` for missing weights. On the old image, do not call absent `model-status` or `register-models` commands. Keep start/end, exit codes, per-role bytes and any verification time. This route may download teacher then student while transferring multiple files inside one model concurrently.
4. **Overlap teaching.** Keep a log and process identity for an authorized background transfer. Proactively offer [single-sample video/file teaching](single-sample-walkthrough.md), rather than waiting for a learner question. Preparation and explanation do not need weights; teacher generation does. A completed student supports a separate load check but cannot bypass the course's generation prerequisite for a current-run before baseline.
5. **Finish or choose a backup.** Verify the local files and actual loading; do not treat an exit code alone as readiness. If the download remains healthy, continue it. At a classroom cutoff, offer prepared evidence instead of discarding progress. Persistent source trouble or a user request can justify the optional route below.

## Optional parallel or mirror route

1. Inspect the available downloader skill and `course.py --help` **before external transfer**. Require a usable local registration interface or an explicitly supplied reviewed compatible bundle/import route. The old image lacks this interface. Do not upgrade runtime dependencies or fabricate manifests to force acceleration.
2. The ModelScope adapter has one real-host transfer plus later pinned-snapshot revalidation; it remains provisional. Qualify manifest and Range behavior. If unsuitable, consider HF/hf-mirror while preserving the chosen model role and independently resolved provider snapshot; provider commit IDs need not match.
3. Require immutable source revision, complete inventory, stable URLs, valid Range behavior and integrity metadata; observe rate-limit responses. Check disk, `aria2c` and execution permissions. A successful Range probe alone does not prove course registration compatibility. HF non-LFS checksums may remain a registration limitation.
4. Run a controlled cold-file comparison only when a performance experiment is requested. An ordinary classroom download needs timing and verification, not two full downloads or a parameter sweep.

Fall back to the official course client or a prepared classroom host whenever a key condition is missing. Never weaken integrity checks merely to claim acceleration.

## Boundary with the current course

`distill-course` obtains its reference teacher and student through ModelScope. The provisional adapter can enumerate, transfer, and verify these two public repositories, but it does not replace the course registration layer. Therefore:

- list it in the README as an optional companion skill;
- assess applicability before choosing this optional external transfer, not before every ordinary official download;
- resolve one repository Git commit and use it for every file URL; ModelScope's per-file `Revision` values are not a repository snapshot;
- generate a parallel download plan only after the provider contract has been checked for the selected repository and host;
- if parallel transfer is unsuitable or the skill unavailable, explain the reason and use a verified cache or official client. Do not insist on installing another skill as a prerequisite.

Before an external parallel transfer, confirm `python scripts/course.py --help` exposes the local registration interface. It is included in the local course candidate recorded in [repository-sync.md](repository-sync.md), NOT in the 2026-09-20 image or historical reference commit. Verify that the candidate bundle or remote ref is actually available and disclose its CPU-tested, not newly GPU-end-to-end-tested scope. After transfer and byte/semantic verification, set the source-manifest variables to the actual files produced by the downloader and register:

```bash
python scripts/course.py register-models \
  --teacher-source-manifest "$TEACHER_SOURCE_MANIFEST" \
  --student-source-manifest "$STUDENT_SOURCE_MANIFEST"
python scripts/course.py model-status
```

Do not rerun the official client merely to create course manifests. ModelScope's client cache index, the downloader source manifest, and the course role registration are separate artifacts.

In particular, `modelscope download` / `snapshot_download` is NOT read-only verification of aria2 output: it can download again and populate its client cache index. Verify existing bytes with the downloader's `verify_manifest.py --semantic`, then register locally. A successful local hash check is not yet a runtime load check. HF manifests may lack upstream SHA256 for non-LFS files; do not invent a trusted checksum from the local file. The current strict course registration requires an upstream SHA256 for every file. If those are unavailable, disclose this registration limitation and obtain a trustworthy matching manifest or use a separately validated import path; do not claim complete HF-to-course compatibility just from Range support.

The first 2026-09-20 observation transferred 16,242,408,362 bytes with aria2 in 1,549 seconds, about 10.5 MB/s, with 14/14 teacher and 10/10 student SHA256 checks plus semantic gates passing. A 1.27 MB/s curl result was only a single-connection probe. A later new-instance official-client run transferred the same payload in 978.135 seconds (about 16 minutes 18 seconds), with local manifest hashes and actual smoke execution verified. Different instances/times make these observations **not a controlled comparison**: neither an acceleration factor nor a universal faster tool is established. This evidence supports keeping the simpler compatible official route as the classroom default, not forcing parallel downloads.

The saved original manifests used incorrect file-level revision labels. Later read-only revalidation matched all existing bytes and file paths to teacher commit `16c174980d8a1492910551634b4969e69cdc2444` and student commit `186d8559ad54c32cf47dc3a8225f993742c507b8`. Do not describe the original transfer as having started with correctly pinned repository manifests.

Performance interpretation: 13 MiB/s was an observed teacher peak, not a proven CDN/export ceiling. `-j2 -x8 -s8` gives an upper connection budget, not 16 continuously active connections. The student's 270-second small-file tail delayed student readiness but overlapped teacher download; eliminating it would not automatically save 17% of total wall time. `-k4M` prevents splitting ranges smaller than 8 MiB, not all files below 32 MB. Small-file-first/single-connection handling and `-j4 -x4` are candidates for controlled tests, not proven improvements. Compare official clients as configured, without calling them single-connection merely because model repositories are processed sequentially.

The current course command calls the teacher and student downloads in sequence. A provider client may still download files concurrently inside one model, but that is different from starting both model repositories in parallel. Do not describe the current course command as a two-model parallel downloader.

## Future benchmark record

```text
Provider or mirror:
Repository and immutable revision:
Files and total bytes:
Machine, region, disk, and network:
Official-client wall time:
Accelerated wall time:
Per-role start/end and exit codes; combined supervisor wall time:
Manifest-generation / transfer / verification / registration times separately:
Sustained throughput, excluding warm-cache peaks:
HTTP 403, 429, and reset counts:
Retry count and unfinished files:
Byte-level verification:
Semantic or index verification:
Measured improvement and limitations:
```

Do not publish a numerical speedup in this skill until this record has been completed with measured evidence.

Compute aggregate payload throughput as total manifest bytes / total wall seconds, with explicit MB or MiB units; log-sample means are a separate statistic. Do not use `du`/directory size as progress when aria2 preallocates files. Preserve resume evidence and structured HTTP errors without exposing signed CDN URLs. Do not run cold baselines, delete caches, or sweep parameters just to answer a teaching question.

Provider discovery and transfer belong in the companion downloader; role registration belongs in the course; timing and teaching overlap belong in the coach. Record failures and fallbacks as honestly as successes.
