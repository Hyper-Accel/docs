## HyperDex Model Software-Development-Kit

HyperDex helps to run LPUs using Hugging Face's checkpoints. The following content explains how to transform Hugging Face models to be executable on LPUs.

## Installation

This repository is tested on Python 3.9+, PyTorch 2.0+ and HuggingFace Transformers 4.35+. HyperDex can be installed using pip as follows:

```bash
pip install hyperdex-sdk
```

## Compile HuggingFace-Hub Model

HyperDex SDK provides a CLI-based program. You can use the `--prefix` option to determine the directory path where the model will be downloaded. For large-sized models or private models that are not uploaded to the HuggingFace-Hub, you can manually move them to the specified path and then use the SDK. (i.g. `facebook/opt-1.3b` model would be downloaded to `/path/to/prefix/facebook/opt-1.3b/ckpt`.)

Start the SDK program:
```bash
$ python -m hyperdex_sdk.cli.run --prefix /path/to/prefix
```
After running the program, you can input the necessary information such as the model's ID, the number of devices, and other details according to the program's instructions.

Here is an example of running the SDK program for the "facebook/opt-1.3b" model:
```shell
**********************************************************************
** HyperDex Model SDK Version 1.3.1
** Copyright (c) 2024 HyperAccel
**********************************************************************

** Phase-1 : Download model from HuggingFace Hub **

In this phase, download the HuggingFace model and save it to the prefix (=/opt/hyperdex/models).
ex) facebook/opt-1.3b ==> download at "/opt/hyperdex/models/facebook/opt-1.3b/ckpt"

Please input the model available on Hugging Face from the options below.
If you are using a custom model, make sure to place it in the prefix path beforehand for it to be usable.

[Option	] Please enter the HuggingFace model id: facebook/opt-1.3b
[Info	] Download model at /opt/hyperdex/models/facebook/opt-1.3b/ckpt

** Phase-2 : Convert the checkpoint format to HyperDex style **

In this phase, we convert the Hugging Face checkpoint into a format that HyperDex can read.
The converter output includes both a JSON file and a binary file.

[Info	] Convert the model to HyperDex model format
[Info	] Save the converted checkpoint at /opt/hyperdex/models/facebook/opt-1.3b/ckpt

** Phase-3 : Generate Optimized Model Mapping **

In this phase, we optimize model mapping for streamlined memory access and efficient model parallelism.
The result of this optimization process is a binary file.

[Option	] Please enter the number of LPU devices: 1
[Info	] Optimize the model paramater
[Info	] Save the optimized data at /opt/hyperdex/models/facebook/opt-1.3b/param

** Phase-4 : Generate Optimized Model Instruction **

In this phase, we analyze the structure of the model to generate optimized instructions.
We use techniques such as Kernel Fusion and Layer Reordering to make hardware processing more efficient.
The result of this optimization is a binary file.
[Info	] Optimize the model instruction
[Info	] Save the optimized instruction at /opt/hyperdex/models/facebook/opt-1.3b/inst
[Info	] Exit SDK program
```

To refer to the list of models supported by the HyperDex SDK, please visit the following page: [Supported Models](./models.md)
