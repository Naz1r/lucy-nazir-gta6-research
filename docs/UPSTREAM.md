# Public upstream research and relevant measurements

**Reference snapshot:** 24 November 2026. External measurements below are attributed to their authors; they are not results of Lucy × Nazir tests. Technical discussions are fast-moving: inspect each linked thread and revision before reusing a conclusion.

## Major projects

| Project | Public entry point | Research area |
| --- | --- | --- |
| Leonida | [ProjectLeonida/Leonida](https://github.com/ProjectLeonida/Leonida) | Xbox Scarlett-to-Windows compatibility and D3D12.X/Vulkan graphics; reports of partial title execution |
| AnyXboxX | [LooperSalty/AnyXboxX](https://github.com/LooperSalty/AnyXboxX) | Native Xbox API shims, XMem and startup experiments |
| Xbox2Windows | [R0Y00/Xbox2Windows](https://github.com/R0Y00/Xbox2Windows) | PE startup, TLS, shared page mappings and compatibility research |
| TomBox | [omonkeyz/xbox-emulator](https://github.com/omonkeyz/xbox-emulator) | Xbox-specific D3D12 interface coverage and emulation experiments |

**Names and source code belong to their respective projects.** Do not copy between repositories without checking licences and obligations.

## First-frame / black-frame research

- [Leonida boot coordination issue #8851](https://github.com/ProjectLeonida/Leonida/issues/8851): title boot, drawing, submission, presentation, and blocked rendering paths. It is a living discussion, not a stable benchmark.
- [RTX 5070, second-offscreen-producer observations](https://github.com/ProjectLeonida/Leonida/issues/8851#issuecomment-7113934937): the 16-byte color multiplier was already zero at draw recording in 388 paired measurements; the writer and intended CB binding were not established.
- [RDNA2, capture of 162 black presenter inputs](https://github.com/ProjectLeonida/Leonida/issues/8851#issuecomment-7114806506): a controlled presenter readback produced uniformly black images, so the issue was not merely screenshot extraction.
- [Shader index and earlier failed submissions](https://github.com/ProjectLeonida/Leonida/issues/8851#issuecomment-7112986626): a missing shader-index step suppressed draws; a subsequent run had unknown-resource failures on command-list submission. **These are different stacks and should not be combined into one run.**
- [Frame-event fix and successful PresentX reports](https://github.com/ProjectLeonida/Leonida/issues/8851#issuecomment-7114186263): reports of more than 1,024 successful presents and 2,392 successful submissions on one Wine/NVIDIA setup; pixel contents were **not** captured in that run.
- [Presentation window routing](https://github.com/ProjectLeonida/Leonida/pull/8591#issuecomment-7115106707): a hidden-window routing problem was diagnosed separately from black frame contents.

The community has reported partial execution, synthetic graphics validations, black frames, and in some threads a nonuniform presentation output. None of these is evidence that **our** runtime executes the retail title or that an independently verified, recognizable GTA VI scene is playable.

## Memory and loader references

- [AnyXboxX PR #107](https://github.com/LooperSalty/AnyXboxX/pull/107): XMem allocation forms and exports; the PR itself documents aliasing/large-page limitations.
- [Xbox2Windows PR #930](https://github.com/R0Y00/Xbox2Windows/pull/930): a source-built EXE and bundled-DLL dependency-loading test.
- [Xbox2Windows PR #934](https://github.com/R0Y00/Xbox2Windows/pull/934): shared physical-page backing with mapping and lifetime controls.
- [Xbox2Windows PR #935](https://github.com/R0Y00/Xbox2Windows/pull/935): synthetic XMemTransferMemory contract tests, **not** independently established console semantics.
- [TomBox PR #296](https://github.com/omonkeyz/xbox-emulator/pull/296): console command-list IID and high-slot experiments; a working implementation of every unknown method is not established.

## Interpreting evidence

- **Observed on a real hardware run** is stronger than *statically inferred* but still depends on its exact test stack.
- **Synthetic acceptance tests** are valuable for validating an adapter, not for proving the console's private ABI.
- **A black presented frame** does not establish why its data is black, whether it is intentional, or whether gameplay is imminent.
- **A recorded draw** is not necessarily a successfully submitted draw; **a successful Present** is not necessarily a recognizable game image.
- Avoid repeating uncertain leaked-file provenance as authenticated origin.

For the related **synthetic-only** diagnostic proposal, see [LEONIDA-CB-DIAGNOSTIC-SPEC.md](LEONIDA-CB-DIAGNOSTIC-SPEC.md).
