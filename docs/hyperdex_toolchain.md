# HyperDex Toolchain

The **HyperDex Toolchain** is a software stack that simplifies running AI workloads on an **LPU**, ensuring they are fully optimized for maximum performance. It includes device drivers, a runtime, and a compiler, and automates all processes required to initialize, compile, and execute models on the LPU.  

This toolchain is delivered as a single integrated package with two core components: **`hyperdex.tools`** and **`hyperdex.transformers`**, serving as the compiler and runtime packages respectively. Together, they enable developers to efficiently run LLMs on LPU hardware with minimal changes to existing workflows.  

---

## hyperdex.tools — Compiler  

`hyperdex.tools` is a **Python-based compiler interface** designed to support HuggingFace Transformers models. It provides a high-level entry point for adapting models to LPU hardware.  

At the center of this component are **AutoCompiler** and **AutoModelConverter**.  
- *AutoCompiler* manages the compilation pipeline : it checks whether the target model is supported by the LPU, downloads the model, and prepares it for execution on LPU instructions.  
- *AutoModelConverter* validates the downloaded checkpoint, inspects the model and tokenizer, and converts them into an LPU-compatible form.  

By handling these steps automatically, `hyperdex.tools` ensures that developers can bring HuggingFace models onto LPU hardware with minimal manual intervention.  

---

## hyperdex.transformers — Runtime  

`hyperdex.transformers` provides the **runtime environment** for executing LLMs on the LPU. It is fully aligned with the HuggingFace Transformers interface, allowing developers to reuse familiar APIs while benefiting from LPU acceleration.  

Key functionality includes:  
- *AutoConfig*, which loads runtime parameters such as the number of devices or maximum token length  
- *AutoModelForCausalLM*, which loads and executes compiled LPU models  
- *AutoTokenizer*, which retrieves the correct tokenizer for each model  
- *TextStreamer*, which streams decoded tokens incrementally in real time, either to stdout or via SSE, so that words appear as they are generated  

In addition, the runtime supports **prefix caching**, allowing repeated or long-running workloads to be processed more efficiently.  

---

## Compatibility  

All classes and tools within the HyperDex Toolchain are **Compatible with HuggingFace’s `transformers` library**. This ensures that developers can leverage the same ecosystem they are familiar with, while seamlessly transitioning their workloads to LPU-based execution.


Before using our HyperDex-Toolchain, please make sure you have met all the requirements of Prerquisites.

---

## Prerequisites
- An LPU device is installed and recognized (`lspci | grep -i xilinx`).
- **XRT** and **Shell** are installed successfully.
- Linux kernel version is supported by both XRT and HyperDex-Toolchain.
- You should have credentials to download required packages.

---

For installation, please move to [HyperDex-Toolchain installation](./huggingface_api.md) or if you prefer to use with vLLM, move to [vLLM installation](./offline_inference.md)