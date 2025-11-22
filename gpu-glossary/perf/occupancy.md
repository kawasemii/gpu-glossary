---
title: What is occupancy?
---

Occupancy is the ratio of the
[active warps](/gpu-glossary/perf/warp-execution-state.md) to the maximum number of
[active warps](/gpu-glossary/perf/warp-execution-state.md) on a device.

![There are four warp slots per cycle on each of four clock cycles and so there are 16=4*4 total warp slots, and there are active warps in 15 of them, for an occupancy of ~94%. Diagram inspired by the [*CUDA Techniques to Maximize Compute and Instruction Throughput*](https://www.nvidia.com/en-us/on-demand/session/gtc25-s72685/) talk at GTC 2025.](../resources/terminal-cycles.svg)

There are two types of occupancy measurements:

- _Theoretical Occupancy_ represents the upper limit for occupancy due to the
  kernel launch configuration and device capabilities.
- _Achieved Occupancy_ measures the actual occupancy during
  [kernel](/gpu-glossary/device-software/kernel.md) execution, aka on
  [active cycles](/gpu-glossary/perf/active-cycle.md).

As part of the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md),
all the [threads](/gpu-glossary/device-software/thread.md) in a
[thread block](/gpu-glossary/device-software/thread-block.md) are scheduled onto
the same
[Streaming Multiprocessor (SM)](/gpu-glossary/device-hardware/streaming-multiprocessor.md).
Each [SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md) has resources
(like space in [shared memory](/gpu-glossary/device-software/shared-memory.md))
that must be partitioned across
[thread blocks](/gpu-glossary/device-software/thread-block.md) and so limit the
number of [thread blocks](/gpu-glossary/device-software/thread-block.md) that can
be scheduled on the
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md).

Let's work through an example. Consider an NVIDIA H100 GPU, which has these
specifications:

```
Maximum warps/SM: 64
Maximum blocks/SM: 32
(32 bit) Registers: 65536
Shared memory (smem): 228 KB
```

For a [kernel](/gpu-glossary/device-software/kernel.md) using 32
[threads](/gpu-glossary/device-software/thread.md) per
[thread block](/gpu-glossary/device-software/thread-block.md), 8
[registers](/gpu-glossary/device-software/registers.md) per
[thread](/gpu-glossary/device-software/thread.md), and 12 KB
[shared memory](/gpu-glossary/device-software/shared-memory.md) per
[thread block](/gpu-glossary/device-software/thread-block.md), we end up limited by
[shared memory](/gpu-glossary/device-software/shared-memory.md):

```
64 > 1   = warps/block = 32 threads/block ÷ 32 threads/warp
32 < 256 = blocks/register-file = 65,536 registers/register-file ÷ (32 threads/block × 8 registers/thread)
32       = blocks/SM
19       = blocks/smem = 228 KB/smem ÷ 12 KB/block
```

Even though our [register file](/gpu-glossary/device-hardware/register-file.md) is
big enough to support 256
[thread blocks](/gpu-glossary/device-software/thread-block.md) concurrently, our
[shared memory](/gpu-glossary/device-software/shared-memory.md) is not, and so we
can only run 19 [thread blocks](/gpu-glossary/device-software/thread-block.md) per
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md), corresponding to
19 [warps](/gpu-glossary/device-software/warp.md). This is the common case where
the size of program intermediates stored in
[registers](/gpu-glossary/device-software/registers.md) is much smaller than
elements of the program's
[working set](https://en.wikipedia.org/wiki/Working_set) that need to stay in
[shared memory](/gpu-glossary/device-software/shared-memory.md).

Low occupancy can hurt performance when there aren't enough
[eligible warps](/gpu-glossary/perf/warp-execution-state.md) to
[hide the latency](/gpu-glossary/perf/latency-hiding.md) of instructions, which
shows up as low instruction
[issue efficiency](/gpu-glossary/perf/issue-efficiency.md) and
[under-utilized pipes](/gpu-glossary/perf/pipe-utilization.md). However, once
occupancy is sufficient for [latency hiding](/gpu-glossary/perf/latency-hiding.md),
increasing it further may actually degrade performance. Higher occupancy reduces
resources per [thread](/gpu-glossary/device-software/thread.md), potentially
[bottlenecking the kernel on registers](/gpu-glossary/perf/register-pressure.md) or
reducing the [arithmetic intensity](/gpu-glossary/perf/arithmetic-intensity.md)
that modern GPU architectures are designed to exploit.

More generally, occupancy measures what fraction of its maximum parallel tasks
the GPU is handling simultaneously, which is not inherently a target of
optimization in most kernels. Instead, we want to maximize the
[utilization](/gpu-glossary/perf/pipe-utilization.md) of compute resources if we
are [compute-bound](/gpu-glossary/perf/compute-bound.md) or memory resources if we
are [memory-bound](/gpu-glossary/perf/memory-bound.md).

In particular, high-performance GEMM kernels on Hopper and Blackwell
[architecture](/gpu-glossary/device-hardware/streaming-multiprocessor-architecture.md)
GPUs often run at single-digit occupancy percentages because they don't need
many [warps](/gpu-glossary/device-software/warp.md) to fully saturate the
[Tensor Cores](/gpu-glossary/device-hardware/tensor-core.md).
