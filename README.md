
# Block ads
http://someonewhocares.org/hosts/
http://pgl.yoyo.org/adservers/serverlist.php?hostformat=dnsmasq-server&showintro=1&mimetype=plaintext

# Grub reinstallation mainly on ubuntu.
Boot with sysrescuecd. If the root is at `sda2` then do the following
```
mkdir /mnt/2
mount /dev/sda2 /mnt/2
mount -o bind /dev /mnt/2/dev
mount -o bind /sys /mnt/2/sys
mount -t proc /proc /mnt/2/proc
```
Now you should be able to
```
chroot /mnt/2 bash
```
If there was no bash installed on the 'failed' OS, the use
```
chroot /mnt/2 /sbin/sh
```
Now time to reinstall grub.
```
grub-install /dev/sda
```
You can try a `update-grub`



# Clone your favorite OS
```
sfdisk -d /dev/sda
sfdisk -d /dev/sda > /tmp/backup-partition-file-sda.bak
```
To restore to a new drive
```
sfdisk /dev/sdb < /tmp/backup-partition-file-sda.bak
```
To directly copy to a new disk
```
sfdisk -d /dev/sda | sfdisk /dev/sdb
```
If you need to backup bootsector/MBR. I do not know why and which command works but running these sucessively made it work. This works only if both disks have same size and partitions. (May be they can be smaller?)

```
dd if=/dev/sda of=/dev/sdb bs=512 count=1
dd if=/dev/sda of=/dev/sdb bs=512 count=63
```


# LXD
How to setup LXD

```
sudo apt-get install bridge-utils
sudo lxd init
Name of the storage backend to use (dir or zfs): dir
Would you like LXD to be available over the network (yes/no)? yes
Do you want to configure the LXD bridge (yes/no)? yes
Would you like to setup a network bridge for LXD containers now? no  
Do you want to use an existing bridge? yes  
Bridge interface name:  br0
```
Setup enable packet forwarding in file `/etc/sysctl.conf` for IPv4 on the LXD host
```
net.ipv4.ip_forward=1
```
Run `sudo sysctl -p` or `reboot`. Now change the adapt ths file describes the network `/etc/network/interfaces` in host for static to bridge `br0` changes

```
#This file is /etc/network/interfaces
source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface
auto br0
iface br0 inet static
	address 1.2.3.111
	netmask 255.255.255.240
	network 1.2.3.112
	broadcast 1.2.3.127
	gateway 1.2.3.126
	# dns-* options are implemented by the resolvconf package, if installed
	dns-nameservers 8.8.8.8 4.4.4.4

##  bridge options
bridge_ports enp2s0  
## auto enp2s0
iface enp2s0 inet manual
```
Verify before restarting network as you lose connection `sudo service networking restart`. If everything was OK, you will see `br0´, `eth0 or enp2s0` and `lo` when you run `ifconfig` like this (redacted partially)
```
br0       Link encap:Ethernet  HWaddr 00:
          collisions:0 txqueuelen:1000 
          RX bytes:417554630 (417.5 MB)  TX bytes:8047958 (8.0 MB)

