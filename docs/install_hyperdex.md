<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

Since the **LPU** is based on [AMD’s Alveo FPGA](https://www.amd.com/ko/products/accelerators/alveo/u55c/a-u55c-p00g-pq-g.html#get-started), using it requires the installation of [Xilinx drivers and the Xilinx Runtime (XRT)](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/alveo/u55c.html) software. In addition to these, HyperAccel has developed its own **HyperDex Runtime Library (HRT)** to run hardware optimized for large language models (LLM) efficiently. This runtime allows the LPU to fully utilize its hardware capabilities, providing enhanced performance for deep learning models.


### STEP 1: Requirements

Currently, the HRT supports RHEL-8/8 and Ubuntu-22.04-LTS, ensuring compatibility with these platforms for optimal performance. Please follow [XRT install guide](./install_xrt.md).


### STEP 2: Install Python Package

You can install `hyperdex python package` using pip, which requires access rights to [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai). To install the HyperDex Python package, run the following command:

```python linenums="1" hl_lines="6 7 9"
$ # (Recommended) Create a new conda environemnt.
$ conda create -n hdex-env python=3.10 -y
$ conda activate hdex-env

$ # Install HyperDex Python Package
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-transformers
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-compiler
$ # (Option)
$ pip install -i https://pypi.hyperaccel.ai/simple hyperdex-vllm
```


### STEP 3: Install RPM Package

(TBD)


### STEP 4: Quick Start

To operate the LPU, all three components (**XRT, Python Package and RPM Package**) must be installed on your server. If you are ready, proceed to the [next step](./quick_start.md).

Refer to the step-by-step installation guide provided below.
Note that you need an HyperDex portal account to proceed the installation.
Please [contact](mailto:contact@hyperaccel.ai) us for more information.

