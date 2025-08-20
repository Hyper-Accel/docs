<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

### Requirements and Install Guide
Requirements and Install Guide is same as[ vLLM API](./offline_inference.md). 


## OpenAI Compatible Server

### Serving Model

hyperdex-vllm also provides an HTTP server that implements OpenAI's Completions API. You can call the server by the command below.
```shell linenums="1"
python -m vllm serve --model facebook/opt-1.3b \
        --device fpga --disable-frontend-multiprocessing

... OMISSION ...

INFO:     Started server process [27157]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

### Client

To call the server, you can use OpenAI Python client library, or any other HTTP client. Change stream option if you want.

```shell linenums="1"
# You can see this file in our vLLM repo. (vllm/examples/openai_chat_completion_client.py)
python openai_chat_completion_client.py
```
example results
```shell linenums="1"

Chat completion results:
ChatCompletion(id='chat-4a3b41b2d4a343ec82fe51458d4e4453', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='The 2020 World Series was played at Globe Life Field in Arlington, Texas. Due to the COVID-19 pandemic, the series was played in a neutral site instead of the traditional home field advantage.', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=[]), stop_reason=None)], created=1754904729, model='MLP-KTLim/llama-3-Korean-Bllossom-8B', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=42, prompt_tokens=59, total_tokens=101, completion_tokens_details=None, prompt_tokens_details=None), prompt_logprobs=None)
```
