#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=zerotier
PKG_VERSION:=1.0.2
PKG_REV:=8f021d8c28e6a52aabc4970dc376c70b61f56a10
PKG_RELEASE:=3

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=git://github.com/mwarning/ZeroTierOne.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_LICENSE:=GPL-3.0

#libstdc++
#PKG_BUILD_DEPENDS:=libstdcpp
PKG_BUILD_DEPENDS:=uclibcxx

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/zerotier
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libpthread +kmod-tun +ip
  TITLE:=Create flat virtual Ethernet networks of almost unlimited size
  URL:=https://www.zerotier.com
  SUBMENU:=VPN
endef

define Package/zerotier/description
	ZeroTier creates a global provider-independent virtual private cloud network.
endef

#libstdc++
#MAKE_FLAGS += LDFLAGS="-static-libgcc -static-libstdc++ -lgcc_s"
MAKE_FLAGS += \
	CFLAGS+="-fno-builtin -nostdinc++ -I$(STAGING_DIR)/usr/include/uClibc++ -DGCC_HASCLASSVISIBILITY -lpthread -lm -Wall -fPIE -fvisibility=hidden -pthread -DNDEBUG" \
	LDFLAGS+="-fno-builtin -nostdinc++ -L$(STAGING_DIR)/usr/lib/uClibc++ -DGCC_HASCLASSVISIBILITY -lpthread -lm -lc -nodefaultlibs -Wl,-Bstatic -luClibc++ -Wl,-Bdynamic -lsupc++ -lc -lgcc -lgcc_eh -lgcc_s"

define Package/zerotier/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/zerotier-one $(1)/usr/bin/
	$(LN) zerotier-one $(1)/usr/bin/zerotier-cli
	$(LN) zerotier-one $(1)/usr/bin//zerotier-idtool

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) files/zerotier.init $(1)/etc/init.d/zerotier
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) files/zerotier.config $(1)/etc/config/zerotier
endef

$(eval $(call BuildPackage,zerotier))

