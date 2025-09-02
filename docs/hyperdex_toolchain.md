# HyperDex Toolchain

The **HyperDex Toolchain** is a comprehensive software stack that simplifies the deployment and execution of AI workloads on **LPU** (Language Processing Unit) hardware, ensuring optimal performance. It includes device drivers, runtime environment, and compiler tools that automate all processes required to initialize, compile, and execute models on the LPU.

This toolchain is delivered as a single, integrated package comprising two core components: **`hyperdex.tools`** and **`hyperdex.transformers`**, which serve as the compiler and runtime packages respectively. Together, they enable developers to efficiently deploy LLMs on LPU hardware with minimal modifications to existing workflows.  

---

## hyperdex.tools — Compiler  

`hyperdex.tools` is a **Python-based compiler interface** specifically designed to support HuggingFace Transformers models. It provides a high-level entry point for seamlessly adapting pre-trained models to LPU hardware architecture.

The core of this component consists of two primary classes: **AutoCompiler** and **AutoModelConverter**.

- **AutoCompiler** orchestrates the entire compilation pipeline by verifying model compatibility with LPU hardware, downloading the target model, and preparing it for execution using LPU-optimized instructions.
- **AutoModelConverter** performs comprehensive validation of downloaded checkpoints, analyzes both the model architecture and tokenizer configuration, and converts them into an LPU-compatible format.

Through automated handling of these complex processes, `hyperdex.tools` enables developers to seamlessly migrate HuggingFace models to LPU hardware with minimal manual intervention.  

---

## hyperdex.transformers — Runtime  

`hyperdex.transformers` provides the **runtime environment** for executing LLMs on the LPU. It is fully aligned with the HuggingFace Transformers interface, enabling developers to leverage familiar APIs while benefiting from LPU acceleration.

Key functionality includes:

- **AutoConfig** - Loads runtime parameters such as the number of devices and maximum token length
- **AutoModelForCausalLM** - Loads and executes compiled LPU models
- **AutoTokenizer** - Retrieves the appropriate tokenizer for each model
- **TextStreamer** - Streams decoded tokens incrementally in real-time, either to stdout or via Server-Sent Events (SSE), enabling words to appear as they are generated

Additionally, the runtime supports **prefix caching**, which allows repeated or long-running workloads to be processed more efficiently.  

---

## Compatibility  

All classes and tools within the HyperDex Toolchain are **compatible with HuggingFace's `transformers` library**. This ensures that developers can leverage the same ecosystem they are familiar with while seamlessly transitioning their workloads to LPU-based execution.

Before using the HyperDex Toolchain, please ensure you have met all the requirements listed in the Prerequisites section.

---

## Prerequisites
- An LPU device is installed and recognized (`lspci | grep -i xilinx`)
- **XRT** is installed successfully
- Linux kernel version is supported by both XRT and HyperDex Toolchain
- You have the necessary credentials to download required packages

---

For installation instructions, please refer to [HyperDex Toolchain Installation](./install_hyperdex.md). If you prefer to use vLLM, please see [vLLM Installation](./offline_inference.md).