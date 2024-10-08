
## Copyright (C) 2014-2015 Galois, Inc.
##
## This program is free software; you can redistribute it and/or modify it
## under the terms of the GNU General Public License** as published by the Free
## Software Foundation; either version 2 of the License, or (at your option)
## any later version.
##
## This program is distributed in the hope that it will be useful, but WITHOUT
## ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
## FITNESS FOR A PARTICULAR PURPOSE.  See the GNU General Public License for
## more details.

## You should have received a copy of the GNU General Public License along with
## this program; if not, write to the Free Software Foundation, Inc., 59 Temple
## Place, Suite 330, Boston, MA  02111-1307  USA

# Variables you almost certainly want to override:
CROSS_COMPILE=arm-linux-gnueabi-
XEN_PREFIX=/usr

RTOS_SOURCE=../../Source
FDT_SOURCE=../../dtc
DEMO_COMMON=../Common/Minimal
DEMO_INCLUDE=../Common/include
PORT=$(RTOS_SOURCE)/portable/GCC/ARM7_CA15_Xen
DEBUG=
OPTIMIZE=

#CC=$(CROSS_COMPILE)gcc
#AR=$(CROSS_COMPILE)ar

CC=arm-linux-gnueabi-gcc
AR=arm-linux-gnueabi-ar

CFLAGS= $(DEBUG) $(OPTIMIZE) \
	-Wall \
	-Wextra \
        -Werror \
        -Wpedantic \
        -Wno-unused-parameter \
        -Wno-builtin-declaration-mismatch \
        -Wno-variadic-macros \
        -Wno-type-limits \
        -Wno-sign-compare \
        -Wno-pedantic \
        -Wno-error \
	-I ./include \
	-I ./platform/include \
	-I platform/minlibc/include/ \
	-I $(RTOS_SOURCE)/include \
	-I $(DEMO_INCLUDE) \
	-I $(PORT)/include \
	-I $(XEN_PREFIX)/include \
	-I $(FDT_SOURCE)/libfdt \
	-D XEN_HAVE_PV_GUEST_ENTRY=1 \
	-D CA15_XEN_GCC \
	-mcpu=cortex-a15 \
	-mfpu=vfpv4 \
	-fomit-frame-pointer \
	-fno-strict-aliasing \
	-nostdlib

CSOURCES= \
	assert.c \
	hooks.c \
	$(PORT)/tick.c \
	$(PORT)/mmu.c \
	$(PORT)/gic.c \
	$(PORT)/port.c \
	$(RTOS_SOURCE)/list.c \
	$(RTOS_SOURCE)/queue.c \
	$(RTOS_SOURCE)/tasks.c \
	$(RTOS_SOURCE)/portable/MemMang/heap_4.c \
	platform/platform.c \
	platform/runtime.c \
	platform/libIVC.c \
	platform/ivcdev.c \
	platform/xen/events_setup.c \
	platform/xen/events.c \
	platform/xen/hypervisor.c \
	platform/xen/xen_setup.c \
	platform/xen/gnttab.c \
	platform/xen/xenbus.c \
	platform/xen/xenbus-arm.c \
	platform/xen/hvm.c \
	platform/util/ldivmod_helper.c \
	platform/util/qdivrem.c \
	platform/util/popcount.c \
	platform/util/ctype.c \
	platform/console/console.c \
	platform/console/xencons_ring.c \
	platform/minlibc/vsnprintf.c \
	platform/minlibc/vfprintf.c \
	platform/minlibc/snprintf.c \
	platform/minlibc/memcpy.c \
	platform/minlibc/memchr.c \
	platform/minlibc/memmove.c \
	platform/minlibc/memset.c \
	platform/minlibc/memcmp.c \
	platform/minlibc/strlen.c \
	platform/minlibc/strnlen.c \
	platform/minlibc/strcmp.c \
	platform/minlibc/strdup.c \
	platform/minlibc/printf.c \
	platform/minlibc/puts.c \
	platform/minlibc/sscanf.c \
	platform/minlibc/stdio_values.c \
	platform/minlibc/errno.c \
	platform/minlibc/abort.c \
	platform/minlibc/sprintf.c \
	platform/minlibc/pabort.c

ASOURCES= \
	  platform/asm/shared_info.s \
	  platform/asm/hypercalls.s \
	  $(PORT)/asm/boot.s \
	  $(PORT)/asm/irq.s \
	  $(PORT)/asm/fpu.s \
	  $(PORT)/asm/timer.s \
	  $(PORT)/asm/vectors.s \
	  $(PORT)/asm/strings.s \
	  $(PORT)/asm/ldivmod.s \
	  $(PORT)/asm/tlb.s \
	  $(PORT)/asm/cache.s

OTHER_A= \
       $(PORT)/asm/print.s \
       $(PORT)/asm/dumpregs.s \
       $(PORT)/asm/tlb.s \
       $(PORT)/asm/stack.s \
       $(PORT)/asm/bss.s

FDT_OBJS= \
    $(FDT_SOURCE)/libfdt/fdt.o \
    $(FDT_SOURCE)/libfdt/fdt_ro.o \
    $(FDT_SOURCE)/libfdt/fdt_wip.o \
    $(FDT_SOURCE)/libfdt/fdt_sw.o \
    $(FDT_SOURCE)/libfdt/fdt_rw.o \
    $(FDT_SOURCE)/libfdt/fdt_strerror.o \
    $(FDT_SOURCE)/libfdt/fdt_empty_tree.o \
    $(FDT_SOURCE)/libfdt/fdt_addresses.o

COBJS = $(CSOURCES:.c=.o)
AOBJS = $(ASOURCES:.s=.o)

FreeRTOS.a: $(AOBJS) $(COBJS)
	$(MAKE) -C $(FDT_SOURCE) CC=$(CC) AR=$(AR)
	$(AR) r FreeRTOS.a $(AOBJS) $(COBJS) ./platform/asm/hypercalls.o $(FDT_OBJS)

$(COBJS): %.o : %.c Makefile include/FreeRTOSConfig.h
	$(CC) -c $(CFLAGS) $*.c -o $*.o

$(AOBJS): %.o : %.s $(OTHER_A) Makefile include/FreeRTOSConfig.h
	$(CC) -c $(CFLAGS) -x assembler-with-cpp $*.s -o $*.o

clean:
	rm -f $(COBJS) $(AOBJS) FreeRTOS.a
