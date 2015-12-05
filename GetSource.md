# Build Android from source #
## Building Odin ##

First, clone the Odin git repository.
Compile Odin (remember, use this tool at your own risk, although there are no known issues or bricking incidents)

```
$ git clone git://gitorious.org/h1droid/odin.git
$ cd odin
$ make brick
```

## ARM Cross Compiler ##

CodeSourcery ARM Compiler should be used for building different kernel distribution and software releases on OMAP platforms. Visit [www.codesourcery.com](http://www.codesourcery.com) for more info.
Quick Link - [CodeSourcery G++ Lite 2009q1-203 for ARM GNU/Linux](http://www.codesourcery.com/sgpp/lite/arm/portal/release858)
Steps
Download .tar from above link and untar to a host pc directory.
Add Compiler directory to PATH, and cross compiler prefix before build

```
$ export PATH=/<toolchain_folder>/bin:$PATH
$ export CROSS_COMPILE=arm-none-linux-gnueabi-
$ export ARCH=arm
```

## Building Bootloader ##

```
$ git clone git://gitorious.org/h1droid/u-boot.git
$ cd u-boot
```

open u-boot/include/configs/omap3\_nowplus.h ,change CONFIG\_EXTRA\_ENV\_SETTINGS and CONFIG\_BOOTCOMMAND
```
#define CONFIG_BOOTDELAY 0
#define CONFIG_EXTRA_ENV_SETTINGS \
	"loadaddr=0x82000000\0" \
	"console=ttyS2,115200n8\0" \
	"usbtty=cdc_acm\0"\
	"stdout=usbtty\0" \
	"stdin=usbtty\0" \
	"stderr=usbtty\0" \
	"bootargs=root=/dev/mmcblk0p2 rw init=/init rootdelay=1 rootfstype=ext3 rootwait debug\0" \
#define CONFIG_BOOTCOMMAND \
		"bootm 0x86C30000"
```

then compile.
```
$ make omap3_nowplus_config
$ make
```

export mkimage's path

```
$ export PATH=/<uboot_folder>/tools:$PATH
```

## Building Kernel ##

```
$ git clone git://gitorious.org/h1droid/i8320kernel.git
$ cd i8320kernel
$ git checkout mergei8520
$ cp arch/arm/configs/omap_nowplus_defconfig .config
$ make uImage
```

## Building boot.bin ##
Get [makeboot](http://h1droid.googlecode.com/files/makeboot.sh) from Download section

```
$ chmod +x makeboot.sh
$ ./makeboot.sh ./u-boot/u-boot.bin ./i8320kernel/arch/arm/boot/uImage
```

## Building Android ##

```
$ export ANDROID=/home/mephisto/h1droid
$ mkdir -p $ANDROID
$ cd $ANDROID
$ repo init -u git://gitorious.org/h1droid/h1droid_platform_manifest.git -m froyo.xml
$ repo sync
```

```
$ cd $ANDROID
```
```
$ source build/envsetup.sh
$ lunch i8320-eng
$ make -j2
```
or
```
make -j2 PRODUCT-i8320-eng
```

```
$ cd $ANDROID
$ $ANDROID/device/samsung/i8320/i8320-image.sh
```

```
$ cd $ANDROID
$ git clone git://gitorious.org/rowboat/ti_android_sgx_sdk.git TI_Android_SGX_SDK
$ cd TI_Android_SGX_SDK
$ ./OMAP35x_Android_Graphics_SDK_setuplinux_3_01_00_03.bin
```

Edit Rules.make in the install directory
```
HOME=$(ANDROID)
GRAPHICS_INSTALL_DIR=$(ANDROID)/TI_Android_SGX_SDK
ANDROID_ROOT=$(ANDROID)/device/samsung/i8320/android
CSTOOL_DIR=$(ANDROID)/prebuilt/linux-x86/toolchain/arm-eabi-4.4.0/
KERNEL_INSTALL_DIR=$(ANDROID)/../i8320kernel
```

```
$ cd $ANDROID/TI_Android_SGX_SDK
$ make
$ make install OMAPES=3.x
```


## Partitioning ##

Create to partition on sdcard. First one is vfat. second is ext3 for save rootfs(>200MB).
then copy all files from $(ANDROID)/device/samsung/i8320/android to second partition.

## Booting ##
Put your device into download mode by first turning it off, then power it on whilst holding down both the volume down and camera button. Upload and run your bootloader/kernel image

```
$ ./odin boot.bin
```