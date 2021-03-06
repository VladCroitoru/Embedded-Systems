###############################################################################
# Makefile for the project lab4
###############################################################################

## General Flags
PROJECT = lab4
MCU = atmega32
TARGET = lab4.elf
CC = avr-gcc

CPP = avr-g++

## Options common to compile, link and assembly rules
COMMON = -mmcu=$(MCU)

## Compile options common for all C compilation units.
CFLAGS = $(COMMON)
CFLAGS += -Wall -gdwarf-2 -std=gnu99     -DF_CPU=8000000UL -Os -funsigned-char -funsigned-bitfields -fpack-struct -fshort-enums
CFLAGS += -MD -MP -MT $(*F).o -MF dep/$(@F).d 

## Assembly specific flags
ASMFLAGS = $(COMMON)
ASMFLAGS += $(CFLAGS)
ASMFLAGS += -x assembler-with-cpp -Wa,-gdwarf2

## Linker flags
LDFLAGS = $(COMMON)
LDFLAGS +=  -Wl,-Map=lab4.map


## Intel Hex file production flags
HEX_FLASH_FLAGS = -R .eeprom -R .fuse -R .lock -R .signature

HEX_EEPROM_FLAGS = -j .eeprom
HEX_EEPROM_FLAGS += --set-section-flags=.eeprom="alloc,load"
HEX_EEPROM_FLAGS += --change-section-lma .eeprom=0 --no-change-warnings


## Objects that must be built in order to link
OBJECTS = lab4.o car_2wd.o gpio.o hbridge.o motor.o pwm.o uart_stdio.o utils.o 

## Objects explicitly added by the user
LINKONLYOBJECTS = 

## Build
all: $(TARGET) lab4.hex lab4.eep lab4.lss size

## Compile
lab4.o: ../lab4.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

car_2wd.o: ../car_2wd.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

gpio.o: ../gpio.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

hbridge.o: ../hbridge.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

motor.o: ../motor.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

pwm.o: ../pwm.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

uart_stdio.o: ../uart_stdio.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

utils.o: ../utils.c
	$(CC) $(INCLUDES) $(CFLAGS) -c  $<

##Link
$(TARGET): $(OBJECTS)
	 $(CC) $(LDFLAGS) $(OBJECTS) $(LINKONLYOBJECTS) $(LIBDIRS) $(LIBS) -o $(TARGET)

%.hex: $(TARGET)
	avr-objcopy -O ihex $(HEX_FLASH_FLAGS)  $< $@

%.eep: $(TARGET)
	-avr-objcopy $(HEX_EEPROM_FLAGS) -O ihex $< $@ || exit 0

%.lss: $(TARGET)
	avr-objdump -h -S $< > $@

size: ${TARGET}
	@echo
	@avr-size -C --mcu=${MCU} ${TARGET}

## Clean target
.PHONY: clean
clean:
	-rm -rf $(OBJECTS) lab4.elf dep/* lab4.hex lab4.eep lab4.lss lab4.map


## Other dependencies
-include $(shell mkdir dep 2>NUL) $(wildcard dep/*)

