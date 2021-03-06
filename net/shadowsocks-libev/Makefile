#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=shadowsocks-libev
PKG_VERSION:=2.5.6
PKG_SOURCE_VERSION:=e1d82525c2aeaf5170b640e7ec363f0b74835c61
PKG_RELEASE:=2017-02-05-$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/shadowsocksr/shadowsocksr-libev.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.xz
PKG_MAINTAINER:=Jian Chang <aa65535@live.com>

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE)

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf
PKG_USE_MIPS16:=0
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/shadowsocks-libev/Default
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Lightweight Secured Socks5 Proxy $(2)
	URL:=https://github.com/breakwa11/shadowsocks-libev.git
	VARIANT:=$(1)
	DEPENDS:=$(3) +libpthread +ipset @(PACKAGE_ip||PACKAGE_ip-full) +iptables-mod-tproxy +libpcre +zlib
endef

CONFIGURE_ARGS += \
	--disable-documentation \

Package/shadowsocks-libev = $(call Package/shadowsocks-libev/Default,openssl,(OpenSSL),+libopenssl)
Package/shadowsocks-libev-mbedtls = $(call Package/shadowsocks-libev/Default,mbedtls,(mbed TLS),+libmbedtls)

define Package/shadowsocks-libev/description
Shadowsocks-libev is a lightweight secured socks5 proxy for embedded devices and low end boxes.
endef

Package/shadowsocks-libev-mbedtls/description = $(Package/shadowsocks-libev/description)

define Package/shadowsocks-libev/conffiles
/etc/config/shadowsocks-libev
endef

Package/shadowsocks-libev-mbedtls/conffiles = $(Package/shadowsocks-libev/conffiles)

define Package/shadowsocks-libev/postinst
#!/bin/sh
uci -q batch <<-EOF >/dev/null
	delete firewall.shadowsocks_libev
	set firewall.shadowsocks_libev=include
	set firewall.shadowsocks_libev.type=script
	set firewall.shadowsocks_libev.path=/usr/share/shadowsocks-libev/firewall.include
	set firewall.shadowsocks_libev.reload=1
	commit firewall
EOF
exit 0
endef

Package/shadowsocks-libev-mbedtls/postinst = $(Package/shadowsocks-libev/postinst)

ifeq ($(BUILD_VARIANT),mbedtls)
	CONFIGURE_ARGS += --with-crypto-library=mbedtls
endif

define Package/shadowsocks-libev/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ss-{redir,tunnel,local,server} $(1)/usr/bin
	$(INSTALL_BIN) ./files/ss-rules $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/shadowsocks-libev.config $(1)/etc/config/shadowsocks-libev
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/shadowsocks-libev.init $(1)/etc/init.d/shadowsocks-libev
	$(INSTALL_DIR) $(1)/usr/share/shadowsocks-libev
	$(INSTALL_DATA) ./files/firewall.include $(1)/usr/share/shadowsocks-libev/firewall.include
endef

Package/shadowsocks-libev-mbedtls/install = $(Package/shadowsocks-libev/install)

$(eval $(call BuildPackage,shadowsocks-libev))
$(eval $(call BuildPackage,shadowsocks-libev-mbedtls))
