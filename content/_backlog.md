

# Real backlog


- [ ] Naive Bayes vídeo :)

## Old stuff

- [ ] Representation

## What I learned

- [ ] Barycentric coordinates
- [ ] Delaunay triangulation utility










# Web - React

- [ ] Types of rendering + components diferences
- [ ] Why new server functions in NExt.js are good?
- [ ] React/NextJS hooks
- [ ] React Functions vs API Calls
- [ ] Query joins types
- [ ] What really are migrations? 

# Linux

- [ ] Pacman
  - [ ] Commands cheat sheet
- [ ] Wayland vs X11, desktops etc...
- [ ] Ram ( virtual, swap etc... )
- [ ] Overall workings of linux 
- [ ] ssh and logic behind `clear` command not working. terminfo etc...

# Temp

Blender / Arch / Hyprland Memory Setup Cheat Sheet

Enable zram (fast, in-RAM swap)

Size: ~½ of RAM (8 GB for 16 GB RAM)

Compression: zstd

Tool: zram-generator

Create disk swap (for safety + hibernation)

LV inside volgroup0

Size: ~8–12 GB

Encrypted automatically (inside LUKS)

mkswap + swapon

Adjust swappiness

echo "vm.swappiness=10" | sudo tee /etc/sysctl.d/99-swappiness.conf


Disable zswap

Only use zram + disk swap for simplicity

Optional future RAM upgrade

Keep zram & disk swap

Kernel will auto-adjust usage

No reconfiguration required

# Temp - how linux works

1️⃣ Boot Process & systemd (FOUNDATIONAL)

If you understand this, everything else makes sense.

What to understand

UEFI vs BIOS (at least conceptually)

Bootloader → kernel → initramfs → systemd

Targets, units, dependencies

Why services start when they do

Key concepts

initramfs

systemd units (service, target, socket, timer)

dependency graph

Tools to master
systemd-analyze
systemd-analyze blame
systemctl cat <unit>
systemctl list-dependencies
journalctl -b

“I understand this when…”

You can explain why your network comes up

You can create or modify a systemd service without copy-paste

You can debug a service failing at boot

2️⃣ The Kernel (without going insane)

You don’t need to be a kernel dev — you need to know what the kernel is responsible for and where its boundary is.

What to understand

Syscalls (conceptually)

Process scheduling

Kernel vs userspace

Modules and drivers

/proc and /sys

Key concepts

PID 1

context switching

kernel modules

virtual filesystems

Tools
uname -a
lsmod
modprobe
dmesg
strace
htop (kernel view)

Exercises

Trace a command with strace

Load/unload a kernel module

Inspect /proc/<pid>

3️⃣ Process Model & Memory

This is where Linux becomes real.

What to understand

Fork/exec model

PIDs, PPIDs, namespaces

Signals

Virtual memory

OOM killer

Tools
ps auxf
top / htop
free -h
vmstat
ulimit
kill -SIGTERM / SIGKILL

“I understand this when…”

You know why killing PID 1 is bad

You know what happens when RAM is full

You can explain zombie processes

4️⃣ Filesystems & Storage

Linux treats everything as a file — learn why.

What to understand

VFS

Filesystem types (ext4, btrfs, tmpfs)

Mounts and namespaces

Permissions and ownership

Inodes

Key concepts

block devices

mount propagation

tmpfs

bind mounts

Tools
lsblk
mount
findmnt
df -h
du -sh
stat

Exercises

Manually mount a filesystem

Create a loopback filesystem

Explore /dev, /proc, /sys

5️⃣ Networking (Linux networking is a system)

Not “how to connect to Wi-Fi”, but how packets move.

What to understand

Network namespaces

Interfaces, routing tables

TCP vs UDP

DNS resolution flow

Firewalling (nftables)

Tools
ip a
ip r
ss -tulpn
nft list ruleset
resolvectl
tcpdump

“I understand this when…”

You know why a port is or isn’t reachable

You can trace a packet from app → NIC

You understand NAT conceptually

6️⃣ Users, Permissions, and Security

Linux security is simple but strict.

What to understand

UID/GID model

Capabilities

sudo

PAM

SELinux/AppArmor (conceptually)

Tools
id
getcap
setcap
loginctl
sudo -l

Exercises

Run a service as a non-root user

Remove root where possible

Understand why permissions fail

7️⃣ Packaging, Libraries, and Linking

This explains why software breaks.

What to understand

Dynamic vs static linking

shared libraries

ABI vs API

Package manager internals

Why Arch breaks faster than Debian

Tools
ldd
readelf
objdump
pacman -Qi
pacman -Ql

“I understand this when…”

You know what libfoo.so errors mean

You can explain rolling-release breakage

You understand dependency chains

8️⃣ Logging, Monitoring, and Debugging

This is how real admins think.

What to understand

journald

log levels

tracing vs logging

metrics vs logs

Tools
journalctl
dmesg
uptime
iostat
perf (optional)

9️⃣ Virtualization & Containers (optional but modern)

