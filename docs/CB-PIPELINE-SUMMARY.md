# Synthetic constant-buffer publication and binding diagnostics

**Status:** local experimental results **reported by Codex**, 23 November 2026. The full source and raw test logs are not yet published or independently audited in this repository.

This experiment is a *diagnostic control*, not an experiment on GTA VI or its shaders.

## Setup

- Device: NVIDIA GeForce RTX 3060.
- API: native Windows D3D12, Debug Layer enabled.
- Test structure: two constant buffers (A and B), an offscreen color producer, a final backbuffer composition, a shader-read probe and independent CPU expected values.
- CB A: updated from a black multiplier to a purple control value.
- CB B: fixed green control to identify a wrong descriptor.
- Two series of three hardware runs; 42 generated test frames and 144/144 reported fence waits.
- D3D12 Debug Layer: 0 reported errors and warnings.
- Reported `Present` calls: `S_OK` for all 42 frames; the final backbuffer was inspected **before** Present, and monitor display was not checked.

## Observed cases

| Controlled condition | First observed mismatch | Expected visible result |
| --- | --- | --- |
| Correct publication and correct CBV | None; the shader reads A and the exact updated 16 color bytes | Purple |
| Skip publication after CPU writes A | GPU-visible A remains the old black multiplier after synchronization; CPU A is purple | Black |
| Swap CBV binding to neighbor B | Both GPU buffers contain correct contents; shader instead reads B and green | Green |

The experiment reported pixel-perfect CPU-reference matching for all 192 pixels of the relevant offscreen and final buffers when the path was correct. Two runs of the test series reportedly produced identical results.

## What this tells us — and what it does not

The measurements show that **these two synthetic failure modes can be distinguished** if the investigator captures each successive value and resource identity. They do **not** establish why a particular GTA VI offscreen shader saw zero color bytes. A zero color multiplier might be intentional (for example, an early fade); wrong binding, missing publication and earlier initialization are alternative hypotheses.

For a compatible, privacy-preserving diagnostic request addressing the published Leonida observation, see [the full trace proposal](LEONIDA-CB-DIAGNOSTIC-SPEC.md). Original community observation: [Leonida #8851](https://github.com/ProjectLeonida/Leonida/issues/8851#issuecomment-7113934937).

**Publication boundary:** no game-specific binary data, constants from proprietary files, logs, addresses or screenshots are included.
