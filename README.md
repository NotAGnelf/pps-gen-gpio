pps-gen-gpio
============

Linux kernel PPS generator using GPIO pins.

In kernel 5.10.162 there is no support for using a GPIO pin as a PPS generator, only a GPIO PPS client is available. This driver is derived from the current parallel port PPS generator and provides a PPS signal through a GPIO pin specified in the device tree. The PPS signal is synchronized to the tv_sec increment of the wall clock.

I have tested the driver with kernel 5.10.162 on a Beaglebone Black (P9.16) and Beaglebone AI64 (P9.16) where the specified pin is used as PPS output. On the Beaglebone Black I have encounters some errors when the pss-gen-gpio module was loaded. For example whole system halted, when I restarted a systemd service (chrony.service). Additionally the pps-gen-gpio driver seems to not always create a pps signal. The behavior is better on the BeagleBone AI64. But some PPS-Signals are lost here aswell.

Usage
-----
- You need to add the Kconfig, Makefile, pps_gen_gpio.c and pps-gen-gpio.txt file to the correct places when building your custom kernel.
	- See [cross compile guide for bbb](guide/cross_compile_bbb.md) on how to build the kernel on another system
	- See [cross compile guide for bbai64](guide/cross_compile_bbb.md) on how to build the kernel on another system.

- Additionally, you need to compile the device tree overlay for the specified system.
	- See [building the device trees](guide/build_device_tree.md) on how to build the device tree on the system.

	- If you use a **Beaglebone Black** and want to change the PPS output pin, you have to change value from the "BB-BONE-P9_16-PPS-GEN.dts" file.  Add the **compiled** overlay to your "/boot/uEnv.txt":
		```
			#skipping lines ...
		#Make sure overlays are enabled
		enable_uboot_overlays=1
			#skipping lines ...
		uboot_overlay_addr4=/lib/firmware/BB-BONE-P9_16-PPS-GEN.dtbo
			#skipping lines ...
		```
	- If you use a **Beaglebone AI64**, you need to edit the "BBAI64-P9_16-PPS-GEN.dts" file. Add the **compiled** overlay to "/boot/firmware/extlinux/extlinux.conf":
	```
		#skipping lines ...
	# Multiple overlays can be added in the following line!
	fdtoverlays /overlays/BBAI64-P9_16-PPS-GEN.dtbo
	#fdtoverlays /overlays/BONE-UART1.dtbo /overlays/BBAI64-P9_18-GPS-PPS.dtbo /overlays/BBAI64-P9_16-PPS-GEN.dtbo
		#skipping lines ...
	```
- Make sure you have the right pin multiplexing setting:

	- **Beaglebone Black:** Check the [BBB System Reference Manual](https://github.com/CircuitCo/BeagleBone-Black/blob/master/BBB_SRM.pdf?raw=true) and the [am335x Technical Reference Manual](https://www.ti.com.cn/cn/lit/ug/spruh73q/spruh73q.pdf) to find out the correct pinmux table offset of the chosen pin.
	- **BBAI64:** You need to check the device tree bindings for the correct name. The current reference for the gpios is here:[BBAI64 Documents](https://docs.beagleboard.org/latest/boards/beaglebone/ai-64/ch07.html)

- The pps-gen-gpios property defines the pin you want to use as a PPS output:

		pps-gen {
			status = "okay";
			pinctrl-names = "default";
			pinctrl-0 = <&pps_gen_pins>;
			compatible = "pps-gen-gpio";
			pps-gen-gpio = <&gpio1 19 GPIO_ACTIVE_HIGH>;
			default-state = "off";

Please note that in order to use the module with any other board using the device tree infrastructure, the following matching definitions are required in the device tree:

		pps-gen               node defined for the PPS GPIO
		pps-gen-gpio          value of ".compatible" property in pps-gen node
		pps-gen-gpio          property in pps-gen node that defines which GPIO pin is used

After modifying the device tree, add the files into drivers/pps/generators and configure the driver to be built as a module. You need to enable PPS support in the kernel.
