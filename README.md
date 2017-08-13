
# Linux Lab Plugin: RLK4.0

This is a clone of <https://github.com/figozhang/runninglinuxkernel_4.0>, added as a plugin of [Linux Lab](https://github.com/tinyclub/linux-lab).

It aims to speed up the experiment environment building for the [RLK4.0](http://www.epubit.com.cn/book/details/4835) Book.

Before using this RLK4.0 plugin, we need to install Linux Lab at first, please refer to <https://tinylab.org/linux-lab>.

To save your install time, we can buy a online Linux Lab from <https://weidian.com/?userid=335178200>

## Using rlk4.0 in Linux Lab

    $ cd /labs/linux-lab/boards
    $ git clone https://github.com/tinyclub/rlk4.0

    $ cd /labs/linux-lab
    $ make list | grep -A 4 rlk4.0
    [ rlk4.0/pc ]:
          ARCH     = x86
          CPU     ?= i686
          LINUX   ?= v4.0
          ROOTDEV ?= /dev/ram0
    [ rlk4.0/vexpress-a9 ]:
          ARCH     = arm
          CPU     ?= cortex-a9
          LINUX   ?= v4.0
          ROOTDEV ?= /dev/ram0
    [ rlk4.0/virt ]:
          ARCH     = arm64
          CPU     ?= cortex-a57
          LINUX   ?= v4.0
          ROOTDEV ?= /dev/ram0

    $ make BOARD=rlk4.0/virt
    $ make boot
