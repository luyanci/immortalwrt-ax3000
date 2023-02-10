include $(TOPDIR)/rules.mk

PKG_NAME:=qca-nss-fw
PKG_VERSION:=11.4.0.5
# PKG_RELEASE:=6
PKG_RELEASE:=5

include $(INCLUDE_DIR)/package.mk

define generate-qca-nss-fw

  define Package/$(PKG_NAME)-$(1)-$(3)
    SECTION:=firmware
    CATEGORY:=Firmware
    TITLE:=NSS firmware binaries for $(1) - $(3)
    DEPENDS:=@TARGET_$(1)
    CONFLICTS:=$($(PKG_NAME)_PREV_$(1))
  endef

  define Package/$(PKG_NAME)-$(1)-$(3)/description
    NSS firmware binaries for $(1) - $(3)
  endef

  define Download/$(PKG_NAME)-$(1)-$(3)
    URL:=http://qcawebsrvr.qualcomm.com/NSS/NSS.FW/$(PKG_VERSION)/$(2)/$(PKG_RELEASE)/
    URL_FILE:=BIN-NSS.$(2).$(PKG_VERSION)-$(PKG_RELEASE)-$(4).tar.bz2
    FILE:=BIN-NSS.$(2).$(PKG_VERSION)-$(PKG_RELEASE)-$(4).tar.bz2
    HASH:=$(6)
  endef
  $$(if $$(CONFIG_PACKAGE_$(PKG_NAME)-$(1)-$(3)), \
    $$(eval $$(call Download,$(PKG_NAME)-$(1)-$(3))) \
  )

  define Prepare/$(PKG_NAME)-$(1)-$(3)
	bzcat $$(DL_DIR)/BIN-NSS.$(2).$(PKG_VERSION)-$(PKG_RELEASE)-$(4).tar.bz2 \
		| $$(HOST_TAR) -C $$(PKG_BUILD_DIR) $$(TAR_OPTIONS)
  endef

  define Package/$(PKG_NAME)-$(1)-$(3)/install
	$$(INSTALL_DIR) $$(1)/lib/firmware/
	$$(INSTALL_DATA) \
		$$(PKG_BUILD_DIR)/BIN-NSS.$(2).$(PKG_VERSION)-$(PKG_RELEASE)-$(4)/$(5)0.bin \
		$$(1)/lib/firmware/qca-nss0-$(3).bin
	-$$(INSTALL_DATA) \
		$$(PKG_BUILD_DIR)/BIN-NSS.$(2).$(PKG_VERSION)-$(PKG_RELEASE)-$(4)/$(5)1.bin \
		$$(1)/lib/firmware/qca-nss1-$(3).bin
  endef

  $(PKG_NAME)_PREV_$(_SOC)+=$(PKG_NAME)-$(1)-$(3)
  ALL_PACKAGE+=$(PKG_NAME)-$(1)-$(3)

endef

# $(eval $(call generate-qca-nss-fw,ipq50xx,MP,enterprise,E,enterprise_ap,))
$(eval $(call generate-qca-nss-fw,ipq50xx,MP,retail,R,retail_router,268ccde195dcdcbf9f6d9ae3af015f14d99db1badd4f779d4f7f6bd1959f961d))
# $(eval $(call generate-qca-nss-fw,ipq60xx,CP,enterprise,E,enterprise_ap,))
$(eval $(call generate-qca-nss-fw,ipq60xx,CP,retail,R,retail_router,b4a5ad069ea2530050d23b6de4833b1f343fe21c00b0472704590afc38eb723f))
# $(eval $(call generate-qca-nss-fw,ipq806x,AK,enterprise,E,enterprise_ap,))
# $(eval $(call generate-qca-nss-fw,ipq806x,AK,retail,R,retail_router,))
# $(eval $(call generate-qca-nss-fw,ipq807x,HK,enterprise,E,enterprise_ap,))
$(eval $(call generate-qca-nss-fw,ipq807x,HK,retail,R,retail_router,791bf9d4718e98bc7a8ff1af3e72e0eae7d269438e22e316fcfe119cce9281da))

define Build/Prepare
	$(foreach pkg, $(ALL_PACKAGE),
		$(if $(CONFIG_PACKAGE_$(pkg)),
			$(call Prepare/$(pkg))
		)
	)
endef

define Build/Compile
endef

$(foreach pkg, $(ALL_PACKAGE), \
	$(eval $(call BuildPackage,$(pkg))) \
)
