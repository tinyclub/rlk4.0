_PLUGIN  = 1
ARCH     = arm64
XARCH    = aarch64
CPU     ?= cortex-a57
SMP     ?= 2
MEM     ?= 128M
XOPTS   ?= -cpu $(CPU)
LINUX   ?= v4.0
BUILDROOT?= 2016.05
NETDEV  ?= virtio
SERIAL  ?= ttyAMA0
ROOTDEV ?= /dev/ram0
# None external initrd
#ROOTDEV ?= /dev/null
FSTYPE  ?= ext2
ORIIMG  ?= arch/$(ARCH)/boot/Image
CCPRE   ?= aarch64-linux-gnu-
KIMAGE  ?= $(TOP_DIR)/boards/$(BOARD)/kernel/$(XARCH)/$(LINUX)/Image
# The old busybox rootfs is not powerful enough
#ROOTFS  ?= $(TOP_DIR)/boards/$(BOARD)/root/rootfs.cpio.gz
# Use the prebuilt one of Linux Lab
ROOTFS  ?= $(PREBUILT_ROOT)/$(XARCH)/$(CPU)/rootfs.cpio.gz
