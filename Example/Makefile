
###########################################################
# Things you probably want to change:
# The FreeRTOS source path
FREERTOS=../
# Where Xen headers are installed
XEN_PREFIX=/usr
# The cross compiler name prefix
CROSS_COMPILE=arm-linux-gnueabi-
###########################################################

FREERTOS_LIB=$(FREERTOS)/Demo/CORTEX_A15_Xen_GCC/FreeRTOS.a
PORT=$(FREERTOS)/Source/portable/GCC/ARM7_CA15_Xen
XEN_INCLUDE=$(XEN_PREFIX)/include
CC=$(CROSS_COMPILE)gcc
OBJCOPY=$(CROSS_COMPILE)objcopy

DEBUG=
LDSCRIPT=$(FREERTOS)/Demo/CORTEX_A15_Xen_GCC/linker.lds
LDFLAGS=-nostartfiles -Xlinker 
OPTIMIZE=-O3
ELFFLAGS=-static -T $(LDSCRIPT)

CFLAGS= $(DEBUG) $(OPTIMIZE) \
	-Wall \
	-Werror \
	-Wextra \
        -Wpedantic \
        -Wno-unused-parameter \
        -Wno-builtin-declaration-mismatch \
        -Wno-variadic-macros \
        -Wno-type-limits \
        -Wno-sign-compare \
        -Wno-pedantic \
        -Wno-error \
	-I $(FREERTOS)/Source/include \
	-I $(FREERTOS)/Demo/CORTEX_A15_Xen_GCC/include \
	-I $(FREERTOS)/Demo/CORTEX_A15_Xen_GCC/platform/include \
	-I $(FREERTOS)/Demo/CORTEX_A15_Xen_GCC/platform/minlibc/include \
        -I $(FREERTOS)/Demo/Common/include \
	-I $(PORT)/include \
	-I $(XEN_INCLUDE) \
	-D XEN_HAVE_PV_GUEST_ENTRY=1 \
	-D CA15_XEN_GCC \
	-mcpu=cortex-a15 \
	-mfpu=vfpv4 \
	-fomit-frame-pointer \
	-fno-strict-aliasing \
	-nostdlib

.SECONDARY: Example.elf

all: Example.bin

%.bin: %.elf
	$(OBJCOPY) $*.elf -O binary $*.bin

%.elf: %.o $(FREERTOS_LIB) Makefile
	$(CC) $(CFLAGS) $*.o $(FREERTOS_LIB) ./mylib.a $(LDFLAGS) -o$*.elf $(ELFFLAGS)

Example.o: %.o : %.c Makefile
	$(CC) -c $(CFLAGS) $*.c -o $*.o

clean:
	rm -f *.elf *.bin *.o
