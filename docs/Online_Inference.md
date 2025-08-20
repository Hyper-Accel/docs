<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

### Requirements and Install Guide
Requirements and Install Guide is same as[ vLLM API](./Offline_Inference.md). 


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
Arguments are the same as [vLLM Engine](./Offline_Inference.md).


### Client

To call the server, you can use the client example provided in `vllm/examples` ensuring that `use_beam_search=False`.

```shell linenums="1"
# You can see this file in our vLLM repo. (vllm/examples/lpu_client.py)
python lpu_client.py --stream
```
example results
```shell linenums="1"
Prompt: 'Act like an experienced HR Manager. Develop a human resources strategy for retaining top talents in a competitive industry. Industry: (e.g Energy( Workforce: (e,g 550) Style: (e.g Formal) Tone: (e.g Convincing)'

Act like an experienced HR Manager. Develop a human resources strategy for retaining top talents in a competitive industry. Industry: (e.g Energy( Workforce: (e,g 550) Style: (e.g Formal) Tone: (e.g Convincing)
Beam candidate 0: 'Act like an experienced HR Manager. Develop a human resources strategy for retaining top talents in a competitive industry. Industry: (e.g Energy( Workforce: (e,g 550) Style: (e.g Formal) Tone: (e.g Convincing) Length: (e.g 2 pages) 1. Introduction 2. Current Situation 3. Retention Strategy 4. Implementation Plan 5. Conclusion 6. References 7.'
```