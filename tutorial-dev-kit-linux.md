# Tutorial on Booting Linux on MS Dev Kit 2023

## Introduction

Currently, according to discussions in [linux-surface issue](https://github.com/linux-surface/surface-pro-x/issues/43), it has become possible to boot linux on Microsoft dev kit with key functions working. The latest progress was made by @merckhung who has provided a working kernel tree. However there has been no tutorial that provide a full description on how to make the magic happen. Discussion in the issue by @jglathe has provided a basic working path and my tutorial resembles in many parts. But please note before you begin, neither I nor the code authors would be responsible for any damage on your device or your data. You go at your own risk.

## Overview

The hardware requirements are one MS Dev Kit with stock Windows and a usb key. You will need to install some software on the windows side which will be described later. Following this, you will have a minimal working ubuntu system running on your dev kit.

There are mainly three steps:
1. compile the kernel
2. configure the rootfs
3. boot and enjoy

### Compile the kernel

I have done the following in a ubuntu 20.04 WSL2 environment running on Dev Kit, to avoid annoying cross compiling. You may refer to steps provided by @jglathe for cross compiling guidance.

Firstly, you should install essential tools (`build-essential`, `git`, `bison`, `flex` etc ) for compiling a kernel. If there are any missing tools called in your process. please do some googling and install any missing tools.
Secondly, you should clone merckhung's kernel source to your local machine. The command should be like 

```shell
git clone https://github.com/merckhung/linux_ms_dev_kit
```

The source tree is so huge that you may want to do a [shallow clone](https://git-scm.com/docs/shallow). Currently the default branch in the repo is for Dev Kit, but if it changes in the future, remember to checkout the correct branch:
```shell
git checkout ms-dev-kit-2023-v6.3.0
```


Secondly, you should compile the kernel with the config for Dev Kit. After you enter the kernel source directory, compile with:
```
make devkit_defconfig # this tells the kernel to use Dev Kit default config
make menuconfig       # this allows you to make any custom config change
                      # for ubuntu's initramfs generation requirement, you should enable initramfs support here
                      # press `esc` and choose save after that to save your config
make -j8              # do the actual work, Dev Kit has 8 cores so use 8 threads here
```

After that you will find `Image` file in `arch/arm64/boot` and `sc8280xp-microsoft-dev-kit-2023.dtb` in `arch/arm64/boot/dts
/qcom/`. Copy them out to your Windows partition. You will also find `.config` file right in the top of the kernel source tree, copy it to `/boot` in your WSL system. Root priviledge are required for this copy. You will need to adjust the config file name to meet ubuntu's requirement later.

Finally, except for the kernel image, you will need kernel modules. You can install it on your WSL system by typing 

```sudo make modules_install```

You will find your kernel modules in `/lib/modules/6.3.0+`, you will need to put them in the incoming rootfs.

