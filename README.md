### Custom linux kernel for old armv7 [chromebook](https://www.samsung.com/us/support/computing/chrome-device/chromebook/chromebook/?modelCode=XE303C12-A01US)

>[short demo](https://www.youtube.com/watch?v=hZt1fPso0e0) of running linux on this device.

#### [archlinux|ARM folder](archlinuxarm) 
- [kernel](archlinuxarm/linux_xe303c12) - Forked from [archlinux|ARM](https://github.com/archlinuxarm/PKGBUILDs/tree/master/core/linux-armv7). Archlinux|ARM package build script. Difference:
  - lighter and faster as a result of build for specific hardware and not for whole armv7 platform
  - single package for kernel, headers and flash image
  - no initrd (any way it is not used)
  - detects Chrome OS Kernel partition for flashing kernel
  - lets user to change kernel boot string during installation (with some conditions)
  - support samsung\google snow - [XE303C12 chromebook](https://archlinuxarm.org/platforms/armv7/samsung/samsung-chromebook) only.
  
 - [firmware](archlinuxarm/linux_xe303c12_firmware) - Archlinux|ARM package build script. It collect a couple of necessary files and saves some space compared to regular linux-firmware.
 - [some forked apps](archlinuxarm/some_forked_apps) may run faster than from repo or may not ( [builded](https://github.com/quarkscript/linux-armv7-xe303c12-only/releases/tag/some_apps_21.10.04) )

Some (not all) packages from [AUR](https://aur.archlinux.org) could be installed by [sfslib](https://github.com/quarkscript/Simple_func_scripts/blob/master/sfslib) like `./sfslib armget 'pkg name'`

#### [Void-linux folder](voidlinux)
The same, but for void-linux. Second forked source is [void-packages](https://github.com/void-linux/void-packages/tree/master/srcpkgs/linux). 

- [kernel](voidlinux/linux_xe303c12) - Void-linux package build script can be cross-compiled with [xbps-src](https://github.com/void-linux/void-packages) into void-linux package with armv7hf-glibc or armv7hf-musl architectures.
- [firmware](voidlinux/linux_xe303c12_firmware) - Void-linux package build script. It collect a couple of necessary files and  takes up less space than a regular linux-firmware package.
- [mesa](voidlinux/xf86-video-armsoc-git) - Void-linux package build script, forked from [void-packages](https://github.com/void-linux/void-packages/tree/master/srcpkgs/mesa)
- [xorg video driver](voidlinux/xf86-video-armsoc-git) - Void-linux package build script, forked from [archlinux|ARM](https://github.com/archlinuxarm/PKGBUILDs/tree/master/alarm/xf86-video-armsoc-git) (it looks like no more required)

> Some already builded packages can be found at [releases](https://github.com/quarkscript/linux-armv7-xe303c12-only/releases)


#### Kali / Devuan linux
To try Kali on that device you may use [official build script from Kali developers](https://gitlab.com/kalilinux/build-scripts/kali-arm/-/blob/master/chromebook-arm-exynos.sh) 
or [my rewrited mod of that script](https://github.com/quarkscript/xe303c12_play_linux) Devuan linux disk image could be build with rewrited script too.

#### Recovery / install / test disk images
- based on archlinux|ARM 
  - [test disk image 1](https://drive.google.com/u/0/uc?id=1O94t7i_gBygdlDLsbyp9D8q7T425sgpM&export=download) ( console, works on baremetal and under qemu )
  - [test disk image 2](https://drive.google.com/u/0/uc?id=1qo4ExfRGK1Sl-Vv_2SRPctM7H7I330y0&export=download) ( autologin to xfce4 )
- based on Void-linux
  - [glibc test disk image 1](https://drive.google.com/u/0/uc?id=1lfwUbqRVG0aSsfNXZBHCmcUbHGK7Cd5W&export=download) ( kernel 5.15.2 / console )
  - [glibc test disk image 2](https://drive.google.com/u/0/uc?id=14y9IOrN4us97aT-5dji7Q8zdkxWOjGmZ&export=download) ( kernel 5.15.2 / autologin to xfce4 )
  - [glibc test disk image 3](https://drive.google.com/u/0/uc?id=1Tg6Z8G87pYsZdqyU8wWkk71evN80y5oY&export=download) ( kernel 5.15.2 / autologin to xfce4 + lxqt, plasma and so on)
  - [musl test disk image 1](https://drive.google.com/u/0/uc?id=1x8jSyQsz-9eYN_zgGciwHBAAJdVFaFD6&export=download) ( kernel 5.15.2 / console )
  - [musl test disk image 2](https://drive.google.com/u/0/uc?id=1b9TusWJabpmkotVppXr0pOkNBqqq-j1Z&export=download) ( kernel 5.15.2 / autologin to xfce4 )
- debian based linux
  - [kali test disk image](https://drive.google.com/u/0/uc?id=1meNMjZaphdySOPjudi1tr-4pjXMNLCBm&export=download) ( kernel 5.13.8 / autologin to xfce4 )
  - [another kali test disk image](https://drive.google.com/u/0/uc?id=1hWoAyJUZY_WY4f_cdS0vUKJbJC-8SNWU&export=download) ( kernel 5.15.55, 3800M, ext4 / gzip compressed / autologin to xfce as root )
  - [devuan test disk image](https://drive.google.com/u/0/uc?id=12rDOgfDg_YptOwp3wKWLqZjO5fxTWOe_&export=download) ( kernel 5.15.5 / autologin to lxqt )
  - [devuan test disk image 2](https://drive.google.com/u/0/uc?id=1Smkv6HW1iJC-Ycm49tVAKdKLLLC2f7gk&export=download) ( kernel 5.15.6 / autologin to xfce )


> X may not work on "console"-s disk images until an fixed version of mesa is released on the appropriate distro

 [empty disk image maker](edim) can be used to create an empty disk image of the required size [demonstration](https://youtu.be/ALJR2doOipc)

#### Example of run under hypervisor (qemu)
```
qemu-system-arm -machine virt,highmem=off -m 1024 -kernel zImage -append "root=/dev/vda2" -serial stdio -drive if=none,file=armv7hf_q.img,format=raw,id=hd0 -device virtio-blk-device,drive=hd0 -netdev user,id=net0 -device virtio-net-device,netdev=net0 
```
![](example.gif)

 zImage could be extracted from second partition of disk image like:
```
mkdir dsk
sudo mount -t btrfs -o,loop,offset=$((512*40960)) armv7hf_q.img dsk
cp dsk/boot/zImage zImage
sudo umount dsk
```

#### Example of cross-compiling Void-linux packages 
(looks like xbps is no longer part of void-packages so this example requires Linux with xbps installed or Void-linux)
``` 
git clone https://github.com/void-linux/void-packages.git
git clone https://github.com/quarkscript/linux-armv7-xe303c12-only.git

cd void-packages/

## copy templates to src-dir
  cp -fr ../linux-armv7-xe303c12-only/voidlinux/linux_xe303c12 srcpkgs/
  cp -fr ../linux-armv7-xe303c12-only/voidlinux/linux_xe303c12_firmware srcpkgs/
    
##############################################################

## make glibc build env.
  ./xbps-src binary-bootstrap
  
## make glibc kernel package
  ./xbps-src -a armv7hf build linux_xe303c12
  ./xbps-src -a armv7hf pkg linux_xe303c12

## make glibc firmware package
  ./xbps-src -a armv7hf pkg linux_xe303c12_firmware

##############################################################

## make musl build env.
  ./xbps-src -m x86_64-musl binary-bootstrap x86_64-musl

## make musl kernel package
  ./xbps-src -m x86_64-musl -a armv7hf-musl build linux_xe303c12
  ./xbps-src -m x86_64-musl -a armv7hf-musl pkg linux_xe303c12 

## make musl firmware package
  ./xbps-src -m x86_64-musl -a armv7hf-musl pkg linux_xe303c12_firmware
  
```

 If you plan to re-sign kernel during installation then you will need to install a uboot-mkimage/uboot-tools firstly.

> Be aware! Given scripts or packages are not officially supported by any mentioned Linux distributions.
