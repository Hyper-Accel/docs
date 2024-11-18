
**HyperDex SDK** provides the **HyperDex-Serve** Python package for serving LLMs. HyperDex-Serve allows the **LPU** (or **LPU-GPU Hybrid System**) to be run via a RESTful API. The API follows the same structure as HuggingFace’s widely-used [**Text-Generation-Inference (TGI)**](https://huggingface.co/docs/text-generation-inference/index) API, which is commonly used for serving open-source LLMs.


### Requirements and Install Guide
Requirements and Install Guide is same as[ Python API](./python_api.md). 


### Serving Model

hyperdex-serve provides an HTTP server that implements TGI API. 
You can execute the server by the command below.


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
