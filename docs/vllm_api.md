<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

HyperDex supports the vLLM framework to run on LPU(LLM Processing Unit). As you know, the vLLM framework officially supports a variety of hardware, including GPU, TPU, and XPU. HyperDex has its own branch of vLLM with a backend specifically designed for LPU, making it very easy to use. If your system is already using vLLM, you can switch hardware from GPU to LPU without changing any code. Below, we'll explain how to install and use this setup.


### Requirements

* **OS**: Ubuntu 22.04 LTS, Rocky 8.4
* **Python**: 3.9 ~ 3.11
* [Xilinx Runtime Library](./install_guide.md)
* [HyperDex Runtime & Compiler stack](./install_guide.md)

### Install with pip
You can install `hyperdex-vllm`(WIP) using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```python linenums="1" hl_lines="5 6"
$ # (Recommended) Create a new conda environemnt.
$ conda create -n hdex-env python=3.10 -y
$ conda activate hdex-env

$ # Install HyperDex-Python
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-vllm
```

## Text Generation with HyperAccel LPU™

HyperDex allows you to output tokens using a function similar to vLLM's `generate` function. Therefore, you can easily generate tokens as shown in the example below.

```python linenums="1"
from vllm import LLM, SamplingParams

# Create prompts and a sampling params object.
prompts = ["Hello, my name is"]
sampling_params = SamplingParams(temperature=0.8, top_p=0.95, top_k=1, min_tokens=30, max_tokens=30)

# Create an LLM.
llm = LLM(model="TinyLlama/TinyLlama-1.1B-Chat-v1.0", device="fpga", tensor_parallel_size=1)

# Generate texts from the prompts. 
outputs = llm.generate(prompts, sampling_params)

# Print the outputs.
for output in outputs:
    prompt = output.prompt
    generated_text = output.outputs[0].text
    print(f"Prompt: {prompt!r}, Generated text: {generated_text!r}")
```

## LPU-GPU Hybrid System (WIP)

TODO(hyunjun): I need one more argument to transfer the number of GPUs to use

## Option for Sampling Params

Sampling Params refer to setting that control how a model generates text. vLLM supports various sampling params to support various features. But because LPU currently supports limited features, only the sampling params listed belows are supported. We are planning to extend and update our coverage.

| Sampling Arguments    | Description                                                                   |
|-----------------------|-------------------------------------------------------------------------------|
| `max_tokens`          | The number of tokens to be generated. Default value is `16`                   |
| `top_p`               | Top-P sampling. Default value is `0.7`                                        |
| `top_k`               | Top-K sampling. Default value is `1`                                          |
| `temperature`         | Smoothing the logit distribution. Defualt value is `1.0`                      |
| `repetition_penalty`  | Give penlaty to logits. Default value is `1.2`                                |
| `stop(WIP)`           | Token ID that signals the end of generation. Default value is `eos_token_id`  |

## Option for LLM Engine

LLM Engine is a core component of vLLM because it performs various functions. For example, it initializes hardware, manages hardware resources, and schedules requests.


| LLM Engine Arguments    | Description                                                                   |
|-----------------------|-------------------------------------------------------------------------------|
| `model`               | Name of path of the huggingface model to use. Default: "facebook/opt-125m"    |
| `device`              | Device type for vLLM execution. Default: "fpga" (WIP)                         |
| `tensor-parallel-size`| Number of tensor parallel replicas. Default: 1                                |
| `tokenizer`           |   Name of path of the huggingface tokenizer to use. If unspecified, model name of path will be used   |
| `trust-remote-code`   |   Trust remote code from huggingface. Default: False                         |


## Streaming Token Generation (WIP)

