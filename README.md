
# Fdroid repos
![alt text](https://raw.githubusercontent.com/realdmitchell/es1/master/fdroid_repo.png "Fdroid repo")

# Sony SRS XB20
With the speaker turned on, press and hold the – (volume) button and the  Power button at the same time for more than 5 seconds until the speaker turns off.




# encfs

```
sudo apt install encfs
encfs ~/.encrypted ~/visible
fusermount -u ~/visible

```




# Samba

```
root@host:~# service smbd restart
root@host:~# netstat -apn | grep 445
tcp        0      0 127.0.0.1:445           0.0.0.0:*               LISTEN      13390/smbd      
tcp        0      0 a.b.c.d:445        0.0.0.0:*               LISTEN      13390/smbd      
tcp        0      0 10.147.20.1:445         0.0.0.0:*               LISTEN      13390/smbd      
tcp        0      0 a.b.c.d:445        a.b.c.55:3029       TIME_WAIT   -               
tcp        0      0 a.b.c.d:445        a.b.c.44:35502      FIN_WAIT2   -               
tcp        0      0 a.b.c.d:445        a.b.c.55:3031       ESTABLISHED 13395/smbd      
tcp6       0      0 ::1:445                 :::*                    LISTEN      13390/smbd      
unix  2      [ ACC ]     STREAM     LISTENING     22445    1/init              /run/snapd.socket
root@host:~# netstat -apn | grep 445
tcp        0      0 127.0.0.1:445           0.0.0.0:*               LISTEN      13390/smbd      
tcp        0      0 a.b.c.d:445        0.0.0.0:*               LISTEN      13390/smbd      
tcp        0      0 10.147.20.1:445         0.0.0.0:*               LISTEN      13390/smbd      
tcp        0      0 a.b.c.d:445        a.b.c.55:3029       TIME_WAIT   -               
tcp        0      0 a.b.c.d:445        a.b.c.44:35502      FIN_WAIT2   -               
tcp        0      0 a.b.c.d:445        a.b.c.55:3031       ESTABLISHED 13395/smbd      
tcp6       0      0 ::1:445                 :::*                    LISTEN      13390/smbd      
unix  2      [ ACC ]     STREAM     LISTENING     22445    1/init              /run/snapd.socket
root@host:~# ps axf | grep smbd
13405 pts/3    S+     0:00  |                   \_ grep --color=auto smbd
13236 ?        Ss     0:00      \_ /usr/sbin/smbd -D
13239 ?        S      0:00          \_ /usr/sbin/smbd -D
13244 ?        S      0:00          \_ /usr/sbin/smbd -D
13390 ?        Ss     0:00 /usr/sbin/smbd -D
13391 ?        S      0:00  \_ /usr/sbin/smbd -D
13393 ?        S      0:00  \_ /usr/sbin/smbd -D
13395 ?        S      0:00  \_ /usr/sbin/smbd -D
```


If you run samba server in both lxd-containers and the lxd-host then you may run to smb starting problems in host.
Edit file `/etc/init.d/smbd`
```
diff --git a/smbd b/smbd
index b6ec38f..1e92d8e 100755
--- a/smbd
+++ b/smbd
@@ -37,7 +37,7 @@ case $1 in
      # Make sure we have our PIDDIR, even if it's on a tmpfs
      install -o root -g root -m 755 -d $PIDDIR

-     if ! start-stop-daemon --start --quiet --oknodo --exec /usr/sbin/smbd -- -D; then
+      if ! start-stop-daemon --start --quiet --oknodo --pidfile /var/run/samba/smbd.pid --exec /usr/sbin/smbd -- -D; then

         log_end_msg 1
         exit 1

```

Verify with

```
root@host:~# ps -ef | grep smbd
100000   13236 12434  0 09:29 ?        00:00:00 /usr/sbin/smbd -D
100000   13239 13236  0 09:29 ?        00:00:00 /usr/sbin/smbd -D
100000   13244 13236  0 09:29 ?        00:00:00 /usr/sbin/smbd -D
root     13390     1  0 09:30 ?        00:00:00 /usr/sbin/smbd -D
root     13391 13390  0 09:30 ?        00:00:00 /usr/sbin/smbd -D
root     13393 13390  0 09:30 ?        00:00:00 /usr/sbin/smbd -D
root     13395 13390  0 09:30 ?        00:00:00 /usr/sbin/smbd -D
root     13435 11720  0 09:35 pts/3    00:00:00 grep --color=auto smbd
```


# annotate images
https://www.getcloudapp.com/apps

# Firefox
 from https://news.ycombinator.com/item?id=16497642
	
For privacy, on a linux box are there any downsides to simply creating one or more extra accounts, and running Firefox in them for privacy ('DISPLAY=:0 firefox')?. I use this approach to set up firefox as I like it on a spare account, then copy '.mozilla' to '.mozilla-base'. Then it's just a simple case of 'su -l guest' and (via a script) 'rm -fr ~/.mozilla; cp -a ~/.mozilla_base .mozilla; DISPLAY=:0 firefox; rm -fr ~/.mozilla' (actually the script deletes the local cache as well).
Net effect is that firefox starts exactly as I like, but forgets everything that happened in the session ('groundhog-day mode').

Edit: added 'su -l' step.

Edit: As an adendum, note that this technique can be extended to the complete 'guest' accounts as well, e.g. 'cd /home; rm -fr guest; cp -a guest.base guest; su -l guest'; the entire 'guest' account is then 'groundhog-dayed'.

```
  #!/bin/sh
  #
  export DISPLAY=:0
  # Set up clean copy
  cd ~
  rm -fr .mozilla
  cp -a .mozilla_base .mozilla
  cd - > /dev/null
  #
  /usr/local/bin/firefox $@
  #
  echo "Holding...."
  sleep 2
  echo "Cleaning...."
  # Clean out junk (so we start clean next time)
  cd ~
  rm -fr .mozilla .cache/mozilla*
  rm -fr .adobe
  rm -fr .macromedia
  cd - > /dev/null
```

alternate option

```
    firejail --jail /tmp/firefox /usr/local/bin/firefox

```


# Block ads

http://someonewhocares.org/hosts/

http://pgl.yoyo.org/adservers/serverlist.php?hostformat=dnsmasq-server&showintro=1&mimetype=plaintext

https://github.com/StevenBlack/hosts

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

```
lxc config device set megasyn-vm  root  size 10GB
lxc config set myn-vm limits.memory 512MB
lxc config set rea-vm3 limits.cpu 1
lxc config set rea-vm3 boot.autostart 1
lxc config set remotus boot.autostart.delay  30
lxc config set core.trust_password a_long_string_may_be
lxc config set core.https_address "[::]:8443"
sudo lxd-p2c https://destination.ip:8443 new-container-name /
```

Changing your storage pool

```
lxc storage list
lxc profile device add default root disk path=/ pool=default
```

Minimal image

``` 
lxc remote add --protocol simplestreams ubuntu-minimal https://cloud-images.ubuntu.com/minimal/releases/
lxc launch ubuntu-minimal:xenial
lxc launch ubuntu-minimal:xenial -t t2.nano

``` 
For a list see https://github.com/dustinkirkland/instance-type/blob/master/tab/aws



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
root@mg50:~# cat /etc/network/interfaces
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).

