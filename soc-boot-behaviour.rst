Embedded SoC Boot from eMMC/SD behaviour rollup
===============================================

This page is a rollup of various SoCs boot behaviour. Many SoCs can load firmware from an SD, eMMC, or USB storage device, but the behaviour may be hard coded in masked ROM and be incompatible with standard MBR or GUID partitioning, requiring special care if the storage is used to load both firmware and the target OS.

Raspberry Pi
------------

Masked ROM looks for first MBR partition formatted as FAT and loads ``bootcode.bin``. ``bootcode.bin`` loads and runs ``start.elf``, which in turn loads ``config.txt``, ``cmdline.txt``, and an executable (defaults to ``kernel*.img``).

Supposedly newer Raspberry Pis can handle GUID Partition Tables, but haven't found any information as to which.

https://github.com/raspberrypi/noobs/wiki/Standalone-partitioning-explained

Dragonboard 410c and 820c (or any derivatives, based on Snapdragon 410 or 820)
------------------------------------------------------------------------------

GUID Partition Table with firmware loaded from specific partitions: ``hyp`` (512KB), ``rpm`` (512KB), ``sbl1`` (512KB on 410, 2MB on 820), ``tz`` (1MB on 410, 2MB on 820), ``aboot`` (1MB), and ``cdt`` (1KB). The size mentioned are the one typically used. On 820 based based it is possible to use eMMC or UFS. When using UFS the partition/LUNs scheme is slightly more complicated...

HiKey
-----
GUID Partition Table on eMMC. Native partitioning on UFS. Firmware loaded from specific partitions: ``xloader``, ``fastboot``, ``nvme``, ``fw_lpm3``, ``trustfirmware``.

i.MX (AArch32)
--------------
These families (such as i.MX6, i.MX7) load firmware found at the one kilobyte offset of an SD or eMMC device.

Allwinner sunxi
---------------
These families (both 32 and 64bit) load firmware found at the 8 kilobyte offset of an SD or eMMC device.
