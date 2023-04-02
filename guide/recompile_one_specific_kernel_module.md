#recompile one specific kernel module if you made any changes.

#for example: pps generators.

```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- scripts prepare modules_prepare
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabi- -C . M=drivers/pps/generators
```