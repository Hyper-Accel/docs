# HyperDex Toolchain

The HyperDex Toolchain is a software stack that makes it easy for users to run AI workloads on an LPU and ensures they are fully optimized for maximum performance.

It is a single integrated package consisting of two components: **hyperdex.tools** and **hyperdex.transformers**, which serve as the compiler and runtime package, respectively, allowing developers to efficiently run LLMs on LPU hardware.

- **hyperdex.tools** is a Python-based compiler interface that supports running HuggingFace Transformers models.  
- **hyperdex.transformers** provides a runtime environment for running LLM models and offers an interface for HuggingFace Transformers.

The HyperDex Toolchain also includes device drivers, runtime, and compiler, and automates all processes necessary to initialize, compile, and run models on the LPU.


---

## Prerequisites
- An LPU device is installed and recognized (`lspci | grep -i xilinx`).
- **XRT** and **Shell** are installed successfully.
- Linux kernel version is supported by both XRT and HRT.
- You have credentials to download required packages.

