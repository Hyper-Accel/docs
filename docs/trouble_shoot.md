!!! warning
    Here are some methods that was shared with previous error cases. Based on the experiences of our previous PoC customers, most of the problems occur when **pressing `Ctrl+C` while running the LPU™**. **Please do not do this!**

## System Management Interface

You can check the status of the device that is running. This code shows online devices and each memory uses.

```shell
$ watch -n 1 hyperdex-smi
Fri Sep  6 17:17:52 2024
+-----------------------------------------------------------------------------+
| HYPERDEX-SMI            XRT Version: 2022.2     HyperDex Version: 1.3.2     |
+-----------------------------------------------------------------------------+
| FPGA Name        Persistence-M| Bus-Id     Bitstream | Volatile Uncorr. ECC |
|      Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | FPGA-Util Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|    0      XILINX U55C    Off  | 0000:b1:00.1      On |                  N/A |
|       48C    P8    34W / 225W |      0MiB / 16384MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|    1      XILINX U55C    Off  | 0000:b2:00.1     Off |                  N/A |
|       34C    P8    16W / 225W |      0MiB /     0MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|    2      XILINX U55C    Off  | 0000:ca:00.1     Off |                  N/A |
|       34C    P8    16W / 225W |      0MiB /     0MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|    3      XILINX U55C    Off  | 0000:cb:00.1     Off |                  N/A |
|       34C    P8    16W / 225W |      0MiB /     0MiB |      0%      Default |
|                               |                      |                  N/A |
+-----------------------------------------------------------------------------+
```

This refreshes the display every second. To make the refresh rate faster, decrease 1 to a lower number(e.g. 0.5). The number on the most left side is the id of the LPU™.

## Reset the LPU device

Instead of pressing `Ctrl+C`, please reset the device. First, check the device id with the hyperdex-smi. Next reset the device with the following code.

```shell linenums="1"
$ hyperdex-reset -d {LPU id}
```