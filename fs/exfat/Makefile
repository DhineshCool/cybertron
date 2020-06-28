# SPDX-License-Identifier: GPL-2.0-or-later
#
# Makefile for the linux exFAT filesystem support.
#

# Never compile as an external module
#ifneq ($(KERNELRELEASE),)
ifneq (1,)
# call from kernel build system

obj-$(CONFIG_EXFAT_FS) += exfat.o

exfat-objs	:= inode.o namei.o dir.o super.o fatent.o cache.o nls.o misc.o \
	   file.o balloc.o xattr.o
else
# external module build

EXTRA_FLAGS += -I$(PWD)

#
# KDIR is a path to a directory containing kernel source.
# It can be specified on the command line passed to make to enable the module to
# be built and installed for a kernel other than the one currently running.
# By default it is the path to the symbolic link created when
# the current kernel's modules were installed, but
# any valid path to the directory in which the target kernel's source is located
# can be provided on the command line.
#
KDIR	:= /lib/modules/$(shell uname -r)/build
MDIR	:= /lib/modules/$(shell uname -r)
PWD	:= $(shell pwd)
KREL	:= $(shell cd ${KDIR} && make -s kernelrelease)
PWD	:= $(shell pwd)

export CONFIG_EXFAT_FS := m

all:
	$(MAKE) -C $(KDIR) M=$(PWD) modules

clean:
	$(MAKE) -C $(KDIR) M=$(PWD) clean

help:
	$(MAKE) -C $(KDIR) M=$(PWD) help

install: exfat.ko
	rm -f ${MDIR}/kernel/fs/exfat/exfat.ko
	install -m644 -b -D exfat.ko ${MDIR}/kernel/fs/exfat/exfat.ko
	depmod -aq

uninstall:
	rm -rf ${MDIR}/kernel/fs/exfat
	depmod -aq

endif

.PHONY : all clean install uninstall
