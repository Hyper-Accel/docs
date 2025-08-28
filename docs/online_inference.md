<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

### Requirements and Install Guide
Requirements and Install Guide is same as[ vLLM API](./offline_inference.md). 


### Serving Model

hyperdex-vllm provides an HTTP server that implements vLLM API. 
You can execute the server by the command below.

```shell linenums="1"
$ NUM_LPU_DEVICES=1 python -m vllm.entrypoints.api_server --model  facebook/opt-1.3b

... OMISSION ...

INFO:     Started server process [27157]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

### Descriptions of HyperDex-vLLM Serve Arguments
Arguments are the same as [vLLM Engine](./Offline_Inference.md).


### Client

To call the server, run this example command at another terminal

```shell linenums="1"
curl http://localhost:8000/generate   -H "Content-Type: application/json"   -d '{
    "prompt": "Hello, my name is",
    "max_tokens": 30,
    "temperature": 0
  }'
```

## Openai Client

### Serving Model

Our vLLM also provides an HTTP server that implements OpenAI's Completions API. You can call the server by the command below.

```shell linenums="1"
$ NUM_GPU_DEVICES=1 NUM_LPU_DEVICES=2 vllm serve facebook/opt-1.3b

... OMISSION ...

INFO:     Started server process [27157]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

### Client
To call the server, you can use OpenAI Python client library, or any other HTTP client.

```shell linenums="1"
curl http://localhost:8000/v1/chat/completions   -H "Content-Type: application/json"   -H "Authorization: Bearer EMPTY"   -d '{
    "messages": [
      {"role": "system", "content": "You are a helpful assistant."},
      {"role": "user", "content": "Who won the world series in 2020?"},
      {"role": "assistant", "content": "The Los Angeles Dodgers won the ^Crld Series in 2020."},
      {"role": "user", "content": "Where was it played?"}
    ]
  }'
```