enp2s0    Link encap:Ethernet  HWaddr 00:
          RX bytes:744425912 (744.4 MB)  TX bytes:14523858 (14.5 MB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          RX bytes:11840 (11.8 KB)  TX bytes:11840 (11.8 KB)
```
IIRC, `lxc profile show default ` shows
```
config: {}
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: br0
    type: nic
name: default
used_by: []
```
Time to create, launch containers. The first time it takes long to download, may be unzip and launch containers.
```sudo lxc launch ubuntu:xenial host222xenial
Creating host222xenial
Starting host222xenial
```
Let see what is running
```
lxc list
+---------------+---------+--------------------------------+------+------------+-----------+
|     NAME      |  STATE  |              IPV4              | IPV6 |    TYPE    | SNAPSHOTS |
+---------------+---------+--------------------------------+------+------------+-----------+
| host222xenial | RUNNING |                                |      | PERSISTENT | 0         |
+---------------+---------+--------------------------------+------+------------+-----------+
```
As you see above it did not get a IPV4 address. To do that `sudo lxc exec host222xenial bash` will log you inside `host222xenial`. Do not edit `/etc/network/interfaces` but edit `/etc/network/interfaces.d/50-cloud-init.cfg`
```
auto lo
iface lo inet loopback

auto eth0
iface eth0 inet dhcp

```
Change it to give static ip, gateway, DNS. For some reason, the containers always have eth0 and not the persistent enp* naming scheme. Verify with a `dmesg` if needed.

```
# The loopback network interface
auto lo
iface lo inet loopback

# The primary network interface of the container
auto eth0
iface eth0 inet static
   address 1.2.3.222
   netmask 255.255.255.240
   network 1.2.3.112
   broadcast 1.2.3.127
   gateway 1.2.3.126
   # dns-* options are implemented by the resolvconf package, if installed
   dns-nameservers 8.8.8.8 4.4.4.4
``` 
Either restart container or may be you can reload networking `sudo lxc restart  host222xenial`

Let see what is running
```
lxc list
+---------------+---------+--------------------------------+------+------------+-----------+
|     NAME      |  STATE  |              IPV4              | IPV6 |    TYPE    | SNAPSHOTS |
+---------------+---------+--------------------------------+------+------------+-----------+
| host222xenial | RUNNING |       1.2.3.222                |      | PERSISTENT | 0         |
+---------------+---------+--------------------------------+------+------------+-----------+
```

To rename host use `lxc move host222xenial host444xenial`

To launch centos `sudo lxc launch images:centos/7/amd64 centos333`

To launch trusty `sudo lxc launch ubuntu:trusty host222`

If you need `screen` inside a container then use

`lxc exec host222xenial -- sh -c "exec >/dev/tty 2>/dev/tty </dev/tty && /usr/bin/screen -s /bin/bash"`




# Tips
for enabling softkeys in android phone just add
```
qemu.hw.mainkeys=0
to /system/build.prop
```

# ZFS
## install ubuntu
```
sudo apt install zfs zfs-dkms zfs-zed zfs-initramfs zfsutils-linux zfsutils-linux
```

## commands

```
sudo zpool create data2 /dev/sdc
#ls --color=never -lh /dev/disk/by-id/
sudo zpool create data2 ata-ST4000DM003-1ER162_AAAAAAAL
sudo zpool status zpool
sudo zfs get all zpool
zfs create <nameofzpool>/<nameofdataset>
sudo zfs set compression=lz4 mypool
sudo zfs get compressratio
sudo zfs list
sudo zfs get used,compressratio,compression,logicalused
chown -R user.user /nameofzpool/datasets

```

## commands
```
find  .  -type f \( -name '*.img' -o -name '*.mccd' -o -name '*.cbf' \) -print0 | xargs -0 gzip -v --best

function jgrep()
{
    find . -name .repo -prune -o -name .git -prune -o  -type f -name "*\.java" -print0 | xargs -0 grep --color -n "$@"
}

function cgrep()
{
    find . -name .repo -prune -o -name .git -prune -o -type f \( -name '*.c' -o -name '*.cc' -o -name '*.cpp' -o -name '*.h' \) -print0 | xargs -0 grep --color -n "$@"
}

function resgrep()
{
    for dir in `find . -name .repo -prune -o -name .git -prune -o -name res -type d`; do find $dir -type f -name '*\.xml' -print0 | xargs -0 grep --color -n "$@"; done;
}

function mangrep()
{
    find . -name .repo -prune -o -name .git -prune -o -path ./out -prune -o -type f -name 'AndroidManifest.xml' -print0 | xargs -0 grep --color -n "$@"
}

function sgrep()
{
    find . -name .repo -prune -o -name .git -prune -o  -type f -iregex '.*\.\(c\|h\|cpp\|S\|java\|xml\|sh\|mk\)' -print0 | xargs -0 grep --color -n "$@"
}

function mgrep()
{
    find . -name .repo -prune -o -name .git -prune -o -path ./out -prune -o -regextype posix-egrep -iregex '(.*\/Makefile|.*\/Makefile\..*|.*\.make|.*\.mak|.*\.mk)' -type f -print0 | xargs -0 grep --color -n "$@"
}

function treegrep()
{
    find . -name .repo -prune -o -name .git -prune -o -regextype posix-egrep -iregex '.*\.(c|h|cpp|S|java|xml)' -type f -print0 | xargs -0 grep --color -n -i "$@"
}

function findfile()
{
    find . -name .repo -prune -o -name .git -prune -o -path ./out -prune -o -type f -iname "$@"| grep -v ".git" | grep -v ".repo"
}

function finddir()
{
    find . -name .repo -prune -o -name .git -prune -o -path ./out -prune -o -type d -iname "$@"| grep -v ".git" | grep -v ".repo"
}

```



