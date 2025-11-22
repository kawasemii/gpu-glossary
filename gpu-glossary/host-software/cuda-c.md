---
title: What is the CUDA C++ programming language?
---

CUDA C++ is an implementation of the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md)
as an extension of the C++ programming language.

CUDA C++ adds several features to C++ to implement the
[CUDA programming model](/gpu-glossary/device-software/cuda-programming-model.md),
including:

- **[Kernel](/gpu-glossary/device-software/kernel.md) definition** with
  **`__global__`**. CUDA [kernels](/gpu-glossary/device-software/kernel.md) are
  implemented as C++ functions that take in pointers and have return type
  `void`, annotated with this keyword.
- **[Kernel](/gpu-glossary/device-software/kernel.md) launches** with **`<<<>>>`**.
  [Kernels](/gpu-glossary/device-software/kernel.md) are executed from the CPU host
  using a triple bracket syntax that sets the
  [thread block grid](/gpu-glossary/device-software/thread-block-grid.md)
  dimensions.
- **[Shared memory](/gpu-glossary/device-software/shared-memory.md) allocation**
  with the `shared` keyword, **barrier synchronization** with the
  `__syncthreads()` intrinsic function, and
  **[thread block](/gpu-glossary/device-software/thread-block.md)** and
  **[thread](/gpu-glossary/device-software/thread.md) indexing** with the
  `blockDim` and `threadIdx` built-in variables.

CUDA C++ programs are compiled by a combination of host C/C++ compiler drivers
like `gcc` and the
[NVIDIA CUDA Compiler Driver](/gpu-glossary/host-software/nvcc.md), `nvcc`.

For information on how to use CUDA C++ on [Modal](https://modal.com), see
[this guide](https://modal.com/docs/guide/cuda).
