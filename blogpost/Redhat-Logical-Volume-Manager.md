
Robb Manes <rmanes@redhat.com>
12:15 PM 02/06/2019
to me, Ashlee

Hi Patrick,

Pleasure talking to you over the phone; just wanted to re-iterate what
we discussed.

Part of the path of becoming a technical support engineer at Red Hat
is passing two particular exams within your first 90 days with the
company, the "Red Hat Certified Systems Administrator" exam and the
"Red Hat Certified Engineer" exam.  They are not trivial exams and
require hands-on knowledge and experience with the utilities, and they
are interactive exams where you have to configure live systems.  We'd
like to be sure that you're able to quickly absorb the material in
this timeframe.

We understand that you might be new to some of these Linux concepts,
and would like to give
you a task that would demonstrate technical aptitude as well as an
ability to learn new and complex material quickly, both of which are
necessary to pass these exams.  To that end, we'll reconvene in about
a week after you've been able to research and utilize one particular
topic; we'll ask your some detailed questions about it to determine
how far you were able to go with the material.

The topic we'd like you to research and get hands-on experience is
Logical Volume Manager (LVM)
[https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)].  

In general, it is a very storage and disk heavy topic, and it's
understanding is vital to passing the exams mentioned above.  Please
feel free to use absolutely any documentation you can find from any
source to read up and experiment with it.  I would highly, highly
recommend installing CentOS version 7, which is based on Red Hat
Enterprise Linux, our primary product, which is a Linux Operating
System.  You can run this in a Virtual Machine or on physical
hardware, whichever is easier for you, but due to the nature of the
topic I'd recommend attaching a second virtual hard drive to
experiment on when practicing with LVM.  I'd also recommend knowing
specific commands at the command line; although GUI tools exist, we
will `prefer command-line answers` for some specific operations and
tasks concerning `logical volumes`, as well as some related tasks like
`disk partitions` and `file-systems` for some extra credit.

Please let me know what day and time next week works for you to
reconvene; we're available M-F 10AM to 6PM MST, so whatever works best
for your schedule in that timeframe, and I'll just make sure there are
no conflicts with that time concerning my peers.

Look forward to speaking with you!  If you have absolutely any
questions or need any clarification at all please don't hesitate to
reach out to me.

Good luck!


-- Robb Manes
Principal Software Maintenance Engineer
Red Hat, Inc.

# Logical Volume Manager (LVM)
# How LVM Works
## Physical Volumes 
### Extents
## Volume Group

## Logical Volume
### Linear Volumes
### Striped Logical Volumes
### RAID Logical Volumes
### Thinly-Provisioned Logical Volumes (Thin Volumes)
### Snapshot Volumes
### Thinly-Provisioned Snapshot Volumes
### Cache Volumes 

# Installing Logical Volume Manager (LVM)
## Register RHEL
You attach a subscription to your RedHat login  (free developer subscription)
``` shell
subscription-manager register --username patrick --password secret --auto-attach
```
## Update RHEL
``` shell
subscription-manager repos --list-enabled
subscription-manager repos --enable rhel-server-rhscl-7-rpms
subscription-manager repos --enable rhel-7-server-optional-rpms

yum makecache
yum update -y
```

## Install LVM service
```shell
yum install lvm2
```

## Partion Disk w/ fdisk
`Find about about hard disks`

``` bash
fdisk -l
```
```shell
WARNING: fdisk GPT support is currently new, and therefore in an experimental phase. Use at your own discretion.

Disk /dev/xvda: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: gpt
Disk identifier: 9D1DC67C-CE93-45F3-BB78-138B371E7678


#         Start          End    Size  Type            Name
 1         2048         4095      1M  BIOS boot
 2         4096     20971486     10G  Microsoft basic

Disk /dev/xvdb: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/xvdc: 10.7 GB, 10737418240 bytes, 20971520 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
```

`Partion /dev/xvdb`
``` shell
sudo fdisk /dev/xvdb
```
`(m for help)`
``` shell
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.

Device does not contain a recognized partition table
Building a new DOS disklabel with disk identifier 0x2402617c.

Command (m for help): m
Command action
   a   toggle a bootable flag
   b   edit bsd disklabel
   c   toggle the dos compatibility flag
   d   delete a partition
   g   create a new empty GPT partition table
   G   create an IRIX (SGI) partition table
   l   list known partition types
   m   print this menu
   n   add a new partition
   o   create a new empty DOS partition table
   p   print the partition table
   q   quit without saving changes
   s   create a new empty Sun disklabel
   t   change a partition's system id
   u   change display/entry units
   v   verify the partition table
   w   write table to disk and exit
   x   extra functionality (experts only)
```

