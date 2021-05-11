include $(TOPDIR)/rules.mk

PKG_NAME:=nDPId
PKG_VERSION:=1.4
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE:=$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/lnslbrty/nDPId/archive/refs/tags
PKG_HASH:=b2a95d7ea1798e48bccabf879f7327c7cc27db83301dafc483e01bff7dc2b4f4

PKG_MAINTAINER:=Toni
PKG_LICENSE:=GPL-3
PKG_LICENSE_FILES:=COPYING
PKG_BUILD_DEPENDS:=libndpi-master

CMAKE_INSTALL:=1

NDPI_STATIC_LIB="$(STAGING_DIR)/usr/lib/libndpi-master.a"
NDPI_INC="$(STAGING_DIR)/usr/include/ndpi-master"

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/nDPId
 TITLE:=nDPId is a tiny nDPI based daemons / toolkit
 SECTION:=net
 CATEGORY:=Network
 DEPENDS:=+libpcap +LIBNDPI_GCRYPT:libgcrypt
 URL:=http://github.com/lnslbrty/nDPId
endef

define Package/nDPId/description
  nDPId is a set of daemons and tools to capture, process and classify network flows.
  It's only dependencies (besides a half-way modern c library and POSIX threads) are libnDPI (>= 3.6.0 or current github dev branch) and libpcap.
endef

define Package/nDPId/config
config NDPID_COLLECTD_SUPPORT
	bool "nDPId collectd support"
	depends on PACKAGE_nDPId && PACKAGE_collectd-mod-exec
	default n
	help
	  This option enables collectd to gather nDPId statistics via plugin-exec.
	  Disabled by default.
endef

CMAKE_OPTIONS += -DBUILD_EXAMPLES=ON
CMAKE_OPTIONS += -DBUILD_NDPI=OFF
CMAKE_OPTIONS += -DNDPI_NO_PKGCONFIG=ON
CMAKE_OPTIONS += -DENABLE_SANITIZER=OFF
CMAKE_OPTIONS += -DENABLE_MEMORY_PROFILING=OFF
CMAKE_OPTIONS += -DLIBNDPI_INC="$(NDPI_INC)"
CMAKE_OPTIONS += -DLIBNDPI_LIB="$(NDPI_STATIC_LIB)"

ifneq ($(CONFIG_LIBNDPI_GCRYPT),)
CMAKE_OPTIONS += -DNDPI_WIDTH_GCRYPT=ON
endif

ifdef NDPID_COLLECTD_SUPPORT
define Package/nDPId/install-collectd-files
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-collectd $(1)/usr/bin/
endef
endif

define Package/nDPId/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/nDPId $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPId-test $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-captured $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-collectd $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-json-dump $(1)/usr/bin/

	$(call Package/nDPId/install-collectd-files)
endef

$(eval $(call BuildPackage,nDPId))
