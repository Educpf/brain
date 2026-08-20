---
title: "Linux Boot"
tags: ["linux"]
draft: true
---



There are a lot of stuff in life that seems easy and simple at a first glance but then ends up being much more than what we imagined. Once we understand things completely it starts getting easy and simple again. 

The booting of a machine is exactly like that. I've already installed/configured arch linux, but now we analyze with more detail the booting process.


# The order


PowerButton -> BIOS/UEFI (POST | BOOT DEVICE ) -> BOOT Loader -> Kernel -> Init Process


# BIOS / UEFI

What is it, what it does.

Differences. 


1
MBR, stored in beggining limits

2
Best security

# Bootloaders

Location operating system kernel 
Load kernel
Start running code

### LILO Grub2


# Kernel

decompress into memory + check hardware + load device drivers and kernel modules


# Systemd

Very complete stuff
