---
title: What is a Thread Block Grid?
---

![Thread block grids are the highest level of the thread group hierarchy of the [CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md) (left). They map onto multiple [Streaming Multiprocessors](/gpu-glossary/device-hardware/streaming-multiprocessor.md) (right, bottom). Modified from diagrams in NVIDIA's [CUDA Refresher: The CUDA Programming Model](https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/) and the NVIDIA [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#programming-model).](../resources/terminal-cuda-programming-model.svg)

When a CUDA [kernel](/gpu-glossary/device-software/kernel.md) is launched, it
creates a collection of [threads](/gpu-glossary/device-software/thread.md) known as
a thread block grid. Grids can be one, two, or three dimensional. They are made
up of [thread blocks](/gpu-glossary/device-software/thread-block.md).

The matching level of the
[memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md) is the
[global memory](/gpu-glossary/device-software/global-memory.md).

[Thread blocks](/gpu-glossary/device-software/thread-block.md) are effectively
independent units of computation. They execute concurrently, that is, with
indeterminate order, ranging from fully sequentially in the case of a GPU with a
single
[Streaming Multiprocessor](/gpu-glossary/device-hardware/streaming-multiprocessor.md)
to fully in parallel when run on a GPU with sufficient resources to run them all
simultaneously.
