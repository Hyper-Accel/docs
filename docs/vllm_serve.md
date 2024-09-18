

### Serving Model

hyperdex-vllm provides an HTTP server that implements vLLM API. You can execute the server by the command below.

```shell linenums="1"
$ python -m vllm.entrypoints.api_server --model facebook/opt-1.3b \
        --device fpga --tensor-parallel-size 1 --stream

... OMISSION ...

INFO:     Started server process [27157]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://0.0.0.0:8000 (Press CTRL+C to quit)
```

### Descriptions of HyperDex-vLLM Serve Arguments

| HyperDex-vLLM Serve Arguments    | Description                                                                   |
|-----------------------|-------------------------------------------------------------------------------|
| `model`               | Name of path of the huggingface model to use. Default: "facebook/opt-125m"    |
| `device`              | Device type for vLLM execution. Default: "fpga" (WIP)                         |
| `tensor-parallel-size`| Number of tensor parallel replicas. Default: 1                                |
| `tokenizer`           |   Name of path of the huggingface tokenizer to use. If unspecified, model name of path will be used   |
| `trust-remote-code`   |   Trust remote code from huggingface. Default: False                         |


### Client

To call the server, you can use the client example provided in `vllm/examples` ensuring that `use_beam_search=False`.

```shell linenums="1"
import argparse
import json
from typing import Iterable, List
import requests

def post_http_request(prompt: str,
                      api_url: str,
                      n: int = 1,
                      stream: bool = False) -> requests.Response:
    headers = {"User-Agent": "Test Client"}
    pload = {
        "prompt": prompt,
        "n": n,
        # LPU Currently does not support beam_search
        "use_beam_search": False,
        "temperature": 0.8,
        "max_tokens": 32,
        "top_p": 0.95,
        "top_k": 1,
        "stream": stream,
    }
    response = requests.post(api_url,
                             headers=headers,
                             json=pload,
                             stream=stream)
    return response

def get_streaming_response(response: requests.Response) -> Iterable[List[str]]:
    for chunk in response.iter_lines(chunk_size=8192,
                                     decode_unicode=False,
                                     delimiter=b"\0"):
        if chunk:
            data = json.loads(chunk.decode("utf-8"))
            output = data["text"]
            yield output

def get_response(response: requests.Response) -> List[str]:
    data = json.loads(response.content)
    output = data["text"]
    return output
if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("--host", type=str, default="localhost")
    parser.add_argument("--port", type=int, default=8000)
    parser.add_argument("--prompt", type=str, default="Hello, my name is")
    parser.add_argument("--stream", action="store_true")
    args = parser.parse_args()
    prompt = args.prompt
    api_url = f"http://{args.host}:{args.port}/generate"
    stream = args.stream

    print(f"Prompt: {prompt!r}\n", flush=True)
    response = post_http_request(prompt, api_url, stream)

    if stream:
        for h in get_streaming_response(response):
            for line in h:
                print(f"{line!r}", flush=True)
    else:
        output = get_response(response)
        for line in output:
            print(f"{line!r}", flush=True)
```


## OpenAI Compatible Server

### Serving Model

hyperdex-vllm also provides an HTTP server that implements OpenAI's Completions API. You can call the server by the command below.
NOTE(hyunjun): (WIP) max_tokens && streaming
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

To call the server, you can use OpenAI Python client library, or any other HTTP client. 

```shell linenums="1"
from openai import OpenAI

# Modify OpenAI's API key and API base to use vLLM's API server.
openai_api_key = "EMPTY"
openai_api_base = "http://localhost:8000/v1"

client = OpenAI(
    api_key=openai_api_key,
    base_url=openai_api_base,
)

models = client.models.list()
model = models.data[0].id

# Completion API
stream=True
prompt="Hello, my name is"
completion = client.completions.create(
    model=model,
    prompt=prompt,
    stream=stream,
    )

print("Prompt:", prompt)
print("Completion results:")
if stream:
    for c in completion:
        print(c.choices[0].text, end="")
    print()
else:
    print(completion)

```

