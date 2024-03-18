<!---
Copyright 2023 The HyperAccel. All rights reserved.
-->

<p align="center">
    <br>
    <img src="./images/logo.png" width="400"/>
    <br>
</p>

Provides an implementation of python package for HyperAccel LPU™, with a focus on performance and versatility.<br>

## Quick tour

Similar to [HuggingFace transformer](https://huggingface.co/docs/transformers/index) package, HyperDex uses an `AutoModelForCausalLM` module to load the Transformers. To load the model parameters, you can simply give the path of the HyperDex model checkpoint.

```python
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer

# Load tokenzier and model
tokenizer = AutoTokenizer.from_pretrained("llama-7b")
model = AutoModelForCausalLM.from_pretrained("llama-7b")

# Text Generation
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
output_ids = model.generate(input_ids, max_length=1024, do_sample=False)
outputs = tokenizer.decode(input_ids)
```

The tokenizer is responsible for all the preprocessing the pretrained model expects, and can be called directly on a single string (as in the above examples) or a list. It will output a dictionary that you can use in downstream code or simply directly pass to your model using the `generate` API.

## Main features

 - APIs of `hyperdex.transformers` are similar to HuggingFace, which are easy to integrate with various LLM applications.
 - Fast model loading scheme with custom checkpoint format
 - Streaming text geneartion