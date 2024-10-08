To ensure optimal performance on the LPU, it's essential to install the **HyperDex Runtime Library (HRT)** alongside the Xilinx Runtime (XRT). The HRT is specifically designed to work with hardware optimized for large language models (LLM), enabling the LPU to maximize its potential in deep learning tasks. The HRT ensures smooth communication between software and hardware and enhances performance for AI workloads.

!!!note
	Before installing <strong>HRT</strong>, you need to ensure that <u>the Xilinx Runtime (XRT) is already installed</u>, as HRT depends on XRT for its operations. Make sure that the versions of both HRT and XRT are compatible with your Linux kernel version, as HRT currently supports Centos-7, Ubuntu 22.04 LTS, Rocky 8.4.

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

	<code>hyperdex-net</code>: This command is used for managing network configurations between multiple FPGAs. It helps generate the <u>table.json</u> file, which defines the communication paths between devices, based on the <u>network.xclbin</u> file.

### Step 2. Connect QSFP Cables
Before generating the `table.json` file, ensure that the QSFP cables are correctly connected to each device in a ring topology. This setup allows network communication between multiple FPGAs. For multi-FPGA configurations, each device should be connected to its neighboring FPGA in the network. 

 **Ring Topology**: Connect the QSFP cable from the last FPGA back to the first FPGA to complete the loop.

!!! warning
	Proper cable connections are essential for the FPGAs to communicate, and without this step, the subsequent network configuration may fail.

### Step 3. Generate table.json File

After connecting the QSFP cables and installing the HRT package, the next step is to generate the **table.json** file, which is essential if you're working in a **multi-FPGA environment**. This file defines the network communication paths between the FPGAs. If you're using a single FPGA, this file may not be required unless your application specifically needs network configuration.

You can use the `hyperdex-net` command to generate and manage the `table.json` file. When you run `hyperdex-net`, the `table.json` file will be generated in the `/opt/hyperdex/hrt/xclbin` directory. If all the values in the generated table are `0`, this indicates that the table was not created correctly and you may need to troubleshoot the configuration.
.

```bash linenums="1""""
# ./hyperdex-net
```

```bash linenums="1"
# vi /opt/hyperdex/hrt/xclbin/table.json

{
	"network" :
	[
		{
			"direction" : "Right",
			"dst_id" : 1,
			"src_id" : 0
		},
		{
			"direction" : "Right",
			"dst_id" : 2,
			"src_id" : 1
		},
		{
			"direction" : "Right",
			"dst_id" : 3,
			"src_id" : 2
		},
		{
			"direction" : "Right",
			"dst_id" : 0,
			"src_id" : 3
		}
         ],
	"num_device" : 4
```
