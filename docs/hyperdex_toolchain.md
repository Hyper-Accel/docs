# HyperDex Toolchain

The HyperDex Toolchain is a Python SDK designed to fully optimize AI workloads on an LPU (Linear Processing Unit).

It combines a compiler, runtime, and device drivers into one integrated package, allowing developers to efficiently run large language models (LLMs) on FPGA/LPU hardware.

XRT (Xilinx Runtime) is required for basic FPGA communication.
HyperDex Toolchain is built specifically for LPU hardware, enabling maximum AI performance through model optimization and hardware acceleration.

All processes — device initialization, compilation, and execution — are automated.

---

## Prerequisites
- An LPU device is installed and recognized (`lspci | grep -i xilinx`).
- **XRT** and **Shell** are installed successfully.
- Linux kernel version is supported by both XRT and HRT.
- You have credentials to download required packages.

