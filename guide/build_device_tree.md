You can probably add the device tree to the kernel but I compiled the device tree overlay on the target system, since I had to change some values until it was functional.

```
git clone https://github.com/beagleboard/BeagleBoard-DeviceTrees.git
cd BeagleBoard-DeviceTrees
#put files in overlay folder
#BBB:       cp <this_repo>/BB-BONE-P9_16-PPS-GEN.dts ./src/arm/overlays/BB-BONE-P9_16-PPS-GEN.dts
#BBAI64:    cp <this_repo>/BBAI64-P9_16-PPS-GEN.dts ./src/arm64/overlays/BBAI64-P9_16-PPS-GEN.dts
make -j2

#BBB: manually copy dtbo to desired install location
#BBB: enable overlay in /boot/uEnv.txt

#BBAI64: sudo make install
#BBAI64: enable overlay in /boot/firmware/extlinux/extlinux.conf
```