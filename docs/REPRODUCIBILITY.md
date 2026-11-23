# Reproducibility standard

Current repository state: **research summaries and a diagnostic proposal**, not complete buildable releases of CPU XMem, VABridge, DescriptorBridge or Runtime M1–M4.5. Their local reports are described in [PROGRESS.md](PROGRESS.md).

This document defines what **must accompany** any later source upload.

## Run manifest (fill from an actual run)

```yaml
experiment: "<name>"
source:
  local_component: "<module>"
  revision: "<exact immutable commit or archive digest>"
  third_party_dependencies: [] # name, licence, URL, pinned revision
environment:
  os_build: "<Windows edition and build>"
  compiler: "<toolset + exact version>"
  windows_sdk: "<version>"
  graphics_api: "<D3D12 or Vulkan>"
  agility_sdk: "<version or not used>"
  gpu: "<model or CPU-only>"
  gpu_driver: "<version or not used>"
run:
  build_command: "<actually tested command>"
  test_command: "<actually tested command>"
  isolation: "<self-authored fixture, network state, runner>"
results:
  expected: "<condition>"
  observed: "<condition>"
  negative_controls: "<which failures were intentionally detected>"
  evidence_files: [] # sanitized files committed to repository
  source_pins_verified: false
limits:
  synthetic_only: true
  retail_abi_confirmed: false
  retail_game_executed: false
```

Replace placeholders with measured values before publication. Do not publish them as if they were completed facts.

## Minimum test evidence

- **CPU memory:** page grants, contiguous and scattered aliases, observable write-through, placeholder geometry, unmap/remap, stale IDs and failed operations without state changes.
- **VA → GPU view:** identity of backing resource, range and format, publication barrier/fence, direct import vs. shadow copy, stale mapping after reuse.
- **Descriptor lifetime:** generation checks across multiple CPU writers/command lists and DIRECT/COMPUTE queues, pending recordings, refusal of early reuse, teardown after last consumer.
- **PE/GDK diagnostic:** only self-built, trusted test modules; stage labels, exact HRESULT/Win32 error, distinction between Windows loader operation and any custom broker logic.
- **GPU validation:** per-run Debug Layer / GPU-based validation counts, CPU-reference checks, GPU/driver/Agility pin and source hash. Headless or software rendering **must not** be reported as a real RTX 3060 run.

## Publication workflow

1. Freeze a clean source revision and record the expected file list and SHA-256 manifest locally.
2. Exclude proprietary binaries, raw executable bytes, game shaders/assets, keys, credentials, personal data and unrelated project folders.
3. Audit the exact candidate tree; remove accidental dependencies on private absolute paths.
4. Run the project's own documented compiler and synthetic tests on the candidate.
5. Prepare a small, reviewable source-only change, not a mass upload.
6. Commit tests, reproducibility instructions and sanitized summaries together with the code.
7. Mark remaining gaps plainly. A code review can identify mistakes, and a later third-party run is needed for independent verification.

## What is *not* established by an acceptance number

A reported `6/6` synthetic test result **does not imply** real Scarlett ABI compatibility, successful GTA VI startup, a real game frame, or legal provenance of a leaked binary.

This policy exists to make the project useful to other developers without overstating its maturity.
