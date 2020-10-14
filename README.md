### Linux kernel for old armv7 [chromebook](https://www.samsung.com/us/support/owners/product/chromebook-xe303c12)

([short demo](https://www.youtube.com/watch?v=hZt1fPso0e0) of running linux on this device)

#### [archlinuxarm folder](archlinuxarm) 
Forked from https://github.com/archlinuxarm/PKGBUILDs/tree/master/core/linux-armv7. Arch Linux Arm compatible kernel package.

Difference:
- lighter
- faster
- single package for kernel, headers and flash image
- no initrd (any way it is not used)
- detects Chrome OS Kernel partition for flashing kernel
- support samsung\google snow - [XE303C12 chromebook](https://archlinuxarm.org/platforms/armv7/samsung/samsung-chromebook) only 

#### [voidlinux folder](voidlinux)
The same, but for voidlinux. Second forked source is https://github.com/void-linux/void-packages/tree/master/srcpkgs/linux5.8. 

- [linux_xe303c12](voidlinux/linux_xe303c12) - kernel package, it can be cross-compiled with [xbps-src](https://github.com/void-linux/void-packages) into voidlinux package with armv7hf-glibc or armv7hf-musl architectures
- [linux_xe303c12_firmware](voidlinux/linux_xe303c12_firmware) - a couple of necessary files. Takes up less space than a regular linux-firmware package
- [xf86-video-armsoc-git](voidlinux/xf86-video-armsoc-git) - xorg video driver, forked from [archlinuxarm](https://github.com/archlinuxarm/PKGBUILDs/tree/master/alarm/xf86-video-armsoc-git) (at least it works with xfce4)
>Some compiled packages can be found at [releases](https://github.com/quarkscript/linux-armv7-xe303c12-only/releases)
