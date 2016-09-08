#
# Copyright (C) 2008-2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=netdata
PKG_VERSION:=1.6.0
PKG_SOURCE_VERSION:=99d44b7d0c4e006b11318a28ba4a7e7d3f9b3bae
PKG_RELEASE:=2017-04-10-$(PKG_SOURCE_VERSION)
PKG_MAINTAINER:=Xuefer <xuefer@gmail.com>
PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=COPYING

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/firehol/netdata
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/netdata
  SECTION:=admin
  CATEGORY:=Administration
  DEPENDS:=+zlib +libuuid
  TITLE:=Real-time performance monitoring tool
  URL:=http://netdata.firehol.org/
endef

define Package/netdata/description
  netdata is a highly optimized Linux daemon providing real-time performance
  monitoring for Linux systems, applications and SNMP devices over the web.
endef

define Package/netdata/conffiles
/etc/netdata/
endef

define Package/netdata/install
	mkdir -p $(1)/etc/netdata
	$(CP) $(PKG_INSTALL_DIR)/etc/netdata $(1)/etc
	$(CP) ./files/netdata.conf $(1)/etc/netdata
	mkdir -p $(1)/usr/share/netdata
	$(CP) $(PKG_INSTALL_DIR)/usr/share/netdata $(1)/usr/share
	rm $(1)/usr/share/netdata/web/images/animated.gif
	rm -rf $(1)/usr/share/netdata/web/images/seo-*.png
	rm -rf $(1)/usr/share/netdata/web/images/post.png
	rm -rf $(1)/usr/share/netdata/web/old
	rm $(1)/usr/share/netdata/web/demo*html
	rm $(1)/usr/share/netdata/web/fonts/*.svg
	rm $(1)/usr/share/netdata/web/fonts/*.ttf
	rm $(1)/usr/share/netdata/web/fonts/*.woff
	rm $(1)/usr/share/netdata/web/fonts/*.otf
	mkdir -p $(1)/usr/lib/netdata
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/netdata $(1)/usr/lib
	rm $(1)/usr/lib/netdata/python.d/python-modules-installer.sh
	rm -rf $(1)/usr/lib/netdata/python.d/python_modules/pyyaml2
	rm -rf $(1)/usr/lib/netdata/python.d/python_modules/pyyaml3
	chmod 4755 $(1)/usr/lib/netdata/plugins.d/apps.plugin
	mkdir -p $(1)/etc/init.d
	$(INSTALL_BIN) ./files/netdata.init $(1)/etc/init.d/netdata
	mkdir -p $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/netdata $(1)/usr/sbin
endef

$(eval $(call BuildPackage,netdata))
