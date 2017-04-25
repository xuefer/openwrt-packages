#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=squid
PKG_VERSION:=3.5.25
PKG_RELEASE:=1

PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=Luka Perkov <luka@openwrt.org>

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_URL:=http://www.squid-cache.org/Versions/v3/3.5/
PKG_HASH:=28959254c32b8cd87e9599b6beb97352cf0638524e0f5ac3e1754f08462f3585

PKG_BUILD_PARALLEL:=1
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/squid/Default
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  URL:=http://www.squid-cache.org/
endef

define Package/squid
  $(call Package/squid/Default)
  MENU:=1
  DEPENDS:=+libopenssl +libpthread +librt +libltdl +libstdcpp +libbsd
  TITLE:=full-featured Web proxy cache
endef

define Package/squid/description
  Squid is a caching proxy for the Web supporting HTTP, HTTPS, FTP, and more.
  It reduces bandwidth and improves response times by caching and reusing
  frequently-requested web pages.
endef

define Package/squid/conffiles
/etc/squid/mime.conf
/etc/squid/squid.conf
/etc/squid/errorpage.css
endef

define Package/squid-purge
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=squid purge
endef

define Package/squid-client
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=squid client
endef

define Package/squid-mod-cachemgr
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=Web based proxy manager and reporting tool
endef

define Package/squid-mod-cachemgr/conffiles
/etc/squid/cachemgr.conf
endef

define Package/squid-mod-basic-auth-getpwnam
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=getpwnam basic authentication helper
endef

define Package/squid-mod-basic-auth-ncsa
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=NCSA basic authentication helper
endef

define Package/squid-mod-basic-auth-smb
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=Samba basic authentication helper
endef

define Package/squid-mod-digest-auth-file
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=Password file digest authentication helper
endef

define Package/squid-mod-external-acl-file-userip
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=IP user external ACL helper
endef

define Package/squid-mod-external-acl-unix-group
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=Unix group external ACL helper
endef

define Package/squid-mod-ntlm-auth-fake
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=Fake NTLM authentication helper
endef

define Package/squid-mod-ntlm-auth-smb-lm
  $(call Package/squid/Default)
  DEPENDS:=squid
  TITLE:=Samba NTLM authentication helper
endef

CONFIGURE_VARS += \
	BUILDCXX=g++ \
	BUILDCXXFLAGS="-O2 -pipe -march=native"

CONFIGURE_ARGS += \
	--config-cache \
	--datadir=/usr/share/squid \
	--libexecdir=/usr/lib/squid \
	--sysconfdir=/etc/squid \
	--enable-shared \
	--disable-static \
	--enable-icmp \
	--enable-delay-pools \
	--enable-icap-client \
	--enable-kill-parent-hack \
	--disable-snmp \
	--enable-ssl \
	--enable-ssl-crtd \
	--enable-cache-digests \
	--enable-auth \
	--enable-auth-basic="getpwnam NCSA SMB" \
	--enable-auth-ntlm="fake smb_lm" \
	--enable-auth-negotiate \
	--enable-auth-digest="file" \
	--enable-external-acl-helpers="file_userip unix_group" \
	--enable-linux-netfilter \
	--enable-unlinkd \
	--enable-storeio=ufs \
	--enable-arp-acl \
	--enable-htcp \
	--enable-ssl-crtd \
	--enable-x-accelerator-vary \
	--disable-translation \
	--disable-auto-locale \
	--with-dl \
	--with-pthreads \
	--without-expat \
	--without-libxml2 \
	--without-gnutls \
	--without-nettle \
	--with-openssl=$(STAGING_DIR)/usr \
	--enable-epoll \
	--with-maxfd=4096 \
	--disable-auth-negotiate \
	--with-krb5-config=no \
	--without-mit-krb5 \
	--without-heimdal-krb5 \
	--without-libcap \
	--without-netfilter-conntrack \
	--enable-err-languages=English \
	--enable-default-err-language=English \
	--enable-referer-log \
	--enable-useragent-log

CONFIGURE_VARS += \
	ac_cv_header_linux_netfilter_ipv4_h=yes \
	ac_cv_epoll_works=yes \
	squid_cv_gnu_atomics=no

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/lib all
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		DESTDIR="$(PKG_INSTALL_DIR)" \
		install
endef

define Package/squid/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/squid $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/usr/lib/squid
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/squid/ssl_crtd $(1)/usr/lib/squid

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) ./files/squid.config $(1)/etc/config/squid

	$(INSTALL_DIR) $(1)/etc/squid
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/squid/{mime,errorpage}* $(1)/etc/squid/
	$(INSTALL_CONF) ./files/squid.conf $(1)/etc/squid/

	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/squid.init $(1)/etc/init.d/squid

	$(INSTALL_DIR) $(1)/usr/share/squid
	$(CP) $(PKG_INSTALL_DIR)/usr/share/squid/* $(1)/usr/share/squid/

	$(INSTALL_DIR) $(1)/usr/share/squid/errors/templates/
	$(CP) $(PKG_INSTALL_DIR)/usr/share/squid/errors/templates/* $(1)/usr/share/squid/errors/templates/

	$(INSTALL_DIR) $(1)/usr/lib/squid
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/squid/{ssl_crtd,unlinkd,pinger,log_file_daemon} $(1)/usr/lib/squid/
endef

define Package/squid-client/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/squidclient $(1)/usr/bin/
endef

define Package/squid-purge/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/purge $(1)/usr/bin/
endef

define Package/squid-mod-cachemgr/install
	$(INSTALL_DIR) $(1)/www/cgi-bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/squid/cachemgr.cgi $(1)/www/cgi-bin/

	$(INSTALL_DIR) $(1)/etc/squid
	$(INSTALL_CONF) $(PKG_INSTALL_DIR)/etc/squid/cachemgr.conf $(1)/etc/squid/
endef

define BuildPlugin
  define Package/$(1)/install
	$(INSTALL_DIR) $$(1)/usr/lib/squid
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/lib/squid/$(2) $$(1)/usr/lib/squid/
  endef

  $$(eval $$(call BuildPackage,$(1)))
endef

$(eval $(call RequireHeader,/usr/include/openssl/ssl.h, \
	$(PKG_NAME) requires the openssl development (named like libssl-dev, \
	depending of your package manager) package be installed on the host-system. \
))

$(eval $(call BuildPackage,squid))
$(eval $(call BuildPackage,squid-client))
$(eval $(call BuildPackage,squid-purge))
$(eval $(call BuildPackage,squid-mod-cachemgr))
$(eval $(call BuildPlugin,squid-mod-basic-auth-getpwnam,basic_getpwnam_auth))
$(eval $(call BuildPlugin,squid-mod-basic-auth-ncsa,basic_ncsa_auth))
$(eval $(call BuildPlugin,squid-mod-basic-auth-smb,basic_smb_auth))
$(eval $(call BuildPlugin,squid-mod-digest-auth-file,digest_file_auth))
$(eval $(call BuildPlugin,squid-mod-external-acl-file-userip,ext_file_userip_acl))
$(eval $(call BuildPlugin,squid-mod-external-acl-unix-group,ext_unix_group_acl))
$(eval $(call BuildPlugin,squid-mod-ntlm-auth-fake,ntlm_fake_auth))
$(eval $(call BuildPlugin,squid-mod-ntlm-auth-smb-lm,ntlm_smb_lm_auth))
