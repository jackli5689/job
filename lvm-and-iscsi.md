#lvm
<pre>
centos7添加一块新硬盘，可使用命令让电脑无需重启即可读取硬盘：
[root@salt-server /sys/class/scsi_host]# echo "- - -" > /sys/class/scsi_host/host0/scan
[root@salt-server /sys/class/scsi_host]# echo "- - -" > /sys/class/scsi_host/host1/scan
[root@salt-server /sys/class/scsi_host]# echo "- - -" > /sys/class/scsi_host/host2/scan
扫描SCSI总线添加设备，有多少条SCSI总线就扫描多少次，脚本如下：
---------------
#!/usr/bin/bash

scsisum=`ll /sys/class/scsi_host/host*|wc -l`

for ((i=0;i<${scsisum};i++))
do
    echo "- - -" > /sys/class/scsi_host/host${i}/scan
done
-------------------

1.新建分区并更改分区类型为lvm
2.新建物理卷（physical volume）
[root@salt-server /sys/class/scsi_host]# pvcreate /dev/sdb{1,2}
  Physical volume "/dev/sdb1" successfully created.
  Physical volume "/dev/sdb2" successfully created.
3.详细查看新建的物理卷（pvs也可简要查看）
[root@salt-server /sys/class/scsi_host]# pvdisplay
  "/dev/sdb2" is a new physical volume of "30.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb2
  VG Name
  PV Size               30.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               4NprMw-yzO8-9UUc-rxrD-IhPi-toUj-qRA5qy

  "/dev/sdb1" is a new physical volume of "30.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name
  PV Size               30.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               uQnOMJ-zjnx-5JiB-V7bB-WycM-wfZj-dmoEr7

4.新建卷组（volume group）:用于把物理卷组成一个组
[root@salt-server /sys/class/scsi_host]# vgcreate -s 16M myvg /dev/sdb{1,2}
  Volume group "myvg" successfully created
5.详细查看卷组：
[root@salt-server /sys/class/scsi_host]# vgdisplay
  --- Volume group ---
  VG Name               myvg
  System ID
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               <59.97 GiB
  PE Size               16.00 MiB
  Total PE              3838
  Alloc PE / Size       0 / 0
  Free  PE / Size       3838 / <59.97 GiB
  VG UUID               SuxdA7-l9lO-oO8n-QinG-ogil-fxzL-8dlJPP

6.新建逻辑卷（logical volume）:类似新加的硬盘一样
[root@salt-server /sys/class/scsi_host]# lvcreate -L 50G -n mylv myvg
  Logical volume "mylv" created.
7.详细查看逻辑卷：
[root@salt-server /sys/class/scsi_host]# lvdisplay
  --- Logical volume ---
  LV Path                /dev/myvg/mylv
  LV Name                mylv
  VG Name                myvg
  LV UUID                963Eh1-FWn9-8HfI-7Zou-NwOd-LyEF-pdkJHg
  LV Write Access        read/write
  LV Creation host, time salt-server, 2019-01-10 16:28:09 +0800
  LV Status              available
  # open                 0
  LV Size                50.00 GiB
  Current LE             3200
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0
8.查看新添加的逻辑卷
[root@salt-server /sys/class/scsi_host]# fdisk -l

Disk /dev/sda: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000af649

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   209715199   103808000   83  Linux

Disk /dev/sdb: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xa6d30b18

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    62916607    31457280   8e  Linux LVM
/dev/sdb2        62916608   125831167    31457280   8e  Linux LVM

Disk /dev/mapper/myvg-mylv: 53.7 GB, 53687091200 bytes, 104857600 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
注：/dev/mapper/myvg-mylv为新添加的逻辑卷

9.格式化逻辑卷：
[root@salt-server /sys/class/scsi_host]# mkfs.ext4 /dev/mapper/myvg-mylv
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
3276800 inodes, 13107200 blocks
655360 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2162163712
400 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

10.挂载逻辑卷：
[root@salt-server /sys/class/scsi_host]# mount /dev/mapper/myvg-mylv /lvm

11.增加逻辑卷容量：(lvresize -l +179 /dev/myvg/mylv也行)
[root@salt-server ~]# lvextend -L +5G /dev/myvg/mylv
  Size of logical volume myvg/mylv changed from 50.00 GiB (3200 extents) to 55.00 GiB (3520 extents).
  Logical volume myvg/mylv successfully resized.

12.写入文件系统，使扩容生效
[root@salt-server ~]# resize2fs /dev/myvg/mylv
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/myvg/mylv is mounted on /lvm; on-line resizing required
old_desc_blocks = 7, new_desc_blocks = 7
The filesystem on /dev/myvg/mylv is now 14417920 blocks long.

13.另外一种方式增加逻辑容量：(lvresize -l +179 /dev/myvg/mylv也行)
[root@salt-server ~]# lvextend -l +100%FREE /dev/myvg/mylv
  Size of logical volume myvg/mylv changed from 55.00 GiB (3520 extents) to <59.97 GiB (3838 extents).
  Logical volume myvg/mylv successfully resized.

[root@salt-server ~]# resize2fs /dev/myvg/mylv
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/myvg/mylv is mounted on /lvm; on-line resizing required
old_desc_blocks = 7, new_desc_blocks = 8
The filesystem on /dev/myvg/mylv is now 15720448 blocks long.

