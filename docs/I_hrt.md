To ensure optimal performance on the LPU, it's essential to install the **HyperDex Runtime Library (HRT)** alongside the Xilinx Runtime (XRT). The HRT is specifically designed to work with hardware optimized for large language models (LLM), enabling the LPU to maximize its potential in deep learning tasks. The HRT ensures smooth communication between software and hardware and enhances performance for AI workloads.

!!!note
	Before installing <strong>HRT</strong>, you need to ensure that the Xilinx Runtime (XRT) is already installed, as HRT depends on XRT for its operations. Make sure that the versions of both HRT and XRT are compatible with your Linux kernel version, as HRT currently supports Centos-7, Ubuntu 22.04 LTS, Rocky 8.4.

### Step 1. Install HRT Library

For now, the HyperDex Portal is under development, but you can still get the HRT package directly from our team. Please contact us to receive the package files.

!!!Note
	 The HRT package is available in two versions <strong>CPU-only version</strong> and <strong>CUDA version</strong>. You can choose the one that best suits your hardware and needs.

For Each-based systems (e.g. Centos-7, Ubuntu 22.04 LTS, Rocky 8.4)
```bash linenums="1"
# yum install hyperdex-hrt_1.3.2_cpu.rpm or hyperdex-hrt_1.3.2_cu.rpm
```
```bash linenums="1"
# dpkg -i hyperdex-hrt_1.3.2_cpu_amd64.deb or hyperdex-hrt_1.3.2_cu_amd64.deb
```
Once the installation is complete, the HRT files will be located under the `/opt/hyperdex/hrt` directory. You will need to source the `/opt/hyperdex/setup.sh` script to use commands such as `hyperdex-smi`, `hyperdex-reset`, `hyperdex-net`.

!!! question "How to use HyperDex Commands"
	<code>hyperdex-smi</code>: This command is used for monitoring the status of HyperDex devices. It provides information about the state and health of your hardware.

	<code>hyperdex-reset</code>: This command resets HyperDex devices to their default state. Use this command to resolve issues or to reset the devices for reconfiguration.

### Step 2. Generate network.json File

After installing the HRT package, the next step is to generate the **network.json** file, which is essential if you're working in a **multi-FPGA environment**. This file defines the network communication paths between the FPGAs. If you're using a single FPGA, this file may not be required unless your application specifically needs network configuration.

You can use the hyperdex-net command to generate and manage the network.json file.

```bash linenums="1""""
# ./hyperdex-net
```
