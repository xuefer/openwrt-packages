#
# Copyright (C) 2007-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=softether
PKG_VERSION:=4.22-9634-beta
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/SoftEtherVPN/SoftEtherVPN.git
PKG_SOURCE_PROTO:=git
PKG_REV:=4df2eb4f9c2a8cf50e0123496bc20470edc21dc4
PKG_SOURCE_VERSION:=$(PKG_REV)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

PKG_BUILD_PARALLEL:=1

TARGET_LDFLAGS+=-liconv

MAKE_FLAGS+= \
	CPPFLAGS="$(TARGET_CPPFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	src/bin/BuiltHamcoreFiles/unix/hamcore.se2 \
	bin/vpnserver/vpnserver \
	bin/vpnbridge/vpnbridge \
	bin/vpnclient/vpnclient \
	bin/vpncmd/vpncmd

define Build/Configure
  cd $(PKG_BUILD_DIR) && (echo 1; echo $(if $(CONFIG_ARCH_64BIT),2,1)) | ./configure
endef

define Package/softether/default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libopenssl +libncurses +libreadline +zlib $(ICONV_DEPENDS)
  TITLE:=Softehter
  URL:=https://www.softether.org
  MAINTAINER:=Daiyuu Nobori
  SUBMENU:=VPN
endef

define Package/softether/default/description
  SoftEther VPN Project develops and distributes SoftEther VPN,
  An Open-Source Free Cross-platform Multi-protocol VPN Program,
  as an academic project from University of Tsukuba.
endef

define Package/softether-common
  $(Package/softether/default)
  TITLE:=Softether VPN - common
endef

define Package/softether-common/description
$(Package/softether/default/description)
endef

define Package/softether-server
  $(Package/softether/default)
  TITLE:=Softether VPN - server
  DEPENDS+=+softether-common
endef

define Package/softether-server/description
$(Package/softether/default/description)
endef

define Package/softether-bridge
  $(Package/softether/default)
  TITLE:=Softether VPN - bridge
  DEPENDS+=+softether-common
endef

define Package/softether-bridge/description
$(Package/softether/default/description)
endef

define Package/softether-client
  $(Package/softether/default)
  TITLE:=Softether VPN - client
  DEPENDS+=+softether-common
endef

define Package/softether-client/description
$(Package/softether/default/description)
endef

define Package/softether-cmd
  $(Package/softether/default)
  TITLE:=Softether VPN - command line tool
  DEPENDS+=+softether-common
endef

define Package/softether-cmd/description
$(Package/softether/default/description)
endef

TARGET_CFLAGS += -std=gnu99

define Package/softether-common/install
	$(INSTALL_DIR) $(1)/opt/softether
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/bin/BuiltHamcoreFiles/unix/hamcore.se2 $(1)/opt/softether/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	touch $(1)/etc/softether/lang.config
	ln -sf /etc/softether/lang.config $(1)/opt/softether/
	ln -sf /var/log/softether/packet_log $(1)/opt/softether/
	ln -sf /var/log/softether/security_log $(1)/opt/softether/
endef

define Package/softether-server/conffiles
/etc/softether
endef

define Package/softether-server/install
	$(INSTALL_DIR) $(1)/opt/softether
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnserver/vpnserver $(1)/opt/softether/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_server.config
	touch $(1)/etc/softether/vpn_server.config

	$(INSTALL_DIR) $(1)/usr/sbin
	echo '#!/bin/sh' > $(1)/usr/sbin/vpnserver
	echo 'exec /opt/softether/vpnserver "$$$$@"' >> $(1)/usr/sbin/vpnserver
	chmod 755 $(1)/usr/sbin/vpnserver
	ln -sf /etc/softether/vpn_server.config $(1)/opt/softether/
	ln -sf /etc/softether/backup.vpn_server.config $(1)/opt/softether/
	ln -sf /var/log/softether/server_log $(1)/opt/softether/
endef

define Package/softether-server/conffiles
/etc/softether
endef

define Package/softether-bridge/install
	$(INSTALL_DIR) $(1)/opt/softether
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnbridge/vpnbridge $(1)/opt/softether/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_bridge.config
	touch $(1)/etc/softether/vpn_bridge.config

	$(INSTALL_DIR) $(1)/usr/sbin
	echo '#!/bin/sh' > $(1)/usr/sbin/vpnbridge
	echo 'exec /opt/softether/vpnbridge "$$$$@"' >> $(1)/usr/sbin/vpnbridge
	chmod 755 $(1)/usr/sbin/vpnbridge
	ln -sf /etc/softether/vpn_bridge.config $(1)/opt/softether/
	ln -sf /etc/softether/backup.vpn_bridge.config $(1)/opt/softether/
endef

define Package/softether-client/install
	$(INSTALL_DIR) $(1)/opt/softether
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnclient/vpnclient $(1)/opt/softether/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_client.config
	touch $(1)/etc/softether/vpn_client.config

	$(INSTALL_DIR) $(1)/usr/sbin
	echo '#!/bin/sh' > $(1)/usr/sbin/vpnclient
	echo 'exec /opt/softether/vpnclient "$$$$@"' >> $(1)/usr/sbin/vpnclient
	chmod 755 $(1)/usr/sbin/vpnclient
	ln -sf /etc/softether/vpn_client.config $(1)/opt/softether/
	ln -sf /etc/softether/backup.vpn_client.config $(1)/opt/softether/
endef

define Package/softether-cmd/install
	$(INSTALL_DIR) $(1)/opt/softether
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpncmd/vpncmd $(1)/opt/softether/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_cmd.config
	touch $(1)/etc/softether/vpn_cmd.config

	$(INSTALL_DIR) $(1)/usr/sbin
	echo '#!/bin/sh' > $(1)/usr/sbin/vpncmd
	echo 'exec /opt/softether/vpncmd "$$$$@"' >> $(1)/usr/sbin/vpncmd
	chmod 755 $(1)/usr/sbin/vpncmd
	ln -sf /etc/softether/vpn_cmd.config $(1)/opt/softether/
	ln -sf /etc/softether/backup.vpn_cmd.config $(1)/opt/softether/
endef

$(eval $(call BuildPackage,softether-common))
$(eval $(call BuildPackage,softether-server))
$(eval $(call BuildPackage,softether-bridge))
$(eval $(call BuildPackage,softether-client))
$(eval $(call BuildPackage,softether-cmd))
