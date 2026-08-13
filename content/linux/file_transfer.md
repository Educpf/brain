---
title: "Android File Transfer"
categories: ["Linux"]
tags: ["linux"]
draft: false
---

# How the communication works:

There are multiple ways to share files with the phone, and we will look into some of them.
The basic idea is that the communication is divided into multiple layers, mainly:

1. Application: the program we interact with
2. Communication protocol: defines how msgs are structured 
3. Transport Layer: USB, WIFI etc..., controls the sending of actual bytes
4. Android: answers accordingly 

The Application may not use the communication protocol directly but be behind some framework as we will see. 


# Wired

## File Manager

If you have a file manager like **dolphin** installed you may notice that it automatically detects connected phones, enabling you to browse and transfer files from the phone directly from there.

In this specific case this happens because dolphin is a **KDE** application that uses the **KIO** (KDE Input/Output), a KDE framework that provides abstraction for accessing different types of resources. For an MTP device, KIO uses an MTP backend that communicates via that protocol.

**MTP** is the communication protocol that defines how the host and the media device exchange information about storages, files, directories, and file data. In this scenario, the phone is not mounted as a normal Linux filesystem. Instead, operations such as listing directories and transferring files are performed through requests and responses exchanged using MTP.

> It is important to distinguish KDE from KDE Plasma. KDE Plasma is a full desktop environment built using KDE technologies and frameworks. KDE applications can be used independently of Plasma, including under other desktop environments or window managers such as Hyprland.

Because all this may be integrated automatically within the file manager using other options that rely on TMP might not work. To fix it finding and killing the process responsible for that ( in this case greping for KIO ) should be enough.

## With GUI

Install the tool and simply run it:
```sh
sudo pacman -S android-file-transfer
```

This works similarly to Dolphin, but without the KIO layer involved. Instead, **android-file-transfer** directly uses an MTP implementation to communicate with the Android device through the MTP protocol and perform the requested operations.

Compared with Dolphin, it is much more specialized, since it is specifically designed for communicating with Android devices over MTP, while also providing a graphical interface.


## For CMD

Install the tools, list
```sh
sudo pacman -S android-tools # Provide adb exe
sudo pacman -S android-udev # Provide connection rules
abd --devices # See device list
```

```sh
adb pull /sdcard/DCIM/photo.jpg . # Download files
adb push photo.jpg /sdcard/DCIM/ # Upload files
adb shell # Interact with a shell on Android
adb shell ls /sdcard # Run command without opening shell
```



`adb` works a little bit differently. Instead of using MTP, it communicates via the **ADB(Android Debug Bridge) Protocol** with the `adbd` daemon running on the Android device. 

This provides a much more broader interface, allowing to transfer files, open a shell on the device, install applications, forward ports, and more debugging operations. It is mainly a **command-line tool**. 


> Note that the phone must have **USB Debugging** enabled

## With mount

Install the program: 

```sh
sudo pacman -S simple-mtpfs
simple-mtpfs --list-devices # List available devices
simple-mtpfs $LOCATION --device $NUMBER
fusermount -u $LOCATION # To umount the FUSE filesystem
# It may be required to reload the USB state ( unplugging or changing connection option )
```

It is genuinely mounted from the perspective of the Linux VirtualFileSystem, but the mounted filesystem is the FUSE filesystem implemented by simple-mtpfs, not the Android filesystem itself.

FUSE ( FileSystem in UserSpace ) is what makes this possible. All the normal calls are routed through by `Linux filesystem API`, that route them through `FUSE` and subsequently `simple-mtpfs`. The latter translates the filesystem operations into `MTP` rather than accessing the phone's storage blocks directly.


# Wireless
## ADB

To connect wirelessly:

```sh
adb pair IP:PORT
adb connect IP:PORT
```

With this approach we are basically using the **ADB** but via the WIFI instead of an USB.

> For this, wireless debugging must be enabled and the devices connected to the same network. IP and PORT should be visible in the same place

## Specific App

In this case there isn't a standardize filesystem/file-transfer protocol like MTP being used. Both systems run the same software that speak a specific language directly via the WIFI. A good example is **LocalSend** or **KDE Connect**. 




