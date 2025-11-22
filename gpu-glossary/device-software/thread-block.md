---
title: What is a CUDA Thread Block?
---

![Thread blocks are an intermediate level of the thread group hierarchy of the [CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md) (left). A thread block executes on a single [Streaming Multiprocessor](/gpu-glossary/device-hardware/streaming-multiprocessor.md) (right, middle). Modified from diagrams in NVIDIA's [CUDA Refresher: The CUDA Programming Model](https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/) and the NVIDIA [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#programming-model).](../resources/terminal-cuda-programming-model.svg)

A thread block is a level of the
[CUDA programming model's](/gpu-glossary/device-software/cuda-programming-model.md)
[thread hierarchy](/gpu-glossary/device-software/thread-hierarchy.md) below a
[grid](/gpu-glossary/device-software/thread-block-grid.md) but above a
[thread](/gpu-glossary/device-software/thread.md). It is the
[CUDA programming model's](/gpu-glossary/device-software/cuda-programming-model.md)
abstract equivalent of the concrete
[cooperative thread arrays](/gpu-glossary/device-software/cooperative-thread-array.md)
in
[PTX](/gpu-glossary/device-software/parallel-thread-execution.md)/[SASS](/gpu-glossary/device-software/streaming-assembler.md).

Blocks are the smallest unit of thread coordination exposed to programmers in
the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md).
Blocks must execute independently, so that any execution order for blocks is
valid, from fully serial in any order to all interleavings.

A single CUDA [kernel](/gpu-glossary/device-software/kernel.md) launch produces one
or more thread blocks (in the form of a
[thread block grid](/gpu-glossary/device-software/thread-block-grid.md)), each of
which contains one or more [warps](/gpu-glossary/device-software/warp.md). Blocks
can be arbitrarily sized, but they are typically multiples of the
[warp](/gpu-glossary/device-software/warp.md) size (32 on all current CUDA GPUs).
