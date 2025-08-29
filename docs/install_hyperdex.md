<!---
Copyright 2024 The HyperAccel Inc. All rights reserved.
-->

# HyperDex Installation Guide

This guide will walk you through installing HyperDex-Toolchain to run large language models on HyperAccel's LPU hardware.

## Overview

The **LPU** is built on [AMD's Alveo FPGA](https://www.amd.com/ko/products/accelerators/alveo/u55c/a-u55c-p00g-pq-g.html#get-started) platform and requires two main components:

1. **Xilinx Runtime (XRT)**: Low-level drivers and runtime for FPGA hardware
2. **HyperDex-Toolchain**: HyperAccel's optimized software stack for LLM inference

The HyperDex-Toolchain includes a runtime package that allows the LPU to fully utilize its hardware capabilities, providing enhanced performance for large language models.


## Prerequisites

### System Requirements
- **Operating System**: RHEL-8/8 or Ubuntu-22.04-LTS
- **Hardware**: AMD Alveo FPGA card (U55C)
- **Xilinx Runtime (XRT)**: Required for FPGA operations

!!! note "XRT Installation"
    Before proceeding, make sure XRT is properly installed. Follow our [XRT install guide](./_install_xrt.md) for detailed instructions.

### Software Compatibility

The table below shows the supported Python, CUDA, and PyTorch versions. Ensure your environment matches one of these configurations:

| **Python Version** | **CUDA Version** | **PyTorch Version** |
|---------------------|------------------|---------------------|
| 3.10, 3.11, 3.12   | CPU-only, 12.6  | 2.7.0+cpu, 2.7.0+cu126 |

## Step 1: Set Up Python Environment

We recommend using `uv`, a fast Python package and virtual environment manager, to set up your environment quickly and efficiently.

### Install uv and Create Virtual Environment

```bash
# Install uv package manager
$ curl -LsSf https://astral.sh/uv/install.sh | sh

# Create a new virtual environment with Python 3.10
$ uv venv -p python==3.10 --no-project --seed .hdex-env

# Activate the virtual environment
$ source .hdex-env/bin/activate
```

## Step 2: Install HyperDex Python Package

HyperDex-Toolchain is distributed through [HyperAccel's private PyPI server](https://pypi.hyperaccel.ai), which requires authentication credentials.

### Set Up PyPI Authentication

!!! warning "Environment Variables Required"
    To run the following commands, you'll need your HyperAccel PyPI credentials. Please [contact our support team](mailto:support@hyperaccel.ai) if you need help obtaining credentials.
    ```bash
    export PYPI_ID="your_pypi_username"
    export PYPI_PW="your_pypi_password"
    ```
    These credentials will be used to authenticate with HyperAccel's private PyPI server.

### Choose Your Installation

Select the appropriate installation option based on your hardware setup:

#### Option A: LPU-Only Environment (CPU PyTorch)

```bash
# Install PyTorch CPU version
$ uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cpu

# Install HyperDex-Toolchain for CPU
$ uv pip install -i https://pypi.hyperaccel.ai/simple hyperdex-toolchain==1.5.2+cpu
```

#### Option B: LPU+GPU Environment (CUDA PyTorch)

```bash
# Install PyTorch CUDA version
$ uv pip install torch==2.7.0 torchvision==0.22.0 torchaudio==2.7.0 --index-url https://download.pytorch.org/whl/cu126

# Install HyperDex-Toolchain for CUDA
$ uv pip install -i https://${PYPI_ID}:${PYPI_PW}@pypi.hyperaccel.ai/simple hyperdex-toolchain==1.5.2+cu126
```

## Step 3: Verification and Next Steps

To operate the LPU successfully, ensure both components are properly installed:

1. ✅ **Xilinx Runtime (XRT)** - Hardware drivers and runtime
2. ✅ **HyperDex-Toolchain** - Python package and dependencies

!!! tip "Verify Installation"
    Use the following commands to confirm your installation is working correctly:

    **Check XRT Installation:**
    ```bash
    # Verify XRT installation and hardware detection
    $ xbutil examine
    ```

    **Check HyperDex-Toolchain Installation:**
    ```bash
    # Verify HyperDex-Toolchain package is installed
    $ uv pip list | grep hyperdex-toolchain
    ```

### Ready to Start?

Once installation is complete and verified, you can begin using HyperDex for LLM inference. Proceed to our [Quick Start Guide](./quick_start.md) to run your first model on the LPU.