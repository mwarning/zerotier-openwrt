# ZeroTier-OpenWrt

[ZeroTier One](https://www.zerotier.com) is a programm to create a global provider-independent virtual private cloud.
This project offers a OpenWrt package for ZeroTier.

To inlcude ZeroTier One into your OpenWRT image or to create
an .ipk package (equivalent to Debians .deb files),
you have to build an OpenWRT image.
These steps were tested using OpenWRT-"Barrier Breaker" (14.07):

For building OpenWrt on Debian, you need to install these packages:
<pre>
apt-get install git subversion g++ libncurses5-dev gawk zlib1g-dev build-essential gettext unzip file
</pre>

Now build OpenWrt:
<pre>
git clone git://git.openwrt.org/14.07/openwrt.git
cd openwrt

./scripts/feeds update -a
./scripts/feeds install -a

git clone https://github.com/mwarning/zerotier-openwrt.git
cp -rf zerotier-openwrt/zerotier package/
rm -rf zerotier-openwrt/

make defconfig
make menuconfig
</pre>

At this point select the appropiate "Target System" and "Target Profile"
depending on what target chipset/router you want to build for.
Also mark the ZeroTier package under "Network" => "VPN".

Now compile/build everything:

<pre>
make
</pre>

The images and all *.ipk packages are now inside the bin/ folder.
You can install the ZeroTier .ipk using "opkg install &lt;ipkg-file&gt;" on the router.

For details please check the OpenWRT documentation.