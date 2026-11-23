# Leonida #8851 · Trace the 16 zero bytes in the second offscreen draw

**Request.** Capture one covered pixel of one draw by the second offscreen producer. Identify the last writer of its 16-byte color multiplier, follow that version into the pixel shader (PS), and follow the pixel through composition. A zero multiplier is an observation. Calling it incorrect requires independent evidence that this draw should receive a different value.

**Published baseline.** On the private Windows / RTX 5070 diagnostic build, the author reports 388 paired observations with the original shaders. In every pair the 96-byte constant buffer (CB) matched at draw recording and later GPU readback. The 16 color bytes were already zero at recording, while the trailing 48 bytes contained nonzero data. The pairs leave the writer and intended binding unidentified. Shader substitutions were separate runs. The RTX 3060 synthetic experiment informs the choice of probes only. [Leonida measurement](https://github.com/ProjectLeonida/Leonida/issues/8851#issuecomment-7113934937) · [Synthetic CB pipeline summary](CB-PIPELINE-SUMMARY.md)

## Minimum capture

Give the run a single ID and record the base commit, local patches, backend, GPU/driver and shader instrumentation. Key every observation to **frame, queue submission, command list and draw ordinal**, with the producer PSO/PS identifier. Give the later composition draw its own ID in the same frame and link it to the producer by resource and subresource. A shader replacement or replay gets a separate run ID.

| Observation | Record locally for this draw |
|---|---|
| 1 · CPU writer | Start at allocation or reuse. Record the last write overlapping the field, its event ID and writer site, backing ID/generation, field offset from the buffer view, and 16 bytes before/after. Include initialization and partial overwrites. If different events supplied the final bytes, retain their byte masks. Mark gaps in writer coverage as **unknown**. |
| 2 · Actual binding | Resolve the PS binding consumed by the submitted draw. Record the binding slot, descriptor version, backing ID/generation, view start and length, and effective field offset. Verify all 16 bytes fit. Join the resolved interval to the writer’s interval, including any intervening upload or shadow-copy mapping. |
| 3 · GPU-visible bytes | Snapshot the resolved 16 bytes in command order immediately before the draw, after the relevant write/copy and visibility dependency. Record the publication event, queue dependency and readback completion fence. Exclude intervening writes or reuse. If the writer targets another backing, capture its published range separately so a binding mismatch is distinguishable from stale data. |
| 4 · Shader operand | At a covered pixel, capture the four raw 32-bit words actually loaded by the PS, before multiplication or conversion. Record pixel/sample, binding and load offset. Compare bitwise with observation 3. Record an unavailable probe as **unknown**. |
| 5 · RT and backbuffer | Record the second render target (RT), subresource, format and pixel before/after this draw. Follow that version to the composition input, recording any intervening clear/write. Record the corresponding output coordinate, swapchain image index and pixel after composition, before Present. Account for sampling and format conversion when comparing colors. |

## How to read the first difference

| Same-draw observation | Supported conclusion |
|---|---|
| A traced CPU write leaves the field zero, the binding matches, and GPU and PS also read zero | Zero is present at that writer. Inspect the writer’s inputs to determine why. If the only known writer is initialization, later writer coverage determines whether that conclusion holds. |
| Writer and resolved binding point to different unmapped intervals or generations | The draw reads a different range/version. Establish which binding was intended before classifying it as a defect. |
| Linked CPU source is nonzero, but the synchronized GPU range is zero | The first difference lies between the observed write and GPU-visible storage. Use the copy range, version and ordering to separate missed publication from a later overwrite. |
| GPU range is nonzero, but the PS operand is zero | Inspect the effective offset, descriptor version and translated load. The snapshot alone cannot select among them. |
| PS reads the expected nonzero value, but the RT pixel stays black; or the RT has color and the corresponding backbuffer pixel is black | In the first case inspect the other operand, shader output, blend and write mask. In the second inspect the next writer, composition binding and sample mapping. Neither observation establishes how the CB became zero in another draw. |

**Return one finding.** Report the earliest observed zero, the first unequal pair (or “all observed values agree”), and any missing link. For a public issue reply, use run-local tokens, equality/zero flags, coverage and ordering results. Keep raw values, offsets and captures local. If writer coverage is incomplete, report “zero by draw recording; writer unknown.”
