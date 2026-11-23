# Source Release v0.1 — publication plan

**Status: preparation only. No implementation source from the local Codex workspaces has been published here yet.**

This document tracks the first public *source* release of Lucy × Nazir Research Lab. It is separate from the previously published research summaries. The proposed release is strictly **self-authored synthetic Windows compatibility code**, not a GTA VI build, an Xbox runtime proven on real console software, or software containing proprietary material.

## Intended source units

| Unit | Stable local baseline reported by Codex | Candidate public content | Review dependency |
| --- | --- | --- | --- |
| CPU XMem | `scarlett_cpu` prototype; 64 KiB page aliases, scatter, placeholder reserve and remap | Selected `.cpp/.h`, public API, self-authored tests and CMake adapter | Check provenance; verify that no AnyXboxX GPL implementation was copied; check actual Scarlett semantics are not claimed |
| ScarlettCompat::VABridge | Local tag `scarlett-va-bridge-v0.1.0` at `d09816c06e3e6123b4aaefbe1d766f5e6c227304` | Guest-VA registry, typed/raw/sliced SRV fixture, direct/shadow synthetic controls | Review imported dependencies and licence; distinguish synthetic slot-133/70 model from measured console API |
| ScarlettCompat::DescriptorBridge | Local tag `descriptor-bridge-v0.1.0` at `60794c863f705748be9eabaa10b1000cd9f5e1b8` | Resource generation, queue/fence tracking, SRV lifetime tests, CMake interface | Preserve recorded lock order Recording → Lane → Bridge; verify public headers and local source pin |
| Integrated Runtime M1–M3 | Local accepted and checked reports; pin source tree and exact fixture versions before release | Minimal independent sample wiring XMem → GPU SRV → readback plus synthetic PE/DLL fixture | M3 uses **native Windows loader** for mapping, IAT, relocations and TLS; do not mislabel as an independent console PE loader |
| Diagnostic Runtime M4.1–M4.5 | Reported synthetic AppContainer path to `GetProcAddress("XMemMapPhysicalPages")` | Clean diagnostic broker, self-authored GDK and XMem fixture interfaces, safe error reports | No unknown game DLL/EXE; no assertion of a real GTA VI boot |

## Mandatory review gates — before any source is committed publicly

1. **Provenance.** For every file identify its author, originating local repository + commit, whether it was generated or copied, and whether any third-party material is included. Do not copy a GPL component into a proposed permissively licensed repository without a licence decision.
2. **Data protection.** Scan the exact staged diff for home/OneDrive paths, usernames, email/IP addresses, API keys, access tokens, system UUIDs, credentials, private TitleIDs, unpublished game-data constants and raw dumps. `.gitignore` is **not** a safety boundary.
3. **Proprietary content.** Exclude all game or package executables/DLLs, RPF archives, shaders/asset bytes, extracted content, screenshots, save files, decryption or protection-bypass material. Only self-authored synthetic fixtures may be published.
4. **Build provenance.** Write the actual compiler, Windows/SDK/D3D12 Agility versions, driver/GPU, build instructions and expected outputs from the component's own `INTEGRATION.md`. Do not invent a command or test result.
5. **Tests and controls.** Retain positive and negative tests in source form. Include independent CPU references, bounded memory/VA error cases, generation and GPU fence correctness, and a no-game-files test path.
6. **Evidence.** Publish sanitized structured test summaries, exact local source pins and reproduction instructions. Raw game-related or personal logs stay private. Distinguish *run locally* from *independently reproduced by another party*.
7. **Licensing.** Ask the repository owner to choose a licence for original code after checking dependencies. A `No license` repository does not grant broad reuse rights.
8. **Human review.** A source-aware Codex agent creates an explicit **file list, source/hash manifest and staged diff**. The repository owner checks these before public upload. Do not bulk-upload a full research folder.

## Proposed layout (not yet created)

```text
src/
  xmem-cpu/
  va-bridge/
  descriptor-bridge/
  runtime-synthetic/
tests/
  xmem-cpu/
  va-bridge/
  descriptor-bridge/
  integration/
examples/
  minimal-consumer/
docs/
  SOURCE-RELEASE-V0.1.md
  REPRODUCIBILITY.md
```

Names are provisional; use the exact component structure only after inspecting the real source trees.

## Release acceptance

The first public technical source release is **accepted only when**:

- The intended components actually appear as source files in this repository, accompanied by their headers and build definitions.
- Build and synthetic tests can be executed by a third party following **checked** commands.
- Results are explicitly tagged `synthetic`; GPU-dependent tests clearly say which hardware was used and are not treated as automatic GitHub Actions passes.
- A reproducibility manifest identifies the source revision, build inputs and sanitized outputs.
- Secrets/proprietary-data scans and licence reviews are complete and recorded.
- No claim of retail GTA VI execution or first frame is attached to the synthetic test pass.

Until then, the release remains **documentation-only / awaiting audited source**.
