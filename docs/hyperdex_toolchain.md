# HyperDex Toolchain

The HyperDex Toolchain is a software stack that makes it easy for users to run AI workloads on an LPU and ensures they are fully optimized for maximum performance. it includes device drivers, runtime, and compiler, and automates all processes necessary to initialize, compile, and run models on the LPU.

It is a single integrated package consisting of two components: **hyperdex.tools** and **hyperdex.transformers**, which serve as the compiler and runtime package, respectively, allowing developers to efficiently run LLMs on LPU hardware.

- **hyperdex.tools** is a Python-based compiler interface that supports running HuggingFace Transformers models.
    here, we use AutoCompiler and AutoModelConverter to modify huggingface's LLM models to fit our LPU
    AutoCompiler is a high-level entry point for compilation pipeline. from checking if LPU supports the model to download and converting the model to be used with our instructions. when converting teh model, AutoModelConverter checks downloaded model's checkpoint, actual model and tokenizer and converts them for LPU

- **hyperdex.transformers** provides a runtime environment for running LLM models and offers an interface for HuggingFace Transformers.
    From loading model and runtime parameters like number of device to run or maximum length of token with AutoConfig, AutoModelForCausalLM loads model that is compiled for our LPU. we support pre-fix caching here to run models more efficently.
    AutoTokenizer gets tokenizer for each models. finally, if user wants to get output in real-time, we support TextStreamer to help our decoder tokens to incrementally and streams text either to stdout or via SSE, allowing users to see words form as the model generates them.

    classes of our transformers is compatiable with huggingface's tranformers library making it easier for developers to use our LPU product.

Before using our HyperDex-Toolchain, please make sure you have met all the requirements of Prerquisites.

---

## Prerequisites
- An LPU device is installed and recognized (`lspci | grep -i xilinx`).
- **XRT** and **Shell** are installed successfully.
- Linux kernel version is supported by both XRT and HyperDex-Toolhain.
- You should have credentials to download required packages.

---

For installation, please move to [HyperDex-Toolchain installation](./huggingface_api.md) or if you prefer to use with vLLM, move to [vLLM installation](./offline_inference.md)