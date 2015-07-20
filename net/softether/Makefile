#
# Copyright (C) 2007-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=softether
PKG_VERSION:=4.17-9566-beta
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/SoftEtherVPN/SoftEtherVPN.git
PKG_SOURCE_PROTO:=git
PKG_REV:=ff497063732fdc08300c8bc166f4d4f62317e98a
PKG_SOURCE_VERSION:=$(PKG_REV)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

TARGET_LDFLAGS+=-liconv

MAKE_FLAGS+= DEBUG=YES \
	CPPFLAGS="$(TARGET_CPPFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	bin/vpnserver/hamcore.se2 bin/vpnserver/vpnserver \
	bin/vpnbridge/hamcore.se2 bin/vpnbridge/vpnbridge \
	bin/vpnclient/hamcore.se2 bin/vpnclient/vpnclient \
	bin/vpncmd/hamcore.se2 bin/vpncmd/vpncmd

define Build/Configure
  cd $(PKG_BUILD_DIR) && (echo 1; echo $(if $(CONFIG_ARCH_64BIT),2,1)) | ./configure
endef

define Package/softether/default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libopenssl +libncurses +libreadline
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

define Package/softether-server
  $(Package/softether/default)
  TITLE:=Softether VPN - server
endef

define Package/softether-server/description
$(Package/softether/default/description)
endef

define Package/softether-bridge
  $(Package/softether/default)
  TITLE:=Softether VPN - bridge
endef

define Package/softether-bridge/description
$(Package/softether/default/description)
endef

define Package/softether-client
  $(Package/softether/default)
  TITLE:=Softether VPN - client
endef

define Package/softether-client/description
$(Package/softether/default/description)
endef

define Package/softether-cmd
  $(Package/softether/default)
  TITLE:=Softether VPN - command line tool
endef

define Package/softether-cmd/description
$(Package/softether/default/description)
endef

TARGET_CFLAGS += -std=gnu99

define Package/softether-server/install
	$(INSTALL_DIR) $(1)/opt/vpnserver
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnserver/{vpnserver,hamcore.se2} $(1)/opt/vpnserver/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_server.config
	touch $(1)/etc/softether/vpn_server.config

	$(INSTALL_DIR) $(1)/usr/sbin
	ln -sf /opt/vpnserver/vpnserver $(1)/usr/sbin
	ln -sf /etc/softether/vpn_server.config $(1)/opt/vpnserver/
	ln -sf /etc/softether/lang.config $(1)/opt/vpnserver/
	ln -sf /etc/softether/backup.vpn_server.config $(1)/opt/vpnserver/
endef

define Package/softether-server/conffiles
/etc/softether
endef

define Package/softether-bridge/install
	$(INSTALL_DIR) $(1)/opt/vpnbridge
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnbridge/{vpnbridge,hamcore.se2} $(1)/opt/vpnbridge/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_bridge.config
	touch $(1)/etc/softether/vpn_bridge.config

	$(INSTALL_DIR) $(1)/usr/sbin
	ln -sf /opt/vpnbridge/vpnbridge $(1)/usr/sbin
	ln -sf /etc/softether/vpn_bridge.config $(1)/opt/vpnbridge/
	ln -sf /etc/softether/lang.config $(1)/opt/vpnbridge/
	ln -sf /etc/softether/backup.vpn_bridge.config $(1)/opt/vpnbridge/
endef

define Package/softether-client/install
	$(INSTALL_DIR) $(1)/opt/vpnclient
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpnclient/{vpnclient,hamcore.se2} $(1)/opt/vpnclient/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_client.config
	touch $(1)/etc/softether/vpn_client.config

	$(INSTALL_DIR) $(1)/usr/sbin
	ln -sf /opt/vpnclient/vpnclient $(1)/usr/sbin
	ln -sf /etc/softether/vpn_client.config $(1)/opt/vpnclient/
	ln -sf /etc/softether/lang.config $(1)/opt/vpnclient/
	ln -sf /etc/softether/backup.vpn_client.config $(1)/opt/vpnclient/
endef

define Package/softether-cmd/install
	$(INSTALL_DIR) $(1)/opt/vpncmd
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bin/vpncmd/{vpncmd,hamcore.se2} $(1)/opt/vpncmd/

	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/softether
	$(INSTALL_DIR) $(1)/etc/softether/backup.vpn_cmd.config
	touch $(1)/etc/softether/vpn_cmd.config

	$(INSTALL_DIR) $(1)/usr/sbin
	ln -sf /opt/vpncmd/vpncmd $(1)/usr/sbin
	ln -sf /etc/softether/vpn_cmd.config $(1)/opt/vpncmd/
	ln -sf /etc/softether/lang.config $(1)/opt/vpncmd/
	ln -sf /etc/softether/backup.vpn_cmd.config $(1)/opt/vpncmd/
endef

$(eval $(call BuildPackage,softether-server))
$(eval $(call BuildPackage,softether-bridge))
$(eval $(call BuildPackage,softether-client))
$(eval $(call BuildPackage,softether-cmd))
