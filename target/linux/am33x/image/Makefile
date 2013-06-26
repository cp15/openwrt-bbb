#
# Copyright (C) 2013 Rodolfo Giometti <giometti@linux.it>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk
include $(INCLUDE_DIR)/host.mk

TARGET_DTBS := am335x-boneblack
JFFS2_BLOCKSIZE=128k
JFFS2OPTS += --little-endian --pagesize=0x800 --no-cleanmarkers --pad

FAT32_SIZE=65536

define Image/Build/DTB
	cp $(LINUX_DIR)/arch/$(ARCH)/boot/dts/$(1).dtb $(BIN_DIR)/$(IMG_PREFIX)-$(1).dtb
	#FIXME
	cp $(LINUX_DIR)/arch/$(ARCH)/boot/dts/$(1).dtb $(BUILD_DIR)/$(TARGET_DTBS).dtb
endef

define Image/Prepare
	cp $(LINUX_DIR)/arch/arm/boot/uImage $(KDIR)/uImage
endef

define Image/BuildKernel
	$(CP) $(LINUX_DIR)/arch/arm/boot/zImage $(BIN_DIR)/$(IMG_PREFIX)-zImage
	chmod 0644 $(BIN_DIR)/$(IMG_PREFIX)-zImage
	$(foreach dtb,$(TARGET_DTBS),$(call Image/Build/DTB,$(dtb)))
endef

#define Image/Build/squashfs
#	$(call prepare_generic_squashfs,$(BIN_DIR)/$(IMG_PREFIX)-root.$(1))
#endef

define Image/Build
	$(CP) $(KDIR)/root.$(1) $(BIN_DIR)/$(IMG_PREFIX)-root.$(1)
	$(call Image/Build/$(1),$(1))

	rm -f $(KDIR)/boot.img

	mkdosfs $(KDIR)/boot.img -C $(FAT32_SIZE)
	mcopy -i $(KDIR)/boot.img $(BUILD_DIR)/$(TARGET_DTBS).dtb ::
	mcopy -i $(KDIR)/boot.img $(LINUX_DIR)/arch/arm/boot/zImage ::
	#FIXME
	mcopy -i $(KDIR)/boot.img MLO ::
	mcopy -i $(KDIR)/boot.img u-boot.img ::
	mcopy -i $(KDIR)/boot.img uEnv.txt ::
	./gen_rpi_sdcard_img.sh $(BIN_DIR)/$(IMG_PREFIX)-sdcard-vfat-$(1).img $(KDIR)/boot.img $(KDIR)/root.$(1) 64 $(CONFIG_TARGET_ROOTFS_PARTSIZE)
endef

$(eval $(call BuildImage))
