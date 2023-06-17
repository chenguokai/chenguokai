# Tutorial on Booting Linux on MS Dev Kit 2023

## Introduction

Currently, according to discussions in [linux-surface issue](https://github.com/linux-surface/surface-pro-x/issues/43), it has become possible to boot linux on Microsoft dev kit with key functions working. The latest progress was made by @merckhung who has provided a working kernel tree. However there has been no tutorial that provide a full description on how to make the magic happen. Discussion in the issue by @jglathe has provided a basic working path and my tutorial resembles in many parts.

## Overview

The hardware requirements are one MS Dev Kit with stock Windows and a usb key. You will need to install some software on the windows side which will be described later. Following this, you will have a minimal working ubuntu system running on your dev kit.

There are mainly three steps:
1. compile the kernel
2. configure the rootfs
3. boot and enjoy

### Compile the kernel

I have done the following in a ubuntu 20.04 WSL2 environment running on Dev Kit, to avoid annoying cross compiling.
