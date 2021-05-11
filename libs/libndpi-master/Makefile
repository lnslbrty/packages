include $(TOPDIR)/rules.mk

PKG_NAME:=libndpi-master
PKG_VERSION:=1.0
PKG_RELEASE:=$(PKT_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/ntop/nDPI.git
PKG_SOURCE_VERSION:=f19937c8c9fd4e1c21988f523d9e78b954f6fcc8
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Toni Uhlig <matzeton@gmail.com>
PKG_LICENSE:=LGPL-3.0-or-later
PKG_LICENSE_FILES:=COPYING

PKG_FIXUP:=autoreconf
PKG_REMOVE_FILES:=autogen.sh
PKG_BUILD_DEPENDS:=libpcap
PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=0

include $(INCLUDE_DIR)/package.mk

ifeq ($(CONFIG_LIBNDPI_MASTER_GCRYPT),)
CONFIGURE_ARGS += --disable-gcrypt
endif

define Package/libndpi-master
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=Library for deep-packet inspection (unstable development version)
  URL:=https://github.com/ntop/nDPI
  DEPENDS:=+LIBNDPI_MASTER_GCRYPT:libgcrypt +libpcap +libjson-c
endef

define Package/libndpi-master/description
  nDPI is an open source LGPLv3 library for deep-packet inspection.
  Based on OpenDPI it includes ntop extensions.
endef

define Package/libndpi-master/config
config LIBNDPI_MASTER_GCRYPT
	bool "GCrypt support"
	depends on PACKAGE_libndpi-master
	default n
	help
	  This option enables QUIC client hello decryption.
	  Disabled by default.
endef

define Build/Prepare
	$(PKG_UNPACK)
	$(Build/Patch)
	mv $(PKG_BUILD_DIR)/configure.seed $(PKG_BUILD_DIR)/configure.ac
	$(SED) "s/@NDPI_MAJOR@/3/g" \
		-e "s/@NDPI_MINOR@/5/g" \
		-e "s/@NDPI_PATCH@/0/g" \
		-e "s/@NDPI_VERSION_SHORT@/3.5.0/g" \
		-e "s/@FUZZY@/dnl> /g" \
		$(PKG_BUILD_DIR)/configure.ac
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/ndpi-master
	$(CP) $(PKG_BUILD_DIR)/src/include/*.h \
		$(1)/usr/include/ndpi-master/

	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/src/lib/libndpi.a \
		$(1)/usr/lib/libndpi-master.a
endef

$(eval $(call BuildPackage,libndpi-master))
