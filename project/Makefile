TARGET := SSL_CtrlBrd
BUILD_DIR := build

TOOLCHAIN_DIR ?= /Applications/ArmGNUToolchain/15.2.rel1/arm-none-eabi/bin
PREFIX := $(TOOLCHAIN_DIR)/arm-none-eabi-
CC := $(PREFIX)gcc
AS := $(PREFIX)gcc
OBJCOPY := $(PREFIX)objcopy
SIZE := $(PREFIX)size

CPU_FLAGS := -mcpu=cortex-m4 -mthumb -mfpu=fpv4-sp-d16 -mfloat-abi=hard
DEFS := -DARM_MATH_CM4 -D__FPU_PRESENT -DUSE_USB_OTG_FS

INCLUDES := \
  -I../inc \
  -I../inc/usb_user \
  -I../lib/CMSIS/inc \
  -I../lib/inc \
  -I../lib/usb_device_lib/Class/cdc/inc \
  -I../lib/usb_device_lib/Core/inc \
  -I../lib/usb_host_lib/Class/HID/inc \
  -I../lib/usb_host_lib/Core/inc \
  -I../lib/usb_otg_driver/inc

C_SOURCES := \
  $(wildcard ../src/*.c) \
  $(wildcard ../src/usb_user/*.c) \
  $(wildcard ../lib/src/*.c) \
  $(wildcard ../lib/usb_host_lib/Core/src/*.c) \
  $(wildcard ../lib/usb_host_lib/Class/HID/src/*.c) \
  $(wildcard ../lib/usb_device_lib/Core/src/*.c) \
  $(wildcard ../lib/usb_device_lib/Class/cdc/src/*.c) \
  $(wildcard ../lib/usb_otg_driver/src/*.c)

ASM_SOURCES := gcc/startup_stm32f407ve.s

OBJECTS := $(patsubst ../%.c,$(BUILD_DIR)/%.o,$(C_SOURCES))
OBJECTS += $(patsubst %.s,$(BUILD_DIR)/%.o,$(ASM_SOURCES))

CFLAGS := $(CPU_FLAGS) $(DEFS) $(INCLUDES) -Og -g3 -Wall -ffunction-sections -fdata-sections -std=c11
ASFLAGS := $(CPU_FLAGS) -g3 -x assembler-with-cpp
LDFLAGS := $(CPU_FLAGS) -Tgcc/STM32F407VE.ld -Wl,-Map=$(BUILD_DIR)/$(TARGET).map -Wl,--gc-sections --specs=nano.specs --specs=nosys.specs
LIBS := -lm

.PHONY: all clean

all: $(BUILD_DIR)/$(TARGET).elf $(BUILD_DIR)/$(TARGET).hex $(BUILD_DIR)/$(TARGET).bin
	$(SIZE) $(BUILD_DIR)/$(TARGET).elf

$(BUILD_DIR)/$(TARGET).elf: $(OBJECTS) gcc/STM32F407VE.ld
	$(CC) $(OBJECTS) $(LDFLAGS) $(LIBS) -o $@

$(BUILD_DIR)/%.hex: $(BUILD_DIR)/%.elf
	$(OBJCOPY) -O ihex $< $@

$(BUILD_DIR)/%.bin: $(BUILD_DIR)/%.elf
	$(OBJCOPY) -O binary $< $@

$(BUILD_DIR)/%.o: ../%.c
	mkdir -p $(dir $@)
	$(CC) $(CFLAGS) -c $< -o $@

$(BUILD_DIR)/gcc/%.o: gcc/%.s
	mkdir -p $(dir $@)
	$(AS) $(ASFLAGS) -c $< -o $@

clean:
	rm -rf $(BUILD_DIR)
