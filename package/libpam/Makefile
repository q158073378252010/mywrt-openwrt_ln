#
# Copyright (C) 2006-2009 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libpam
PKG_VERSION:=1.1.8
PKG_RELEASE:=3

PKG_SOURCE:=Linux-PAM-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=http://www.linux-pam.org/library/
PKG_MD5SUM:=35b6091af95981b1b2cd60d813b5e4ee
PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

PKG_BUILD_DIR:=$(BUILD_DIR)/Linux-PAM-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/libpam/Default
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=free implementation of the following DCE-RFC.
  URL:=http://www.kernel.org/pub/linux/libs/pam
endef

define Package/libpam
$(call Package/libpam/Default)
  VARIANT:=nodb
  TITLE+= (no DB support)
endef

define Package/libpam-db
$(call Package/libpam/Default)
  VARIANT:=db
  TITLE+= (DB support)
  DEPENDS:=+libdb47
endef

define Package/libpam/description
	Linux-PAM is a free implementation of the following DCE-RFC from Sunsoft.
endef

Package/libpam-db/description=$(Package/libpam/description)

TARGET_CFLAGS += $(FPIC)

ifeq ($(BUILD_VARIANT),nodb)
 define Build/Configure
	$(call Build/Configure/Default, \
		--enable-shared \
		--enable-static \
		--enable-pamlocking \
		--disable-prelude \
		--disable-lckpwdf \
		--disable-selinux \
		--disable-nls \
		--disable-rpath \
		--enable-db=no \
	)
 endef
endif

ifeq ($(BUILD_VARIANT),db)
 define Build/Configure
	$(call Build/Configure/Default, \
		--enable-shared \
		--enable-static \
		--enable-pamlocking \
		--disable-prelude \
		--disable-lckpwdf \
		--disable-selinux \
		--disable-nls \
		--disable-rpath \
	)
 endef
endif

define Build/InstallDev
	$(INSTALL_DIR) $(1)/lib
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/lib/* $(1)/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/* $(1)/usr/include
endef

define Package/libpam/install
	$(INSTALL_DIR) $(1)/lib $(1)/lib/security $(1)/lib/security/pam_filter
	$(INSTALL_DIR) $(1)/etc $(1)/etc/pam.d
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/lib/*.so* $(1)/lib/
	$(CP) $(PKG_INSTALL_DIR)/lib/security/*.so* $(1)/lib/security/
	$(CP) $(PKG_INSTALL_DIR)/lib/security/pam_filter/* $(1)/lib/security/pam_filter/
	$(CP) $(PKG_INSTALL_DIR)/etc/* $(1)/etc/
	$(CP) ./files/* $(1)/etc/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/* $(1)/usr/sbin/
endef

Package/libpam-db/install=$(Package/libpam/install)

$(eval $(call BuildPackage,libpam))
$(eval $(call BuildPackage,libpam-db))
