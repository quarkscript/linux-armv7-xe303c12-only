### Custom linux kernel for old armv7 [chromebook](https://www.samsung.com/us/support/owners/product/chromebook-xe303c12)

>[short demo](https://www.youtube.com/watch?v=hZt1fPso0e0) of running linux on this device.

#### [archlinuxarm folder](archlinuxarm) 
- [kernel](archlinuxarm/linux_xe303c12) - Forked from [archlinux|ARM](https://github.com/archlinuxarm/PKGBUILDs/tree/master/core/linux-armv7). Archlinux|ARM package build script. Difference:
  - lighter and faster as a result of build for specific hardware and not for whole armv7 platform
  - single package for kernel, headers and flash image
  - no initrd (any way it is not used)
  - detects Chrome OS Kernel partition for flashing kernel
  - support samsung\google snow - [XE303C12 chromebook](https://archlinuxarm.org/platforms/armv7/samsung/samsung-chromebook) only.
  
 - [firmware](archlinuxarm/linux_xe303c12_firmware) - Archlinux|ARM package build script. It collect a couple of necessary files and saves some space compared to regular linux-firmware.

#### [voidlinux folder](voidlinux)
The same, but for void-linux. Second forked source is [void-packages](https://github.com/void-linux/void-packages/tree/master/srcpkgs/linux5.8). 

- [kernel](voidlinux/linux_xe303c12) - Void-linux package build script can be cross-compiled with [xbps-src](https://github.com/void-linux/void-packages) into void-linux package with armv7hf-glibc or armv7hf-musl architectures.
- [firmware](voidlinux/linux_xe303c12_firmware) - Void-linux package build script. It collect a couple of necessary files and  takes up less space than a regular linux-firmware package.
- [xorg video driver](voidlinux/xf86-video-armsoc-git) - Void-linux package build script (at least it works for xfce4). Forked from [archlinux|ARM](https://github.com/archlinuxarm/PKGBUILDs/tree/master/alarm/xf86-video-armsoc-git) 

> Some already builded packages can be found at [releases](https://github.com/quarkscript/linux-armv7-xe303c12-only/releases)

> Be aware! Given scripts or packages are not officially supported by any mentioned Linux distributions.

> Known issue: void-linux glibc may fail to flash kernel, the cause may be a damaged cgpt from vboot-utils; as a solution you may flash kernel manually like 'dd if=/boot/vmlinux.kpart of=/dev/[mmcblk0p1 or mmcblk1p1 or sda1 etc.]'.

> Based on archlinux|ARM recovery/install [disk image for xe303c12](https://drive.google.com/file/d/17X-DlPpTQlipDR5Z5uZ29qQr8UXBKZED/view?usp=sharing)
