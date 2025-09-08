To use the LPU effectively, installing the Xilinx Runtime (XRT) software is essential. XRT ensures smooth integration with Xilinx's Alveo FPGA, optimizing software-hardware communication for peak performance. It also supports the HyperDex Runtime Library (HRT) and ensures compatibility with Centos-7, Ubuntu 22.04 LTS, Rocky 8.4.

Before starting the installation, make sure that the LPU is properly connected to the device. Then, refer to the step-by-step installation guide provided below. To proceed, you will need to download the necessary packages from the [Xilinx website](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/alveo/u55c.html).

### Step 1. Install XRT Library

!!! note
    To install XRT, ensure that the **kernel version** and **kernel-headers** version match. Additionally, if your operating system is Ubuntu and the kernel version is **5.15.0-41-generic or higher**, you will need to **downgrade** the kernel. The recommended kernel version is 5.15.0-25.

For Each-based systems (e.g. Centos-7, Ubuntu 22.04 LTS, Rocky 8.4)

* Centos-7, Rocky 8.4

```bash linenums="1"
# yum install xrt_202310.2.15.225_8.1.1911-x86_64-xrt.rpm
```

* Ubuntu 22.04 LTS

```bash linenums="1"
# dpkg -i xrt_202310.2.15.225_22.04-amd64-xrt.deb
```
Once the installation is complete, the **XRT files** will be located under the `/opt/xilinx/xrt/` directory. You will need to source the `/opt/xilinx/xrt/setup.sh` script to use commands such as `xbutil` and `xbmgmt`.

```bash linenums="1"
$ source /opt/xilinx/xrt/setup.sh
```

<details>
<summary><strong>How to use XRT Commands</strong></summary>
<strong>xbutil</strong> : Used primarily for device status monitoring and diagnostics.<br /> 
<strong>xbmgmt</strong> : Focuses on firmware management, flashing, and hardware-related tasks.<br /><br />

You can use <code>xbutil</code> and <code>xbmgmt</code> commands to examine device information. For more details, you can use the <code>--help</code> option with these commands or refer to the documentation on the <a href="https://xilinx.github.io/XRT/master/html/xbutil.html">XRT Master documentation</a>.
<br />
```text
$ xbutil examine
System Configuration
  OS Name              : Linux
  Release              : 5.15.0-25-generic
  Version              : #25-Ubuntu SMP Wed Mar 30 15:54:22 UTC 2022
  Machine              : x86_64
  CPU Cores            : 48
  Memory               : 257574 MB
  Distribution         : Ubuntu 22.04.4 LTS
  GLIBC                : 2.35
  Model                : ESC4000-E10

XRT
  Version              : 2.15.225
  Branch               : 2023.1
  Hash                 : adf27adb3cfadc6e4c41d6db814159f1329b24f3
  Hash Date            : 2023-05-03 10:13:19
  XOCL                 : 2.15.225, adf27adb3cfadc6e4c41d6db814159f1329b24f3
  XCLMGMT              : 2.15.225, adf27adb3cfadc6e4c41d6db814159f1329b24f3

Devices present
BDF             :  Shell                            Platform UUID        Device ID Device Ready*
------------------------------------------------------------------------------------------------
[0000:17:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  user(inst=---)  Yes
[0000:18:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  user(inst=---)  Yes
[0000:31:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  user(inst=---)  Yes
[0000:32:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  user(inst=---)  Yes
```

```text
$ xbmgmt examine
System Configuration
  OS Name              : Linux
  Release              : 5.15.0-25-generic
  Version              : #25-Ubuntu SMP Wed Mar 30 15:54:22 UTC 2022
  Machine              : x86_64
  CPU Cores            : 48
  Memory               : 257574 MB
  Distribution         : Ubuntu 22.04.4 LTS
  GLIBC                : 2.35
  Model                : ESC4000-E10

XRT
  Version              : 2.15.225
  Branch               : 2023.1
  Hash                 : adf27adb3cfadc6e4c41d6db814159f1329b24f3
  Hash Date            : 2023-05-03 10:13:19
  XOCL                 : 2.15.225, adf27adb3cfadc6e4c41d6db814159f1329b24f3
  XCLMGMT              : 2.15.225, adf27adb3cfadc6e4c41d6db814159f1329b24f3

Devices present
BDF             :  Shell                            Platform UUID        Device ID Device Ready*
------------------------------------------------------------------------------------------------
[0000:17:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  mgmt(inst=---)  Yes
[0000:18:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  mgmt(inst=---)  Yes
[0000:31:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  mgmt(inst=---)  Yes
[0000:32:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------------  mgmt(inst=---)  Yes
```
</details>


### Step 2. Install XRT Firmware

The next step is to install the XRT Firmware, which enables the FPGA to handle both hardware acceleration and general-purpose processing. To complete the installation, you need to flash the firmware onto the FPGA card. Follow the instructions provided with the firmware package and use the specified shell commands to deploy it onto the card.

**Download the Deployment Target Platform:** This is the communication layer physically implemented and flashed into the card.<br />

For Each-based systems (e.g. Centos-7, Ubuntu 22.04 LTS, Rocky 8.4)

* Centos-7, Rocky 8.4

```bash linenums="1"
# tar -xvf xilinx-u55c-gen3x16-xdma_2023.1_2023_0507_2220-noarch.rpm.tar.gz
# yum install xilinx-*.rpm
```

* Ubuntu 22.04 LTS

```bash linenums="1"
# tar -xvf xilinx-u55c-gen3x16-xdma_2023.1_2023_0507_2220-all.deb.tar.gz
# dpkg -i  xilinx-*.deb
```

 **Flash the Firmware:** Execute the following command in the shell to flash the firmware onto the FPGA card:
```bash linenums="1"
# xbmgmt program --base --device <BDF> --image xilinx_u55c_gen3x16_xdma_base_3
```

### Step 3. Cold Reboot
A cold reboot is required after installing the Xilinx firmware to ensure the system initializes and applies the **updated firmware to the FPGA hardware**.

The FPGA may appear as two logical devices. This happens because the FPGA is operating in dual mode, with each logical device assigned to handle different tasks, such as hardware acceleration and general-purpose processing.

After applying the shell, the Device Ready status is marked as "Yes".
```text
Devices present
BDF             :  Shell                            Platform UUID        Device ID Device Ready*
------------------------------------------------------------------------------------------------
[0000:17:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------  user(inst=---)  Yes
[0000:18:00.1]  :  xilinx_u55c_gen3x16_xdma_base_3  --------------  user(inst=---)  Yes
```
