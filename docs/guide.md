## Text Generation with HyperAccel LPU™

HyperDex allows you to output tokens using a function similar to HuggingFace's `generate` function. Therefore, you can easily generate tokens as shown in the example below.
`text_generation.py' is an introductory example code that uses the Hyperdex APIs to generate tokens using the LPU.

```python
# Immport HyperDex transformers
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer

# Path to hyperdex checkpoint (MODIFY model path and model here)
hyperdex_ckpt = "/path/to/llama-7b"

# Load tokenzier and model (MODIFY hardware configuration here)
tokenizer = AutoTokenizer.from_pretrained(ckpt=hyperdex_ckpt)
model = AutoModelForCausalLM.from_pretrained(ckpt=hyperdex_ckpt, num_device=1)

# Input text (MODIFY your input here)
inputs = "Hello world!"

# 1. Encode input text to input token ids
input_ids = tokenizer.encode(inputs, return_tensors="np")
# 2. Generate output token ids with LPU™ (MODIFY sampling parameters here)
output_ids = model.generate(
  inputs,
  max_length=1024,
  # Sampling
  do_sample=True,
  top_p=0.7,
  top_k=50,
  temperature=1.0,
  repetition_penalty=1.2,
  # Stopping
  early_stopping=False
)
# 3. Decode output token ids to output text
outputs = tokenizer.decode(output_ids, skip_special_tokens=True)

# Print the output context
print(outputs)
```

## Sampling

Sampling works in the same way as HuggingFace. For sampling, you have options like top_p, top_k, temperature, and repetition penalty. Please refer to the HuggingFace documentation for explanations of each option. Additionally, the `generate` function allows you to directly control randomness using the seed argument. If `do_smaple` is `False`, LPU does not perform sampling and uses greedy method.

| Sampling Arguments | Description |
|-|-|
| `top_p` | Top-P sampling. Default value is `0.7` |
| `top_k` | Top-K sampling. Default value is `1` |
| `temperature` | Smoothing the logit distribution. Defualt value is `1.0` |
| `repetition_penalty` | Give penlaty to logits. Default value is `1.2` |

## Streaming Token Generation

HyperDex supports streaming token generation in a similar manner to HuggingFace. You can activate it by passing the TextStreamer module as an argument to the `generate` function.

```python
# Immport HyperDex transformers
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer
from hyperdex.transformers import TextStreamer

# Path to hyperdex checkpoint
hyperdex_ckpt = "/path/to/llama-7b"

# Load tokenzier and model
tokenizer = AutoTokenizer.from_pretrained(ckpt=hyperdex_ckpt)
model = AutoModelForCausalLM.from_pretrained(ckpt=hyperdex_ckpt, num_device=1)
# Config streamer module
streamer = TextStreamer(tokenizer, skip_special_tokens=True)
```

Since the `TextStreamer` module includes the process of decoding through the `tokenizer` and printing internally, you only need to call the `generate` function.

```python
# Input text
inputs = "Hello world!"

# 1. Encdoe input text to input token ids
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
# 2. Generate streaming output token ids with LPU™
model.generate(
  inputs,
  max_length=1024,
  # Sampling
  do_sample=True,
  top_p=0.7,
  top_k=50,
  temperature=1.0,
  repetition_penalty=1.2,
  # Streaming
  streamer=streamer
)
```

## How to use streaming with other Application?

HyperDex utilizes the `yield` keyword in Python to enable streaming for use in other applications. When you call the `generate_yield` function, it returns using `yield`, making it easy to use in other Python applications.

```python
# Config streamer module with disable print to activate yield mode
streamer = TextStreamer(tokenizer, use_print=False, skip_special_tokens=True)

# Input text
inputs = "Hello world!"

# 1. Encdoe input text to input token ids
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
# 2. Generate streaming output token ids with LPU™
output_ids = model.generate_yield(
  inputs,
  max_length=1024,
  # Sampling
  do_sample=True,
  top_p=0.7,
  top_k=50,
  temperature=1.0,
  repetition_penalty=1.2,
  # Streaming
  streamer=streamer
)

# Use outputs_ids which type is generator
```
