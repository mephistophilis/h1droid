# Need a Linux #

# Download #
Download odin(x64). It's a x64 version. if you using a x86 linux, you have to compile the odin from source.
Download boot.tar.bz2, rootfs.tar.bz2.

## Partitioning ##

Create to partition on sdcard. First one is vfat. second is ext3 for save rootfs(>200MB).
then extract files from rootfs.tar.bz2 to second partition.

## Booting ##
Extract boot.tar.bz2.
Put your device into download mode by first turning it off, then power it on whilst holding down both the volume down and camera button. Upload and run your bootloader/kernel image

```
$ ./odin boot.bin
```

### Use At Your Own Risk ###
The "Use at your own risk" was just added by j.de.gram to emphasize that I'd be in no way responsible for you bricking your phone (using odin). That said, there's no reason for it to do that unless you mess with the source.

And yes, if you follow the steps in this guide you'll boot from SD using odin - it does not install anything on NAND/moviNAND, as soon as you reboot/pull the battery, it will just boot LiMo.