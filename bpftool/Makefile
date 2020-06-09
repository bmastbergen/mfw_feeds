#
# Copyright (C) 2011-2013 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=bpftool
PKG_VERSION:=$(LINUX_VERSION)
PKG_RELEASE:=1

PKG_BUILD_PARALLEL:=1
PKG_MAINTAINER:=Felix Fietkau <nbd@nbd.name>
PKG_FLAGS:=nonshared

# bpftool's makefile and headers are not relocatable and must be built from the
# Linux sources directory
PKG_BUILD_DIR:=$(LINUX_DIR)/tools/bpf-$(TARGET_DIR_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/bpftool
  SECTION:=devel
  CATEGORY:=Development
  DEPENDS:= +libelf1 +libbfd +USE_MUSL:musl-fts +libopcodes
  TITLE:=Linux bpf monitoring tool
  VERSION:=$(LINUX_VERSION)-$(PKG_RELEASE)
  URL:=http://www.kernel.org
endef

define Package/bpftool/description
  bpftool is the Linux bpf monitoring tool
endef

define Build/Prepare
	$(CP) $(LINUX_DIR)/tools/bpf/* $(PKG_BUILD_DIR)/
endef

MAKE_FLAGS = \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	CC="$(TARGET_CC)" \
	LD="$(TARGET_CROSS)ld" \
	CFLAGS="$(TARGET_CFLAGS) $(TARGET_CPPFLAGS)" \
	LDFLAGS="$(TARGET_LDFLAGS)" \
	$(if $(findstring c,$(OPENWRT_VERBOSE)),V=1,V='') \
	WERROR=0 \
	prefix=/usr


define Build/Compile
	+$(MAKE_FLAGS) $(MAKE) $(PKG_JOBS) \
		-C $(PKG_BUILD_DIR) \
		bpftool
endef

define Package/bpftool/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/bpftool/bpftool $(1)/usr/bin/
endef

$(eval $(call BuildPackage,bpftool))
