#
# Copyright (C) 2022 TDT AG <development@tdt.de>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
#
include $(TOPDIR)/rules.mk

PKG_NAME:=basicstation
PKG_VERSION:=2.0.7
PKG_RELEASE:=$(AUTORELEASE)

PKG_MAINTAINER:=Marcus Schref <mschref@tdt.de>
PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/basicstation
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=LoRaWAN
  TITLE:=LoRa Basic Station
  DEPENDS:=+kmod-spi-dev +liburcu +libmbedtls +sx1302_hal_wrt
endef

define Package/basicstation/description
  LoRa Basic Station. The LoRaWAN Gateway Software.
endef

define Package/basicstation/conffiles
/etc/config/basicstation
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	rsync -avh --delete --exclude=".git**" ./basicstation-src/* $(PKG_BUILD_DIR)/
	rm -rf $(PKG_BUILD_DIR)/deps/
	rm -f $(PKG_BUILD_DIR)/makefile
	rm -f $(PKG_BUILD_DIR)/makefile.s2core
	rm -f $(PKG_BUILD_DIR)/setup.gmk
	$(CP) ./files/* $(PKG_BUILD_DIR)/
	$(call Build/Prepare/Default)
endef

define Package/basicstation/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/station $(1)/usr/bin
	# $(INSTALL_DIR) $(1)/etc/config
	# $(INSTALL_CONF) ./files/etc/config/basicstation $(1)/etc/config/basicstation
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/basicstation $(1)/etc/init.d/basicstation

	$(INSTALL_DIR) $(1)/etc/basicstation
	#$(INSTALL_BIN) ./files/etc/basicstation/reset_lgw.sh $(1)/etc/basicstation/
	$(INSTALL_BIN) ./files/etc/basicstation/tc.trust $(1)/etc/basicstation/
	# those are same
	$(INSTALL_BIN) ./files/etc/basicstation/tc.trust $(1)/etc/basicstation/cups.trust
	$(INSTALL_DATA) ./files/etc/config/station.conf $(1)/etc/basicstation/

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) ./files/usr/bin/rinit.sh $(1)/usr/bin

	$(INSTALL_DIR) $(1)/etc/ssl/gw/tc_trust
	$(INSTALL_BIN) ./files/etc/basicstation/tc.trust $(1)/etc/ssl/gw/tc_trust

	$(INSTALL_DIR) $(1)/etc/ssl/gw/cups_boot_trust
	$(INSTALL_BIN) ./files/etc/basicstation/tc.trust $(1)/etc/ssl/gw/cups_boot_trust/cups.trust

	#chmod +x $(1)/etc/basicstation/reset_lgw.sh
	chmod +x $(1)/usr/bin/rinit.sh
endef

$(eval $(call BuildPackage,basicstation))