### Create a new Primary Partition & set the Patrition type as Linux LVM
``` shell
Command (m for help): n
Partition type:
   p   primary (0 primary, 0 extended, 4 free)
   e   extended
Select (default p): p
Partition number (1-4, default 1): 1
First sector (2048-20971519, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-20971519, default 20971519):
Using default value 20971519
Partition 1 of type Linux and of size 10 GiB is set
```

## Partion Disk w/ parted
I want to check on the partition created with `fdisk`. Remove that partition & recreate with `parted`
### Using elevated shell
`This opens the virtual disk b`
``` shell
parted /dev/xvdb
```
`This show the disk just created with fdisk. `
```shell
GNU Parted 3.1
Using /dev/xvdb
Welcome to GNU Parted! Type 'help' to view a list of commands.
(parted) print
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start   End     Size    Type     File system  Flags
 1      1049kB  10.7GB  10.7GB  primary               lvm
```
`Remove the fdisk partition`
```shell
(parted) rm 1
(parted) print
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: msdos
Disk Flags:

Number  Start  End  Size  Type  File system  Flags
```
`Create new partion in a 1 liner  or interactlvely `
```shell
(parted) mkpart primary ext4 0% 100%
```
```shell
(parted) mkpart
Partition name?  []? diskB
File system type?  [ext2]? xfs
Start? 0%
End? 100%
(parted) print
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name   Flags
 1      1049kB  10.7GB  10.7GB               diskB
 ```

`Set partion to lvm`
```shell
(parted) set 1 lvm on
(parted) print
Model: Xen Virtual Block Device (xvd)
Disk /dev/xvdb: 10.7GB
Sector size (logical/physical): 512B/512B
Partition Table: gpt
Disk Flags:

Number  Start   End     Size    File system  Name     Flags
 1      1049kB  10.7GB  10.7GB               primary  lvm
```

## Configuring LVM
### Create Physical Volumes
I had 2 drives to be created as physical volumes. One `/dev/xvdb1` I partioned before adding a physical volume and the other `/dev/xvdbc` i added raw. 

```shell
# pvcreate /dev/xvdb1
  Physical volume "/dev/xvdb1" successfully created.

# pvcreate /dev/xvdc
  Physical volume "/dev/xvdc" successfully created.
```
`display the newly created physical volumes`
```shell
# pvdisplay
  PV         VG Fmt  Attr PSize   PFree
  /dev/xvdb1    lvm2 ---  <10.00g <10.00g
  /dev/xvdc     lvm2 ---   10.00g  10.00g
```


Once a physical volume is added to a Volume Group you can see the group attached 
```shell
# pvscan
  PV /dev/xvdb1   VG VG_One          lvm2 [<10.00 GiB / <10.00 GiB free]
  PV /dev/xvdc    VG VG_One          lvm2 [<10.00 GiB / <10.00 GiB free]
  Total: 2 [19.99 GiB] / in use: 2 [19.99 GiB] / in no VG: 0 [0   ]
```
```shell
# pvdisplay
  --- Physical volume ---
  PV Name               /dev/xvdb1
  VG Name               VG_One
  PV Size               <10.00 GiB / not usable 2.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              2559
  Free PE               2559
  Allocated PE          0
  PV UUID               Db3elD-fnDZ-QqVl-TG30-jo2t-t7aS-CoBUlh

  --- Physical volume ---
  PV Name               /dev/xvdc
  VG Name               VG_One
  PV Size               10.00 GiB / not usable 4.00 MiB
  Allocatable           yes
  PE Size               4.00 MiB
  Total PE              2559
  Free PE               2559
  Allocated PE          0
  PV UUID               x6yHDy-1gdX-dLxd-q5iJ-B6qq-yr3D-4vl6WC
  ```
