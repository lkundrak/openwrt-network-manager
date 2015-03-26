#
# Copyright (C) 2015 Lubomir Rintel <lkundrak@v3.sk>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

# TODO: initscript
# TODO: nmstatedir & nmrundir
# TODO: fix uClibc so that it builds systemd-dhcp code. Currently glibc is needed.

include $(TOPDIR)/rules.mk

PKG_NAME:=network-manager
PKG_VERSION:=1.1.0
PKG_RELEASE:=$(PKG_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://anongit.freedesktop.org/NetworkManager/NetworkManager
PKG_SOURCE_VERSION:=ca9679334cb7abf66a9e80050d229d82e1823625
PKG_SOURCE_SUBDIR:=NetworkManager-$(PKG_VERSION)-$(PKG_SOURCE_VERSION)
PKG_SOURCE:=NetworkManager-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_SOURCE_SUBDIR)

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1
PKG_LICENSE:=GPL-2+

PKG_BUILD_DEPENDS:=gnutls gudev1 libdbus-glib libgcrypt libiconv libintl libncurses libndp libnl libreadline udev

# FIXME: this depends on tools outside toolchain. Autoreconf fixup itself
# is not sufficient, as the toolchain does not provide gtkdocize.
# Not needed for tarball builds.
define autogen
	(cd $(PKG_BUILD_DIR); gtkdocize; autopoint --force; AUTOPOINT='intltoolize --automake --copy' autoreconf --force --install --verbose);
endef

Hooks/Configure/Pre += autogen

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk

# gtk-doc.make

define Package/network-manager
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Network Management Daemon and Utilities
  URL:=http://www.freedesktop.org/wiki/NetworkManager/
  DEPENDS:=+glib2 +udev +libndp +libnl +libgnutls +libgcrypt +libdbus-glib +libdbus +libuuid +libncurses +libreadline
endef

define Package/network-manager/description
  NetworkManager is a system service that manages network interfaces and
  connections based on user or automatic configuration. It supports
  Ethernet, Bridge, Bond, VLAN, Team, InfiniBand, Wi-Fi, mobile broadband
  (WWAN), PPPoE and other devices, and supports a variety of different VPN
  services.
endef

CONFIGURE_ARGS += --disable-config-plugin-ibft
CONFIGURE_ARGS += --disable-ifcfg-rh
CONFIGURE_ARGS += --disable-ifcfg-suse
CONFIGURE_ARGS += --disable-ifupdown
CONFIGURE_ARGS += --disable-ifnet
CONFIGURE_ARGS += --without-netconfig
CONFIGURE_ARGS += --with-systemd-logind=no
CONFIGURE_ARGS += --with-session-tracking=no
CONFIGURE_ARGS += --with-crypto=gnutls

CONFIGURE_VARS+= \
	CPPFLAGS="$$$$CPPFLAGS -I$(ICONV_PREFIX)/include" \
	CPPFLAGS="$$$$CPPFLAGS -I$(INTL_PREFIX)/include" \
	LDFLAGS="$$$$LDFLAGS -L$(ICONV_PREFIX)/lib -Wl,-rpath-link=$(ICONV_PREFIX)/lib" \
	LDFLAGS="$$$$LDFLAGS -L$(INTL_PREFIX)/lib -Wl,-rpath-link=$(INTL_PREFIX)/lib" \
	LDFLAGS="$$$$LDFLAGS -Wl,-rpath-link=$(STAGING_DIR)/lib" \

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libnm $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/libnm-glib $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/NetworkManager $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/*.pc $(1)/usr/lib/pkgconfig/
endef

define Package/network-manager/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/NetworkManager $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/nm-online $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/nmcli $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/*.so.* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/nm-avahi-autoipd.action $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/nm-iface-helper $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/nm-dispatcher $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/nm-dhcp-helper $(1)/usr/lib

	$(INSTALL_DIR) $(1)/lib/udev/rules.d
	$(CP) $(PKG_INSTALL_DIR)/lib/udev/rules.d/*.rules $(1)/lib/udev/rules.d

	# TODO: Separate packages
	$(INSTALL_DIR) $(1)/usr/lib/NetworkManager
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/NetworkManager/libnm-device-plugin-adsl.so $(1)/usr/lib/NetworkManager
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/NetworkManager/libnm-device-plugin-wifi.so $(1)/usr/lib/NetworkManager

	$(INSTALL_DIR) $(1)/usr/lib/pppd
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pppd/* $(1)/usr/lib/pppd

	$(INSTALL_DIR) $(1)/usr/share/dbus-1
	$(CP) $(PKG_INSTALL_DIR)/usr/share/dbus-1/* $(1)/usr/share/dbus-1

	$(INSTALL_DIR) $(1)/usr/share/polkit-1/actions
	$(CP) $(PKG_INSTALL_DIR)/usr/share/polkit-1/actions/org.freedesktop.NetworkManager.policy $(1)/usr/share/polkit-1/actions

	$(INSTALL_DIR) $(1)/etc/NetworkManager/system-connections
	$(INSTALL_DIR) $(1)/etc/NetworkManager/dispatcher.d/pre-up.d
	$(INSTALL_DIR) $(1)/etc/NetworkManager/dispatcher.d/pre-down.d
endef

$(eval $(call BuildPackage,network-manager))