This ties everything together.

What to understand

cgroups

namespaces

chroot vs containers

KVM conceptually

Tools
unshare
lsns
systemd-cgls
docker inspect


# Temp


Arch Linux Boot & Initramfs Summary
1️⃣ Kernel

The kernel is the core of Linux:

Handles hardware management: CPU, memory, drivers

Provides mechanisms like mounting filesystems, device I/O, encryption APIs

Does not handle policy: deciding which disk to mount, asking for passwords, or managing services

Needs initramfs to prepare the system before mounting root

2️⃣ Initramfs (Initial RAM Filesystem)

Purpose:

Temporary Linux environment loaded into RAM before the real root filesystem exists

Tasks:

Load kernel modules (storage, network, GPU)

Load firmware

Initialize devices (udev)

Enable keyboard / input

Configure console (font, keymap)

Unlock encrypted disks (LUKS)

Assemble LVM / RAID volumes

Mount the real root filesystem

Hand off to the real system (switch_root or pivot_root)

Provide emergency shell if mounting fails

Optionally show a UI via Plymouth

Key points:

Disappears after root is mounted

Must be small (RAM-limited)

Contains binaries, libraries, configs, scripts, systemd units

Stored as a compressed CPIO archive, e.g., /boot/initramfs-linux.img

Built by mkinitcpio, based on hooks and configuration

3️⃣ mkinitcpio

Tool that builds initramfs

Reads /etc/mkinitcpio.conf

Uses HOOKS to include:

Scripts and binaries (encrypt, sd-encrypt, plymouth)

Capabilities like keyboard, kms, lvm2

Generates a single initramfs image to be loaded by the bootloader

Editing config requires rebuilding:

mkinitcpio -P

4️⃣ HOOKS

Define what capabilities and files are included in initramfs

Examples:

base → essential binaries

systemd → early systemd PID 1 in initramfs

sd-encrypt → systemd-based LUKS unlock

plymouth → graphical splash and password UI

keyboard, keymap, kms → input + framebuffer support

Order matters: earlier hooks provide support for later ones

5️⃣ GRUB (Bootloader)

Self-contained program that:

Loads kernel

Loads initramfs

Passes kernel parameters (root=UUID=... quiet splash rd.luks.name=...)

Does not manage disks, decryption, or mount root

Reads config from /boot/grub/grub.cfg

Kernel parameters can be changed:

Permanently: /etc/default/grub → grub-mkconfig -o /boot/grub/grub.cfg

Temporarily: edit boot menu at startup

Points GRUB to the partition containing /boot or kernel files

6️⃣ Kernel Parameters

Text key=value pairs passed from GRUB → kernel → initramfs → systemd / Plymouth

Examples:

quiet splash → suppress messages, allow Plymouth

rd.luks.name=UUID=cryptroot → LUKS disk identification

loglevel=3 → less kernel output

systemd.log_level=debug → systemd debug messages

View current parameters:

cat /proc/cmdline

7️⃣ Plymouth

Graphical front-end running inside initramfs

Intercepts systemd-ask-password to provide:

Password input box for LUKS

Splash animations / progress

Runs as a daemon (plymouthd) during initramfs

Handoff occurs after root is mounted:

plymouth-quit.service stops the daemon

Controlled by:

Hook: plymouth in mkinitcpio

Kernel parameters: quiet splash

Themes: /usr/share/plymouth/themes/

8️⃣ systemd

Init system / service manager

Exists in two instances on Arch:

Instance	PID 1	Purpose
Initramfs systemd	early systemd	Prepares root, LUKS unlock, LVM, optionally Plymouth
Real systemd	after switch_root	Manages services, targets, userspace, login

systemctl communicates only with the real systemd

You cannot use systemctl to control initramfs systemd

Early initramfs systemd is temporary and disappears after handoff

9️⃣ Boot Flow Summary
Firmware (UEFI / BIOS)
  ↓
GRUB bootloader
  - loads kernel
  - loads initramfs
  - passes kernel parameters
  ↓
Kernel
  - mounts initramfs into RAM as /
  - starts initramfs systemd (PID 1)
  ↓
Initramfs systemd
  - loads modules, firmware, keyboard, LVM
  - unlocks LUKS (sd-encrypt)
  - optionally runs Plymouth for UI
  - mounts real root filesystem
  ↓
switch_root
  - handoff to real systemd
  ↓
Real systemd (PID 1)
  - starts services, targets
  - manages login

10️⃣ Key Mental Models

Kernel: “mechanisms only” (drivers, crypto, mounting API)

Initramfs: “temporary Linux rescue system to prepare root”

mkinitcpio: “compiler” for initramfs, includes hooks and files

Hooks: “capabilities and scripts included in initramfs”

GRUB: “loader + menu + kernel parameter provider”

Plymouth: “graphical front-end for early systemd / password prompts”

systemd: “orchestrator / service manager” (two instances: initramfs vs real OS)

systemctl: “controller for real systemd only”


  
