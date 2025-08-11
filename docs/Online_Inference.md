<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

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
example results
```shell linenums="1"
Prompt: 'Act like an experienced HR Manager. Develop a human resources strategy for retaining top talents in a competitive industry. Industry: (e.g Energy( Workforce: (e,g 550) Style: (e.g Formal) Tone: (e.g Convincing)'

Act like an experienced HR Manager. Develop a human resources strategy for retaining top talents in a competitive industry. Industry: (e.g Energy( Workforce: (e,g 550) Style: (e.g Formal) Tone: (e.g Convincing)
Beam candidate 0: 'Act like an experienced HR Manager. Develop a human resources strategy for retaining top talents in a competitive industry. Industry: (e.g Energy( Workforce: (e,g 550) Style: (e.g Formal) Tone: (e.g Convincing) Length: (e.g 2 pages) 1. Introduction 2. Current Situation 3. Retention Strategy 4. Implementation Plan 5. Conclusion 6. References 7.'
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
python openai_chat_completion_client.py
```
example results
```shell linenums="1"
Chat completion results:
ChatCompletion(id='chat-4a3b41b2d4a343ec82fe51458d4e4453', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='The 2020 World Series was played at Globe Life Field in Arlington, Texas. Due to the COVID-19 pandemic, the series was played in a neutral site instead of the traditional home field advantage.', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[]), stop_reason=None)], created=1754904729, model='MLP-KTLim/llama-3-Korean-Bllossom-8B', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=42, prompt_tokens=59, total_tokens=101, completion_tokens_details=None, prompt_tokens_details=None), prompt_logprobs=None)
```
