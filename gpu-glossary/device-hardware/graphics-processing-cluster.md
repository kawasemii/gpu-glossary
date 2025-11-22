---
title: What is a Graphics/GPU Processing Cluster?
abbreviation: GPC
---

A GPC is a collection of
[Texture Processing Clusters (TPCs)](/gpu-glossary/device-hardware/texture-processing-cluster.md)
(themselves groups of
[Streaming Multiprocessors](/gpu-glossary/device-hardware/streaming-multiprocessor.md)
or SMs) plus a raster engine. Apparently, some people use NVIDIA GPUs for
graphics, for which the raster engine is important. Relatedly, the name used to
stand for Graphics Processing Cluster, but is now, e.g. in the
[NVIDIA CUDA C++ Programming Guide](https://docs.nvidia.com/cuda/cuda-c-programming-guide/index.html),
expanded as "GPU Processing Cluster".

Since the introduction of
[compute capability](/gpu-glossary/device-software/compute-capability.md) 9.0 GPUs
like H100s, there is an additional layer of the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md)'s
[thread hierarchy](/gpu-glossary/device-software/thread-hierarchy.md), a "cluster"
of [thread blocks](/gpu-glossary/device-software/thread-block.md) that are
scheduled onto the same GPC, just as the threads of a
[thread block](/gpu-glossary/device-software/thread-block.md) are scheduled onto
the same [SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md), and have
their own level of the
[memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md), distributed
shared memory. Elsewhere, we elide discussion of this feature.
