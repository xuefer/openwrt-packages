#
# Copyright (C) 2016 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# 

include $(TOPDIR)/rules.mk

PKG_NAME:=rjsmin
PKG_VERSION:=1.0.12
PKG_RELEASE:=1

PKG_SOURCE:=rjsmin-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://pypi.python.org/packages/10/9c/2c45f57d43258b05bf33cf8f6c8161ea5abf8b4776a5c59d12646727cd98/
PKG_MD5SUM:=9f67e133c88df5497d3da847603da9bf

PKG_BUILD_DEPENDS:=python python-setuptools
HOST_BUILD_DEPENDS:=python/host python-setuptools/host

include $(INCLUDE_DIR)/host-build.mk
include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)
$(call include_mk, python-host.mk)

define Package/python-rjsmin
	SUBMENU:=Python
	SECTION:=lang
	CATEGORY:=Languages
	TITLE:=rJSmin
	URL:=http://opensource.perlig.de/rjsmin/
	DEPENDS:=+python +python-setuptools
	MAINTAINER:=André “nd” Malo <nd perlig.de>
endef

define Package/python-rjsmin/description
  rJSmin is a javascript minifier written in python
endef

define Build/Compile
	$(call Build/Compile/PyMod,,\
		install --root="$(PKG_INSTALL_DIR)" --prefix="/usr" \
		--single-version-externally-managed \
	)
endef

define Host/Compile
	$(call Build/Compile/HostPyMod,,install --prefix="" --root="$(STAGING_DIR)/host")
endef

define Host/Install
endef

$(eval $(call HostBuild))

$(eval $(call PyPackage,python-rjsmin))
$(eval $(call BuildPackage,python-rjsmin))

