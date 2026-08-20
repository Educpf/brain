---
title: "File Systems and Partitions"
categories: ["Linux"]
tags: ["linux"]
draft: true
---


# File System 

A **file system** is how an operating system stores, organizes, retrieves and manages data on a physical storage device. In essence a storage drive is a pool of billions of raw binary bits, so there needs to exist some kind of structure that helps define where files are ( begin and end ), how to name them etc...

This may seem very basic but multiple implementation exist and are chosen by each specific OSs.

The specific responsibilities and mechanisms of a file system will be discussed in other post.


| File System | Primary OS        | Windows Support                                           | Linux Support                             | macOS Support                                    | Best Used For                                         |
| ----------- | ----------------- | --------------------------------------------------------- | ----------------------------------------- | ------------------------------------------------ | ----------------------------------------------------- |
| exFAT       | Universal         | Native (Read/Write)                                       | Native (Kernel module)                    | Native (Read/Write)                              | External drives, shared dual-boot media partitions    |
| FAT32       | Legacy            | Native (Read/Write)                                       | Native (Read/Write)                       | Native (Read/Write)                              | EFI Boot partitions, old USB drives (4 GB file limit) |
| NTFS        | Windows           | Native (Read/Write)                                       | Native (Kernel ntfs3 driver)              | Read-Only (Requires 3rd-party drivers for Write) | Windows OS installation drive                         |
| ext4        | Linux             | No Native Support (Requires WSL2 or 3rd-party software)   | Native (Read/Write)                       | No Native Support (Requires 3rd-party software)  | Linux OS system partitions                            |
| Btrfs       | Linux             | No Native Support (Requires experimental WinBtrfs driver) | Native (Read/Write)                       | No Native Support                                | Modern Linux installations, snapshots, NAS setups     |
| APFS        | macOS             | No Native Support (Requires 3rd-party software)           | Read-Only (Via apfs-fuse community tools) | Native (Read/Write)                              | Mac system drives and external SSDs                   |
| ZFS         | Enterprise / Unix | No Native Support                                         | Supported (Via OpenZFS module)            | Supported (Via OpenZFS on OS X)                  | Multi-disk storage servers, software RAID, data pools |


## Converting File Systems

Moving Files Between Different File SystemsYes, absolutely. You can copy, move, and edit files between an ext4 partition and an exFAT partition seamlessly, provided the operating system you are running understands both file systems.How It Works Under the HoodWhen you copy a file from ext4 to exFAT in Linux:Reading Phase: The Linux kernel uses its native ext4 driver to read the raw data blocks from the ext4 partition and loads the file into temporary system memory (RAM).Translation Phase: The kernel strips away Linux-specific metadata that exFAT doesn't support (such as POSIX user ownership UID/GID or strict system file permissions like rwxr-xr-x).Writing Phase: The Linux kernel uses its exFAT driver to write the data blocks onto the exFAT partition and creates a simple file entry in its table.To you, it looks like a simple drag-and-drop in your file manager, happening almost instantaneously.Important Things to Keep in MindConsiderationWhat Happens When Moving from ext4 ➔ exFATFile PermissionsLost. exFAT does not support Linux file ownership or security permissions. Every file on exFAT will be readable/writable by everyone.Case Sensitivityext4 distinguishes Video.mp4 from video.mp4 as two different files. exFAT does not. Moving two files with identical names differing only in capital letters to exFAT will cause a naming conflict/overwrite.Executable FlagsYou cannot mark a script or binary file as executable (chmod +x) directly on an exFAT drive due to lack of Linux permission bits.SymlinksSymbolic links created in ext4 will not work or be preserved when moved to exFAT.




# Partition 

A partition is a logical division of the memory card, that information is stored in the **first sectors** of the disk, in what's called **Partition Table**.

This is important for multiple reasons:

- Different OS may use different file systems, structures and boot mechanisms. In order for them to live in the same machine they have to be separated logically.
- Separate system files from data, which helps with system recovery or storage quotas.
- Security measures, such as encryption can be applied per partition.

# Partition Table

Defines the boundaries ( start and end sectors ) of each chunk(partition). There are two main formats:

- **MBR (Master Boot Record)** Legacy 32-bit partition with **limit** of **2TB disks** and **4 partitions**
- **GPT (GUID Partition Table)** Modern 64-bit used by UEFI systems. 
    - Supports **128 primary partitions** and gigantic disk sizes
    - Stores duplicates copies of the table at the end of disk for recovery


# LVM (Logical Volume Management)

This framework allows some abstraction between the physical storage devices and the filesystems and partitions. Instead of a partition being tied to rigid physical sectors, LVM breaks this limitation by turning the disk into a elastic pool of storage.

### Concepts

1. Physical Volume (PV)
    - The real storage device
    - LVM writes an **header**, turning them into building blocks 
2. Volume Group (VG)
    - Unified Pool created by combining one or more PV, can contain multiple logical volumes inside.
    - A large virtual hard drive where all the space is aggregated no matter the physical disk locations.
3. Logical Volume (LV)
    - Virtual partitions
    - Act like regular block devices accepting standard file systems


### Advantages

- Dynamic Resizing: on-the-fly while system is running
- Merge Disks: merge space from completely different disks
- Snapshots: LVM supports point-in-time snapshots.
- LUKS: Can group logical volumes together and encrypt all the partitions as one.


