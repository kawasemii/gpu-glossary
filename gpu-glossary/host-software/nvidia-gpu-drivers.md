---
title: What are the NVIDIA GPU Drivers?
---

The NVIDIA GPU drivers mediate the interaction between host programs or the host
operating system and the GPU device. The primary interfaces to the GPU drivers
for applications are, in order, the
[CUDA Runtime API](/gpu-glossary/host-software/cuda-runtime-api.md) and the
[CUDA Driver API](/gpu-glossary/host-software/cuda-driver-api.md).

![The CUDA Toolkit. The NVIDIA GPU Driver is the only component that communicates directly with the GPU. Adapted from the *Professional CUDA C Programming Guide*.](../resources/terminal-cuda-toolkit.svg)

NVIDIA has released the
[source](https://github.com/NVIDIA/open-gpu-kernel-modules) for their Linux Open
GPU [Kernel Module](/gpu-glossary/host-software/nvidia-ko.md).
