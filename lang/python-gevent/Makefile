#
# Copyright (C) 2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=gevent
PKG_VERSION:=1.2.1
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://pypi.python.org/packages/54/dd/17dc7e899ac7c1de2d19b367b29d90fdb4cfe83bda8c2581464906c9399d/
PKG_MD5SUM:=6700a2433c8e0635425e6798760efc81

PKG_BUILD_DEPENDS:=python python-setuptools

PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=Xuefer <xuefer@gmail.com>

include $(INCLUDE_DIR)/package.mk
$(call include_mk, python-package.mk)

define Package/python-gevent
	SECTION:=lang
	CATEGORY:=Languages
	SUBMENU:=Python
	TITLE:=python-gevent
	URL:=https://pypi.python.org/pypi/gevent
	DEPENDS:=+python-light +python-greenlet
endef

define Package/python-gevent/description
gevent is a coroutine-based Python networking library.
endef

define Build/Prepare
$(call Build/Prepare/Default)
	rm -f $(PKG_BUILD_DIR)/src/gevent/{_socket,_ssl}3.py
endef

define Build/Compile
	$(call Build/Compile/PyMod,,install --prefix=/usr --root=$(PKG_INSTALL_DIR))
endef

$(eval $(call PyPackage,python-gevent))
$(eval $(call BuildPackage,python-gevent))
