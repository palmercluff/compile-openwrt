# compile-openwrt
How to compile and build your own version of OpenWrt and include custom packages.

We first need to get all the development packages. Assuming you are the root user, do:
`apt-get install build-essential subversion libncurses5-dev zlib1g-dev gawk gcc-multilib flex quilt git-core gettext qemu openssl libssl-dev git`

Then clone the OpenWrt GitHub repository. I will be using Chaos Calmer:
`git clone -b chaos_calmer git://github.com/openwrt/openwrt.git`

Then update and install all available OpenWrt packages to be included for selection when we get to the compiling stage (This does not include all the packages in the OpenWrt build):
```
./openwrt/scripts/feeds update -a
./openwrt/scripts/feeds install -a
```

Go into your openwrt directory
`cd openwrt`

Then type:
`make menuconfig`

- For QEMU compilation:
  - Set the "Target System" to "MIPS Malta CoreLV board (qemu)"
  - "Subtarget" should be "Little Endian"
  - "Target Profile" should stay "Default"
  - Press Enter on "Target Images" and make sure that "ramdisk" is selected
  - Highlight "Advanced configuration optins" and hit the spacebar so that there appears an asterisk in the box, then hit Enter
  - Highlight "Target Options" and hit the spacebar, then press Enter
  - Highlight "Build packages with MIPS16 instructions" and hit the spacebar to deselect the category
  - From there, if you navigate back to the 1st page of the menuconfig, you can select and deselect packages you want to include before you build the firmware
  - Exit the menuconfig

- From here, run `make` (this will take a long time on a slow computer)
- `qemu-system-mipsel -kernel bin/malta/openwrt-malta-le-vmlinux-initramfs.elf -nographic`
- Towards the end of this boot mechanism, you'll see the following line of output: `Please press Enter to activate this console.`
