<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

Since the **LPU** is based on [AMD’s Alveo FPGA](https://www.amd.com/ko/products/accelerators/alveo/u55c/a-u55c-p00g-pq-g.html#get-started), using it requires the installation of [Xilinx drivers and the Xilinx Runtime (XRT)](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/alveo/u55c.html) software. In addition to these, HyperAccel has developed its own **HyperDex-Toolchain** to run hardware optimized for large language models (LLM) efficiently. HyperDex-Toolchain includes runtime package which allows the LPU to fully utilize its hardware capabilities, providing enhanced performance for deep learning models.


### STEP 1: Requirements

Currently, the HyperDex-Toolchain supports RHEL-8/8 and Ubuntu-22.04-LTS, ensuring compatibility with these platforms for optimal performance. Please follow [XRT install guide](./_install_xrt.md).

The table below shows the compatibility of Python, CUDA, and Torch versions for the HyperDex package. Please ensure your environment matches one of the supported configurations before installation.

| **Python Version** | **CUDA Version** | **Torch Version**     |
|---------------------|------------------|------------------------|
| 3.10, 3.11, 3.12    | CPU-only, 12.6   | 2.7.0+cpu, 2.7.0+cu126 |

To start installation, run following command to make new enviroment.
We will use uv—a fast Python package and virtual-environment manager—in this docs to setup enviroment for HyperDex-Toolchain fast and easy.

```python
$ # (Recommended) Create a new virtual environemnt. We recommend to use uv(https://docs.astral.sh/uv/)
$ curl -LsSf https://astral.sh/uv/install.sh | sh
$ uv venv -p python==3.10 --no-project --seed .hdex-env
$ source .hdex-env/bin/activate
```

### STEP 2: Install Python Package

You can install `hyperdex python package` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:


!!! note
    To run following command, use given hyperaccel pypi id and password for pypi_id and pypi_pw.
    Please [contact](mailto:support@hyperaccel.ai) us if you need help

```python

$ # Install HyperDex-Toolchain in LPU only env
$ uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cpu
$ uv pip install -i https://<pypi_id:pypi_pw>@pypi.hyperaccel.ai/simple hyperdex-toolchain==1.5.1+cpu

$ # Install HyperDex-Toolchain in LPU+GPU env
$ uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cu126
$ uv pip install -i https://<pypi_id:pypi_pw>@pypi.hyperaccel.ai/simple hyperdex-toolchain==1.5.1+cu126
```

### STEP 3: Quick Start

To operate the LPU, all two components (**XRT, Python Package**) must be installed on your server. If you are ready, proceed to the [next step](./quick_start.md).