
# Quick Start Guide

Get started with HyperDex in just a few minutes! This guide shows you how to run your first large language model inference on HyperAccel's LPU.

## Prerequisites

Before you begin, make sure you have:

- ✅ **Xilinx Runtime (XRT)** installed on your system
- ✅ **HyperDex-Toolchain** Python package installed
- 📝 If you haven't completed the installation, follow our [Installation Guide](./install_hyperdex.md)

## Your First LLM Inference

### Step 1: Import HyperDex Libraries

HyperDex provides a familiar API similar to [HuggingFace Transformers](https://huggingface.co/docs/transformers/index), making it easy to get started.

```python linenums="1"
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer
```

### Step 2: Load Model and Tokenizer

```python linenums="3"
# Load tokenizer from HuggingFace Hub
tokenizer = AutoTokenizer.from_pretrained("facebook/opt-125m")

# Load model and map it to LPU device
model = AutoModelForCausalLM.from_pretrained("facebook/opt-125m", device_map={"lpu": 1})
```

!!! tip "Device Mapping"
    The `device_map={"lpu": 1}` parameter tells HyperDex to load the model onto the LPU hardware.

### Step 3: Generate Text

```python linenums="7"
# Encode input text
input_ids = tokenizer.encode("Hello world!", return_tensors="np")

# Generate text using the LPU
output_ids = model.generate(input_ids, max_length=1024, do_sample=False)

# Decode the generated output
outputs = tokenizer.decode(output_ids)
print(outputs)
```

### Complete Example

Here's the complete working example:

```python linenums="1"
from hyperdex.transformers import AutoModelForCausalLM
from hyperdex.transformers import AutoTokenizer

# Load tokenizer and model
tokenizer = AutoTokenizer.from_pretrained("facebook/opt-1.3b")
model = AutoModelForCausalLM.from_pretrained("facebook/opt-1.3b", device_map={"lpu": 1})

# Generate text
input_ids = tokenizer.encode("Hello world!", return_tensors="np")
output_ids = model.generate(input_ids, max_length=1024, do_sample=False)
outputs = tokenizer.decode(output_ids)
print(outputs)
```

## Key Design Principles

- **Familiar APIs**: HyperDex mirrors HuggingFace Transformers APIs, so you can leverage your existing knowledge
- **Hardware Abstraction**: LPU optimization schemes are handled automatically behind the scenes
- **Zero Learning Curve**: If you know HuggingFace, you already know HyperDex

## Next Steps

🎯 **Ready to explore more?** Check out these resources:

- **[Advanced Examples](./offline_inference.md)**: vLLM API for LPU
- **[API Documentation](./hyperdex_toolchain.md)**: Detailed API reference

## Additional Resources

📖 **Quick Reference Guide**: If you have a server with HyperDex already installed, you can also refer to our [PDF user guide](./LPU_user_guide_v1.1.pdf) for a quick overview.

