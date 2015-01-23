#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=zerotier
PKG_VERSION:=1.0.2
PKG_REV:=045b47852d75c47754e2a2fad678e5f0c91fa7da
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=git://github.com/mwarning/ZeroTierOne.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_LICENSE:=GPL-3.0

include $(INCLUDE_DIR)/package.mk

define Package/zerotier
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libpthread +kmod-tun +ip +libstdcpp
  TITLE:=ZeroTier
  URL:=https://www.zerotier.com
  SUBMENU:=VPN
endef

define Package/zerotier/description
	ZeroTier creates a global provider-independent virtual private cloud network.
endef

MAKE_FLAGS += \
	CFLAGS="-Os" \
	LDFLAGS="-pthread"

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

