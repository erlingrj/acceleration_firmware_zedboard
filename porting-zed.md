# Porting KRS to ZedBoard
- Apparantly only need to re-create acceleration_firmware_ZedBoard
- This pretty much boils down to using Petalinux to create a bunch of artifacts



## Petalinux
- Download petalinux v2022.1
- Download ZedBoard BSP from AVNET (version 2020.2-final). NB: could be a problem here with compatability?
- UG1144 is PetaLinux guide and 
- Petalinux should not be installed as root. Maybe it cannot be installed in data then?
- Downloaded 2021.2 instead, to match Vitis installation
- warning: This is not an supported OS (wtf?)
- ./petalinux... --dir /data/petalinux/v2021.2/ --platform "arm"
- THis only installs the Zynq stuff. ALso downloaded ZED.BSP from Xilinx pages

Create Petalinux project from BSP
```petalinux-create --type project --name zedboard_linux --source avnet-digilent-zedboard-v2021.2-final.bsp 
 ```

Create HW platform using Vivado with just instantiating the ZYNQ7 Processing System and compiling.

Then configure the project with right hw platform
```petalinux-config --get-hw-description dev/vivado/zedboard/zedboard_empty_wrapper.xsa --project petalinux/zedboard_linux/
```

This opens menuconfig. Dont change anything yet. But we might have to do so to get everything working later.
We can also configure the root file-system to install packages with
```petalinux-config -c rootfs --project petalinux/zedboard_linux/
```

Lastly, build the image with
``` petalinux-build --project zedboard_linux/
``` 


## Making firmware repo
- images/linux is where stuff is built. There is also a prebuilt repo
- u-boot.bin
- u-boot.elf
- zynq_fsbl.elf
- system.bit
- rootfs.cpio.gz
- boot.scr
- system.dtb
- image.ub -> kernel
- zImage -> kernel


## What about
- pmufw.elf and pmu_rom_qemy_sha3.elf


