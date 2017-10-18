# es1
Contains script and dot files for my es1 and raspi

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
