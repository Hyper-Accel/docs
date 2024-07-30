## Text Generation with API

The `~/example/api_server` allows to make an API setting. This requires two terminals at once, one to run the server and another to act as a client using the server.

## Server Setting

Run the following code to run the server.

```bash
$ python -m hserve.huggingface.api_server 
    --served-model-path /opt/hyperdex/models
    --served-model-name {Hugging Face Model ID} 
    --served-lpu-device-num {# of LPU} 
    --served-gpu-device-num {# of GPU} 
    --verbose
```

Example 
```bash
$ python -m hserve.huggingface.api_server 
    --served-model-path /opt/hyperdex/models 
    --served-model-name TinyLlama/TinyLlama-1.1B-Chat-v1.0 
    --served-lpu-device-num 1 
    --served-gpu-device-num 0 
    --verbose
```

## Client

Run `client.py` inside `~/example/api_server`.

```bash
$ python client.py
```

This will run the LLM model when setting the server(--served-model-name) with (--served-lpu-device-num) number of LPUs. `client.py` is an introductory example code that uses the APIs to generate tokens using the LPU.
```python
import argparse
import requests

# HuggingFace text-generation-inference API
from huggingface_hub import InferenceClient

##########################################################################
## Main
##########################################################################

def main():

  # Get arguments
  parser = argparse.ArgumentParser()
  parser.add_argument("--host", type=str, default="localhost")
  parser.add_argument("--port", type=int, default=8000)
  parser.add_argument("--key", type=str, default="EMPTY")
  parser.add_argument("--stream", action="store_true")
  args = parser.parse_args()
  base_url = "http://{}:{:d}".format(args.host, args.port)

  # TGI client
  client = InferenceClient(model=base_url)
  # Get the response
  output = client.text_generation(
    prompt="Do you know KAIST?",
    max_new_tokens=512,
    # Sampling
    do_sample=False,
    top_p=0.7,
    top_k=50,
    temperature=1.0,
    repetition_penalty=1.2,
    # Streaming
    stream=args.stream
  )

  # Streaming
  if args.stream:
    for text in output:
      print(text, end="", flush=True)
    print()
  else:
    print(output)

if __name__ == "__main__":
  main()

##########################################################################
```
## Customizing

The text_generation() function allows to generate tokens and reponses. To customize the prompt and response length, change the `prompt` and `max_new_tokens` inside the function.

```python
  # Get the response
  output = client.text_generation(
    prompt="Do you know KAIST?",
    max_new_tokens=512,
    # Sampling
    do_sample=False,
    top_p=0.7,
    top_k=50,
    temperature=1.0,
    repetition_penalty=1.2,
    # Streaming
    stream=args.stream
  )
```
From the code above, change the "Do you know KAIST" to generate a different response and change 512 to generate longer response.

## Streaming Option

When generating response, tokens can be displayed as they are made if stream option is added as below.

```bash
$ python client.py --stream
```