---
title: What is Shared Memory?
---

![Shared memory is the abstract memory associated with the [thread block](/gpu-glossary/device-software/thread-block.md) level (left, center) of the CUDA thread group hierarchy (left). Modified from diagrams in NVIDIA's [CUDA Refresher: The CUDA Programming Model](https://developer.nvidia.com/blog/cuda-refresher-cuda-programming-model/) and the NVIDIA [CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html#programming-model).](../resources/terminal-cuda-programming-model.svg)

Shared memory is the level of the
[memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md) corresponding
to the [thread block](/gpu-glossary/device-software/thread-block.md) level of the
[thread hierarchy](/gpu-glossary/device-software/thread-hierarchy.md) in the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md).
It is generally expected to be much smaller but much faster (in throughput and
latency) than the [global memory](/gpu-glossary/device-software/global-memory.md).

A fairly typical [kernel](/gpu-glossary/device-software/kernel.md) therefore looks
something like this:

- load data from [global memory](/gpu-glossary/device-software/global-memory.md)
  into shared memory
- perform a number of arithmetic operations on that data via the
  [CUDA Cores](/gpu-glossary/device-hardware/cuda-core.md) and
  [Tensor Cores](/gpu-glossary/device-hardware/tensor-core.md)
- optionally, synchronize [threads](/gpu-glossary/device-software/thread.md) within
  a [thread block](/gpu-glossary/device-software/thread-block.md) by means of
  barriers while performing those operations
- write data back into
  [global memory](/gpu-glossary/device-software/global-memory.md), optionally
  preventing races across
  [thread blocks](/gpu-glossary/device-software/thread-block.md) by means of
  atomics

Shared memory is stored in the
[L1 data cache](/gpu-glossary/device-hardware/l1-data-cache.md) of the GPU's
[Streaming Multiprocessor (SM)](/gpu-glossary/device-hardware/streaming-multiprocessor.md).
