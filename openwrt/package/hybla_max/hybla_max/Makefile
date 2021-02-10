include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=hybla_max
PKG_RELEASE:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/hybla_max
	SUBMENU:=Network Support
	TITLE:=Modified hybla tcp congestion control
	DEPENDS:=@!LINUX_3_18
	FILES:=$(PKG_BUILD_DIR)/tcp_hybla_max.ko
	KCONFIG:=
endef

define KernelPackage/hybla_max/description
	Kernel module of modified HYBLA tcp congestion control
endef

EXTRA_KCONFIG:= \
    

EXTRA_CFLAGS:= \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=m,%,$(filter %=m,$(EXTRA_KCONFIG)))) \
	$(patsubst CONFIG_%, -DCONFIG_%=1, $(patsubst %=y,%,$(filter %=y,$(EXTRA_KCONFIG)))) \

MAKE_OPTS:= \
	ARCH="$(LINUX_KARCH)" \
	CROSS_COMPILE="$(TARGET_CROSS)" \
	SUBDIRS="$(PKG_BUILD_DIR)" \
	EXTRA_CFLAGS="$(EXTRA_CFLAGS)" \
	$(EXTRA_KCONFIG)

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./src/* $(PKG_BUILD_DIR)/
endef

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" \
	$(MAKE_OPTS) \
	modules
endef

$(eval $(call KernelPackage,hybla_max))
