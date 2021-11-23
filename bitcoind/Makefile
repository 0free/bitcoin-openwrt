
include $(TOPDIR)/rules.mk

PKG_NAME:=bitcoind
PKG_VERSION:=22.x
PKG_RELEASE:=1

PKG_SOURCE:=bitcoind-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/bitcoin/bitcoin.git
PKG_HASH:=a35d5b99332e0d152a33a47bf6d924165efda7f077980a7bcbd976c0312dd961
PKG_MIRROR_HASH:=d8d020fbf206682463623b982e51843f935cf45abea26d101ff1cd1dc3305591

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/bitcoind
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=bitcoind
	DEPENDS := +libcurl +libpthread +jansson +udev +libncurses 
	DEPENDS += +boost-chrono +boost-filesystem +boost-program_options +boost-thread +boost-test
	DEPENDS += +libopenssl +libstdcpp +libevent2 +libevent2-pthreads
endef

define Package/bitcoind/description
Bitcoind is a bitcoin full node
endef

define Package/bitcoind/config
	menu "Configuration"
	depends on PACKAGE_bitcoind
	source "$(SOURCE)/Config.in"
	endmenu
endef

TARGET_CFLAGS += -std=c11 -std=c++17
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib,-I$(STAGING_DIR)/usr/include

CONFIGURE_ARGS += --without-miniupnpc
CONFIGURE_ARGS += --disable-wallet
CONFIGURE_ARGS += --disable-tests
CONFIGURE_ARGS += --disable-iptv6
CONFIGURE_ARGS += --disable-ccache
CONFIGURE_ARGS += --enable-reduce-exports
CONFIGURE_ARGS += --with-boost-libdir=$(STAGING_DIR)/usr/lib

define Build/Compile
	$(call Build/Compile/Default)
endef

define Package/bitcoind/install

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/usr/root
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DIR) $(1)/etc/uci-defaults
	$(INSTALL_DIR) $(1)/root/.bitcoin

	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/bitcoind       $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/bitcoin-cli    $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/bitcoin-tx     $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/bitcoin-util   $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/bench_bitcoin  $(1)/usr/bin

ifeq ($(CONFIG_BITCOIND),y)
	$(CP)		$(FILES_DIR)/bitcoind-monitor			$(1)/usr/bin/bitcoind-monitor
	$(CP)		$(FILES_DIR)/bitcoind.init				$(1)/etc/init.d/bitcoind
	$(CP)		$(FILES_DIR)/bitcoind.config			$(1)/etc/config/bitcoind
	$(CP)		$(FILES_DIR)/fstab						$(1)/etc/config/fstab
	$(CP)		$(FILES_DIR)/bitcoind.uci-defaults		$(1)/etc/uci-defaults/01-bitcoind
	$(CP)		$(FILES_DIR)/bitcoin.conf				$(1)/root/.bitcoin/bitcoin.conf
endif

endef

$(eval $(call BuildPackage,bitcoind))
