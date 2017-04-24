#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=msgpack-python
PKG_VERSION:=0.4.8
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://pypi.python.org/packages/21/27/8a1d82041c7a2a51fcc73675875a5f9ea06c2663e02fcfeb708be1d081a0/
PKG_MD5SUM:=dcd854fb41ee7584ebbf35e049e6be98

PKG_BUILD_DEPENDS:=python python-setuptools

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Xuefer <xuefer@gmail.com>

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/python-msgpack-python
	SECTION:=lang
	CATEGORY:=Languages
	SUBMENU:=Python
	TITLE:=python-msgpack-python
	URL:=https://pypi.python.org/pypi/msgpack-python
	DEPENDS:=+python-light
endef

define Package/python-msgpack-python/description
MessagePack is a fast, compact binary serialization format,
suitable for similar data to JSON.
This package provides CPython bindings for reading and writing MessagePack data.
endef

define Build/Compile
	$(call Build/Compile/PyMod,,install --prefix=/usr --root=$(PKG_INSTALL_DIR))
endef

$(eval $(call PyPackage,python-msgpack-python))
$(eval $(call BuildPackage,python-msgpack-python))
