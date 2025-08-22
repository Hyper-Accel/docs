<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->


HyperDex supports the vLLM framework to run on LPU. As you know, the vLLM framework officially supports a variety of hardware including GPU, TPU, and XPU. HyperDex has its own branch of vLLM with a backend specifically designed for LPU, making it very easy to use. If your system is already using vLLM, you can switch hardware from GPU to LPU without changing any code.


### Requirements

* **OS**: Ubuntu 22.04 LTS, Rocky 8.4
* **Python**: 3.10 ~ 3.12
* **torch**: 2.7.0+cpu (in LPU only env) or 2.7.0+cu126 (in LPU+GPU env)
* [Xilinx Runtime Library](./install_guide.md)
* HyperDex-Toolchain

### Install with pip
You can install `hyperdex-vllm` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```python linenums="1" hl_lines="6 7 10 11 14"
$ # (Recommended) Create a new virtual environemnt. We recommend to use uv(https://docs.astral.sh/uv/)
$ curl -LsSf https://astral.sh/uv/install.sh | sh
$ uv venv --no-project --seed .hdex-env
$ source .hdex-env/bin/activate

$ # Install HyperDex Python Package
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-toolchain

$ # Install HyperDex-vLLM in LPU only env
$ pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cpu
$ pip install -i https://pypi.hyperaccel.ai/simple vllm == 0.9.0+orion

$ # Install HyperDex-vLLM in LPU+GPU env
$ pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cu126
$ pip install -i https://pypi.hyperaccel.ai/simple vllm == 0.9.0+orion

$ # Install HyperDex-vLLM source code
$ git clone git@github.com:Hyper-Accel/vllm.git
```



## Text Generation with HyperAccel LPU™

HyperDex-vLLM generates tokens very similar to vLLM's `generate` function, enabling you to easily generate tokens as demonstrated in the example below. Ensure that **device="fpga"**, and **num_lpu_devices=1** are set.

!!! note
    currently, LPU supports one batch at a time. do not put more than one prompt. 
    multi-batch will be supported soon



```python linenums="1"
# You can see this file in our vLLM repo. (vllm/examples/lpu_inference.py)

from vllm import LLM, SamplingParams

# Create prompts and a sampling params object.
prompts = ["Hello, my name is"]
sampling_params = SamplingParams(temperature=0.8, top_p=0.95, top_k=1, min_tokens=30, max_tokens=30)

# Create an LLM
llm = LLM(model="TinyLlama/TinyLlama-1.1B-Chat-v1.0", device="fpga", num_lpu_devices=1)

# Generate texts from the prompts. 
outputs = llm.generate(prompts, sampling_params)

# Print the outputs.
for output in outputs:
    prompt = output.prompt
    generated_text = output.outputs[0].text
    print(f"Prompt: {prompt!r}, Generated text: {generated_text!r}")
```

## GPU-LPU Hybrid System

HyperDex supports a heterogeneous GPU-LPU hardware system for executing large language models (LLM). Each hardware type offers distinct strengths: GPU excels in large-scale parallel computations, while LPU is designed to fully optimize memory bandwidth utilization. 

Since the prefill stage is compute-bound and the decode stage is memory-bound, the hybrid system processes prefill stage using a GPU, and decode stage using a LPU. This approach sighificantly boosts LLM performance!

To enable the hybrid system, simply add the option **num_gpu_devices=1**.

```python linenums="1"
# Create an LLM.
llm = LLM(model="TinyLlama/TinyLlama-1.1B-Chat-v1.0", device="fpga", num_lpu_devices=1, **num_gpu_devices=1**)
```


## Option for Sampling Params

Sampling Params refer to setting that control how a model generates text. vLLM supports various sampling params to support various features. However, due to the current limitations of the LPU, only the sampling parameters listed below are supported. We are planning to extend and update our coverage.

| Sampling Arguments    | Description                                                                   |
|-----------------------|-------------------------------------------------------------------------------|
| `max_tokens`          | The number of tokens to be generated. Default value is `16`                   |
| `top_p`               | Top-P sampling. Default value is `0.7`                                        |
| `top_k`               | Top-K sampling. Default value is `1`                                          |
| `temperature`         | Smoothing the logit distribution. Defualt value is `1.0`                      |
| `repetition_penalty`  | Give penlaty to logits. Default value is `1.2`                                |
| `stop     `           | Token ID that signals the end of generation. Default value is `eos_token_id`  |

## Option for LLM Engine

LLM Engine is a core component of vLLM because it performs various functions. For example, it initializes hardware, manages hardware resources, and schedules requests.


| LLM Engine Arguments    | Description                                                                 |
|-----------------------|-------------------------------------------------------------------------------|
| `model`               | Name of path of the huggingface model to use. Default: "facebook/opt-125m"    |
| `device`              | Device type for vLLM execution. Default: "cuda"                               |
| `num_lpu_devices`     | Number of LPU to compute in parallel. Default: 1                              |
| `num_gpu_devices`     | Number of GPU to compute in parallel. Default: 0                              |
| `tokenizer`           | Name of path of the huggingface tokenizer to use. If not specified, model name of path will be used   |
| `trust-remote-code`   | Trust remote code from huggingface. Default: False                            |




