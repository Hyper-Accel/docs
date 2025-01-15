<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

Since the **LPU** is based on [AMD’s Alveo FPGA](https://www.amd.com/ko/products/accelerators/alveo/u55c/a-u55c-p00g-pq-g.html#get-started), using it requires the installation of [Xilinx drivers and the Xilinx Runtime (XRT)](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/alveo/u55c.html) software. In addition to these, HyperAccel has developed its own **HyperDex Runtime Library (HRT)** to run hardware optimized for large language models (LLM) efficiently. This runtime allows the LPU to fully utilize its hardware capabilities, providing enhanced performance for deep learning models.


### STEP 1: Requirements

Currently, the HRT supports RHEL-8/8 and Ubuntu-22.04-LTS, ensuring compatibility with these platforms for optimal performance. Please follow [XRT install guide](./install_xrt.md).

The table below shows the compatibility of **Python**, **CUDA**, and **Torch** versions for the HyperDex package. Please ensure your environment matches one of the supported configurations before installation.

| **Python Version** | **CUDA Version** | **Torch Version**     |
|---------------------|------------------|------------------------|
| 3.9                | 12.1, 12.4       | 2.1.0, 2.4.0          |
| 3.10               | 12.1, 12.4       | 2.1.0, 2.4.0          |
| 3.11               | 12.1, 12.4       | 2.1.0, 2.4.0          |
| 3.12               | 12.1             | 2.4.0                 |

---

### Notes:
- **Python 3.12**:
  - Torch 2.1.0 is **not supported**.
  - CUDA 12.4 is **not compatible**.
- **CUDA Compatibility**:
  - Torch 2.1.0 does **not support** CUDA 12.4.
  - Torch 2.4.0 supports both CUDA 12.1 and 12.4.

---

### STEP 2: Install Python Package

You can install `hyperdex python package` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```python
$ # (Recommended) Create a new conda environemnt.
$ conda create -n hdex-env python=3.10 -y
$ conda activate hdex-env

$ # Install HyperDex Python Package
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-transformers
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-compiler
```


### STEP 3: Install Device Package

For security issues, we are directly providing it to those who have inquired.
>>>>>>> feature/install-guide


### STEP 4: Quick Start

To operate the LPU, all three components (**XRT, Python Package and RPM Package**) must be installed on your server. If you are ready, proceed to the [next step](./quick_start.md).

Refer to the step-by-step installation guide provided below.
Note that you need an HyperDex portal account to proceed the installation.
Please [contact](mailto:contact@hyperaccel.ai) us for more information.

