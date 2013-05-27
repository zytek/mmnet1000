mmnet1000
=========

Patches for Propox MMNET1000 AT91 board

Structure
---------

* kernel/ - patches for different **vanilla** kernels with board definition and sample config file.
* openwrt/ - patches for OpenWrt, adding MMNET1000 as a target (TODO)


*Note:* kernel config is not perfectly suited to mmnet1000 board and may seem a little bit bloated. Please send patches if you make smaller one.

Quick guide
-----------
One way of upgraiding mmnet1000 to OpenWRT 12.09 with some decent kernel

1. Download and build OpenWRT for AT91 SAM9260 board. This will give you ARM toolchain needed to build the kernel. You can use Propox sample config. Build and flash UBI image - original docs will tell you how.
2. Build the kernel (change paths)
  1. `cd linux-3.4.46`
  2. `patch -p1 -i ../kernel_X.Y.X_mmnet1000.patch`
  3. `cp ../kernel_X.Y.Z_mmnet1000.config .config`
  4. `export STAGING_DIR=~/openwrt/attitude_adjustment/staging_dir`
  5. `export $PATH=~/openwrt/attitude_adjustment/staging_dir/toolchain-arm_v5te_gcc-4.6-linaro_uClibc-0.9.33.2_eabi/bin:$PATH`
  5. `make ARCH=arm CROSS_COMPILE=arm-openwrt-linux- menuconfig`
  6. `make ARCH=arm CROSS_COMPILE=arm-openwrt-linux- uImage`
  7. Copy uImage to your tftp directory
3. Test it. Go into u-boot and boot it over the network.
 * `tftp 0x22000000 uImage`
 * `bootm 0x22000000`

*Note:* I am using original u-boot 2009 and machine id 2329 (0x919) and it works. 

FAQ
---
1. Q: Why not using openwrt's default 3.3.8 kernel?
  * A: because I was frustraded by openwrt's way of kernel configuration and module handling. Building kernel on my own and adding modules to openwrt rootfs seemed easier. 
2. Q: But can I use it?
  * A: Of course. Send me patches if you craft your own system ;-)

Info
----
Work based on patches sent by Przemysław Rudy to openwrt mailing list, which themselve origin from Propox patches for 2.6.

Usefuk inks
* MMNET1000 [CD content](http://www.propox.com/download/software/MMnet1000/)
* MMNET1000 docs: [EN](http://www.propox.com/download/docs/MMnet1000_linux_en.pdf) [PL](http://www.propox.com/download/docs/MMnet1000_linux_pl.pdf)
* P.Rudy patches: [bootstrap](http://patchwork.openwrt.org/patch/1241/) [uboot](http://patchwork.openwrt.org/patch/1240/) [openwrt](https://lists.openwrt.org/pipermail/openwrt-devel/2012-January/013641.html)