[root@salt-server ~]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/sda2               99G  2.0G   97G   3% /
devtmpfs               1.9G     0  1.9G   0% /dev
tmpfs                  1.9G   28K  1.9G   1% /dev/shm
tmpfs                  1.9G  120M  1.8G   7% /run
tmpfs                  1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/sda1             1014M  140M  875M  14% /boot
/dev/mapper/myvg-mylv   59G   52M   56G   1% /lvm
tmpfs                  380M     0  380M   0% /run/user/0

新增硬盘扩容：
14.对新增硬盘进行分一个区，并指定类型为8e
15.新建物理卷：
[root@salt-server /sys/class/scsi_host]# pvcreate /dev/sdc1
  Physical volume "/dev/sdc1" successfully created.
16.扩展卷组容量：
[root@salt-server /sys/class/scsi_host]# vgextend myvg /dev/sdc1
  Volume group "myvg" successfully extended
17.扩展逻辑卷容量：
[root@salt-server /sys/class/scsi_host]# lvextend -l +100%FREE /dev/myvg/mylv
  Size of logical volume myvg/mylv changed from <59.97 GiB (3838 extents) to 109.95 GiB (7037 extents).
  Logical volume myvg/mylv successfully resized.
18.写入文件系统：
[root@salt-server /sys/class/scsi_host]# resize2fs  /dev/myvg/mylv
19.对比查看：
[root@salt-server /sys/class/scsi_host]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/sda2               99G  2.0G   97G   3% /
devtmpfs               1.9G     0  1.9G   0% /dev
tmpfs                  1.9G   28K  1.9G   1% /dev/shm
tmpfs                  1.9G  120M  1.8G   7% /run
tmpfs                  1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/sda1             1014M  140M  875M  14% /boot
/dev/mapper/myvg-mylv  148G   60M  141G   1% /lvm
tmpfs                  380M     0  380M   0% /run/user/0

[root@salt-server /sys/class/scsi_host]# fdisk -l

Disk /dev/sda: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000af649

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200   209715199   103808000   83  Linux

Disk /dev/sdb: 107.4 GB, 107374182400 bytes, 209715200 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xa6d30b18

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb1            2048    62916607    31457280   8e  Linux LVM
/dev/sdb2        62916608   125831167    31457280   8e  Linux LVM
/dev/sdb3       125831168   209715199    41942016   8e  Linux LVM

Disk /dev/mapper/myvg-mylv: 161.0 GB, 160994164736 bytes, 314441728 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes


Disk /dev/sdc: 53.7 GB, 53687091200 bytes, 104857600 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0xb3b4092e

   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1            2048   104857599    52427776   8e  Linux LVM

注意：sdb和sdc两个实际容量为100G和50G，我这里是虚拟机所以读取有误差，LVM只持在线扩展，不支持在线压缩

#压缩容量：
1.[root@salt-server /]# umount /lvm/
2.[root@salt-server /]# resize2fs /dev/myvg/mylv 30000M
resize2fs 1.42.9 (28-Dec-2013)
Please run 'e2fsck -f /dev/myvg/mylv' first.
3.[root@salt-server /]# e2fsck -f /dev/myvg/mylv
e2fsck 1.42.9 (28-Dec-2013)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/myvg/mylv: 13/6553600 files (0.0% non-contiguous), 459546/26198016 blocks
4.[root@salt-server /]#  resize2fs /dev/myvg/mylv 30000M
resize2fs 1.42.9 (28-Dec-2013)
Resizing the filesystem on /dev/myvg/mylv to 7680000 (4k) blocks.
The filesystem on /dev/myvg/mylv is now 7680000 blocks long.
5.[root@salt-server /]# mount /dev/myvg/mylv /lvm/
6.[root@salt-server /]# df -h
Filesystem             Size  Used Avail Use% Mounted on
/dev/sda2               99G  2.0G   97G   3% /
devtmpfs               1.9G     0  1.9G   0% /dev
tmpfs                  1.9G   28K  1.9G   1% /dev/shm
tmpfs                  1.9G  120M  1.8G   7% /run
tmpfs                  1.9G     0  1.9G   0% /sys/fs/cgroup
/dev/sda1             1014M  140M  875M  14% /boot
tmpfs                  380M     0  380M   0% /run/user/0
/dev/mapper/myvg-mylv   29G   45M   28G   1% /lvm
7.[root@salt-server /]# lvresize -L -70G /dev/myvg/mylv
  WARNING: Reducing active and open logical volume to <29.94 GiB.
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce myvg/mylv? [y/n]: y
  Size of logical volume myvg/mylv changed from <99.94 GiB (6396 extents) to <29.94 GiB (1916 extents).
  Logical volume myvg/mylv successfully resized.
8.[root@salt-server /lvm]# vgdisplay
  --- Volume group ---
  VG Name               myvg
  System ID
  Format                lvm2
  Metadata Areas        4
  Metadata Sequence No  11
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                2
  Open LV               1
  Max PV                0
  Cur PV                4
  Act PV                4
  VG Size               <149.94 GiB
  PE Size               16.00 MiB
  Total PE              9596
  Alloc PE / Size       5116 / <79.94 GiB
  Free  PE / Size       4480 / 70.00 GiB
  VG UUID               SuxdA7-l9lO-oO8n-QinG-ogil-fxzL-8dlJPP

</pre>


#iscsi