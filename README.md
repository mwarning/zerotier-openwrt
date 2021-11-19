# ZeroTier-OpenWrt

*ZeroTier has been merged into the official package repository and can be selected using the opkg package management tool.*

[ZeroTier One](https://www.zerotier.com) is a program to create a global provider-independent virtual private cloud.
This project offers OpenWrt packages for ZeroTier.

## Installing package

Download the [prebuild package](https://github.com/mwarning/zerotier-openwrt/releases) and copy it onto your OpenWrt installation, preferably into the /tmp folder.

Then install the ipk package file:

```
opkg install zerotier_*.ipk
```

Now start ZeroTier:

```
/etc/init.d/zerotier start
```

## Compiling from Sources

To include ZeroTier One into your OpenWrt image or to create
an .ipk package (equivalent to Debians .deb files),
you have to build an OpenWrt image.

To build OpenWrt on Debian, you need to install these packages:

```
sudo apt-get install subversion g++ zlib1g-dev build-essential git python
sudo apt-get install libncurses5-dev gawk gettext unzip file libssl-dev wget
```

Now prepare OpenWrt:

```
git clone https://github.com/openwrt/openwrt
cd openwrt

./scripts/feeds update -a
./scripts/feeds install -a
```

Now you can insert the zerotier package using a package feed or add the package manually.

### Add package by feed

A feed is the standard way packages are made available to the OpenWrt build system.

Put this line in your feeds list file (e.g. feeds.conf.default)

```
src-git zerotier https://github.com/mwarning/zerotier-openwrt.git
```

Update and install the new feed

```
./scripts/feeds update zerotier
./scripts/feeds install zerotier
```

Now continue with the building packages section.

### Add package by hand

```
git clone https://github.com/mwarning/zerotier-openwrt.git
cp -rf zerotier-openwrt/zerotier package/
rm -rf zerotier-openwrt/
```

Now continue with the building packages section.

### Building Packages

Configure packages:

```
make menuconfig
```

Now select the appropiate "Target System" and "Target Profile"
depending on what target chipset/router you want to build for.
Also mark the ZeroTier package under Network ---> VPN ---> <\*> zerotier.

Now compile/build everything:

```
make
```

The images and all \*.ipk packages are now inside the bin/ folder, including the zerotier package.
You can install the ZeroTier .ipk on the target device using `opkg install <ipkg-file>`.

For details please check the OpenWrt documentation.

#### Build bulk packages

For a release, it is useful the build packages at a bulk for multiple targets:

```
#!/bin/sh

# dump-target-info.pl is used to get all targets configurations:
# https://git.openwrt.org/?p=openwrt/openwrt.git;a=blob;f=scripts/dump-target-info.pl

./scripts/dump-target-info.pl architectures | while read pkgarch target1 rest; do
  echo "CONFIG_TARGET_${target1%/*}=y" > .config
  echo "CONFIG_TARGET_${target1%/*}_${target1#*/}=y" >> .config
  echo "CONFIG_PACKAGE_example1=y" >> .config

  # Debug output
  echo "pkgarch: $pkgarch, target1: $target1"

  make defconfig
  make -j4 tools/install
  make -j4 toolchain/install

  # Build package
  make package/zerotier/{clean,compile}

  # Free space (optional)
  rm -rf build_dir/target-*
  rm -rf build_dir/toolchain-*
done
```
