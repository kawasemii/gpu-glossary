---
title: What are Registers?
---

![Registers are the memory of the [memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md) associated with individual [threads](/gpu-glossary/device-software/thread.md) (left). Modified from diagrams in NVIDIA's [CUDA Refresher: The CUDA Programming Model](https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/) and the NVIDIA [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#programming-model).](../resources/terminal-cuda-programming-model.svg)

At the lowest level of the
[memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md) are the
registers, which store information manipulated by a single
[thread](/gpu-glossary/device-software/thread.md).

The values in registers are generally stored in the
[register file](/gpu-glossary/device-hardware/register-file.md) of the
[Streaming Multiprocessor (SM)](/gpu-glossary/device-hardware/streaming-multiprocessor.md),
but they can also spill to the
[global memory](/gpu-glossary/device-software/global-memory.md) in the
[GPU RAM](/gpu-glossary/device-hardware/gpu-ram.md) at a substantial performance
penalty.

As when programming CPUs, these registers are not directly manipulated by
high-level languages like [CUDA C](/gpu-glossary/host-software/cuda-c.md). They are
only visible to a lower-level language, here
[Parallel Thread Execution (PTX)](/gpu-glossary/device-software/parallel-thread-execution.md).
They are typically managed by a compiler like `ptxas`. Among the compiler's
goals is to limit the register space used by each
[thread](/gpu-glossary/device-software/thread.md) so that more
[thread blocks](/gpu-glossary/device-software/thread-block.md) can be
simultaneously scheduled into a single
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md), increasing
[occupancy](/gpu-glossary/perf/occupancy.md).

The registers used in the
[PTX](/gpu-glossary/device-software/parallel-thread-execution.md) instruction set
architecture are documented
[here](https://docs.nvidia.com/cuda/parallel-thread-execution/#register-state-space).
The registers used in [SASS](/gpu-glossary/device-software/streaming-assembler.md)
are not, to our knowledge, documented.
