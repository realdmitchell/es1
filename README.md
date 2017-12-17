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



