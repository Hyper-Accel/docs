**HyperDex SDK** provides the **HyperDex-Serve** Python package for serving LLMs. HyperDex-Serve allows the **LPU** (or **LPU-GPU Hybrid System**) to be run via a RESTful API. The API follows the same structure as HuggingFace’s widely-used [**Text-Generation-Inference (TGI)**](https://huggingface.co/docs/text-generation-inference/index) API, which is commonly used for serving open-source LLMs.


!!! tip
    Currently, **HyperDex-Serve** does not support the [**OpenAI API**](https://platform.openai.com/docs/guides/chat-completions). However, in the next release, we plan to introduce support for the [**vLLM framework**](https://docs.vllm.ai), which will enable compatibility with the OpenAI API by utilizing vLLM’s OpenAI Compatible Server.

## Installation

!!! note
    **HyperDex-Serve** is a Python package that internally relies on the **HyperDex-Python** package. Therefore, the required installation environment is the same as that of **HyperDex-Python**.

### Requirements

* **OS**: Ubuntu 22.04 LTS, Rocky 8.4
* **Python**: 3.9 ~ 3.11
* [Xilinx Runtime Library](./install_guide.md)
* [HyperDex Runtime & Compiler stack](./install_guide.md)
* [HyperDex-Python](./python_api.md) Package

### Install with pip
You can install `hyperdex-serve` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```shell linenums="1" hl_lines="6 7"
$ # (Recommended) Create a new conda environment.
$ conda create -n hdex-env python=3.10 -y
$ conda activate hdex-env

$ # Install HyperDex-Python and HyperDex-Serve
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-python
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-serve
```

## Usage

### Command Line Interface

Once the HyperDex-Serve package is installed, you can use the `hdex-serve` command with the following options:

```shell linenums="1"
$ hdex-serve --help
usage: hdex-serve [-h] [--host HOST] [--port PORT]
                  [--allow-credentials] [--allowed-origins ALLOWED_ORIGINS]
                  [--allowed-methods ALLOWED_METHODS] [--allowed-headers ALLOWED_HEADERS]
                  [--api-key API_KEY]
                  [--served-model-path SERVED_MODEL_PATH] 
                  [--served-model-name SERVED_MODEL_NAME]
                  [--served-lpu-device-num SERVED_LPU_DEVICE_NUM]
                  [--served-gpu-device-num SERVED_GPU_DEVICE_NUM]
                  [--response-role RESPONSE_ROLE]
                  [--ssl-keyfile SSL_KEYFILE] [--ssl-certfile SSL_CERTFILE]
                  [--root-path ROOT_PATH] [--verbose]
```

### Serving Model

Below is an example of serving a HuggingFace model. The model to be served must be pre-compiled using the [**HyperDex Compiler SDK**](./model_compile.md).

```shell linenums="1"
$ hdex-serve \
  --host 0.0.0.0 \
  --port 8000 \
  --served-model-path /opt/hyperdex/models \
  --served-model-name TinyLlama/TinyLlama-1.1B-Chat-v1.0 \
  --served-lpu-device-num 1 \
  --served-gpu-device-num 1 \
  --verbose

INFO:     Started server process [380461]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

!!! tip
    HyperDex-Serve can be used to start the server with the above command, and you can test it using the client code from TGI. Example code is available in `/opt/hyperdex/examples` for reference.

### Descriptions of HyperDex-Serve Arguments

| Arguments                                         | Description                                                                                                   |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------|
| `-h, --help `                                     | show help message and exit                                                                               |
| `--host HOST `                                    | host name                                                                                                     |
| `--port PORT `                                    | port number                                                                                                   |
| `--allow-credentials `                            | allow credentials                                                                                             |
| `--allowed-origins ALLOWED_ORIGINS `              | allowed origins                                                                                               |
| `--allowed-methods ALLOWED_METHODS `              | allowed methods                                                                                               |
| `--allowed-headers ALLOWED_HEADERS `              | allowed headers                                                                                               |
| `--api-key API_KEY `                              | If provided, the server will require this key to be presented in the header.                                  |
| `--served-model-path SERVED_MODEL_PATH `          | The path to model checkpoint used in the API.                                                                 |
| `--served-model-name SERVED_MODEL_NAME `          | The model name used in the API. If not specified, the model name will be the same as the huggingface name.    |
| `--served-lpu-device-num SERVED_LPU_DEVICE_NUM`   | The total number of LPU device used in the API                                                                |
| `--served-gpu-device-num SERVED_GPU_DEVICE_NUM`   | The total number of GPU device used in the API                                                                |
| `--response-role RESPONSE_ROLE `                  | The role name to return if `request.add_generation_prompt=true`.                                              |
| `--ssl-keyfile SSL_KEYFILE `                      | The file path to the SSL key file                                                                             |
| `--ssl-certfile SSL_CERTFILE `                    | The file path to the SSL cert file                                                                            |
| `--root-path ROOT_PATH `                          | FastAPI root_path when app is behind a path based routing proxy                                               |
| `--verbose `                                      | Lanch the program with verbose mode                                                                           |
