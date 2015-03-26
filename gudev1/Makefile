include $(TOPDIR)/rules.mk

PKG_NAME:=gudev1
PKG_VERSION:=219
PKG_RELEASE:=1

PKG_SOURCE:=systemd-$(PKG_VERSION).tar.xz
PKG_SOURCE_SUBDIR:=systemd-$(PKG_VERSION)
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)
PKG_SOURCE_URL:=http://www.freedesktop.org/software/systemd/
PKG_MD5SUM:=e0d6c9a4b4f69f66932d2230298c9a34

PKG_BUILD_DEPENDS:=udev glib2

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

GUDEV_NAME=gudev
GUDEV_VERSION=1.0
SONAME=soname$(GUDEV_NAME)-$(GUDEV_VERSION).so.0

define Package/gudev1
  SECTION:=base
  CATEGORY:=Base system
  TITLE:=Libraries for adding libudev support to applications that use glib
  URL:=http://www.freedesktop.org/wiki/Software/systemd/
  MAINTAINER:=Lubomir Rintel <lkundrak@v3.sk>
  DEPENDS:=+udev +glib2
endef

define Package/gudev1/description
  This package contains the libraries that make it easier to use libudev
  functionality from applications that use glib.
endef

define Build/Configure
	: Do nothing
endef

define Build/Compile
	glib-genmarshal --prefix=g_udev_marshal --header $(PKG_BUILD_DIR)/src/gudev/gudevmarshal.list >$(PKG_BUILD_DIR)/src/gudev/gudevmarshal.h
	echo '#include "gudevmarshal.h"' >$(PKG_BUILD_DIR)/src/gudev/gudevmarshal.c
	glib-genmarshal --prefix=g_udev_marshal --body $(PKG_BUILD_DIR)/src/gudev/gudevmarshal.list >>$(PKG_BUILD_DIR)/src/gudev/gudevmarshal.c
	$(TARGET_CC) -L$(INTL_PREFIX)/lib -L$(STAGING_DIR)/lib -shared -o $(SONAME) -Wl,-soname=$(SONAME) -fPIC -I$(PKG_BUILD_DIR)/src `pkg-config --cflags --libs libudev glib-2.0 gobject-2.0` -D_GUDEV_COMPILATION $(PKG_BUILD_DIR)/src/gudev/*.c
	sed 's,@prefix@,/usr,;s,@exec_prefix@,/usr,;s,@libdir@,/usr/lib,;s,@includedir@,/usr/include,;s,@VERSION@,$(PKG_VERSION),' $(PKG_BUILD_DIR)/src/gudev/$(GUDEV_NAME)-$(GUDEV_VERSION).pc.in >$(GUDEV_NAME)-$(GUDEV_VERSION).pc
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/$(GUDEV_NAME)-$(GUDEV_VERSION)/$(GUDEV_NAME)
	$(CP) $(PKG_BUILD_DIR)/src/gudev/*.h $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_DATA) $(SONAME) $(1)/usr/lib/
	$(LN) $(SONAME) $(1)/usr/lib/lib$(GUDEV_NAME)-$(GUDEV_VERSION).so
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(INSTALL_DATA) $(GUDEV_NAME)-$(GUDEV_VERSION).pc $(1)/usr/lib/pkgconfig/
endef

define Package/gudev1/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(SONAME) $(1)/usr/lib/
endef

$(eval $(call BuildPackage,gudev1))
