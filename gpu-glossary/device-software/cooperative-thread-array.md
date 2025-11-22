---
title: What is a Cooperative Thread Array?
---

![Cooperative thread arrays correspond to the [thread block](/gpu-glossary/device-software/thread-block.md) level of the thread block hierarchy in the [CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md). Modified from diagrams in NVIDIA's [CUDA Refresher: The CUDA Programming Model](https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/) and the NVIDIA [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#programming-model).](../resources/terminal-cuda-programming-model.svg)

A cooperative thread array (CTA) is a collection of threads scheduled onto the
same
[Streaming Multiprocessor (SM)](/gpu-glossary/device-hardware/streaming-multiprocessor.md).
CTAs are the
[PTX](/gpu-glossary/device-software/parallel-thread-execution.md)/[SASS](/gpu-glossary/device-software/streaming-assembler.md)
implementation of the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md)'s
[thread blocks](/gpu-glossary/device-software/thread-block.md). CTAs are composed
of one or more [warps](/gpu-glossary/device-software/warp.md).

Programmers can direct [threads](/gpu-glossary/device-software/thread.md) within a
CTA to coordinate with each other. The programmer-managed
[shared memory](/gpu-glossary/device-software/shared-memory.md), in the
[L1 data cache](/gpu-glossary/device-hardware/l1-data-cache.md) of the
[SMs](/gpu-glossary/device-hardware/streaming-multiprocessor.md), makes this
coordination fast. Threads in different CTAs cannot coordinate with each other
via barriers, unlike threads within a CTA, and instead must coordinate via
[global memory](/gpu-glossary/device-software/global-memory.md), e.g. via atomic
update instructions. Due to driver control over the scheduling of CTAs at
runtime, CTA execution order is indeterminate and blocking a CTA on another CTA
can easily lead to deadlock.

The number of CTAs that can be scheduled onto a single
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md) sets the
[achievable occupancy](/gpu-glossary/perf/occupancy.md) and depends on a number of
factors. Fundamentally, the
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md) has a limited set
of resources — lines in the
[register file](/gpu-glossary/device-hardware/register-file.md), "slots" for
[warps](/gpu-glossary/device-software/warp.md), bytes of
[shared memory](/gpu-glossary/device-software/shared-memory.md) in the
[L1 data cache](/gpu-glossary/device-hardware/l1-data-cache.md) — and each CTA uses
a certain amount of those resources (as calculated at
[compile](/gpu-glossary/host-software/nvcc.md) time) when scheduled onto an
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md).
