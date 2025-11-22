---
title: What is register pressure?
---

Register pressure is a colorful term used when the
[register file](/gpu-glossary/device-hardware/register-file.md) is a
[bottleneck](/gpu-glossary/perf/performance-bottleneck.md).

[Registers](/gpu-glossary/device-software/registers.md) in the
[Parallel Thread eXecution (PTX)](/gpu-glossary/device-software/parallel-thread-execution.md)
language are virtual and unlimited, but the
[register files](/gpu-glossary/device-hardware/register-file.md) of the
[Streaming Multiprocessor (SM)](/gpu-glossary/device-hardware/streaming-multiprocessor.md)
are physical and so limited.

The amount of space in the
[register file](/gpu-glossary/device-hardware/register-file.md) consumed by a
[thread](/gpu-glossary/device-software/thread.md) is determined by the
[Streaming ASSembler (SASS)](/gpu-glossary/device-software/streaming-assembler.md)
code for the [kernel](/gpu-glossary/device-software/kernel.md), and since all
[threads](/gpu-glossary/device-software/thread.md) in a
[thread block](/gpu-glossary/device-software/thread-block.md) are scheduled onto
the same [SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md), the total
space required by a [thread block](/gpu-glossary/device-software/thread-block.md)
is determined also by the [kernel](/gpu-glossary/device-software/kernel.md) launch
configuration. As the space allocated per
[thread block](/gpu-glossary/device-software/thread-block.md) increases, fewer
[thread blocks](/gpu-glossary/device-software/thread-block.md) can be scheduled
onto the same [SM](/gpu-glossary/device-hardware/streaming-multiprocessor.md),
reducing [occupancy](/gpu-glossary/perf/occupancy.md) and making it more difficult
to [hide latency](/gpu-glossary/perf/latency-hiding.md).

See
[this excellent article by SemiAnalysis](https://semianalysis.com/2025/06/23/nvidia-tensor-core-evolution-from-volta-to-blackwell/)
for an account of the relationship between register pressure and key features
added in recent
[Streaming Multiprocessor architectures](/gpu-glossary/device-hardware/streaming-multiprocessor-architecture.md),
like asynchronous copies (added in Ampere), the
[Tensor Memory Accelerator](/gpu-glossary/device-hardware/tensor-memory-accelerator.md)
(TMA, added in Hopper), and
[tensor memory](/gpu-glossary/device-hardware/tensor-memory.md) (added in
Blackwell).

Register pressure also occurs in CPUs, where similar register
[bottlenecks](/gpu-glossary/perf/performance-bottleneck.md) limit the degree to
which loops can be
[strip-mined during auto-vectorization](https://hogback.atmos.colostate.edu/rr/old/tidbits/intel/macintel/doc_files/source/extfile/optaps_for/common/optaps_vec_mine.htm).
