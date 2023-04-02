```
git clone https://github.com/beagleboard/linux.git
cd linux
See [cross compile guide for bbb](guide/cross_compile_bbb.md)
git checkout 5.10.162-ti-arm64-r97

## Add pps-gen-gpio files to kernel.
# I put the files to the following places. I replaced any existing files which had the same name
#Documentation/devicetree/bindings/pps/pps-gen-gpio.txt
#drivers/pps/generators/Kconfig
#drivers/pps/generators/Makefile
#drivers/pps/generators/pps_gen_gpio.c

## compile kernel
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- bb.org_defconfig
#next command is optional, if you want to edit via "gui"
#make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- menuconfig
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu-


## install kernel on sd card
#mount rootfs of beagleboneai64
#rename or delete old Image file
mv /mnt/rootfs/boot/firmware/Image /mnt/rootfs/boot/firmware/Image.old
#replace with new kernel
cp arch/arm64/boot/Image /mnt/rootfs/boot/firmware/Image
#install modules on sd card
make ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- INSTALL_MOD_STRIP=1 INSTALL_MOD_PATH=/mnt/rootfs modules_install
```