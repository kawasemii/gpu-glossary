---
title: What is a Register File?
---

The register file of the
[Streaming Multiprocessor](/gpu-glossary/device-hardware/streaming-multiprocessor.md)
is the primary store of bits in between their manipulation by the
[cores](/gpu-glossary/device-hardware/core.md).

![The internal architecture of an H100 SM. The register file is depicted in blue. Modified from NVIDIA's [H100 white paper](https://modal-cdn.com/gpu-glossary/gtc22-whitepaper-hopper.pdf)([local copy here](../resources/gtc22-whitepaper-hopper.pdf)).](../resources/terminal-gh100-sm.svg)

Like registers in CPUs, these registers are made from very fast memory
technology that can keep pace with the compute
[cores](/gpu-glossary/device-hardware/core.md), about an order of magnitude faster
than the [L1 data cache](/gpu-glossary/device-hardware/l1-data-cache.md).

The register file is split into 32 bit registers that can be dynamically
reallocated between different data types, like 32 bit integers, 64 bit floating
point numbers, and (groups of) 16 bit or smaller floating point numbers. These
physical registers back the
[virtual registers](/gpu-glossary/device-software/registers.md) in the
[Parallel Thread eXecution (PTX)](/gpu-glossary/device-software/parallel-thread-execution.md)
intermediate representation.

Allocation of physical registers to
[threads](/gpu-glossary/device-software/thread.md) in
[Streaming Assembler (SASS)](/gpu-glossary/device-software/streaming-assembler.md)
is managed by a compiler like `ptxas`, which optimizes register file usage by
[thread blocks](/gpu-glossary/device-software/thread-block.md). If each
[thread block](/gpu-glossary/device-software/thread-block.md) consumes too much of
the register file (colloquially, high
"[register pressure](/gpu-glossary/perf/register-pressure.md)"), then the number of
concurrently schedulable [threads](/gpu-glossary/device-software/thread.md) will be
reduced, leading to a low [occupancy](/gpu-glossary/perf/occupancy.md) and possibly
impacting performance by reducing opportunities for
[latency hiding](/gpu-glossary/perf/latency-hiding.md).
