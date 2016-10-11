#
# Copyright (C) 2006,2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=wisitor
PKG_VERSION:=1.3.0
PKG_RELEASE=1


PKG_LICENSE:=GPL-2.0
# Note: Packaging is maintained upstream at
# https://github.com/wisitor/packages
PKG_MAINTAINER:=Turan Almammadov <t.almammadov@behrud.com.tr>
PKG_LICENSE_FILES:=COPYING


PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/wisitor/wisitor-gateway.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=1.3.0
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz

PKG_FIXUP:=autoreconf
# do not run make install
PKG_INSTALL:=0
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/wisitor/Default
  SUBMENU:=Captive Portals
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+iptables-mod-extra +iptables-mod-ipopt +iptables-mod-nat-extra +libpthread
  TITLE:=A wireless captive portal solution
  URL:=http://www.wisitor.org
endef

define Package/wisitor
$(call Package/wisitor/Default)
  VARIANT:=normal
endef

define Package/wisitor-tls
$(call Package/wisitor/Default)
  DEPENDS+= +libcyassl
  VARIANT:=tls
endef


define Package/wisitor/description
	The Wisitor project is a complete and embeddable captive
	portal solution for wireless community groups or individuals
	who wish to open a free Hotspot while still preventing abuse
	of their Internet connection.
endef

define Package/wisitor-tls/description
$(call Package/wisitor/description)

This variant can talk TLS to the auth server.
endef

define Package/wisitor/conffiles
/etc/wisitor.conf
endef

Package/wisitor-tls/conffiles = $(Package/wisitor/conffiles)


define Package/wisitor/configure
	$(call Build/Configure/Default)
endef

ifeq ($(BUILD_VARIANT),tls)

CONFIGURE_ARGS += \
  --enable-cyassl

endif

define Package/wisitor/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/wisitor $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/wdctl $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libhttpd.so* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/wisitor.conf $(1)/etc/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/wisitor-msg.html $(1)/etc/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/wisitor.init $(1)/etc/init.d/wisitor
endef

Package/wisitor-tls/install = $(Package/wisitor/install)

$(eval $(call BuildPackage,wisitor))
$(eval $(call BuildPackage,wisitor-tls))
