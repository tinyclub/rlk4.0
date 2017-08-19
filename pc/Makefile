_PLUGIN  = 1
ARCH     = x86
XARCH    = i386
CPU     ?= i686
MEM     ?= 128M
QEMU    ?= v0.10.6
SMP     ?= 2
LINUX   ?= v4.0
#LINUX   ?= v2.6.35.14
BUILDROOT?= 2016.05
NETDEV  ?= ne2k_pci
SERIAL  ?= ttyS0
ORIIMG  ?= arch/$(ARCH)/boot/bzImage
ROOTDEV ?= /dev/ram0
#ROOTDEV ?= /dev/null
#CCPRE   = i686-pc-linux-gnu-
KIMAGE  ?= $(TOP_DIR)/boards/$(BOARD)/kernel/$(XARCH)/$(LINUX)/bzImage
#KIMAGE  ?= $(PREBUILT_KERNEL)/$(XARCH)/$(BOARD)/$(LINUX)/bzImage
# use prebuilt one
ROOTFS  ?= $(PREBUILT_ROOT)/$(XARCH)/$(CPU)/rootfs.cpio.gz
HROOTFS ?= $(PREBUILT_ROOT)/$(XARCH)/$(CPU)/rootfs.ext2
# Fixup: MP-BIOS bug: 8254 timer not connected to IO-APIC
XKCLI   ?= noapic
