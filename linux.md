$TERM Environment Variable
-------- 

The TERM environment variable in Linux is used to specify the type of terminal used.

Here are a few common values for TERM:

* **xterm:** Standard terminal type for X Window System
* **vt100:** Terminal type for older VT100 terminals
* **linux:** Terminal type for Linux console
* **screen:** Terminal type used within screen sessions

Streams
---
* **1>** or **>** to stream stdout.
* **2>** to stream stderr.
* **&>** to stream both.

Variables
----
* Use **export** to create environment variables.
* **unset** to remove environment variables.

Filesystem Commands
-------
- **lsblk:** List all block devices
- **fdisk,parted:** Manage disk partitions
- **blkid:** Show block device attributes
- **hwinfo:** Show hardware information
- **file -s:** Show filesystem and partition information
- **statd, df -i, lst -i:** Show and list inode-related information

Common Top Level Directories
---
- **bin, sbin:** System programs and commands (usually links to /usr.bin and /usr/sbin)
- **boot:** Kernel images and related components
- **dev:** Devices (terminals, drives, etc.)
- **home:** User home directories
- **lib:** Shared system librarires
- **mnt, media:** Mount points for removable media
- **opt:** Distro specific, can host package manager files
- **proc, sys:** Kernel interfaces
- **tml:** Temporary files
- **usr:** User programs (Usually read only)
- **var:** User programs (logs, backups, network caches, etc.)

systemd and units
----
In systemd, units are configuration files that encapsulate information about system services, resources, and other components that systemd manages.
  
| Unit Type |	Purpose	| Examples |
| ----------|-----------|----------|
Service Units|	Define and manage services | sshd.service, nginx.service
Target Units|	Group other units	|multi-user.target, graphical.target
Socket Units|	Manage sockets for IPC	|sshd.socket, docker.socket
Device Units|	Represent hardware devices	|dev-sda1.device, sys-devices-*.device
Mount Units|	Define mount points	|home.mount, mnt-data.mount
Automount Units|	Manage automount points	|home.automount, mnt-data.automount
Timer Units|	Trigger services at specific times	|backup.timer, logrotate.timer
Swap Units|	Manage swap space	|dev-sda2.swap, swapfile.swap
Path Units|	Path-based activation	|myapp.path, backup.path
Slice Units|	Manage hierarchical cgroups	|system.slice, user.slice
Scope Units|	Manage externally created processes	|session-1.scope, user-1000.slice
Busname Units|	Manage D-Bus names	|org.freedesktop.systemd1.busname

Systemd unit files are stored in several different directories, depending on whether they are system-wide units or user-specific units.

### 1. System-Wide Unit Files

#### 1.1 System Unit Directory:

**/lib/systemd/system/**

This directory contains the default unit files provided by the system and package maintainers.

#### 1.2 Administrator Unit Directory:

**/etc/systemd/system/**

This directory contains unit files installed or modified by the system administrator. It has a higher priority than the system unit directory.

### 2. User-Specific Unit Files

#### 2.1 User Unit Directory (Per User):

**~/.config/systemd/user/**

This directory is used for unit files that are specific to a single user.

#### 2.2 System User Unit Directory:

**/etc/systemd/user/**

This directory contains unit files for user services that are managed by the system administrator.

     journaljtl -f -u myservice.service

- -f: follow 
- -u: the selected unit

Network Related Topics
====
An *interface* is a network connection. It can ben pyhsical or virtual (like loopback)

**NIC** = Network Interface Controller

#### ifconfig ( We can say it is deprecated):
- ifconfig -a
- ifconfig  eth0 up

> ip link show


https://superuser.com/questions/342149/what-is-the-difference-between-a-link-local-address-and-an-ip-address-in-the-pri
