# Development Environment

I am starting this with no idea what I'm doing this doc will probably change.

## Requirements

I'm trying to start this project as cheaply as possible. That means figuring out development without buying any hardware yet. Here's a list of things I think I'll need (probably will change)

- virtual machine for testing the OS (qemu) - installed
- docker for compiling RPIOS - installed

## compiling Linux

We're going to compile RPIOS and hopefully get an image runnable by qemu
Following this guide: https://www.raspberrypi.com/documentation/computers/linux_kernel.html#building

Going to cross compile natively, because docker is for NERDS

1. sudo dnf group install development-tools
2. `sudo dnf install` bc, bison, flex, openssl-devel, ncurses-devel, elfutils-libelf-devel, gcc-aarch64-linux-gnu, binutils-aarch64-linux-gnu, git, make
3. git clone https://github.com/raspberrypi/linux
4. cd linux
5. export ARCH=arm64
6. export CROSS_COMPILE=aarch64-linux-gnu-
7. make bcm2712_defconfig
8. make -j$(nproc) Image modules dtbs
9. sudo make -j(nproc) modules_install

Now it's compiled, let's make an image

1. `sudo dnf install kpartx` install kpartx
2. `sudo losetup -fP raspios.img` attach a loop device
3. `losetup -a` find the loop device
4. `sudo kpartx -av /{loop device}` create partition mappings
5. sudo mount /dev/mapper/loop0p1 /mnt/boot
6. sudo mount /dev/mapper/loop0p2 /mnt/root

This direction is for the raspberry pi 4, and I'm running these commands on fedora, the process will be different for RPI5 or in other dev environments


