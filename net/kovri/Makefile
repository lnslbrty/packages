include $(TOPDIR)/rules.mk

PKG_NAME:=kovri
PKG_VERSION:=2018-05-07
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.xz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/monero-project/kovri
PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=6b19905030c6b3f03661a4d0f83717b695f9ebb5
PKG_HASH:=a012b8b70a86e905e3d05851157aac26ad0674e168277377884fda17f792c92c

PKG_MAINTAINER:=Toni Uhlig <matzeton@googlemail.com>
PKG_LICENSE:=BSD-3
PKG_LICENSE_FILES:=LICENSE.md

CMAKE_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

CMAKE_OPTIONS += -DWITH_BINARY:BOOL=ON
CMAKE_OPTIONS += -DBUILD_CRYPTOPP:BOOL=OFF
CMAKE_OPTIONS += -DWITH_CRYPTOPP:BOOL=ON

EXTRA_CFLAGS += -DKOVRI_GIT_REVISION="\\\"$(PKG_SOURCE_VERSION)\\\""
EXTRA_CFLAGS += -Wno-psabi
EXTRA_CFLAGS += -I"$(PKG_BUILD_DIR)/deps"

define Package/kovri
	SECTION:=net
	CATEGORY:=Network
	TITLE:=fast and secure i2p implementation
	URL:=https://getkovri.org
	DEPENDS:=+boost-log +boost-program_options +libcryptopp +libopenssl +libpthread
endef

define Package/kovri/description
A free, decentralized, anonymity technology based on I2P's
open specifications.
endef

define Package/kovri/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/kovri $(1)/usr/bin/
endef

$(eval $(call BuildPackage,kovri))
