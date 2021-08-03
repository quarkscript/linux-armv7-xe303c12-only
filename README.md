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


#### Recovery/install disk image
- based on archlinux|ARM 
  - [xe303c12 baremetal](https://drive.google.com/file/d/17X-DlPpTQlipDR5Z5uZ29qQr8UXBKZED/view?usp=sharing) 
  - [xe303c12 baremetal + virtio drivers](https://drive.google.com/file/d/1O94t7i_gBygdlDLsbyp9D8q7T425sgpM/view?usp=sharing)
  - [kernel 5.13.6 +xorg +xfce4 +firefox](https://drive.google.com/u/0/uc?id=1fuvSD4JI4uHUUmsGSx2-tupWola-BXJo&export=download)


#### Example of run under hypervisor (qemu)
```
qemu-system-arm -machine virt -m 1024 -kernel zImage -append "root=/dev/vda2" -serial stdio -drive if=none,file=armv7hf_q.img,format=raw,id=hd0 -device virtio-blk-device,drive=hd0 -netdev user,id=net0 -device virtio-net-device,netdev=net0 
```
![](example.gif)
> zImage could be extracted from disk image or from [5.10.3](https://github.com/quarkscript/linux-armv7-xe303c12-only/releases/tag/5.10.3-xe303c12) (or last release)

#### Example of cross-compiling void-linux kernel 
from [archlinux x86_64](https://archlinux.org/) with [xbps](https://aur.archlinux.org/packages/xbps/) installed 
``` 
git clone https://github.com/quarkscript/linux-armv7-xe303c12-only.git
git clone git://github.com/void-linux/void-packages.git
cd void-packages/
## copy linux_xe303c12 to src-dir
cp -fr ../linux-armv7-xe303c12-only/voidlinux/linux_xe303c12 srcpkgs/
    ## downgrade uboot-tools template
    sed -i "s/version=.*/version=2020.10/g" srcpkgs/u-boot-tools/template
    sed -i "s/revision=.*/revision=1/g" srcpkgs/u-boot-tools/template
    sed -i "s/checksum=.*/checksum=0d481bbdc05c0ee74908ec2f56a6daa53166cc6a78a0e4fac2ac5d025770a622/g" srcpkgs/u-boot-tools/template
## make glibc kernel
./xbps-src binary-bootstrap
    ## build uboot-tools
    ./xbps-src pkg u-boot-tools
./xbps-src -a armv7hf build linux_xe303c12
./xbps-src -a armv7hf pkg linux_xe303c12
## make musl kernel
./xbps-src -m x86_64-musl binary-bootstrap x86_64-musl
    ## build uboot-tools
    ./xbps-src -m x86_64-musl pkg uboot-mkimage
./xbps-src -m x86_64-musl -a armv7hf-musl build linux_xe303c12
./xbps-src -m x86_64-musl -a armv7hf-musl pkg linux_xe303c12
```
> Since the signing of the kernel fails with the latest uboot-tools, downgrade of uboot-tools is requred.

> 

> If you plan to re-sign kernel during installation then you will need to install a downgraded uboot-mkimage/uboot-tools firstly



#### Kali linux
To try Kali on that device you may use official build script from Kali developers at gitlab 

or [my old mod of that script](https://github.com/quarkscript/xe303c12_play_linux)
