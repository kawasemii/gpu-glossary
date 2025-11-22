---
title: What is the CUDA Memory Hierarchy?
---

![[Shared memory](/gpu-glossary/device-software/shared-memory.md) and [global memory](/gpu-glossary/device-software/global-memory.md) are two levels of the memory hierarchy in the [CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md) (left), mapping onto the [L1 data cache](/gpu-glossary/device-hardware/l1-data-cache.md) and [GPU RAM](/gpu-glossary/device-hardware/gpu-ram.md), respectively. Modified from diagrams in NVIDIA's [CUDA Refresher: The CUDA Programming Model](https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/) and the NVIDIA [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#programming-model).](../resources/terminal-cuda-programming-model.svg)

As part of the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md),
each level of the
[thread hierarchy](/gpu-glossary/device-software/thread-hierarchy.md) has access to
a distinct block of memory shared by all
[threads](/gpu-glossary/device-software/thread.md) in a group at that level: a
"memory hierarchy". This memory can be used for coordination and communication
and is managed by the programmer (not the hardware or a runtime).

For a [thread block grid](/gpu-glossary/device-software/thread-block-grid.md), that
shared memory is in the [GPU's RAM](/gpu-glossary/device-hardware/gpu-ram.md) and
is known as the [global memory](/gpu-glossary/device-software/global-memory.md).
Access to this memory can be coordinated with atomic operations and barriers,
but execution order across
[thread blocks](/gpu-glossary/device-software/thread-block.md) is indeterminate.

For a single [thread](/gpu-glossary/device-software/thread.md), the memory is a
chunk of the
[Streaming Multiprocessor's (SM's)](/gpu-glossary/device-hardware/streaming-multiprocessor.md)
[register file](/gpu-glossary/device-hardware/register-file.md). According to the
original semantics of the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md),
this memory is private to a [thread](/gpu-glossary/device-software/thread.md), but
certain instructions added to
[PTX](/gpu-glossary/device-software/parallel-thread-execution.md) and
[SASS](/gpu-glossary/device-software/streaming-assembler.md) to target matrix
multiplication on [Tensor Cores](/gpu-glossary/device-hardware/tensor-core.md)
share inputs and outputs across [threads](/gpu-glossary/device-software/thread.md).

In between, the [shared memory](/gpu-glossary/device-software/shared-memory.md) for
the [thread block](/gpu-glossary/device-software/thread-block.md) level of the
thread hierarchy is stored in the
[L1 data cache](/gpu-glossary/device-hardware/l1-data-cache.md) of each
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md). Careful management
of this cache — e.g. loading data into it to support the
[maximum number of arithmetic operations before new data is loaded](/gpu-glossary/perf/arithmetic-intensity.md)
— is key to the art of designing [high-performance](/gpu-glossary/perf.md) CUDA
[kernels](/gpu-glossary/device-software/kernel.md).
