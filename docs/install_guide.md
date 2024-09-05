<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

HyperDex SDK consists of compiler stack and runtime stack.

- HyperDex Runtime Library (`rpm`/`deb` package)
- HyperDex Compiler Library (`whl pacakge`)

Refer to the step-by-step installation guide provided below.
Note that you need an HyperDex portal account to proceed the installation.
Please [contact](mailto:contact@hyperaccel.ai) us for more information.

### Step 1. HyperDex Runtime Library (HRT)

!!! note
    `Step 1`. `HRT` is intended for **on-premise server** users, and to install the `HRT`, users need root privileges on the server.
    Typically, the stable version of the `HRT` is already installed on the **cloud server** you are currently using.
    If you can see the HyperAccel NPU devices by executing the command `ls /dev/xfpga*`, then you can skip `Step 1`.
    `HRT` and proceed directly to `Step 2`. `HyperDex Compiler`.

<!-- 
Provides an implementation of python package for HyperAccel LPU™, with a focus on performance and versatility.

## Quick Start
This guide shows how to use our LPU and sofware stack to:
- run offline LLM optimized inference on a dataset
- software development kit for easy model compilation
- chatbot demos with HyperAccel Orion server

## Installation

This repository is tested on Python 3.9+, PyTorch 2.0+ and HuggingFace Transformers 4.31+.
HyperDex can be installed using pip as follows:

```bash
pip install hyperdex-python
```

## Example

Similar to [HuggingFace transformer](https://huggingface.co/docs/transformers/index) package, HyperDex uses an `AutoModelForCausalLM` module to load the Transformers. To load the model parameters, you can simply give the path of the HyperDex model checkpoint.

```python
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer

# Load tokenzier and model
tokenizer = AutoTokenizer.from_pretrained("llama-7b")
model = AutoModelForCausalLM.from_pretrained("llama-7b", device_map={"lpu": 1})

# Text Generation
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
output_ids = model.generate(input_ids, max_length=1024, do_sample=False)
outputs = tokenizer.decode(input_ids)
```

The tokenizer is responsible for all the preprocessing the pretrained model expects, and can be called directly on a single string (as in the above examples) or a list. It will output a dictionary that you can use in downstream code or simply directly pass to your model using the `generate` API.

## Main features

 - APIs of `hyperdex.transformers` are similar to HuggingFace, which are easy to integrate with various LLM applications.
 - Fast model loading scheme with custom checkpoint format
 - Streaming text geneartion -->
