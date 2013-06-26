																																																																																																																																																																																																																																																																																																																																																																																																																																																																																																												#
# Copyright (C) 2013 Artur Wronowski <arteqw@gmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

ARCH:=arm
BOARD:=am33x
BOARDNAME:=TI AM33x
SUBTARGETS:=boneblack
MAINTAINER:=Artur Wronowski <arteqw@gmail.com>
FEATURES:=targz usb usbgadget display gpio audio
LINUX_VERSION:=3.8.13

DEVICE_TYPE=developerboard

define Target/Description
    TI AM335x
endef

# --with-float=soft
CFLAGS:=-O2 -pipe -march=armv7-a -mfpu=vfpv3-d16 -mfloat-abi=hard  \
	-mfpu=neon -mcpu=cortex-a8 -mtune=cortex-a8

KERNELNAME:="zImage dtbs"
DEFAULT_PACKAGES +=

include $(INCLUDE_DIR)/target.mk
$(eval $(call BuildTarget))
