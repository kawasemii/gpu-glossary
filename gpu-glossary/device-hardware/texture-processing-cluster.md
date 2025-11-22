---
title: What is a Texture Processing Cluster?
abbreviation: TPC
---

A Texture Processing Cluster (TPC) is a pair of adjacent
[Streaming Multiprocessors (SMs)](/gpu-glossary/device-hardware/streaming-multiprocessor.md).

Before the Blackwell
[SM architecture](/gpu-glossary/device-hardware/streaming-multiprocessor-architecture.md),
TPCs were not mapped onto any level of the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md)'s
[memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md) or
[thread hierarchy](/gpu-glossary/device-software/thread-hierarchy.md).

The fifth-generation [Tensor Cores](/gpu-glossary/device-hardware/tensor-core.md)
in the Blackwell
[SM architecture](/gpu-glossary/device-hardware/streaming-multiprocessor-architecture.md)
added the "CTA pair" level of the
[Parallel Thread eXecution (PTX)](/gpu-glossary/device-software/parallel-thread-execution.md)
[thread hierarchy](/gpu-glossary/device-software/thread-hierarchy.md), which maps
onto TPCs. Many `tcgen05`
[PTX](/gpu-glossary/device-software/parallel-thread-execution.md) instructions
include a `.cta_group` field that can use a single
[SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md) (`.cta_group::1`)
or a pair of [SMs](/gpu-glossary/device-hardware/streaming-multiprocessor.md) in a
TPC (`::2`), which are mapped to `1SM` and `2SM` variants of
[Streaming Assembler (SASS)](/gpu-glossary/device-software/streaming-assembler.md)
instructions like `MMA`.
