# forked from https://github.com/archlinuxarm/PKGBUILDs/tree/master/core/linux-armv7

pkgbase=linux-armv7-xe303c12
pkgname=$pkgbase
_srcname=linux-5.8
_kernelname=${pkgbase#linux}
_desc="ARMv7 XE303C12-platform"
pkgver=5.8.13
pkgrel=3
rcnver=5.8.5
rcnrel=armv7-x11
arch=('armv7h')
url="http://www.kernel.org/"
license=('GPL2')
makedepends=('xmlto' 'docbook-xsl' 'kmod' 'inetutils' 'bc' 'git' 'uboot-tools' 'vboot-utils' 'dtc')
options=('!strip')
source=("http://www.kernel.org/pub/linux/kernel/v5.x/${_srcname}.tar.xz"
        "http://www.kernel.org/pub/linux/kernel/v5.x/patch-${pkgver}.xz"
        "http://rcn-ee.com/deb/stretch-armhf/v${rcnver}-${rcnrel}/patch-${rcnver%.0}-${rcnrel}.diff.xz"
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0001-ARM-atags-add-support-for-Marvell-s-u-boot.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0002-ARM-atags-fdt-retrieve-MAC-addresses-from-Marvell-bo.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0003-SMILE-Plug-device-tree-file.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0004-fix-mvsdio-eMMC-timing.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0005-net-smsc95xx-Allow-mac-address-to-be-set-as-a-parame.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0006-set-default-cubietruck-led-triggers.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0007-exynos4412-odroid-set-higher-minimum-buck2-regulator.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0008-ARM-dove-enable-ethernet-on-D3Plug.patch'
        'https://raw.githubusercontent.com/archlinuxarm/PKGBUILDs/master/core/linux-armv7/0009-USB-Armory-MkII-support.patch'
        'config'
        'kernel.its'
        'https://github.com/archlinuxarm/PKGBUILDs/raw/master/core/linux-armv7/kernel.keyblock'
        'https://github.com/archlinuxarm/PKGBUILDs/raw/master/core/linux-armv7/kernel_data_key.vbprivk'
)
md5sums=('SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP'
         'SKIP')

prepare() {
  cd "${srcdir}/${_srcname}"

  # add upstream patch
  git apply --whitespace=nowarn ../patch-${pkgver}

  # RCN patch
  git apply ../patch-${rcnver%.0}-${rcnrel}.diff

  # ALARM patches
  git apply ../0001-ARM-atags-add-support-for-Marvell-s-u-boot.patch
  git apply ../0002-ARM-atags-fdt-retrieve-MAC-addresses-from-Marvell-bo.patch
  git apply ../0003-SMILE-Plug-device-tree-file.patch
  git apply ../0004-fix-mvsdio-eMMC-timing.patch
  git apply ../0005-net-smsc95xx-Allow-mac-address-to-be-set-as-a-parame.patch
  git apply ../0006-set-default-cubietruck-led-triggers.patch
  git apply ../0007-exynos4412-odroid-set-higher-minimum-buck2-regulator.patch
  git apply ../0008-ARM-dove-enable-ethernet-on-D3Plug.patch
  git apply ../0009-USB-Armory-MkII-support.patch

  cat "${srcdir}/config" > ./.config

  # add pkgrel to extraversion
  sed -ri "s|^(EXTRAVERSION =)(.*)|\1 \2-${pkgrel}|" Makefile

  # don't run depmod on 'make install'. We'll do this ourselves in packaging
  sed -i '2iexit 0' scripts/depmod.sh
}

build() {
  cd "${srcdir}/${_srcname}"
  make prepare
  make menuconfig
  ff="-O2 -pipe -fno-plt -mfloat-abi=hard -mfpu=neon-vfpv4 -pipe -fstack-protector-strong -march=armv7-a+mp+neon --param l1-cache-size=64 --param l2-cache-size=1024 -fno-strict-aliasing -faggressive-loop-optimizations -fcombine-stack-adjustments -fcprop-registers -fcrossjumping -fdce -fdelayed-branch -fdelete-dead-exceptions -fdelete-null-pointer-checks -fdse -fforward-propagate -fgcse -fgcse-after-reload -fguess-branch-probability -fif-conversion2 -fipa-sra -fira-hoist-pressure -fira-loop-pressure -fira-share-save-slots -fivopts -fjump-tables -floop-interchange -floop-nest-optimize -floop-unroll-and-jam -fmove-loop-invariants -fomit-frame-pointer -foptimize-sibling-calls -foptimize-strlen -fpeel-loops -fpeephole -fpeephole2 -fpredictive-commoning -fprefetch-loop-arrays -freg-struct-return -frename-registers -frerun-cse-after-loop -fschedule-fusion -fsel-sched-pipelining -fsel-sched-pipelining-outer-loops -fshrink-wrap-separate -fsimd-cost-model=dynamic -fsplit-ivs-in-unroller -fsplit-loops -fsplit-paths -fssa-backprop -fssa-phiopt -fstdarg-opt -fthread-jumps -ftree-bit-ccp -ftree-builtin-call-dce -ftree-ccp -ftree-cselim -ftree-dce -ftree-dominator-opts -ftree-dse -ftree-forwprop -ftree-loop-distribution -ftree-loop-optimize -ftree-loop-vectorize -ftree-pre -ftree-slp-vectorize -ftree-vectorize -funroll-loops -fvect-cost-model=dynamic "
#  CFLAGS+="$ff" 
#  CXXFLAGS+="$ff"
  export CFLAGS+="$ff" 
  export CXXFLAGS+="$ff"
  
  echo force custom flags
  for tmpcycle in $(find -name Makefile); do
    sed -i "s/armv7-a/armv7-a+mp+neon-vfpv4/g" $tmpcycle
    sed -i "s/vfvp /vfpv4 /g" $tmpcycle
    sed -i "s/vfvp,/vfpv4,/g" $tmpcycle
    sed -i "s/-O2 /-O2 -march=armv7-a+mp+neon-vfpv4 --param l1-cache-size=64 --param l2-cache-size=1024 -faggressive-loop-optimizations -fguess-branch-probability -floop-nest-optimize -fomit-frame-pointer -fsel-sched-pipelining -fsel-sched-pipelining-outer-loops -fpredictive-commoning -fprefetch-loop-arrays -fsimd-cost-model=dynamic -fvect-cost-model=dynamic -ftree-loop-optimize -funroll-loops -floop-unroll-and-jam /g" $tmpcycle
  done
  
  make ${MAKEFLAGS} zImage modules dtbs
}

package() {
  pkgdesc="The Linux Kernel, flash image, modules and headers for samsung/google snow XE303C12 chromebook - ${_desc}"
  depends=('coreutils' 'linux-firmware' 'kmod' 'mkinitcpio>=0.7')
  optdepends=('crda: to set the correct wireless channels of your country')
  backup=("etc/mkinitcpio.d/${pkgbase}.preset")
  provides=("linux=${pkgver}" "WIREGUARD-MODULE" "linux-headers=${pkgver}" )
  conflicts=('linux' 'linux-headers' )
  replaces=('linux-armv7' 'linux-armv7-chromebook' 'linux-armv7-rc-chromebook'  'linux-armv7-rc' 'linux-mvebu' 'linux-udoo' 'linux-sun4i' 'linux-sun5i' 'linux-sun7i' 'linux-usbarmory' 'linux-wandboard' 'linux-clearfog' 'linux-mvebu-headers' 'linux-sun4i-headers' 'linux-sun5i-headers' 'linux-sun7i-headers' 'linux-usbarmory-headers' 'linux-wandboard-headers' 'linux-clearfog-headers' 'linux-armv7-rc-chromebook' 'linux-armv7-chromebook' 'linux-armv7-headers')

  cd "${srcdir}/${_srcname}"

  KARCH=arm

  # get kernel version
  _kernver="$(make kernelrelease)"
  _basekernel=${_kernver%%-*}
  _basekernel=${_basekernel%.*}

  mkdir -p "${pkgdir}"/{boot,usr/lib/modules}
  make INSTALL_MOD_PATH="${pkgdir}/usr" modules_install
  make INSTALL_DTBS_PATH="${pkgdir}/boot/dtbs" dtbs_install
  cp arch/$KARCH/boot/zImage "${pkgdir}/boot/zImage"

  # make room for external modules
  local _extramodules="extramodules-${_basekernel}${_kernelname}"
  ln -s "../${_extramodules}" "${pkgdir}/usr/lib/modules/${_kernver}/extramodules"

  # add real version for building modules and running depmod from hook
  echo "${_kernver}" |
    install -Dm644 /dev/stdin "${pkgdir}/usr/lib/modules/${_extramodules}/version"

  # remove build and source links
  rm "${pkgdir}"/usr/lib/modules/${_kernver}/{source,build}

  # now we call depmod...
  depmod -b "${pkgdir}/usr" -F System.map "${_kernver}"


echo 'flash_kernel() {
  krnprts=""
  krnprtc=0
  for i in $(ls /dev | grep -x --regexp="mmcblk[0-9]" --regexp="sd[a-z]"); do
    if $(cgpt show /dev/$i | grep Label | grep -q [Kk]ernel); then
      if $(echo $i | grep -q mmcblk); then
        krnprts+="$i""p$(cgpt show /dev/$i | grep Label | grep [Kk]ernel -m 1 | sed '"'"'s/  / /g'"'"' | sed '"'"'s/  / /g'"'"' | sed '"'"'s/ La.*//g'"'"' | sed '"'"'s/.* //g'"'"') "
      else
        krnprts+="$i$(cgpt show /dev/$i | grep Label | grep [Kk]ernel -m 1 | sed '"'"'s/  / /g'"'"' | sed '"'"'s/  / /g'"'"' | sed '"'"'s/ La.*//g'"'"' | sed '"'"'s/.* //g'"'"') "
      fi
      krnprtc=$(($krnprtc+1))
    fi
  done
  
  echo ""
  if [ "$krnprtc" -eq 0 ]; then
    echo "ChromeOs kernel partition did not detected." 
    echo "You must reflash kernel manually."
  elif [ "$krnprtc" -eq 1 ]; then
    echo "Finded ChromeOs kernel partition - $krnprts"
    echo "Do you want to flash a new kernel to it? (y/n)"
    read -r shouldwe
    if [[ $shouldwe =~ ^([yY][eE][sS]|[yY])$ ]]; then
      dd if=/boot/vmlinux.kpart of=/dev/$krnprts
      sync
    else
      echo "You may flash kernel manually like:"
      echo "dd if=/boot/vmlinux.kpart of=/dev/$krnprts"
    fi
  else
    echo "Finded more than one ChromeOs kernel partition."
    echo "You need to select next action
    "
    echo "0 for do not flash"
    numpart=0
    for i in $krnprts; do
      numpart=$(($numpart+1))
      echo "$numpart for flash $i partition"
    done
    echo ""
    echo "Wrong partition may lead to bootfail or disk fail. Be aware!"
    read -r shouldwe
    if [ $shouldwe -gt 0 ]&&[ $shouldwe -le $numpart ]; then
      numpart=0
      for i in $krnprts; do
        numpart=$(($numpart+1))
        if [ $numpart -eq $shouldwe ]; then
          dd if=/boot/vmlinux.kpart of=/dev/$i
          sync
        fi
      done
    fi
  fi
}

post_install () {
  flash_kernel
}

post_upgrade() {
  flash_kernel
}

post_remove() {
  rm -f boot/initramfs-linux.img
}
' >../../${pkgbase}.pkg
  
true && install=${pkgbase}.pkg   

# install mkinitcpio preset file...
echo "# mkinitcpio preset file for the '${pkgbase}' package

#ALL_config='/etc/mkinitcpio.conf'
#ALL_kver='${_kernver}'

#PRESETS=('default')

#default_config='/etc/mkinitcpio.conf'
#default_image='/boot/initramfs-linux.img'
#default_options=''
" | install -Dm644 /dev/stdin "${pkgdir}/etc/mkinitcpio.d/${pkgbase}.preset"
    
## install pacman hooks
echo "[Trigger]
Type = File
Operation = Install
Operation = Upgrade
Operation = Remove
Target = usr/lib/modules/${_kernver}/*
Target = usr/lib/modules/${_extramodules}/*

[Action]
Description = Updating ${pkgbase} module dependencies...
When = PostTransaction
Exec = /usr/bin/depmod ${_kernver}
" | install -Dm644 /dev/stdin "${pkgdir}/usr/share/libalpm/hooks/60-${pkgbase}.hook"

## not needed because kernel do not use initrd
# echo "[Trigger]
# Type = File
# Operation = Install
# Operation = Upgrade
# Target = boot/zImage
# Target = usr/lib/initcpio/*
# 
# [Action]
# Description = Updating ${pkgbase} initcpios...
# When = PostTransaction
# Exec = /usr/bin/mkinitcpio -p ${pkgbase}
# " | install -Dm644 /dev/stdin "${pkgdir}/usr/share/libalpm/hooks/90-${pkgbase}.hook"


## include headers


 cd "${srcdir}/${_srcname}"
  local _builddir="${pkgdir}/usr/lib/modules/${_kernver}/build"

  install -Dt "${_builddir}" -m644 Makefile .config Module.symvers
  install -Dt "${_builddir}/kernel" -m644 kernel/Makefile

  mkdir "${_builddir}/.tmp_versions"

  cp -t "${_builddir}" -a include scripts

  install -Dt "${_builddir}/arch/${KARCH}" -m644 arch/${KARCH}/Makefile
  install -Dt "${_builddir}/arch/${KARCH}/kernel" -m644 arch/${KARCH}/kernel/asm-offsets.s arch/$KARCH/kernel/module.lds

  cp -t "${_builddir}/arch/${KARCH}" -a arch/${KARCH}/include
  for i in dove exynos omap2; do
    mkdir -p "${_builddir}/arch/${KARCH}/mach-${i}"
    cp -t "${_builddir}/arch/${KARCH}/mach-${i}" -a arch/$KARCH/mach-${i}/include
  done
  for i in omap orion samsung versatile; do
    mkdir -p "${_builddir}/arch/${KARCH}/plat-${i}"
    cp -t "${_builddir}/arch/${KARCH}/plat-${i}" -a arch/$KARCH/plat-${i}/include
  done

  install -Dt "${_builddir}/drivers/md" -m644 drivers/md/*.h
  install -Dt "${_builddir}/net/mac80211" -m644 net/mac80211/*.h

  # http://bugs.archlinux.org/task/13146
  install -Dt "${_builddir}/drivers/media/i2c" -m644 drivers/media/i2c/msp3400-driver.h

  # http://bugs.archlinux.org/task/20402
  install -Dt "${_builddir}/drivers/media/usb/dvb-usb" -m644 drivers/media/usb/dvb-usb/*.h
  install -Dt "${_builddir}/drivers/media/dvb-frontends" -m644 drivers/media/dvb-frontends/*.h
  install -Dt "${_builddir}/drivers/media/tuners" -m644 drivers/media/tuners/*.h

  # add xfs and shmem for aufs building
  mkdir -p "${_builddir}"/{fs/xfs,mm}

  # copy in Kconfig files
  find . -name Kconfig\* -exec install -Dm644 {} "${_builddir}/{}" \;

  # remove unneeded architectures
  local _arch
  for _arch in "${_builddir}"/arch/*/; do
    [[ ${_arch} == */${KARCH}/ ]] && continue
    rm -r "${_arch}"
  done

  # remove files already in linux-docs package
  rm -r "${_builddir}/Documentation"

  # remove now broken symlinks
  find -L "${_builddir}" -type l -printf 'Removing %P\n' -delete

  # Fix permissions
  chmod -R u=rwX,go=rX "${_builddir}"

  # strip scripts directory
  local _binary _strip
  while read -rd '' _binary; do
    case "$(file -bi "${_binary}")" in
      *application/x-sharedlib*)  _strip="${STRIP_SHARED}"   ;; # Libraries (.so)
      *application/x-archive*)    _strip="${STRIP_STATIC}"   ;; # Libraries (.a)
      *application/x-executable*) _strip="${STRIP_BINARIES}" ;; # Binaries
      *) continue ;;
    esac
    /usr/bin/strip ${_strip} "${_binary}"
  done < <(find "${_builddir}/scripts" -type f -perm -u+w -print0 2>/dev/null)


## include kernel flash image


cd "${srcdir}/${_srcname}"

  cp ../kernel.its .
  mkimage -D "-I dts -O dtb -p 2048" -f kernel.its vmlinux.uimg
  dd if=/dev/zero of=bootloader.bin bs=512 count=1
  echo 'console=tty0 init=/sbin/init root=PARTUUID=%U/PARTNROFF=1 rootwait rw noinitrd zswap.compressor=zstd zswap.max_pool_percent=40' > cmdline
  vbutil_kernel \
    --pack vmlinux.kpart \
    --version 1 \
    --vmlinuz vmlinux.uimg \
    --arch arm \
    --keyblock ../kernel.keyblock \
    --signprivate ../kernel_data_key.vbprivk \
    --config cmdline \
    --bootloader bootloader.bin
  cp vmlinux.kpart "${pkgdir}/boot"    
    
}
