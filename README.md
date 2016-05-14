# ZeroTier-OpenWrt

[ZeroTier One](https://www.zerotier.com) is a programm to create a global provider-independent virtual private cloud.
This project offers a OpenWrt packages for ZeroTier.

## Installing prebuild package

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

To inlcude ZeroTier One into your OpenWRT image or to create
an .ipk package (equivalent to Debians .deb files),
you have to build an OpenWRT image.

For building OpenWrt on Debian, you need to install these packages:
```
sudo apt-get install subversion g++ zlib1g-dev build-essential git python
sudo apt-get install libncurses5-dev gawk gettext unzip file libssl-dev wget
```

Now build OpenWrt:
```
git clone git://git.openwrt.org/15.05/openwrt.git
cd openwrt

./scripts/feeds update -a
./scripts/feeds install -a

git clone https://github.com/mwarning/zerotier-openwrt.git
cp -rf zerotier-openwrt/zerotier package/
rm -rf zerotier-openwrt/

make defconfig
make menuconfig
```

At this point select the appropiate "Target System" and "Target Profile"
depending on what target chipset/router you want to build for.
Also mark the ZeroTier package under "Network" => "VPN".

Now compile/build everything:

```
make
```

The images and all *.ipk packages are now inside the bin/ folder.
You can install the ZeroTier .ipk using "opkg install &lt;ipkg-file&gt;" on the router.

For details please check the OpenWRT documentation.