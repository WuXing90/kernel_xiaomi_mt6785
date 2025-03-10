#
# Copyright (C) 2015 MediaTek Inc.
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License version 2 as
# published by the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#

###############################################################################
# Necessary Check
# Force build fail on modpost warning
KBUILD_MODPOST_FAIL_ON_WARNINGS := y

# for fmradio options
         MTK_PLATFORM_ID := $(patsubst CONSYS_%,%,$(subst ",,$(CONFIG_MTK_COMBO_CHIP)))
         ifneq (,$(filter $(CONFIG_MTK_COMBO_CHIP), "CONSYS_6885" "CONSYS_6893"))
             export CFG_BUILD_CONNAC2=true
         else
             export CFG_BUILD_CONNAC2=false
         endif
         FM_6631_CHIPS := 6758 6759 6771 6775 6765 6761 3967 6797 6768 6785 8168
         FM_6635_CHIPS := 6779 6885 6873 6893
         ifneq ($(filter $(FM_6631_CHIPS), $(MTK_PLATFORM_ID)),)
             FM_CHIP := mt6631
         else ifneq ($(filter $(FM_6635_CHIPS), $(MTK_PLATFORM_ID)),)
             FM_CHIP := mt6635
         endif
         export CFG_FM_CHIP_ID=$(MTK_PLATFORM_ID)
         export CFG_FM_CHIP=$(FM_CHIP)

###############################################################################

MTK_PLATFORM := $(subst ",,$(CONFIG_MTK_PLATFORM))

# only WMT align this design flow
#ccflags-y += -D MTK_WCN_REMOVE_KERNEL_MODULE

ifeq ($(CONFIG_ARM64), y)
    ccflags-y += -D CONFIG_MTK_WCN_ARM64
endif

ifeq ($(CONFIG_MTK_CONN_LTE_IDC_SUPPORT),y)
    ccflags-y += -D WMT_IDC_SUPPORT=1
else
    ccflags-y += -D WMT_IDC_SUPPORT=0
endif
ccflags-y += -D MTK_WCN_WMT_STP_EXP_SYMBOL_ABSTRACT

ccflags-y += -I$(srctree)/drivers/misc/mediatek/include
ccflags-y += -I$(srctree)/drivers/misc/mediatek/include/mt-plat
ccflags-y += -I$(srctree)/drivers/misc/mediatek/include/mt-plat/$(MTK_PLATFORM)/include

ifeq ($(CONFIG_FM_USER_LOAD),1)
    ccflags-y += -D CONFIG_MTK_USER_BUILD
endif

# Makefile generated by Mediatek
# fm support

ifneq ($(CONFIG_MTK_FMRADIO),)

MODULE_NAME := fmradio_drv
obj-y += $(MODULE_NAME).o

ifeq ($(CFG_FM_CHIP),)
    FM_CHIP := mt6631_6635

    ccflags-y += -I$(src)/chips/mt6631/inc -I$(src)/chips/mt6635/inc

    $(FM_CHIP)-objs += chips/mt6631/pub/mt6631_fm_rds.o
    $(FM_CHIP)-objs += chips/mt6635/pub/mt6635_fm_rds.o
    $(FM_CHIP)-objs += chips/mt6631/pub/mt6631_fm_lib.o
    $(FM_CHIP)-objs += chips/mt6635/pub/mt6635_fm_lib.o

else

ifeq ($(CONFIG_MTK_FM_CHIP),)
    $(error CONFIG_MTK_FM_CHIP not defined)
endif

ifneq ($(CFG_FM_CHIP_ID),)
    ccflags-y += -D CFG_FM_CHIP_ID=0x$(CFG_FM_CHIP_ID)
endif

    FM_CHIP_MACRO := $(subst ",,$(CONFIG_MTK_FM_CHIP))
    FM_CHIP := $(subst _FM,,$(subst MT,mt,$(subst ",,$(CONFIG_MTK_FM_CHIP))))

ifeq ($(FM_CHIP), mt6625)
    FM_CHIP := mt6627
    ccflags-y += -DMT6625_FM
endif

ifeq ($(FM_CHIP), mt6627)
    FM_CHIP := mt6627
    ccflags-y += -DMT6627_FM
endif

ifeq ($(FM_CHIP), mt6580)
    FM_CHIP := soc
    ccflags-y += -Dsoc
endif

ifeq ($(FM_CHIP), mt0633)
    FM_CHIP := soc
    ccflags-y += -Dsoc
endif

ifeq ($(FM_CHIP), mt6630)
    ccflags-y += -DMT6630_FM
endif

ifeq ($(FM_CHIP), mt6632)
    ccflags-y += -DMT6632_FM
endif

ifeq ($(FM_CHIP), mt6631)
    ccflags-y += -DMT6631_FM
endif

ifeq ($(FM_CHIP), mt6635)
    ccflags-y += -DMT6635_FM
endif

    FM_CHIP_PATH := $(FM_CHIP)/pub/$(FM_CHIP)
    ccflags-y += -I$(src)/chips/$(FM_CHIP)/inc

    $(FM_CHIP)-objs += chips/$(FM_CHIP_PATH)_fm_rds.o
ifneq ($(CFG_BUILD_CONNAC2), true)
    $(FM_CHIP)-objs += chips/$(FM_CHIP_PATH)_fm_lib.o
endif
endif


ccflags-y += -I$(src)/inc \
             -I$(src)/plat/inc

$(FM_CHIP)-objs    += core/fm_module.o \
                core/fm_main.o \
                core/fm_config.o \
                core/fm_rds_parser.o \
                core/fm_patch.o \
                core/fm_utils.o \
                core/fm_link.o \
                core/fm_eint.o \
                core/fm_cmd.o \
                core/fm_reg_utils.o

ifeq ($(CFG_BUILD_CONNAC2), true)
    CONNINFRA_SRC_FOLDER := $(srctree)/drivers/misc/mediatek/connectivity/conninfra

    ccflags-y += -I$(CONNINFRA_SRC_FOLDER)/include
    ccflags-y += -DCFG_FM_CONNAC2=1
    $(FM_CHIP)-objs += chips/mt6635/pub/mt6635_2_fm_lib.o
    $(FM_CHIP)-objs += plat/conn_infra.o
else
    WMT_SRC_FOLDER := $(srctree)/drivers/misc/mediatek/connectivity/common
    WMT_INCLUDE_PATH := common_main

    ccflags-y += -I$(WMT_SRC_FOLDER)/$(WMT_INCLUDE_PATH)/include \
                 -I$(WMT_SRC_FOLDER)/$(WMT_INCLUDE_PATH)/linux/include
    ccflags-y += -DCFG_FM_CONNAC2=0
    $(FM_CHIP)-objs += plat/legacy_wmt.o
endif

    $(MODULE_NAME)-objs += $($(FM_CHIP)-objs)
    #obj-$(CONFIG_MTK_FMRADIO) += private/
endif

obj-y += dummy.o
