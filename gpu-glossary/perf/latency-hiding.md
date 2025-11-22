---
title: What is latency hiding?
---

Latency hiding is a strategy to mask long-latency operations by
[running many of them concurrently](/gpu-glossary/perf/littles-law.md).

Performant GPU programs hide latency by interleaving the execution of many
[threads](/gpu-glossary/device-software/thread.md). This allows programs to
maintain high throughput despite long instruction latencies. When one
[warp stalls](/gpu-glossary/perf/warp-execution-state.md) on a slow memory
operation, the GPU immediately switches to execute instructions from another
[eligible warp](/gpu-glossary/perf/warp-execution-state.md).

This keeps all execution units busy concurrently. While one
[warp](/gpu-glossary/device-software/warp.md) uses
[Tensor Cores](/gpu-glossary/device-hardware/tensor-core.md) for matrix
multiplication, another might execute arithmetic on
[CUDA Cores](/gpu-glossary/device-hardware/cuda-core.md) (say,
[quantizing or dequantizing matrix multiplicands](https://arxiv.org/abs/2408.11743)),
and a third could be fetching data through the
[load/store units](/gpu-glossary/device-hardware/load-store-unit.md).

Concretely, consider the following simple instruction sequence in
[Streaming Assembler](/gpu-glossary/device-software/streaming-assembler.md).

```nasm
LDG.E.SYS R1, [R0]        // memory load, 400 cycles
IMUL R2, R1, 0xBEEF       // integer multiply, 6 cycles
IADD R4, R2, 0xAFFE       // integer add, 4 cycles
IMUL R6, R4, 0x1337       // integer multiply, 6 cycles
```

Executed sequentially, this would take 416 cycles to complete. We can hide this
latency by operating concurrently. If we assume we can issue one instruction
every cycle, then, by [Little's Law](/gpu-glossary/perf/littles-law.md), if we run
416 concurrent [threads](/gpu-glossary/device-software/thread.md), we can still
finish the sequence once per cycle (on average), hiding the latency of memory
from consumers of the data in `R6`.

Note that [threads](/gpu-glossary/device-software/thread.md) are not the unit of
instruction issuance, [warps](/gpu-glossary/device-software/warp.md) are. Each
[warp](/gpu-glossary/device-software/warp.md) contains 32
[threads](/gpu-glossary/device-software/thread.md), and so our fragment requires
416 ÷ 32 = 13 [warps](/gpu-glossary/device-software/warp.md). When successfully
hiding latency, the GPU's scheduling system maintains this many
[warps](/gpu-glossary/device-software/warp.md) in flight, switching between them
whenever one stalls, ensuring the execution units never idle while waiting for
slow operations to complete.

For a deep dive into latency hiding on
pre-[Tensor Core](/gpu-glossary/device-hardware/tensor-core.md) GPUs, see
[Vasily Volkov's PhD thesis](https://arxiv.org/abs/2206.02874).
