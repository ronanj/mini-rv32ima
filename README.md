# riscv_emufun

## Introduction

I'm working on a really really simple Risc-V 32 IMA emultor. So simple it doesn't even have an MMU (Memory Management Unit). I have a few goals, they include:
 * Furthering RV32-NOMMU work to improve Linux support for RV32-NOMMU.  (Imagine if we could run Linux on the $1 ESP32-C3)
 * Learning more about RV32 and writing emulators.
 * Being further inspired by @pimaker's amazing work on [Running Linux in a Pixel Shader](https://blog.pimaker.at/texts/rvc1/) and having the sneaking suspicion performance could be even better!
 * Hoping to port it to some weird places.

What this is: A single-file-header library that:
 * Implements a full RISC-V 32-bit IMA, with CLINT and MMIO.
 * Is about 400 lines of actual code.
 * Has no dependencies, not even libc.
 * Compiles down to a ~18kB exectuable.
 * Has a demo wrapper that implements a wrapper, UART, DTB and Kernel image loading.

Just see the `mini-rv32ima` folder.

It's "fully functional" now in that I can run Linux, apps, etc.  Compile flat binaries and drop them in an image.

To test this, you will need a Linux box with `git build-essential` and whatever other requirements are in place for [buildroot](https://buildroot.org/).
 * Clone this repo.
 * `make everything`
 * About 15 minutes.  (Or 4+ hours if you're on [Windows Subsytem for Linux 2](https://github.com/microsoft/WSL/issues/4197)
 * And you should be dropped into a shell.

...And you can almost run Linux in Linux in Linux (though not quite yet).

## Personal Notes

## VERY EARLY ROUGH DO NOT USE

## Processor type

Probably/maybe something like: rv32ima/??su+Zifencei+Zicsr

Right now I am just using an rv32im.  But wouldn't it be cool if we could get buildroot running on that?

TODO:
 * Actually make OpenSBI work, it's what buildroot uses to load the Kernel.


## Prereq

For bare metal:
```
sudo apt-get install gcc-multilib gcc-riscv64-unknown-elf make
```

## Buildroot Notes

Neat tools:

In root:
`make toolchain`

From `buildroot-2022.02.6`:
`make linux-menuconfig`
`make menuconfig`

## QEMU Test

```
sudo apt install qemu-system-misc
```

```
output/host/bin/qemu-system-riscv32 -cpu rv32,mmu=false -m 128M -machine virt -nographic -kernel output/images/Image -bios none -drive file=output/images/rootfs.ext2,format=raw,id=hd0 -device virtio-blk-device,drive=hd0
output/host/bin/qemu-system-riscv32 -cpu rv32,mmu=false -m 128M -machine virt -nographic -kernel output/images/Image -bios none -drive file=output/images/rootfs.ext2,format=raw,id=hd0 -device virtio-blk-device,drive=hd0 -machine dumpdtb=../dtb.dtb
```

## Running from INITRD instead of DISK
 * In buildroot: Filesystem Images, check cpio, no compression.
 * In kernel: 

## Building Tests

(This does not work, now)
```
cd riscv-tests
export CROSS_COMPILE=riscv64-linux-gnu-
export PLATFORM_RISCV_XLEN=32
CC=riscv64-linux-gnu-gcc ./configure
make XLEN=32 RISCV_PREFIX=riscv64-unknown-elf- RISCV_GCC_OPTS="-g -O1 -march=rv32imaf -mabi=ilp32f -I/usr/include"
```


## Building OpenSBI

```
cd opensbi
export CROSS_COMPILE=riscv64-unknown-elf-
export PLATFORM_RISCV_XLEN=32
make
```

## Resources

 * https://jborza.com/emulation/2020/04/09/riscv-environment.html
 * https://blog.pimaker.at/texts/rvc1/


## General notes:
 * QEMU
 * Pi's Linux
 * https://github.com/cnlohr/riscv_emufun/commit/2f09cdeb378dc0215c07eb63f5a6fb43dbbf1871#diff-b48ccd795ae9aced07d022bf010bf9376232c4d78210c3113d90a8d349c59b3dL440
 * Making the kernel assembly to dig through.
 * touch kernel_config && make
 * Debugging process.
 * test-driven-development.
   -> Pitfalls.
 * Talk about converting the style to HLSL
 * People are working on relocatable ELFs.



