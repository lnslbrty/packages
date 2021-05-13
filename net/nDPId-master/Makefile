include $(TOPDIR)/rules.mk

PKG_NAME:=nDPId-master
PKG_VERSION:=1.4
PKG_RELEASE:=$(AUTORELEASE)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/lnslbrty/nDPId.git
PKG_SOURCE_VERSION:=6f04807236f55c734816ddcc5ab80d06f89a3a9b
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Toni
PKG_LICENSE:=GPL-3
PKG_LICENSE_FILES:=COPYING

CMAKE_INSTALL:=1

NDPI_STATIC_LIB="$(PKG_BUILD_DIR)/libnDPI/install/lib/libndpi.a"
NDPI_INC="$(PKG_BUILD_DIR)/libnDPI/install/include/ndpi"

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/nDPId-master
 TITLE:=nDPId is a tiny nDPI based daemons / toolkit (master branch)
 SECTION:=net
 CATEGORY:=Network
 DEPENDS:=+libpcap +zlib +LIBNDPI_GCRYPT:libgcrypt
 URL:=http://github.com/lnslbrty/nDPId
endef

define Package/nDPId-master/description
  nDPId is a set of daemons and tools to capture, process and classify network flows.
  It's only dependencies (besides a half-way modern c library and POSIX threads) are libnDPI (>= 3.6.0 or current github dev branch) and libpcap.
endef

define Package/nDPId-master/config
config NDPID_MASTER_COLLECTD_SUPPORT
   bool "nDPId collectd support"
   depends on PACKAGE_nDPId-master && PACKAGE_collectd-mod-exec && !CONFIG_NDPID_COLLECTD_SUPPORT
   default n
   help
     This option enables collectd to gather nDPId statistics via plugin-exec.
     Disabled by default.
endef

CMAKE_OPTIONS += -DBUILD_EXAMPLES=ON
CMAKE_OPTIONS += -DBUILD_NDPI=OFF
CMAKE_OPTIONS += -DNDPI_NO_PKGCONFIG=ON
CMAKE_OPTIONS += -DENABLE_ZLIB=ON
CMAKE_OPTIONS += -DENABLE_SANITIZER=OFF
CMAKE_OPTIONS += -DENABLE_MEMORY_PROFILING=OFF
CMAKE_OPTIONS += -DLIBNDPI_INC="$(NDPI_INC)"
CMAKE_OPTIONS += -DLIBNDPI_LIB="$(NDPI_STATIC_LIB)"

TARGET_CFLAGS += -DLIBNDPI_STATIC=1

ifneq ($(CONFIG_LIBNDPI_GCRYPT),)
CMAKE_OPTIONS += -DNDPI_WIDTH_GCRYPT=ON
endif

ifdef NDPID_MASTER_COLLECTD_SUPPORT
define Package/nDPId-master/install-collectd-files
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-collectd $(1)/usr/bin/nDPIsrvd-master-collectd
endef
endif

define Build/Prepare
	$(PKG_UNPACK)
	$(Build/Patch)
	$(SED) 's/^#define NDPI_API_VERSION.*/#define NDPI_API_VERSION 1/' \
		$(PKG_BUILD_DIR)/libnDPI/src/include/ndpi_api.h.in
	env \
		CFLAGS="$(TARGET_CFLAGS)" \
		CPPFLAGS="$(TARGET_CPPFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		CROSS_COMPILE_TRIPLET="$(GNU_TARGET_NAME)" \
		MAKE_PROGRAM="env -uCFLAGS -uCPPFLAGS $(MAKE)" \
		$(PKG_BUILD_DIR)/scripts/get-and-build-libndpi.sh
endef

define Package/nDPId-master/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/nDPId $(1)/usr/sbin/nDPId-master

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPId-test $(1)/usr/bin/nDPId-master-test
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd $(1)/usr/bin/nDPIsrvd-master
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-captured $(1)/usr/bin/nDPIsrvd-master-captured
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-collectd $(1)/usr/bin/nDPIsrvd-master-collectd
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/nDPIsrvd-json-dump $(1)/usr/bin/nDPIsrvd-master-json-dump

	$(call Package/nDPId-master/install-collectd-files)
endef

$(eval $(call BuildPackage,nDPId-master))