### Create Volume Group
`Here i am creating "VG_One" from the two Physical Volumes I created.`
```shell
# vgcreate VG_One /dev/xvdb1 /dev/xvdc
  Volume group "VG_One" successfully created

`Show the Volume Group with vgs or vgdisplay.  You can see the size is the combined available between the two Physical Volumes`
# vgs
  VG     #PV #LV #SN Attr   VSize  VFree
  VG_One   2   0   0 wz--n- 19.99g 19.99g
```
## Attach a Logical Volume to a Volume Group
Here I create a 100Mb Logical Volume named "LV_One" & attach it to the Volume Group "VG_One". I recreate that step and create a second Logical Volume named LV_Two. This creates 2 Linear Volumes, you can check the LV type with `lvdisplay -vm`
```shell
# lvcreate --size 100M --name LV_One VG_One
  Logical volume "LV_One" created.
# lvscan
  ACTIVE            '/dev/VG_One/LV_One' [100.00 MiB] inherit
# lvcreate --size 2GB --name LV_Two VG_One
  Logical volume "LV_Two" created.
# lvscan
  ACTIVE            '/dev/VG_One/LV_One' [100.00 MiB] inherit
  ACTIVE            '/dev/VG_One/LV_Two' [2.00 GiB] inherit
  ```

  Display more information about a specific LV 
  ```shell
  # lvdisplay VG_One/LV_Two
  --- Logical volume ---
  LV Path                /dev/VG_One/LV_Two
  LV Name                LV_Two
  VG Name                VG_One
  LV UUID                vvo6Z9-UwBP-dAfq-ZMOV-E7CF-kUcQ-lO37S7
  LV Write Access        read/write
  LV Creation host, time ip-172-31-37-49.us-west-2.compute.internal, 2019-02-07 05:10:20 +0000
  LV Status              available
  # open                 0
  LV Size                2.00 GiB
  Current LE             512
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1
  ```

## Formatting and Mounting Logical Volumes:
Logical volumes show up under /dev/ under the Volume Group name. 
Here is 
```shell
# ls /dev/VG_One/
LV_One  LV_Two
```
`create a file-system by using the mkfs cmd. xfs is preffered for RHEL/CentOS 7`

```shell
# mkfs.xfs /dev/VG_One/LV_Two
meta-data=/dev/VG_One/LV_Two     isize=512    agcount=4, agsize=131072 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=524288, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

`Create a mount point for the folder & mount the Logical Volume with the folder.`
```shell
# mkdir -pv /var/www/website_2
mkdir: created directory ‘/var/www’
mkdir: created directory ‘/var/www/website_2’
# mount /dev/VG_One/LV_Two /var/www/website_2
# df -h
Filesystem                 Size  Used Avail Use% Mounted on
/dev/xvda2                  10G  3.1G  7.0G  31% /
devtmpfs                   473M     0  473M   0% /dev
tmpfs                      495M     0  495M   0% /dev/shm
tmpfs                      495M   13M  482M   3% /run
tmpfs                      495M     0  495M   0% /sys/fs/cgroup
tmpfs                       99M     0   99M   0% /run/user/1000
/dev/mapper/VG_One-LV_Two  2.0G   33M  2.0G   2% /var/www/website_2
```


## Striped Logical Volume

using the `-i` specifiys the number of stipes to use. This number cannot be greater than the number of PV's attached to the Volume Group. 

Below I am creating a 5gb striped logical volume across 2 physical volumes on the volume group VG_One

```shell
# lvcreate -L 5gb -i 2 -n StripedLV_One VG_One
  Using default stripesize 64.00 KiB.
  Logical volume "StripedLV_One" created.
# lvs
  LV            VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  LV_One        VG_One -wi-a----- 100.00m
  LV_Two        VG_One -wi-ao----   2.00g
  StripedLV_One VG_One -wi-a-----   5.00g
  ```

## RAID Logical Volume
Raid can be created and activated exclusively on one machine. If you require a cluster mirrors volumes will need to be used. 
### Raid1 
When you create a RAID logical volume, LVM creates a metadata subvolume that is one extent in size for every data or parity subvolume in the array. 

This can be seen with the `lvs -a` showing 2 addtional subvolumes named raid1vg_meta0/1
You can specify how many mirrors to create with the `-m #` command. One is created by default. 


```shell
# lvcreate --type raid1 -L 100M -n raid1vg VG_One
  Logical volume "raid1vg" created.

# lvs -a
  LV                 VG     Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  LV_One             VG_One -wi-a----- 100.00m
  LV_Two             VG_One -wi-ao----   2.00g
  StripedLV_One      VG_One -wi-a-----   5.00g
  raid1vg            VG_One rwi-a-r--- 100.00m                                    100.00
  [raid1vg_rimage_0] VG_One iwi-aor--- 100.00m
  [raid1vg_rimage_1] VG_One iwi-aor--- 100.00m
  [raid1vg_rmeta_0]  VG_One ewi-aor---   4.00m
  [raid1vg_rmeta_1]  VG_One ewi-aor---   4.00m
```

## Creating Mirrored Volumes
 While RAID logical volumes can be created and activated exclusively on one machine, they cannot be activated simultaneously on more than one machine. If you require non-exclusive mirrored volumes, you must create the volumes with a mirror segment type, as described in this section.

