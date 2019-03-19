include $(TOPDIR)/rules.mk

PKG_NAME:=tools
PKG_VERSION:=1.0
PKG_RELEASE:=$(PKT_SOURCE_VERSION)

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=git://github.com/lnslbrty/tools.git
PKG_SOURCE_VERSION:=fcf4582f7967ae17b33bf8649ba55cc73bafc1d3
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MIRROR_HASH:=ccff5d4a64af9bc5d67b838c20ef1c85ce9182cf3db69c87755535a0fd972843
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Toni Uhlig <matzeton@gmail.com>

include $(INCLUDE_DIR)/package.mk

define Package/tools
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=My Tools
  URL:=https://github.com/lnslbrty/tools
  DEPENDS:=+libncurses
endef

define Package/tools/description
  My Tools
endef

define Build/Compile
	rm -rf $(PKG_INSTALL_DIR)
	mkdir -p $(PKG_INSTALL_DIR)
	$(MAKE) -C $(PKG_BUILD_DIR) \
		CC="$(TARGET_CC) $(TARGET_CFLAGS)" LDFLAGS="$(TARGET_LDFLAGS)" \
		MAKE_NCURSES=1 PREFIX="$(1)"
endef

define Package/tools/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) \
		$(addprefix $(PKG_BUILD_DIR)/,aes asciihexer dummyshell suidcmd scrambler gol) \
		$(1)/usr/bin/
endef

$(eval $(call BuildPackage,tools))
