#
# Copyright (C) 2006,2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=wifidog
PKG_VERSION:=1.2.1
PKG_RELEASE=1


PKG_LICENSE:=GPL-2.0
PKG_MAINTAINER:=Michael Haas <haas@computerlinguist.org>
PKG_LICENSE_FILES:=COPYING

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/wifidog/wifidog-gateway.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=717512c17e2689386c663869368d40e8e71e0bc6
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/wifidog/Default
  SUBMENU:=Captive Portals
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+iptables-mod-extra +iptables-mod-ipopt +iptables-mod-nat-extra +libpthread
  TITLE:=A wireless captive portal solution
  URL:=http://www.wifidog.org
endef

define Package/wifidog
$(call Package/wifidog/Default)
endef

define Package/wifidog-ssl
$(call Package/wifidog/Default)
  DEPENDS+=+libcyassl
  VARIANT:=cyassl
endef


define Package/wifidog/description
	The Wifidog project is a complete and embeddable captive
	portal solution for wireless community groups or individuals
	who wish to open a free Hotspot while still preventing abuse
	of their Internet connection.
endef

define Package/wifidog-ssl/description
$(call Package/wifidog/description)

This is a variant which can use SSL to talk to the auth server.
endef

define Package/wifidog/conffiles
/etc/wifidog.conf
endef

Package/wifidog-ssl/conffiles = $(Package/wifidog/conffiles)

COPTS = ""

ifeq ($(BUILD_VARIANT),cyassl)
        COPTS += --enable-cyassl
endif

CONFIGURE_ARGS += \
        $(COPTS)
define Package/wifidog/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/scripts/init.d/wifidog $(1)/usr/bin/wifidog-init
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/wifidog $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/wdctl $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libhttpd.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/wifidog.conf $(1)/etc/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/wifidog-msg.html $(1)/etc/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/wifidog.init $(1)/etc/init.d/wifidog
endef

Package/wifidog-ssl/install = $(Package/wifidog/install)

$(eval $(call BuildPackage,wifidog))
$(eval $(call BuildPackage,wifidog-ssl))
