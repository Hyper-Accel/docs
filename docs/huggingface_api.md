<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

HyperDex provides a Python API designed to make running workloads on the LPU both easy and efficient. The Python API uses function calls similar to those found in HuggingFace’s [transformers](https://huggingface.co/docs/transformers/index) library, allowing users familiar with HuggingFace to quickly adapt to and utilize the HyperDex system. This enables existing HuggingFace users to seamlessly integrate HyperDex into their workflows without a steep learning curve.​



### Requirements

* **OS**: Ubuntu 22.04 LTS, Rocky 8.4
* **Python**: 3.10 ~ 3.12
* [Xilinx Runtime Library](./_install_xrt.md)

### Install with pip
You can install `hyperdex-toolchain` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```python linenums="1"
$ # (Recommended) Create a new environemnt.
$ curl -LsSf https://astral.sh/uv/install.sh | sh
$ uv venv -p python==3.10 --no-project --seed .hdex-env
$ source .hdex-env/bin/activate

$ # Install HyperDex-Python
$ uv pip install -i https://<pypi_id:pypi_pw>@pypi.hyperaccel.ai/simple hyperdex-toolchain==1.5.1+cpu
```

## Text Generation with HyperAccel LPU™

HyperDex allows you to generate output tokens using a function similar to HuggingFace's `generate` function. Therefore, you can easily generate tokens as shown in the example below.

!!! tip "Tip"
    You may need to log in to Hugging Face Hub.

```python linenums="1"
import os
from hyperdex.tools import AutoCompiler
from hyperdex.transformers import AutoModelForCausalLM, AutoTokenizer

# Path to hyperdex checkpoint (MODIFY model path and model here)
model_id = "facebook/opt-1.3b"

# MODIFY hardware configuration here
lpu_device = 2
gpu_device = 0

def main():
    hdex = AutoCompiler()
    hdex.compile(
        model_id=model_id,
        num_device=lpu_device,
        max_length=4096,
        low_memory_usage=False,
        fast_compile=True,
    )

    model = AutoModelForCausalLM.from_pretrained(
        model_id=model_id,
        device_map={"gpu": gpu_device, "lpu": lpu_device}
    )
    tokenizer = AutoTokenizer.from_pretrained(model_id)

   
    # Input text (MODIFY your input here)
    inputs = "Hello, my name"

    # 1. Encode input text to input token ids
    input_ids = tokenizer.encode(inputs)

    # 2. Generate output token ids with LPU™ (MODIFY sampling parameters here)
    output_ids = model.generate(
        input_ids,
        max_new_tokens=512,
        # Sampling
        do_sample=True,
        top_p=0.7,
        top_k=1,
        temperature=1.0,
        repetition_penalty=1.2
    )

    # 3. Decode output token ids to output text
    outputs = tokenizer.decode(output_ids, skip_special_tokens=True)

    # Print the output context
    print(outputs)

if __name__ == "__main__":
    main()
```

## LPU-GPU Hybrid System

Starting from version 1.3.2, HyperDex-Python supports the LPU-GPU hybrid system. The GPU, which has relatively higher computing power, handles the Prefill part of the Transformer, while the LPU, which efficiently utilizes memory bandwidth, processes the Decode part. The Key-Value transfer between Prefill and Decode can be performed without overhead using HyperDex's proprietary technology. You can select the number of devices to use for both GPU and LPU through the `device_map` option.

!!! note
    To run the `LPU-GPU hybrid system`, you need to have `CUDA 12.6` installed on your system. Additionally, since the GPU utilizes `PyTorch` to run LLMs, it is recommended to install `torch version 2.7.0 or later` to ensure optimal compatibility and performance.​

```python linenums="1"
import os
from hyperdex.tools import AutoCompiler
from hyperdex.transformers import AutoModelForCausalLM, AutoTokenizer

# Path to hyperdex checkpoint (MODIFY model path and model here)
model_id = "facebook/opt-1.3b"

# MODIFY hardware configuration here
lpu_device = 2
gpu_device = 2

def main():
    hdex = AutoCompiler()
    hdex.compile(
        model_id=model_id,
        num_device=lpu_device,
        max_length=4096,
        low_memory_usage=False,
        fast_compile=True,
    )

    model = AutoModelForCausalLM.from_pretrained(
        model_id=model_id,
        device_map={"gpu": gpu_device, "lpu": lpu_device}
    )
    tokenizer = AutoTokenizer.from_pretrained(model_id)

   
    # Input text (MODIFY your input here)
    inputs = "Hello, my name"

    # 1. Encode input text to input token ids
    input_ids = tokenizer.encode(inputs)

    # 2. Generate output token ids with LPU™ (MODIFY sampling parameters here)
    output_ids = model.generate(
        input_ids,
        max_new_tokens=512,
        # Sampling
        do_sample=True,
        top_p=0.7,
        top_k=1,
        temperature=1.0,
        repetition_penalty=1.2
    )

    # 3. Decode output token ids to output text
    outputs = tokenizer.decode(output_ids, skip_special_tokens=True)

    # Print the output context
    print(outputs)

if __name__ == "__main__":
    main()
```

To use the hybrid system, you need CUDA version 12.1 or later and the corresponding version of PyTorch.

## Sampling

Sampling works in the same way as HuggingFace. For sampling, you have options like top_p, top_k, temperature, and repetition penalty. Please refer to the HuggingFace documentation for explanations of each option. Additionally, the `generate` function allows you to directly control randomness using the seed argument. If `do_smaple` is `False`, LPU does not perform sampling and uses greedy method.

| Sampling Arguments    | Description                                                                   |
|-----------------------|-------------------------------------------------------------------------------|
| `top_p`               | Top-P sampling. Default value is `0.7`                                        |
| `top_k`               | Top-K sampling. Default value is `1`                                          |
| `temperature`         | Smoothing the logit distribution. Defualt value is `1.0`                      |
| `repetition_penalty`  | Give penlaty to logits. Default value is `1.2`                                |
| `stop`                | Token ID that signals the end of generation. Default value is `eos_token_id`  |

## Streaming Token Generation

HyperDex supports streaming token generation in a similar manner to HuggingFace. You can activate it by passing the TextStreamer module as an argument to the `generate` function.

```python linenums="1" 
# Import HyperDex transformers
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer
from hyperdex.transformers import TextStreamer

# Path to hyperdex model
hyperdex_model = "facebook/opt-1.3b"

# Load tokenzier and model
tokenizer = AutoTokenizer.from_pretrained(model_id=hyperdex_model)
model = AutoModelForCausalLM.from_pretrained(model_id=hyperdex_model, device_map={"lpu": 1})
# Config streamer module
streamer = TextStreamer(tokenizer, skip_special_tokens=True)
```

Since the `TextStreamer` module includes the process of decoding through the `tokenizer` and printing internally, you only need to call the `generate` function.

```python linenums="1"
# Input text
inputs = "Hello world!"

# 1. Encode input text to input token ids
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
# 2. Generate streaming output token ids with LPU™
model.generate(
  inputs,
  max_length=1024,
  # Sampling
  do_sample=True,
  top_p=0.7,
  top_k=50,
  temperature=1.0,
  repetition_penalty=1.2,
  # Streaming
  streamer=streamer
)
```

## How to use streaming with other Application?

HyperDex utilizes the `yield` keyword in Python to enable streaming for use in other applications. When you call the `generate_yield` function, it returns using `yield`, making it easy to use in other Python applications.

```python linenums="1"
# Config streamer module with disable print to activate yield mode
streamer = TextStreamer(tokenizer, use_print=False, skip_special_tokens=True)

# Input text
inputs = "Hello world!"

# 1. Encode input text to input token ids
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
# 2. Generate streaming output token ids with LPU™
output_ids = model.generate_yield(
  inputs,
  max_length=1024,
  # Sampling
  do_sample=True,
  top_p=0.7,
  top_k=50,
  temperature=1.0,
  repetition_penalty=1.2,
  # Streaming
  streamer=streamer
)

# Use outputs_ids which type is generator
```
