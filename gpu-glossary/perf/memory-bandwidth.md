---
title: What is memory bandwidth?
---

Memory bandwidth is the maximum rate at which data can be transferred between
different levels of the
[memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md).

It represents the theoretical maximum achievable throughput for moving data in
bytes per second. It determines the slope of the "memory roof" in a
[roofline model](/gpu-glossary/perf/roofline-model.md) of the hardware.

There are many memory bandwidths in a complete system — one between each level
of the [memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md).

The most important bandwidth is that between the
[GPU RAM](/gpu-glossary/device-hardware/gpu-ram.md) and the
[register files](/gpu-glossary/device-hardware/register-file.md) of the
[Streaming Multiprocessors (SMs)](/gpu-glossary/device-hardware/streaming-multiprocessor.md),
because the [working sets](https://en.wikipedia.org/wiki/Working_set_size) of
most [kernels](/gpu-glossary/device-software/kernel.md) only fit in
[GPU RAM](/gpu-glossary/device-software/memory-hierarchy.md), not anywhere higher
up in the [memory hierarchy](/gpu-glossary/device-software/memory-hierarchy.md). It
is for this reason that that bandwidth is the primary one used in
[roofline modeling](/gpu-glossary/perf/roofline-model.md) of GPU
[kernel](/gpu-glossary/device-software/kernel.md) performance.

Contemporary GPUs have memory bandwidths measured in terabytes per second. For
example, [B200 GPUs](https://modal.com/blog/introducing-b200-h200) have a
(bidirectional) memory bandwidth of 8 TB/sec to their HBM3e memory. This is much
lower than the [arithmetic bandwidth](/gpu-glossary/perf/arithmetic-bandwidth.md)
of the [Tensor Cores](/gpu-glossary/device-hardware/tensor-core.md) in these GPUs,
leading to increased [ridge point](/gpu-glossary/perf/roofline-model.md)
[arithmetic intensity](/gpu-glossary/perf/arithmetic-intensity.md).

Representative bandwidth numbers for NVIDIA data center GPUs between the Ampere
and Blackwell
[Streaming Multiprocessor architectures](/gpu-glossary/device-hardware/streaming-multiprocessor-architecture.md)
are listed in the table below.

| **System (Compute / Memory)**                                                                                                                               | **[Arithmetic Bandwidth](/gpu-glossary/perf/arithmetic-bandwidth.md) (TFLOPs/s)** | **Memory Bandwidth (TB/s)** | **[Ridge Point](/gpu-glossary/perf/roofline-model.md) (FLOPs/byte)** |
| :---------------------------------------------------------------------------------------------------------------------------------------------------------- | -----------------------------------------------------------------------------: | --------------------------: | ----------------------------------------------------------------: |
| [A100 80GB SXM BF16 TC / HBM2e](https://www.nvidia.com/content/dam/en-zz/Solutions/Data-Center/a100/pdf/nvidia-a100-datasheet-us-nvidia-1758950-r4-web.pdf) ([local copy here](../resources/nvidia-a100-datasheet-nvidia-us-2188504-web.pdf) |                                                                            312 |                           2 |                                                               156 |
| [H100 SXM BF16 TC / HBM3](https://resources.nvidia.com/en-us-gpu-resources/h100-datasheet-24306)                                                            |                                                                            989 |                        3.35 |                                                               295 |
| [B200 BF16 TC / HBM3e](https://resources.nvidia.com/en-us-dgx-systems/dgx-b200-datasheet)                                                                   |                                                                           2250 |                           8 |                                                               281 |
| [H100 SXM FP8 TC / HBM3](https://resources.nvidia.com/en-us-gpu-resources/h100-datasheet-24306)                                                             |                                                                           1979 |                        3.35 |                                                               592 |
| [B200 FP8 TC / HBM3e](https://resources.nvidia.com/en-us-dgx-systems/dgx-b200-datasheet)                                                                    |                                                                           4500 |                           8 |                                                               562 |
| [B200 FP4 TC / HBM3e](https://resources.nvidia.com/en-us-dgx-systems/dgx-b200-datasheet)                                                                    |                                                                           9000 |                           8 |                                                              1125 |