source /etc/network/interfaces.d/*

# The loopback network interface
auto lo
iface lo inet loopback

auto br0
iface br0 inet static
   address bare.metal.host.ip
   netmask 255.255.255.224
   network a.b.c.d
   gateway a.b.c.d
   dns-nameservers a.b.c.d
   dns-search host.domain
   bridge_ports eno1


auto br1
iface br1 inet manual
    bridge_ports enxlkldkflkdkfj

```


Verify before restarting network as you lose connection 

```
ip addr flush eno1 &&  ip addr flush enxlkldkflkdkfj  && systemctl restart networking.service
```



If everything was OK, you will see `br0´, `eth0 or enp2s0` and `lo` when you run `ifconfig` like this (redacted partially)

```
br0       Link encap:Ethernet  HWaddr 00:
          collisions:0 txqueuelen:1000 
          RX bytes:417554630 (417.5 MB)  TX bytes:8047958 (8.0 MB)

br1    Link encap:Ethernet  HWaddr 00:
          RX bytes:744425912 (744.4 MB)  TX bytes:14523858 (14.5 MB)


eno1      Link encap:Ethernet  HWaddr d4::::::


enx0022cf932bbc Link encap:Ethernet  HWaddr 00:2::::::  


lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          RX bytes:11840 (11.8 KB)  TX bytes:11840 (11.8 KB)
```

IIRC, `lxc profile show default ` shows
```lxc profile show default
config:
  environment.http_proxy: ""
  user.network_mode: ""
description: Default LXD profile
devices:
  eth0:
    name: eth0
    nictype: bridged
    parent: br1
    type: nic
  root:
    path: /
    pool: default
    type: disk
name: default
used_by:
- /1.0/containers/climbing-crow

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



