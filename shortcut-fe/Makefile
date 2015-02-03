#
# Copyright (c) 2014 Qualcomm Atheros, Inc.
#
# All Rights Reserved.
# Qualcomm Atheros Confidential and Proprietary.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=shortcut-fe
PKG_SOURCE_PROTO:=git
PKG_BRANCH:=master
PKG_RELEASE:=1

include $(INCLUDE_DIR)/local-development.mk
ifeq ($(DUMP)$(PKG_VERSION),)
  PKG_REV:=$(shell git ls-remote $(PKG_SOURCE_URL) $(PKG_BRANCH) | cut -b -7)
  PKG_VERSION:=g$(PKG_REV)
endif

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

include $(INCLUDE_DIR)/package.mk

define KernelPackage/shortcut-fe
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Support
  DEPENDS:=
  TITLE:=Kernel driver for SFE
  FILES:=$(PKG_BUILD_DIR)/shortcut-fe/shortcut-fe.ko
  KCONFIG:=CONFIG_NF_CONNTRACK_EVENTS=y
  AUTOLOAD:=$(call AutoLoad,09,shortcut-fe)
endef

define KernelPackage/shortcut-fe/Description
Shortcut is an in-Linux-kernel IP packet forwarding engine.
endef

define KernelPackage/shortcut-fe/install
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/etc/init.d/shortcut-fe $(1)/etc/init.d
endef

define KernelPackage/shortcut-fe-cm
  SECTION:=kernel
  CATEGORY:=Kernel modules
  SUBMENU:=Network Support
  DEPENDS:=+kmod-ipt-conntrack +kmod-shortcut-fe
  TITLE:=Kernel driver for SFE
  FILES:=$(PKG_BUILD_DIR)/shortcut-fe/shortcut-fe-cm.ko
  KCONFIG:=CONFIG_NF_CONNTRACK_CHAIN_EVENTS=y
endef

define KernelPackage/shortcut-fe-cm/Description
Simple connection manager for the Shortcut forwarding engine.
endef

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		SUBDIRS="$(PKG_BUILD_DIR)/shortcut-fe" \
		EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
		modules
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include/shortcut-fe
	$(CP) -rf $(PKG_BUILD_DIR)/shortcut-fe/sfe.h $(1)/usr/include/shortcut-fe
endef

$(eval $(call KernelPackage,shortcut-fe))
$(eval $(call KernelPackage,shortcut-fe-cm))
