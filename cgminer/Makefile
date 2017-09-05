#
# Author: Xiangfu Liu <xiangfu@openmobilefree.net>
# Address: 12h6gdGnThW385JaX1LRMA8cXKmbYRTP8Q
#
# This is free and unencumbered software released into the public domain.
# For details see the UNLICENSE file at the root of the source tree.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=cgminer

#ifeq ($(CONFIG_CGMINER_AVALON4),y)
#	PKG_VERSION:=git-$(shell git ls-remote https://github.com/csa402/cgminer-gekko avalon4 | cut -f1 | cut -c1-7)
#	PKG_REV:=avalon4
#else
#	PKG_VERSION:=4.6.0
#	PKG_REV:=06b44fcc2ed64a3e0d8288287d28176132433642
#endif
ifeq ($(CONFIG_CGMINER_GEKKO),y)
	PKG_VERSION:=git-$(shell git ls-remote https://github.com/wareck/cgminer-gekko.git master | cut -f1 | cut -c1-7)
	PKG_REV:=f8b12c8f424ef11d65be425db243416e9d0ef1d3
else ifeq ($(CONFIG_CGMINER_ROCKMINER),y)
        PKG_VERSION:=git-$(shell git ls-remote https://github.com/wareck/cgminer-gekko.git master | cut -f1 | cut -c1-7)
        PKG_REV:=f8b12c8f424ef11d65be425db243416e9d0ef1d3
endif

PKG_RELEASE:=1
PKG_INSTALL:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_REV).tar.bz2
ifeq ($(CONFIG_CGMINER_AVALON4),y)
	PKG_SOURCE_URL:=git://github.com/wareck/cgminer-gekko.git
endif
ifeq ($(CONFIG_CGMINER_GEKKO),y)
	PKG_SOURCE_URL:=git://github.com/wareck/cgminer-gekko.git
endif
ifeq ($(CONFIG_CGMINER_X11),y)
        PKG_SOURCE_URL:=git://github.com/wareck/cgminer-gekko.git
endif
ifeq ($(CONFIG_CGMINER_ROCKMINER),y)
        PKG_SOURCE_URL:=git://github.com/wareck/cgminer-gekko.git
endif
ifeq ($(CONFIG_CGMINER_BITMAIN),y)
	PKG_SOURCE_URL:=git://github.com/csa402/cgminer-ants3.git
endif

PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/cgminer
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=cgminer
	URL:=https://github.com/ckolivas/cgminer
ifeq ($(CONFIG_TARGET_brcm2708_RaspberryPi),)
	DEPENDS:=+libcurl +libpthread +jansson +udev
else
	DEPENDS:=+libcurl +libpthread +jansson +udev +libncurses
endif
endef

define Package/cgminer/description
Cgminer is a multi-threaded multi-pool GPU, FPGA and CPU miner with ATI GPU
monitoring, (over)clocking and fanspeed support for bitcoin and derivative
coins. Do not use on multiple block chains at the same time!
endef

define Package/cgminer/config
	menu "Configuration"
	depends on PACKAGE_cgminer
	source "$(SOURCE)/Config.in"
	endmenu
endef

TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

ifeq ($(CONFIG_CGMINER_AVALON1),y)
	CONFIGURE_ARGS += --enable-avalon
endif
ifeq ($(CONFIG_CGMINER_AVALON2),y)
	CONFIGURE_ARGS += --enable-avalon2
endif
ifeq ($(CONFIG_CGMINER_AVALON4),y)
	CONFIGURE_ARGS += --enable-avalon4
endif
ifeq ($(CONFIG_CGMINER_GEKKO),y)
	CONFIGURE_ARGS += --enable-gekko --enable-bflsc --enable-bitfury
endif
ifeq ($(CONFIG_CGMINER_DRAGON),y)
        CONFIGURE_ARGS += --enable-zeus --enable-lketc --enable-scrypt --enable-gridseed
endif
ifeq ($(CONFIG_CGMINER_ROCKMINER),y)
        CONFIGURE_ARGS += --enable-icarus --disable-avalon4
endif
ifeq ($(CONFIG_CGMINER_GEKKO),y)
        CONFIGURE_ARGS += --enable-icarus --disable-avalon4
endif
ifeq ($(CONFIG_CGMINER_BITMAIN),y)
        CONFIGURE_ARGS += --without-curses --enable-ants3
endif

ifeq ($(CONFIG_TARGET_brcm2708_RaspberryPi),)
	CONFIGURE_ARGS += --without-curses
endif

define Build/Compile
	$(call Build/Compile/Default)
	(cd $(PKG_BUILD_DIR) && \
	 $(TARGET_CC) -Icompat/jansson-2.6/src -Icompat/libusb-1.0/libusb api-example.c -o cgminer-api;)
endef

define Package/cgminer/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cgminer-api	   $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/cgminer  $(1)/usr/bin

ifeq ($(CONFIG_CGMINER_AVALON1),y)
	$(INSTALL_BIN) $(FILES_DIR)/cgminer-avalon1-monitor        $(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN) $(FILES_DIR)/cgminer.avalon1.init           $(1)/etc/init.d/cgminer
	$(CP)          $(FILES_DIR)/cgminer.avalon1.config         $(1)/etc/config/cgminer
endif
ifeq ($(CONFIG_CGMINER_AVALON2),y)
	$(INSTALL_BIN) $(FILES_DIR)/cgminer-avalon2-monitor        $(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN) $(FILES_DIR)/cgminer.avalon2.init           $(1)/etc/init.d/cgminer
	$(CP)          $(FILES_DIR)/cgminer.avalon2.config         $(1)/etc/config/cgminer
endif
ifeq ($(CONFIG_CGMINER_AVALON4),y)
	$(INSTALL_BIN) $(FILES_DIR)/cgminer-avalon4-monitor        $(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN) $(FILES_DIR)/cgminer.avalon4.init           $(1)/etc/init.d/cgminer
	$(CP)          $(FILES_DIR)/cgminer.avalon4.config         $(1)/etc/config/cgminer
	$(INSTALL_BIN) $(FILES_DIR)/mm-avalon4-upgrade             $(1)/usr/bin/mmupgrade
endif
ifeq ($(CONFIG_CGMINER_GEKKO),y)
	$(INSTALL_BIN) $(FILES_DIR)/cgminer-gekko-monitor          $(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN) $(FILES_DIR)/cgminer.gekko.init             $(1)/etc/init.d/cgminer
	$(CP)          $(FILES_DIR)/cgminer.gekko.config           $(1)/etc/config/cgminer
endif
ifeq ($(CONFIG_CGMINER_ROCKMINER),y)
	$(INSTALL_BIN) $(FILES_DIR)/cgminer-rockminer-monitor      $(1)/usr/bin/cgminer-monitor
	$(INSTALL_BIN) $(FILES_DIR)/cgminer.rockminer.init         $(1)/etc/init.d/cgminer
	$(CP)          $(FILES_DIR)/cgminer.rockminer.config       $(1)/etc/config/cgminer
endif
ifeq ($(CONFIG_CGMINER_X11),y)
        $(INSTALL_BIN) $(FILES_DIR)/cgminer-x11-monitor      $(1)/usr/bin/cgminer-monitor
        $(INSTALL_BIN) $(FILES_DIR)/cgminer.x11.init         $(1)/etc/init.d/cgminer
        $(CP)          $(FILES_DIR)/cgminer.x11.config       $(1)/etc/config/cgminer
endif
ifeq ($(CONFIG_CGMINER_BITMAIN),y)
        $(INSTALL_BIN) $(FILES_DIR)/cgminer-bitmain-monitor      $(1)/usr/bin/cgminer-monitor
        $(INSTALL_BIN) $(FILES_DIR)/cgminer.bitmain.init         $(1)/etc/init.d/cgminer
        $(CP)          $(FILES_DIR)/cgminer.bitmain.config       $(1)/etc/config/cgminer
endif

endef

$(eval $(call BuildPackage,cgminer))
