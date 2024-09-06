<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

Since the **LPU** is based on [Xilinx’s Alveo FPGA](https://www.amd.com/ko/products/accelerators/alveo/u55c/a-u55c-p00g-pq-g.html#get-started), using it requires the installation of [Xilinx drivers and the Xilinx Runtime (XRT)](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/alveo/u55c.html) software. In addition to these, HyperAccel has developed its own **HyperDex Runtime Library (HRT)** to efficiently run hardware optimized for large language models (LLM). This runtime allows the LPU to fully utilize its hardware capabilities, providing enhanced performance for deep learning models.

!!! note
    Since the HyperDex Runtime Library is linked with the Xilinx Runtime (XRT), it is important to install the appropriate versions of both HRT and XRT depending on the Linux kernel version in use. Currently, the HRT supports RHEL-7/8 and Ubuntu-22.04-LTS, ensuring compatibility with these platforms for optimal performance

**HyperDex Compiler** also plays a crucial role in this ecosystem by offering one-click compilation for HuggingFace models, allowing them to seamlessly adapt to the LPU architecture. This enables pre-trained models to be converted into a format optimized for inference on LPU-based hardware with minimal effort.

To operate the LPU, all three components (**XRT, HRT and Compiler**) must be installed on your server. These software stacks are available as both RPM and DEB packages, making them easy to install depending on your operating system.

- Xilinx Runtime Library (`rpm`/`deb` package)
- HyperDex Runtime Library (`rpm`/`deb` package)
- HyperDex Compiler Library (`rpm`/`deb` package)

!!! note
    For the Docker setup we will discuss later, the installation of the XRT is also required. You can find more detailed instructions on how to install XRT through this [link](https://xilinx.github.io/XRT/master/html/index.html).

Refer to the step-by-step installation guide provided below.
Note that you need an HyperDex portal account to proceed the installation.
Please [contact](mailto:contact@hyperaccel.ai) us for more information.

!!! warning
    We are currently in the process of building the **HyperDex Portal**, but for now, package files are being distributed directly by our team. The installation process after receiving the packages remains the same, so please follow the standard procedure. 