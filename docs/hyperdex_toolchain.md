# HyperDex Toolchain

The HyperDex Toolchain is a Python SDK designed to fully optimize AI workloads on an LPU (Linear Processing Unit).

It is a one integrated package consisted of two components, hyperdex.tools and hyperdex.transformers. which is a compiler and runtime package. allowing developers to efficiently run LLM on LPU hardware.

hyperdex.tools is a Python based compiler interface which supports to run HuggingFace Transfromers models.
hyperdex.transformers provides runtime enviroment to run LLM models by providing interface for HuggingFace Transformers.


---

## Prerequisites
- An LPU device is installed and recognized (`lspci | grep -i xilinx`).
- **XRT** and **Shell** are installed successfully.
- Linux kernel version is supported by both XRT and HRT.
- You have credentials to download required packages.

