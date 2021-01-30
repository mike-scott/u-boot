# SPDX-License-Identifier: GPL-2.0+
#
# Copyright 2017 NXP

obj-y += lowlevel_init.o
obj-y += clock_slice.o soc.o
obj-$(CONFIG_IMX8MQ) += clock_imx8mq.o
obj-$(CONFIG_IMX8MM)$(CONFIG_IMX8MN)$(CONFIG_IMX8MP) += clock_imx8mm.o
ifdef CONFIG_SPL_BUILD
ifeq ($(CONFIG_SPL_WITH_DDR_FIRMWARE), y)
obj-y += piggy_spl-ddr-firmware.o

$(obj)/piggy_spl-ddr-firmware.o: $(src)/piggy_spl-ddr-firmware.S
	$(Q)mkdir -p $(dir $@)
ifeq ($(CONFIG_SPL_WITH_DDR_FIRMWARE_LPDDR_FLAG), y)
	$(OBJCOPY) -I binary -O binary --pad-to 0x8000 --gap-fill=0x0 \
		lpddr4_pmu_train_1d_imem.bin 1d_imem_pad.bin
	$(OBJCOPY) -I binary -O binary --pad-to 0x4000 --gap-fill=0x0 \
		lpddr4_pmu_train_1d_dmem.bin 1d_dmem_pad.bin
	$(OBJCOPY) -I binary -O binary --pad-to 0x8000 --gap-fill=0x0 \
		lpddr4_pmu_train_2d_imem.bin 2d_imem_pad.bin
	$(OBJCOPY) -I binary -O binary --pad-to 0x4000 --gap-fill=0x0 \
		lpddr4_pmu_train_2d_dmem.bin 2d_dmem_pad.bin
else
	$(OBJCOPY) -I binary -O binary --pad-to 0x8000 --gap-fill=0x0 \
		ddr4_imem_1d.bin 1d_imem_pad.bin
	$(OBJCOPY) -I binary -O binary --pad-to 0x4000 --gap-fill=0x0 \
		ddr4_dmem_1d.bin 1d_dmem_pad.bin
	$(OBJCOPY) -I binary -O binary --pad-to 0x8000 --gap-fill=0x0 \
		ddr4_imem_2d.bin 2d_imem_pad.bin
	$(OBJCOPY) -I binary -O binary --pad-to 0x4000 --gap-fill=0x0 \
		ddr4_dmem_2d.bin 2d_dmem_pad.bin
endif # CONFIG_SPL_WITH_DDR_FIRMWARE_LPDDR_FLAG
	@cat 1d_imem_pad.bin 1d_dmem_pad.bin 2d_imem_pad.bin 2d_dmem_pad.bin > spl-ddr-firmware.o
	@rm 1d_imem_pad.bin 1d_dmem_pad.bin 2d_imem_pad.bin 2d_dmem_pad.bin
	$(call if_changed_dep,as_o_S)

endif # CONFIG_SPL_WITH_DDR_FIRMWARE
endif # CONFIG_SPL_BUILD
