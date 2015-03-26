#
# Copyright (C) 2015 Lubomir Rintel <lkundrak@v3.sk>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libndp
PKG_VERSION:=1.4
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://libndp.org/files

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_LICENSE:=LGPL-2.1+

include $(INCLUDE_DIR)/package.mk

define Package/libndp
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Library for Neighbor Discovery Protocol
  URL:=http://libndp.org/
endef

define Package/libndp/description
  This package contains a library which provides a wrapper for IPv6 Neighbor
  Discovery Protocol. It also provides a tool named ndptool for sending and
  receiving NDP messages.  
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ndp.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libndp.{a,so*} $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libndp.pc $(1)/usr/lib/pkgconfig/
endef

define Package/libndp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libndp.so.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/ndptool $(1)/usr/bin/
endef

$(eval $(call BuildPackage,libndp))
