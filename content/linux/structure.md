---
title: "Linux structure"
tags: ["linux"]
draft: true
---

# Introduction

I've been using linux for a while, but with all the copy-paste tutorials there is I always find excuses to actually learn how the system works and how it is structured. If you feel the same, welcome to this small journey of understanding linux file structure.

## General hierarchy 

The first step is to understand at an high level what each top-level directory is for.

| Directory      | Purpose                                              |
| -------------- | ---------------------------------------------------- |
| /              | Root of the file system                              |
| /bin           | Essential command binaries for **all users**         |
| /sbin          | System binaries for admin tasks (fsck, ifconfig)     |
| /usr           | User program and data. Can be shared across systems  |
| /var           | Variable data files ( logs, databases, mail spools ) |
| /etc           | System-wide configuration files                           |
| /home          | User's home directories                              |
| /tmp           | Temporary files                                      |
| /dev           | Device files ( interface to hardware )               |
| /proc          | Virtual filesystem for kernel and process info       |
| /lib or /lib64 | Shared libraries                                     |
| /opt           | Optional/add-on software                             |
| /mnt or /media | Mount points for external storage                    |

## How to practice


# 1. usr

| path | purpose |
| ---- | ------- |
|  /usr |   stuff installed by package manager      |
| /usr/local | installed manually | 
| */lib | libraries (system & user-installed ) |
| */bin | non essential binaries for users  |
| */sbin | non essential binaries for admin tasks |
| */include | headers |

The separation exists because of:
- **Safety**: OS upgrades don't overwrite/delete custom libraries
- **Tradition**: old convention

# 2. etc : "et cetera"

Only configuration usually without any binaries. The **control panel of the entire system**.

Programs read files in `/etc` at startup to learn:
- How to behave
- Where things live
- What services to start
- network settings
- users and groups
- permissions
- paths
- system policies

| path            | purpose                                                 |
| --------------- | ------------------------------------------------------- |
| /passwd         | users                                                   |
| /group          | groups                                                  |
| /fstab          | disk mounts                                             |
| /hosts          | hostname mappings                                       |
| /resolv.conf    | DNS                                                     |
| ssh/sshd_config | SSH server config                                       |
| /systemd        | service configs                                         |
| /ld.so.conf     | define additional directories for dynamic linker search |
|                 |                                                         |







# File types

## Libraries

- **Shared**: `.so` 
- **Static**: `.a`

### Linker:
**Dynamic linker** searches for libraries in places like:

- `/lib`
- `/usr/lib`
- `/usr/local/lib`
- paths in `/etc/id.so.conf`
- `LD_LIBRARY_PATH`

To refresh **linker cache** run `sudo idconfig`







