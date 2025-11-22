---
title: What is an active cycle?
---

An active cycle is a clock cycle in which a
[Streaming Multiprocessor](/gpu-glossary/device-hardware/streaming-multiprocessor.md)
has at least one [active warp](/gpu-glossary/perf/warp-execution-state.md)
resident. The [warp](/gpu-glossary/device-software/warp.md) may be
[eligible](/gpu-glossary/perf/warp-execution-state.md) or
[stalled](/gpu-glossary/perf/warp-execution-state.md).

![All cycles depicted in this diagram are active cycles. Diagram inspired by the [*CUDA Techniques to Maximize Compute and Instruction Throughput*](https://www.nvidia.com/en-us/on-demand/session/gtc25-s72685/) talk at GTC 2025.](../resources/terminal-cycles.svg)
