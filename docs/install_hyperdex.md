<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

Since the **LPU** is based on [AMD’s Alveo FPGA](https://www.amd.com/ko/products/accelerators/alveo/u55c/a-u55c-p00g-pq-g.html#get-started), using it requires the installation of [Xilinx drivers and the Xilinx Runtime (XRT)](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/alveo/u55c.html) software. In addition to these, HyperAccel has developed its own **HyperDex-Toolchain** to run hardware optimized for large language models (LLM) efficiently. HyperDex-Toolchain includes runtime package which allows the LPU to fully utilize its hardware capabilities, providing enhanced performance for deep learning models.


### STEP 1: Requirements

Currently, the HyperDex-Toolchain supports RHEL-8/8 and Ubuntu-22.04-LTS, ensuring compatibility with these platforms for optimal performance. Please follow [XRT install guide](./_install_xrt.md).

HyperDex-Toolchain supports Python 3.10, 3.11, 3.12 with Torch 2.7.0 and CUDA 12.6. Please ensure your environment matches one of the supported configurations before installation.

---

### Notes:
- **CUDA Compatibility**:
  - Torch 2.7.0 supports CUDA 12.6

---

### STEP 2: Install Python Package

You can install `hyperdex python package` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```python
$ # (Recommended) Create a new virtual environemnt. We recommend to use uv(https://docs.astral.sh/uv/)
$ curl -LsSf https://astral.sh/uv/install.sh | sh
$ uv venv --no-project --seed .hdex-env
$ source .hdex-env/bin/activate

$ # Install HyperDex Python Package
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-toolchain
```

### STEP 3: Install Device Package

For security issues, we are directly providing it to those who have inquired.

### STEP 4: Quick Start

To operate the LPU, all two components (**XRT, Python Package**) must be installed on your server. If you are ready, proceed to the [next step](./quick_start.md).

Refer to the step-by-step installation guide provided below.
Note that you need an HyperDex portal account to proceed the installation.
Please [contact](mailto:contact@hyperaccel.ai) us for more information.