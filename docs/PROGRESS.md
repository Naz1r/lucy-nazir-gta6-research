# Research progress and evidence ledger

**Snapshot:** 24 November 2026. This ledger summarizes milestone reports supplied by the project owner from three local Codex research sessions. **The reports are not independently reproduced from raw files in this repository.** Each figure is therefore a *locally reported test result*, not independent certification or evidence of GTA VI gameplay.

## Archives and early static research

| Area | Locally reported observations | Limit |
| --- | --- | --- |
| `script.rpf` | RPF9 header; 1,012 PSIN resources examined, CHKS checked; metadata and schema exploration | Origin/authenticity not verified |
| `data.rpf` | 2,281/2,281 PSIN reconstruction with mixed compressed/raw cases | This is archive parsing, not engine loading |
| Other archives | Static scans of common, shaders, textures and animation containers | No game assets are distributed here |
| Suspected Scarlett PE | Static metadata observed: PE32+ AMD64, 587 named imports across 68 DLL names, with 37 console-side imports from seven libraries lacking local providers | File origin/authenticity unproven; file not published or run as part of these studies |

## Synthetic GPU and ABI work

| Experiment | Reported acceptance evidence | What remains unproven |
| --- | --- | --- |
| D3D12/Vulkan Stage 2 | 64×64 RGBA8, 16,384 bytes, matching CPU data and D3D12 Debug Layer without errors | Actual retail renderer |
| Stage 3 | 63×65 RGBA8, 16,380 useful bytes; padded row handling for D3D12 and Vulkan | Full texture interoperability |
| Stages 4–5 | Four mip levels; BC1 Stage 5: 356 blocks / 2,848 bytes, 3/3 runs for each API | Xbox BCPack is distinct from plain BC1 |
| Stage 6 | Synthetic `SetDescriptorHeaps` slot-32-to-28 adapter, six GPU runs, 30 negative checks | Real Scarlett ABI requires external corroboration |
| Stage 7–9 | SRV reads from two heaps, two queues, multiple in-flight command lists and generation checks | Guest handle layout remains unknown |
| Stage 10 | Four CPU threads, DIRECT + COMPUTE, 6/6 hardware runs, 1,248/1,248 uint32 words in readback | Synthetic resource lifetime, not retail workload |
| Synthetic VA bridge | Model of device slots 133 → 70 with direct import and shadow-copy fallback; 12/12 runs and 90/90 readbacks | Actual console call/descriptor encoding |
| CB pipeline | 42 frames; distinguished missing publication from wrong CBV binding; zero D3D12 Debug Layer errors | Why Leonida's original draw used zeros |

Reported immutable local component pins (the commits are **local research repositories**, not public code in this repo):

- `ScarlettCompat::DescriptorBridge`: tag `descriptor-bridge-v0.1.0`, local commit `60794c863f705748be9eabaa10b1000cd9f5e1b8`.
- `ScarlettCompat::VABridge`: tag `scarlett-va-bridge-v0.1.0`, local commit `d09816c06e3e6123b4aaefbe1d766f5e6c227304`.

## CPU XMem, TLS and isolation

- CPU-only research reported bidirectional aliases of 64 KiB backing pages, scatter mapping, five placeholder subranges and unmap/remap checks.
- TLS studies used independently compiled native modules and an isolated Unicorn model for `/GA`; this does not prove every native guest/host TLS interaction.
- Console allocation attribute values `0x280` and `0x380` remain semantically unresolved.
- Windows Sandbox was enabled and two offline synthetic-only sessions reportedly verified read-only input mapping, absence of external network paths and clean reset. This is **not a guarantee of security**, nor verification of RTX 3060 passthrough.

## Integrated Runtime v0.1

| Milestone | Reported evidence | Limit |
| --- | --- | --- |
| M1 | CPU alias → GPU; 6 runs, 72/72 readbacks, 1,179,648/1,179,648 uint32 words | Synthetic data |
| M1.1 | 6 runs, 240/240 readbacks, 3,932,160/3,932,160 words; direct/shadow fallback, fence identity and VA/PageId generation checks | Windows D3D12 profile |
| M2 | 6 runs, 576/576 readbacks, 3,556,728/3,556,728 words through own COM-like guest interface; typed/raw/structured SRVs | Not a verified Xbox ABI |
| M3 | Separately compiled synthetic x64 EXE + 2 DLL; a guest DLL reached GPU; M2/M1.1 regressions passed | PE mapping, relocations, imports and TLS done by native Windows loader |
| M4.1 | Manifest-controlled synthetic PE package; `EntryReached`, GDK call traced; missing provider diagnostic | No retail executable |
| M4.2 | Synthetic `XGameRuntimeInitialize` provider with absent/incompatible/valid outcomes; reached `XGameGetXboxTitleId` | No official console GDK |
| M4.3 | Mock TitleID read successful; next synthetic `XMemVirtualAlloc` form unsupported | Synthetic call order only |
| M4.4 | Guest `XMemVirtualAlloc` succeeds; next `GetProcAddress` for `XMemAllocatePhysicalPages` absent | Not retail semantics |
| M4.5 | Export `XMemAllocatePhysicalPages` succeeds, page count checked; next missing export is `XMemMapPhysicalPages` | Synthetic caller; M4.6 not completed |

**Last established local synthetic stop:** `GetProcAddress("XMemMapPhysicalPages")` → `NULL / ERROR_PROC_NOT_FOUND (127)`. This is **not a demonstrated crash in GTA VI**.

## Evidence maturity

1. **Published and inspectable now:** this ledger, original CB diagnostic proposal, summarized synthetic measurements and upstream links.
2. **Reported locally, not yet inspectable here:** source trees, CMake fixtures, raw GPU readbacks, scripts, acceptance JSON and full test logs.
3. **Externally reported:** partial retail-executable boot progress in community projects. We link sources in [UPSTREAM.md](UPSTREAM.md) and do not present their measurements as our own.
4. **Not established:** authenticity of alleged leak, playable GTA VI on Windows, a recognizable retail frame from our runtime, comprehensive console ABI or complete graphics/game functionality.

Updates must cite test provenance and keep these categories separate.
