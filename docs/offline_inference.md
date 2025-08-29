<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->


HyperDex supports the vLLM framework to run on LPU. As you know, the vLLM framework officially supports a variety of hardware including GPU, TPU, and XPU. HyperDex has its own branch of vLLM with a backend specifically designed for LPU, making it very easy to use. If your system is already using vLLM, you can switch hardware from GPU to LPU without changing any code.


### Requirements

* **OS**: Ubuntu 22.04 LTS, Rocky 8.4
* **Python**: 3.10 ~ 3.12
* **torch**: 2.7.0+cpu (in LPU only env) or 2.7.0+cu126 (in LPU+GPU env)
* [Xilinx Runtime Library](./_install_xrt.md)
* HyperDex-Toolchain

### Install with pip
You can install `hyperdex-vllm` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```python linenums="1"
$ # (Recommended) Create a new virtual environemnt. We recommend to use uv(https://docs.astral.sh/uv/)
$ curl -LsSf https://astral.sh/uv/install.sh | sh
$ uv venv -p python==3.10 --no-project --seed .hdex-env
$ source .hdex-env/bin/activate

$ # Register HyperAccel PyPI credentials. Please [contact our support team](mailto:support@hyperaccel.ai) if you need help obtaining credentials.
$ export PYPI_ID="your_pypi_username"
$ export PYPI_PW="your_pypi_password"

$ # Install HyperDex-Toolchain and vLLM in LPU only env
$ uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cpu
$ uv pip install -i https://${PYPI_ID}:${PYPI_PW}@@pypi.hyperaccel.ai/simple vllm-orion==0.9.0+orion.toolchain151.fpga

$ # Install HyperDex-Toolchain and vLLM in LPU+GPU env
$ uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cu126
$ uv pip install -i https://${PYPI_ID}:${PYPI_PW}@@pypi.hyperaccel.ai/simple vllm-orion==0.9.0+orion.toolchain151.hybrid
```


## Text Generation with HyperAccel LPU™

HyperDex-vLLM generates tokens very similar to vLLM's `generate` function, enabling you to easily generate tokens as demonstrated in the example below. to use LPU, you must give this environment variable "NUM_LPU_DEVICES" with number of devices to use


!!! note
    currently, LPU supports one batch at a time. do not put more than one prompt. 
    multi-batch will be supported soon



```python linenums="1"
# You can see this file in our vLLM repo. (vllm/examples/vllm_lpu_model_runner.py.py)
from vllm import LLM, SamplingParams

# Configure logging
logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

# Create prompts and a sampling params object.
prompts = ["Hello, my name is"]
sampling_params = SamplingParams(temperature=0.8, top_p=0.95, top_k=1, min_tokens=30, max_tokens=30)

# Create an LLM
llm = LLM(model="/path/to/llama-7b")

# Generate texts from the prompts. 
outputs = llm.generate(prompts, sampling_params)

# Log the outputs
logger.info("-" * 60)
for output in outputs:
    prompt = output.prompt
    generated_text = output.outputs[0].text
    logger.info(f"Prompt:    {prompt!r}")
    logger.info(f"Output:    {generated_text!r}")
    logger.info("-" * 60)
```


```bash linenums="1"
# run the code with command 
NUM_LPU_DEVICES=2 uv run vllm_lpu_model_runner.py
```


## GPU-LPU Hybrid System

HyperDex-Toolchain supports a heterogeneous GPU-LPU hardware system for executing large language models (LLM). Each hardware type offers distinct strengths: GPU excels in large-scale parallel computations, while LPU is designed to fully optimize memory bandwidth utilization. 

Since the prefill stage is compute-bound and the decode stage is memory-bound, the hybrid system processes prefill stage using a GPU, and decode stage using a LPU. This approach sighificantly boosts LLM performance!

To enable the hybrid system, simply add NUM_GPU_DEVICES=2 at command.

```bash linenums="1"
# run with this command
NUM_GPU_DEVICES=2 NUM_LPU_DEVICES=2 uv run vllm_lpu_model_runner.py
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

## Option for LLM Engine

LLM Engine is a core component of vLLM because it performs various functions. For example, it initializes hardware, manages hardware resources, and schedules requests.


| LLM Engine Arguments    | Description                                                                 |
|-----------------------|-------------------------------------------------------------------------------|
| `model`               | Name of path of the huggingface model to use. Default: "facebook/opt-125m"    |
| `device`              | Device type for vLLM execution. Default: "cuda"                               |
| `tokenizer`           | Name of path of the huggingface tokenizer to use. If not specified, model name of path will be used   |
| `trust-remote-code`   | Trust remote code from huggingface. Default: False                            |
