**HyperDex-Compiler SDK** helps to run LPUs using Hugging Face's checkpoints. The following content explains how to transform HuggingFace models to be executable on LPUs.


### Requirements

* **OS**: Ubuntu 22.04 LTS, Rocky 8.4
* **Python**: 3.9 ~ 3.11
* [Xilinx Runtime Library](./install_xrt.md)
* [HyperDex Runtime & Compiler stack](./install_hyperdex.md)

### Install with pip
You can install `hyperdex-compiler` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```shell linenums="1" hl_lines="5 6 7"
$ # (Recommended) Create a new conda environemnt.
$ conda create -n hdex-env python=3.10 -y
$ conda activate hdex-env

$ # Install HyperDex-Python and HyperDex-Compiler SDK
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-transformers
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-compilers
```

## Compile HuggingFace-Hub Model

HyperDex-Compiler SDK provides a CLI-based program that converts and compile HyperDex supported models. To refer to the list of models supported by the HyperDex SDK, please visit the following page: [Supported Models](./supported_models.md). Any model in Huggingface that matches the architecture can be converted and run by HyperAccel LPU™. You can easily compile models as shown in the exmaple below using 'AutoCompiler'.

Start the SDK program:
```python linenums="1" hl_lines="2"
from hyperdex.tools import AutoCompiler
model_id = 'facebook/opt-1.3b'
hdex = AutoCompiler()
hdex.compile(
    model_id=model_id,
    num_device=lpu_device,
    max_length=4096,
    low_memory_usage=True,
    token=HF_TOKEN,
)
```
The model_id can be a id from Huggingface(i.g. [facebook/opt-1.3b](https://huggingface.co/facebook/opt-1.3b)). The model will be installed in ~/.cache/hyperdex/hub. Another option is to spciefy a directory to install the model. Note that the Huggingface model id has be at the end of the directory. HyperDex SDK will download, convert, and save files to run the LPU at the directory described above.
```python linenums="1"
model_id = '/opt/hyperdex/models/facebook/opt-1.3b'
```

Here is an example of running the SDK program for the `facebook/opt-1.3b` model:
```shell linenums="1"
[ INFO ] : ** HyperDex  Model  SDK  Version **
[ INFO ] : ** Copyright (c) 2024 HyperAccel **
[ INFO ] : ** Phase-0 : Check supported model **
[ INFO ] : In this phase, check if given model is supported by HyperDex.
config.json: 100%|█████████████████████████████████████████████| 653/653
[ INFO ] : ** Phase-1 : Download model from HuggingFace Hub **
[ INFO ] : In this phase, download the HuggingFace model and save it to (=/home/members/user/.cache/hyperdex/hub).
[ INFO ] : Download model at /home/members/user/.cache/hyperdex/hub/facebook/opt-1.3b/ckpt
pytorch_model.bin: 100%|███████████████████████████████████| 2.63G/2.63G
generation_config.json: 100%|██████████████████████████████████| 137/137
tokenizer_config.json: 100%|███████████████████████████████████| 685/685
vocab.json: 100%|████████████████████████████████████████████| 899k/899k
merges.txt: 100%|████████████████████████████████████████████| 456k/456k
special_tokens_map.json: 100%|█████████████████████████████████| 441/441
Not logged in!
[ INFO ] : ** Phase-2 : Convert the checkpoint format to HyperDex style **
[ INFO ] : In this phase, convert the Huggingface checkpoint into HyperDex format.
[ INFO ] : The output creates both JSON and binary file.
[ INFO ] : Convert the model to HyperDex model format
[ INFO ] : Save the converted checkpoint at /home/members/user/.cache/hyperdex/hub/facebook/opt-1.3b/ckpt
[ INFO ] : ** Phase-3 : Generate Optimized Model Mapping **
[ INFO ] : In this phase, optimize model mapping for streamlined memory access and efficient model parallelism.
[ INFO ] : The result of the optimization process is a binary file.
[ INFO ] : Optimize the model paramater
[ INFO ] : Save the optimized data at /home/members/user/.cache/hyperdex/hub/facebook/opt-1.3b/param
[ INFO ] : ** Phase-4 : Generate Optimized Model Instruction **
[ INFO ] : In this phase, analyze the structure of the model to generate optimized instructions.
[ INFO ] : Techniques like Kernel Fusion and Layer Reordering techniques are used to make hardware processing more efficient.
[ INFO ] : The result of the optimization is a binary file.
[ INFO ] : Optimize the model instruction
[ INFO ] : Save the optimized instruction at /home/members/user/.cache/hyperdex/hub/facebook/opt-1.3b/inst
[ INFO ] : Model compile Complete!
```

## AutoCompiler.compile( ) Parameters

| Arguments             | Description                                                                                   |
|-----------------------|-----------------------------------------------------------------------------------------------|
| `model_id`            | Huggingface model id. Default value is `facebook/opt-1.3b`                                    |
| `num_device`          | Number of LPU to use. Default value is `1`                                                    |
| `max_length`          | Maximum length of response. Defualt value is `1`                                              |
| `low_memory_usage`    | Remove bin/safetensors file to save memory. Default value is `False`(Doesnt' remove)          |
| `token`               | Huggingface Acess Token. Default value is `None`                                              |

If the model is gated in huggingface like [mistralai/Mistral-7B-v0.1](https://huggingface.co/mistralai/Mistral-7B-v0.1) or [meta-llama/Llama-2-7b-chat-hf](https://huggingface.co/meta-llama/Llama-2-7b-chat-hf), user has to get access via huggingface before using. After getting access from the author, HyperDex SDK will download the model. Access token can be passed either by parameter or after running the SDK program like below.
```shell linenums="1"
    _|    _|  _|    _|    _|_|_|    _|_|_|  _|_|_|  _|      _|    _|_|_|      _|_|_|_|    _|_|      _|_|_|  _|_|_|_|
    _|    _|  _|    _|  _|        _|          _|    _|_|    _|  _|            _|        _|    _|  _|        _|
    _|_|_|_|  _|    _|  _|  _|_|  _|  _|_|    _|    _|  _|  _|  _|  _|_|      _|_|_|    _|_|_|_|  _|        _|_|_|
    _|    _|  _|    _|  _|    _|  _|    _|    _|    _|    _|_|  _|    _|      _|        _|    _|  _|        _|
    _|    _|    _|_|      _|_|_|    _|_|_|  _|_|_|  _|      _|    _|_|_|      _|        _|    _|    _|_|_|  _|_|_|_|

    To log in, `huggingface_hub` requires a token generated from https://huggingface.co/settings/tokens .
Enter your token (input will not be visible):
```


!!! warning
    When trying to compile model for LPU-GPU Hybrid System, the `low_memory_usage` must be False. This causes error when GPU is running. If a model was compiled with `low_memory_usage`=True, compile again with `low_memory_usage`=False.
