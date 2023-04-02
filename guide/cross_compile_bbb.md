```
git clone https://github.com/beagleboard/linux.git
cd linux
#checkout your desired verion. I used the following
git checkout 5.10.162-ti-r58

## Add pps-gen-gpio files to kernel.
# I put the files to the following places. I replaced any existing files which had the same name
#Documentation/devicetree/bindings/pps/pps-gen-gpio.txt
#drivers/pps/generators/Kconfig
#drivers/pps/generators/Makefile
#drivers/pps/generators/pps_gen_gpio.c

## compile kernel
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- bb.org_defconfig
#next command is optional, if you want to edit via "gui"
#make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- menuconfig
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- LOADADDR=0x80000000 uImage dtbs

## install kernel on sd card
#mount rootfs of beagleboneblack
#rename or delete old zImage file
mv /mnt/rootfs/boot/vmlinuz-5.10.162-ti-r58 /mnt/rootfs/boot/vmlinuz-5.10.162-ti-r58.old
#replace with new kernel
cp arch/arm/boot/zImage /mnt/rootfs/boot/vmlinuz-5.10.162-ti-r58
#install modules on sd card
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- INSTALL_MOD_STRIP=1 INSTALL_MOD_PATH=/mnt/rootfs modules_install
```