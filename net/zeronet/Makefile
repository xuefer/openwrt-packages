#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=zeronet
PKG_VERSION:=0.5.7
PKG_RELEASE:=1
PKG_MAINTAINER:=Xuefer <xuefer@gmail.com>
PKG_LICENSE:=GPL-2

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/HelloZeroNet/ZeroNet.git
PKG_SOURCE_VERSION:=eb97ea4f34a021813d87a4aab739a0d96c7c04a8
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/zeronet
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=Web Servers/Proxies
	TITLE:=zeronet
	URL:=https://zeronet.io/
	DEPENDS:=+python +python-sqlite3 +python-gevent +python-msgpack-python
endef

define Package/zeronet/description
 Decentralized websites using Bitcoin crypto and the BitTorrent network
endef

define Build/Configure
endef
define Build/Compile
endef

define Package/zeronet/install
	$(INSTALL_DIR) $(1)/opt/zeronet
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/{update,start,zeronet}.py $(1)/opt/zeronet/
	$(CP) $(PKG_BUILD_DIR)/{src,plugins,tools} $(1)/opt/zeronet/
	$(CP) $(PKG_BUILD_DIR)/README*.md $(1)/opt/zeronet/
endef

$(eval $(call BuildPackage,zeronet))
