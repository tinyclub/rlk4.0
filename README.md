
# Linux Lab Plugin: RLK4.0

This is a clone of <https://github.com/figozhang/runninglinuxkernel_4.0>, added as a plugin of [Linux Lab](https://github.com/tinyclub/linux-lab).

It aims to speed up the experiment environment building for the [RLK4.0](http://www.epubit.com.cn/book/details/4835) Book.

Before using this RLK4.0 plugin, we need to install [Linux Lab](https://github.com/tinyclub/linux-lab) at first, please refer to <https://tinylab.org/linux-lab>.

To save your install time, we can buy a online Linux Lab from <https://weidian.com/?userid=335178200>

The step-by-step usage is documented below, or you can watch recorded videos in [showterm](http://showterm.io/e786d08e0ea0964f3efb1) and in [showdesk]().

## Quick start

### Download rlk4.0 to boards/

    $ cd /labs/linux-lab/boards
    $ git clone https://github.com/tinyclub/rlk4.0

### Select the arm64 board: rlk4.0/virt

    $ cd /labs/linux-lab
    $ make list plugin=rlk4.0
    [ rlk4.0/pc ]:
          ARCH     = x86
          CPU     ?= i686
          LINUX   ?= v4.0
          ROOTDEV ?= /dev/null
    [ rlk4.0/vexpress-a9 ]:
          ARCH     = arm
          CPU     ?= cortex-a9
          LINUX   ?= v4.0
          ROOTDEV ?= /dev/null
    [ rlk4.0/virt ]:
          ARCH     = arm64
          CPU     ?= cortex-a57
          LINUX   ?= v4.0
          ROOTDEV ?= /dev/null

    $ make BOARD=rlk4.0/virt

### Boot prebuilt kernel and rootfs

    $ make boot

## Hacking Linux Kernel

### Download, Configure and Compile Kernel

Download [linux stable](https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git) repo to `linux-stable/`:

    $ make kernel-source

Checkout kernel version specified by `LINUX` in `boards/rlk4.0/virt/Makefile`:

    $ make kernel-checkout

Configure it with the default `boards/rlk4.0/virt/linux_v4.0_defconfig`:

    $ make kernel-defconfig
    $ make kernel-menuconfig

Compile it with the pre-installed `aarch64-linux-gnu-gcc` in `/usr/local/bin`:

    $ make kernel

### Using Kernel Modules

    $ make module-list PLUGIN=rlk4.0
     1	ldt
     2	mutexlock_test
     3	oops_test
     4	kmemleak_test
     5	rcu_test
     6	lock_test
     7	slub_test

    $ make module m=oops_test
    $ make module-install
    $ make root-rebuild

    $ make boot
    (qemu) modprobe oops

### Debugging with gdb

Compile the kernel with `CONFIG_DEBUG_INFO=y` and debug it directly:

    $ make debug

Or debug it with two steps, boot it at first:

    $ make boot DEBUG=1

Open a new terminal and use gdb to debug it:

    $ make env | grep KERNEL_OUTPUT
    KERNEL_OUTPUT = /labs/linux-lab/output/aarch64/linux-v4.0-rlk4.0/virt/

    $ aarch64-linux-gnu-gdb output/aarch64/linux-v4.0-rlk4.0/virt/vmlinux
    ...
    Reading symbols from output/aarch64/linux-v4.0-rlk4.0/virt/vmlinux...done.
    (gdb) b do_fork
    Breakpoint 1 at 0xffff8000000d6fa4: file /labs/linux-lab/linux-stable/kernel/fork.c, line 1638.
    (gdb) b start_kernel
    Breakpoint 2 at 0xffff800000ce6918: file /labs/linux-lab/linux-stable/init/main.c, line 499.
    (gdb) target remote :1234
    Remote debugging using :1234
    0x0000000040000000 in ?? ()
    (gdb) c
    Continuing.

    Breakpoint 2, start_kernel () at /labs/linux-lab/linux-stable/init/main.c:499
    499		set_task_stack_end_magic(&init_task);
    (gdb) c
    Continuing.

    Breakpoint 1, do_fork (clone_flags=8389376, stack_start=18446603336232349608, stack_size=0,
        parent_tidptr=0x0, child_tidptr=0x0) at /labs/linux-lab/linux-stable/kernel/fork.c:1638
    1638		int trace = 0;


### Testing

Linux Lab allows us to do some simple testings:

Simply boot and poweroff:

    $ make test

Don't poweroff after testing (Simply boot):

    $ make test TEST_FINISH=echo

Run guest test case:

    $ make test TEST_CASE=/tools/ftrace/trace.sh

Reboot the guest system for several times:

    $ make test TEST_REBOOT=2

Test a kernel module:

    $ make module-test m=oops_test

Test a kernel module and make some targets before testing:

    $ make module-test m=oops_test TEST=kernel-checkout,kernel-patch

## Hacking rootfs with buildroot

### Install tools into rootfs

Put the new tools in `system/tools`, install tools and rebuild the rootfs:

    $ make rootdir                # Decompress the rootfs.cpio.gz
    $ make root-install
    $ make root-rebuild

### Transfer files between host and board

Note, only **vexpress-a9** and **virt** support this feature.

Host:

    $ touch hostshare/test  # Create a file in host
    $ make boot SHARE=1

Qemu Board:

    $ ls /hostshare/        # Access the file in guest
    test
    $ touch /hostshare/guest_test  # Create a file in guest

### Download, Configure and Compile rootfs

Download [buildroot](https://github.com/buildroot/buildroot) repo to `buildroot/`:

    $ make root-source

Checkout buildroot version specified by `BUILDROOT` in `boards/rlk4.0/virt/Makefile`:

    $ make root-checkout

Configure it with the default `boards/rlk4.0/virt/buildroot_cortex-a57_defconfig`:

    $ make root-defconfig
    $ make root-menuconfig

Build the rootfs from scratch:

    $ make root

Boot with this new rootfs:

    $ make boot

If a new rootfs exists, to install tools to the prebuilt rootfs, please pass `PBR=1`.
