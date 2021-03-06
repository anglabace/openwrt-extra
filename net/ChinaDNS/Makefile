#
# Copyright (C) 2015 OpenWrt-dist
#
# This is free software, licensed under the GPLv3.
# See /COPYING for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ChinaDNS
PKG_VERSION:=1.3.2
PKG_RELEASE:=3
PKG_REV:=9e4261c78e911851f76a9b9113d48fcdc44ce32e

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.bz2
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/shadowsocks/ChinaDNS.git

PKG_LICENSE:=GPLv3
PKG_LICENSE_FILES:=COPYING
PKG_MAINTAINER:=clowwindy <clowwindy42@gmail.com>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)/$(PKG_NAME)-$(PKG_VERSION)

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf
PKG_USE_MIPS16:=0
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/ChinaDNS
	SECTION:=net
	CATEGORY:=Network
	TITLE:=ChinaDNS
	URL:=https://github.com/shadowsocks/ChinaDNS
endef

define Package/ChinaDNS/description
A DNS forwarder that ignores incorrect(you knew it) responses.
endef

define Package/ChinaDNS/conffiles
/etc/config/chinadns
/etc/chinadns_chnroute.txt
endef

# Remove local implementation of ns_parser functions since this set of functions
# already been fixed by musl-libc upstream.
define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(call Build/Prepare/Default)
	rm -f $(PKG_BUILD_DIR)/src/local_ns_parser.{c,h}
endef

# get china routing list from https://github.com/17mon/china_ip_list provided by ipip.net
define Package/ChinaDNS/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/chinadns.init $(1)/etc/init.d/chinadns
	$(INSTALL_CONF) ./files/ipip_chnroute.txt $(1)/etc/chinadns_chnroute.txt
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/chinadns.config $(1)/etc/config/chinadns
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/chinadns $(1)/usr/bin
endef

$(eval $(call BuildPackage,ChinaDNS))
