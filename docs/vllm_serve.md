

### Requirements and Install Guide
Requirements and Install Guide is same as[ vLLM API](./vllm_api.md). 


### Serving Model

hyperdex-vllm provides an HTTP server that implements vLLM API. 
You can execute the server by the command below.

```shell linenums="1"
$ python -m vllm.entrypoints.api_server --model facebook/opt-1.3b \
        --device fpga --num_lpu_devices 1

... OMISSION ...

INFO:     Started server process [27157]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

### Descriptions of HyperDex-vLLM Serve Arguments
Arguments are the same as [vLLM Engine](./vllm_api.md).


### Client

To call the server, you can use the client example provided in `vllm/examples` ensuring that `use_beam_search=False`.

```shell linenums="1"
# You can see this file in our vLLM repo. (vllm/examples/lpu_client.py)
python lpu_client.py --stream
```


## OpenAI Compatible Server

### Serving Model

hyperdex-vllm also provides an HTTP server that implements OpenAI's Completions API. You can call the server by the command below.
```shell linenums="1"
python -m vllm.entrypoints.openai.api_server --model facebook/opt-1.3b \
        --device fpga --tensor-parallel-size 1

... OMISSION ...

INFO:     Started server process [27157]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

### Client

To call the server, you can use OpenAI Python client library, or any other HTTP client. Change stream option if you want.

```shell linenums="1"
# You can see this file in our vLLM repo. (vllm/examples/lpu_openai_client.py)
python lpu_openai_client.py
```
