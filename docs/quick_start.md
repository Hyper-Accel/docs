
## Text Generation with HyperAccel LPU™

Similar to [HuggingFace transformer](https://huggingface.co/docs/transformers/index) package, HyperDex uses an `AutoModelForCausalLM` module to load the Transformers. To load the model parameters, you can simply give the path of the HyperDex model checkpoint.

```python linenums="1"
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer

# Load tokenzier and model
tokenizer = AutoTokenizer.from_pretrained("facebook/opt-1.3b")
model = AutoModelForCausalLM.from_pretrained("facebook/opt-1.3b", device_map={"lpu": 1})

# Text Generation
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
output_ids = model.generate(input_ids, max_length=1024, do_sample=False)
outputs = tokenizer.decode(output_ids)
print(outputs)
```

The tokenizer is responsible for all the preprocessing the pretrained model expects, and can be called directly on a single string (as in the above examples) or a list. It will output a dictionary that you can use in downstream code or simply directly pass to your model using the `generate` API.


!!! note
    To run the above steps, you must first install the `hyperdex-python` package using pip. For detailed instructions on the installation process, please refer to [Python API](./python_api.md) page of the documentation.​

## Main features

 - APIs of `hyperdex.transformers` are similar to HuggingFace, which are easy to integrate with various LLM applications.
 - Fast model loading scheme with custom checkpoint format
 - Streaming text generation

## Quick Guide (PDF)
If you have a server with HyperDex installed, please refer [this PPT](./LPU_user_guide_v1.1.pdf).

