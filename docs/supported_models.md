# Supported Models

HyperDex supports a variety of generative Transformer models in [HuggingFace Transformers](https://huggingface.co/models). The following is the list of model architectures that are currently supported by HyperDex. Alongside each architecture, we include some popular models that use it.

|**Architecture**         |**Models**                             |**Example HuggingFace Models**                           |**Orion**        |**Hybrid**       |
|-------------------------|---------------------------------------|---------------------------------------------------------|-----------------|-----------------|
|`CohereForCausalLM`      |Cohere                                 |`CohereForAI/c4ai-command-r-v01`, etc.                   |:material-check: |:material-check: |
|`ExaoneForCausalLM`      |EXAONE                                 |`LGAI-EXAONE/EXAONE-3.0-7.8B`, etc.                      |:material-check: |:material-close: |
|`FalconForCausalLM`      |Falcon                                 |`tiiuae/falcon-7b`, `tiiuae/falcon-40b`, etc.            |:material-check: |:material-close: |
|`GemmaForCausalLM`       |Gemma                                  |`google/gemma-2b`, `google/gemma-7b`, etc.               |:material-check: |:material-close: |
|`GPT2LMHeadModel`        |GPT-2                                  |`gpt2`, `gpt2-xl`, etc.                                  |:material-check: |:material-check: |
|`GPTBigCodeForCausalLM`  |StarCoder, SantaCoder, WizardCoder     |`bidcode/starcoder`, etc.                                |:material-check: |:material-close: |
|`GPTJForCausalLM`        |GPT-J                                  |`EleutherAI/gpt-j-6b`, etc.                              |:material-check: |:material-close: |
|`GPTNeoXForCausalLM`     |GPT-NeoX,<br>Pythia,<br>StableLM       |`EleutherAI/gpt-neox-20b`, `EleutherAI/pythia-12b`, etc. |:material-check: |:material-close: |
|`LlamaForCausalLM`       |LLaMA,<br>LLAMA-2,<br>LLAMA-3          |`meta-llama/Llama-2-7b-hf`, `01-ai/Yi-6B`, etc.          |:material-check: |:material-check: |
|`MistralForCausalLM`     |Mistral<br>Mistral-Instruct            |`mistralai/Mistral-7B-v0.1`, etc.                        |:material-check: |:material-check: |
|`OPTForCausalLM`         |OPT                                    |`facebook/opt-1.3b`, `facebook/opt-66b`, etc.            |:material-check: |:material-check: |
|`OlmoForCausalLM`        |OLMo                                   |`allenai/OLMo-7B`, etc.                                  |:material-check: |:material-close: |
|`PhiForCausalLM`         |Phi                                    |`microsoft/phi-1_5`, `microsoft/phi-2`, etc.             |:material-check: |:material-close: |
|`Phi3ForCausalLM`        |Phi3                                   |`microsoft/phi-3`, etc.                                  |:material-check: |:material-check: |
|`StableLmForCausalLM`    |StableLM                               |`stabilityai/stablelm-3b-4e1t/`, etc.                    |:material-check: |:material-close: |
|`StarCoder2ForCausalLM`  |StarCoder2                             |`bigcode/starcoder2-15b`, etc.                           |:material-check: |:material-close: |

!!! note
    Models marked under the “**Hybrid**” tab have been validated for use in the LPU-GPU Hybrid system. For GPU validation, the models have been tested with NVIDIA’s Ampere, Hopper, and Ada Lovelace product lines, ensuring compatibility and performance across these architectures.
