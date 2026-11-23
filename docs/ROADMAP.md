# Roadmap — from synthetic compatibility to observable boot behavior

**Goal:** reduce the number of *measured* blockers between our prototype and a reproducible, legitimate compatibility implementation. A complete GTA VI launch is **not currently demonstrated by this project**, and no timetable is promised.

## Priority 0 — preserve evidence and freeze working baselines

- Keep accepted local component revisions immutable and record hashes, compiler/SDK/driver and exact fixture versions.
- Review any material proposed for public upload for legal provenance, proprietary data, personal paths, credentials and licence requirements.
- Publish sanitized reproduction materials only after review. This public repository currently holds **documentation**, not ready-to-build code or tested binaries.

**Success:** every published claim has an evidence level and a link or a reproducible non-proprietary fixture.

## Priority 1 — diagnose the first unsupported guest interface

- Current synthetic-guest path reaches `XMemAllocatePhysicalPages`, then stops when resolving `XMemMapPhysicalPages`.
- Compare published public signatures, the local CPU-only page model and ownership conditions; do not infer unknown console semantics from matching export names.
- Use trusted, self-authored fixture modules, vetted test environments and precisely attributed failure reports.
- Do not use denied requests as a reason to circumvent developer-platform safeguards.

**Success:** a controlled test clearly identifies the next missing contract without disguising errors as success. **This is not a retail-title boot milestone.**

## Priority 2 — pursue the earliest demonstrable real rendering difference

- Coordinate with community measurements of the second offscreen producer and black frames.
- For one identified frame/draw, establish writer provenance, actual CBV resource and generation, GPU-visible values, shader operands, offscreen RT and final backbuffer.
- If only zero values are observed and the writer is not known, report that limit. Test whether black is an *expected early-state or fade* before classifying it as broken rendering.
- Compare only measurements from the same branch, patch stack, backend and GPU.

**Success:** a publicly reviewable causal observation about the first divergence, **not** an unsupported claim that the black screen is fixed.

## Priority 3 — reuse and integrate compatible open work

- Review Leonida, AnyXboxX, Xbox2Windows and TomBox at pinned revisions.
- Avoid duplicating already functioning features; verify ABI correctness, lifetime, concurrency and licence compatibility first.
- Keep a minimal synthetic integration fixture for any imported change, with positive and negative checks.
- Prefer a diagnosed blocker fixed on a clean build to an ever-growing number of synthetic test pixels.

**Success:** one reproducer and one measurable incompatibility resolved without regressions.

## Priority 4 — complete the first-frame and gameplay criteria

These milestones must be evaluated separately:

1. **Entry:** a trusted, lawful target is loaded, initialized, and runs its code; evidence includes stage and error tracing.
2. **GPU work:** real command buffers execute with shaders and resources; successful recording alone is insufficient.
3. **Presentation:** a submitted image reaches a visible output, and the captured backbuffer is correlated with the displayed frame.
4. **Recognizable frame:** a reviewer can independently corroborate actual game imagery, not just a diagnostic rectangle, window, black frame or stand-in shader.
5. **Interactive gameplay:** controls, asset streaming, audio, saves, stability and performance work in repeated sessions.

**We have not established steps 1–5 for a retail GTA VI build.**

## Help wanted

- Review the [constant-buffer trace specification](LEONIDA-CB-DIAGNOSTIC-SPEC.md) for missing causal links.
- Suggest a clean, legal, self-authored shader+CBV fixture for comparing GPU-visible values against an independent CPU reference.
- Report upstream corrections with links to exact revision and a clear measurement, not guesses.
- Help plan publication of non-proprietary sources and sanitized evidence while respecting upstream licences.
