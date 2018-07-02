include $(TOPDIR)/rules.mk

PKG_NAME=cryptopp
PKG_LICENSE:=Boost Software License - Version 1.0
PKG_LICENSE_FILES:=License.txt
PLG_VERSION:=7.0
PKG_RELEASE:=1

PKG_HASH:=a4bc939910edd3d29fb819a6fc0dfdc293f686fa62326f61c56d72d0a366ceb0
PKG_SOURCE:=cryptopp700.zip
PKG_SOURCE_URL:=https://www.cryptopp.com

include $(INCLUDE_DIR)/package.mk

PKG_UNPACK:=unzip -q -d $(PKG_BUILD_DIR) $(DL_DIR)/$(PKG_SOURCE)
PKG_BUILD_PARALLEL:=1

define Package/libcryptopp
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=C++ class library of cryptographic schemes
  URL:=https://www.cryptopp.com
  DEPENDS:=+libstdcpp
endef

define Package/libcryptopp/description
Crypto++ Library is a free C++ class library of cryptographic schemes.
Lookup supported algorithms: https://www.cryptopp.com
endef

CRYPTOPP_EXTRA_CXXFLAGS:=-DNDEBUG=1 -DCRYPTOPP_DISABLE_ASM=1

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) -f GNUmakefile-cross \
		AR="$(TARGET_AR)" \
		RANLIB="$(TARGET_RANLIB)" \
		CXX="$(TARGET_CXX)" \
		CXXFLAGS="$(TARGET_CXXFLAGS) $(CRYPTOPP_EXTRA_CXXFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)" \
		all
endef

define Build/InstallDev
	$(MAKE) -C $(PKG_BUILD_DIR) -f GNUmakefile-cross install PREFIX=/usr DESTDIR="$(1)"
endef

define Package/libcryptopp/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libcryptopp.so $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libcryptopp))
