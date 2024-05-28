# Supported Models

HyperDex supports a variety of generative Transformer models in [HuggingFace Transformers](https://huggingface.co/models). The following is the list of model architectures that are currently supported by HyperDex. Alongside each architecture, we include some popular models that use it.

|Architecture|Models|Example HuggingFace Models|Hybrid Support|
|-|-|-|-|
|`FalconForCausalLM`      |Falcon                                                       |`tiiuae/falcon-7b`, `tiiuae/falcon-40b`, etc.            |X  |
|`GemmaForCausalLM`       |Gemma                                                        |`google/gemma-2b`, `google/gemma-7b`, etc.               |X  |
|`GPT2LMHeadModel`        |GPT-2                                                        |`gpt2`, `gpt2-xl`, etc.                                  |O  |
|`GPTBigCodeForCausalLM`  |StarCoder, SantaCoder, WizardCoder                           |`bidcode/starcoder`, etc.                                |X  |
|`GPTJForCausalLM`        |GPT-J                                                        |`EleutherAI/gpt-j-6b`, etc.                              |X  |
|`GPTNeoXForCausalLM`     |GPT-NeoX,<br>Pythia,<br>OpenAssistant,<br>Dolly,<br>StableLM |`EleutherAI/gpt-neox-20b`, `EleutherAI/pythia-12b`, etc. |X  |
|`InternLM2ForCausalLM`   |InternLM2                                                    |`internlm/internlm2-7b`, etc.                            |X  |
|`LlamaForCausalLM`       |LLaMA,<br>LLAMA-2,<br>LLAMA-3,<br>Alpaca,<br>Yi              |`meta-llama/Llama-2-7b-hf`, `01-ai/Yi-6B`, etc.          |O  |
|`MistralForCausalLM`     |Mistral<br>Mistral-Instruct                                  |`mistralai/Mistral-7B-v0.1`, etc.                        |X  |
|`OPTForCausalLM`         |OPT                                                          |`facebook/opt-1.3b`, `facebook/opt-66b`, etc.            |O  |
|`OrionForCausalLM`       |Orion                                                        |`OrionStarAI/Orion-14B-Base`, etc.                       |X  |
|`PhiForCausalLM`         |Phi                                                          |`microsoft/phi-1_5`, `microsoft/phi-2`, etc.             |X  |
|`Phi3ForCausalLM`        |Phi3                                                         |`microsoft/phi-3`, etc.                                  |O  |
|`Qwen2ForCausalLM`       |Qwen2                                                        |`Qwen/Qwen2-beta-7B`, `Qwen/Qwen2-beta-7B-Chat`, etc.    |X  |
|`StableLmForCausalLM`    |StableLM                                                     |`stabilityai/stablelm-3b-4e1t/`, etc.                    |X  |
|`StarCoder2ForCausalLM`  |StarCoder2                                                   |`bigcode/starcoder2-15b`, etc.                           |X  |