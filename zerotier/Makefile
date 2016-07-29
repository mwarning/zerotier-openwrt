#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=zerotier
PKG_VERSION:=1.1.14
PKG_REV:=ae491c277e6f35d1acbdcbf700e2b834957295ae
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=git://github.com/zerotier/ZeroTierOne.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_LICENSE:=GPL-3.0

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

define Package/zerotier/config
	source "$(SOURCE)/Config.in"
endef

ifeq ($(CONFIG_ZEROTIER_ENABLE_NETWORK_CONTROLLER),y)
MAKE_FLAGS += ZT_ENABLE_NETWORK_CONTROLLER=1
DEPENDS+=+sqlite
endif

ifeq ($(CONFIG_ZEROTIER_USE_MINIUPNPC),y)
MAKE_FLAGS += ZT_USE_MINIUPNPC=1
DEPENDS+=+libminiupnpc
endif

ifeq ($(CONFIG_ZEROTIER_ENABLE_DEBUG),y)
TARGET_CXXFLAGS += -DZT_TRACE -ggdb3
endif

MAKE_FLAGS += \
	LDFLAGS+=" -L$(STAGING_DIR)/usr/lib/uClibc++ -pthread " \
	LDLIBS+=" -fno-builtin -nodefaultlibs -Wl,-Bstatic -luClibc++ -Wl,-Bdynamic  -lpthread -lm -lc -lsupc++ -lc -lgcc -lgcc_eh -lgcc_s  -lpthread -lm " \
	CXXFLAGS+=" -fno-builtin -nostdinc++ -I$(STAGING_DIR)/usr/include/uClibc++ -DGCC_HASCLASSVISIBILITY -Wall -fPIE -fvisibility=hidden "

define Build/Compile
	$(call Build/Compile/Default,one)
ifeq ($(CONFIG_ZEROTIER_ENABLE_SELFTEST),y)
	$(call Build/Compile/Default,selftest)
endif
endef

define Package/zerotier/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/zerotier-one $(1)/usr/bin/
	$(LN) zerotier-one $(1)/usr/bin/zerotier-cli
	$(LN) zerotier-one $(1)/usr/bin//zerotier-idtool

ifeq ($(CONFIG_ZEROTIER_ENABLE_SELFTEST),y)
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/zerotier-selftest $(1)/usr/bin/
endif

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) files/zerotier.init $(1)/etc/init.d/zerotier
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) files/zerotier.config $(1)/etc/config/zerotier
endef

$(eval $(call BuildPackage,zerotier))

