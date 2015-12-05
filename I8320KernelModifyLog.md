# Modify I8320 Kernel Source Code #
## For Key Function ##

arch/arm/mach-omap2/board-nowplus.c

```

static int board_keymap[] = {
#if 0
KEY(0, 0, KEY_FRONT),
KEY(1, 0, KEY_SEARCH),
KEY(2, 0, KEY_CAMERA_FOCUS),
KEY(0, 1, KEY_PHONE),
KEY(2, 1, KEY_CAMERA),
KEY(0, 2, KEY_EXIT),
KEY(1, 2, KEY_VOLUMEUP),
KEY(2, 2, KEY_VOLUMEDOWN),
#elif 1 //macroliu
KEY(0, 0, KEY_MENU), 
KEY(1, 0, KEY_BACK), 
KEY(2, 0, KEY_CAMERA_FOCUS), 
KEY(0, 1, KEY_CALL
KEY(2, 1, KEY_CAMERA),
KEY(0, 2, KEY_HOME), 
KEY(1, 2, KEY_VOLUMEUP),
KEY(2, 2, KEY_VOLUMEDOWN),
#endif
};

```

include/linux/input.h

```

 #if 1 //macroliu
#define KEY_CALL        231
#else
#define KEY_SEND                231        /* AC Send */
#endif

```

For power key, it's a GPIO key, we already defined keymap and keyfunction in board-nowplus.c.
But we need active gpio key,and config it.
Modify .config,or do make menuconfig:

```

CONFIG_KEYBOARD_GPIO=y

```