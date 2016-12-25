# ZeroTier-OpenWrt

[ZeroTier One](https://www.zerotier.com) is a programm to create a global provider-independent virtual private cloud.
This project offers a LEDE packages for ZeroTier.

## Installing prebuild package

Download the [prebuild package](https://github.com/mwarning/zerotier-openwrt/releases) and copy it onto your LEDE installation, preferably into the /tmp folder.
Then install the ipk package file:
```
opkg install zerotier_*.ipk
```

Now start ZeroTier:
```
/etc/init.d/zerotier start
```

## Compiling from Sources

### Feed
A working LEDE build enviroment is expected (else see below).

Put this line in your feed definition (e.g. feeds.conf.default)
```
src-git zerotier https://github.com/mwarning/zerotier-openwrt.git
```

Update and install the new feed
```
./scripts/feeds update zerotier
./scripts/feeds install zerotier
```

Select and build the package
```
make menuconfig
make
```
(Network ---> VPN ---> <*> zerotier)

### The other way

To inlcude ZeroTier One into your LEDE image or to create
an .ipk package (equivalent to Debians .deb files),
you have to build an LEDE image.

For building LEDE on Debian, you need to install these packages:
```
sudo apt-get install subversion g++ zlib1g-dev build-essential git python
sudo apt-get install libncurses5-dev gawk gettext unzip file libssl-dev wget
```

Now build LEDE:
```
git clone https://git.lede-project.org/source.git lede
cd lede

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

For details please check the LEDE documentation.
