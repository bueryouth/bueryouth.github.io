


@[TOC](Linux新建用户，并挂载空余分区)

## 示例说明
 - 以Centos6系统为例
 - 在root管理权限下进行

## 新建用户

 - 使用 **ls** 查看已有用户
  ```cs
  [root@buer ~]# ls /home/
  buer
  ```
 - 新建用户 **bueryouth** 
  ```cs
  [root@buer ~]# adduser -d /home/bueryouth -m bueryouth
  [root@buer ~]# ls /home/
  buer  bueryouth
  ```
  *此命令创建了一个用户bueryouth，其中 -d 指定用户主目录，如果此目录不存在，则同时使用 -m 创建主目录。*
 - 使用 **ls** 查看用户
  ```cs
  [root@buer ~]# ls /home/
  buer  bueryouth
  ```
 - 复制配置文件
*由于缺少用户登入需要的环境配置文件.bash_profile .bashrc等，需要复制一份*
 ```cs
[root@buer ~]# cp -a /etc/skel/. /home/bueryouth/
 ```
## 查看分区


 - 查看当前分区情况 **df -h**

  ```cs
  [root@buer ~]# df -h
  Filesystem                   Size  Used Avail Use% Mounted on
  /dev/mapper/vg_buer-lv_root  22G  4.5G   17G  22% /
  tmpfs                        931M   72K  931M   1% /dev/shm
  /dev/sda1                    477M   41M  411M   9% /boot
  ```

 - 判断是否有剩余空间
  ```cs
  [root@buer ~]# fdisk -l
  Disk /dev/sda: 29.0 GB, 28991029248 bytes  # 查看总空间大小
  255 heads, 63 sectors/track, 3524 cylinders
  Units = cylinders of 16065 * 512 = 8225280 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disk identifier: 0x0001812f

     Device Boot      Start         End      Blocks   Id  System
  /dev/sda1   *           1          64      512000   83  Linux
  Partition 1 does not end on cylinder boundary.
  /dev/sda2              64        3264    25701376   8e  Linux LVM
  # 查看已用空间，是否有剩余，如果空间不够，需要扩展磁盘
  ······
  ```
 
## 新建分区
 - 输入 **fdisk [磁盘]** 命令开始分区（一定要谨慎操作）
  ```cs
  [root@buer ~]# fdisk /dev/sda

  WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off the mode (command 'c') and change display units to
           sectors (command 'u').

  Command (m for help): 
  ```
  
 - 使用 **fdisk** 新建分区
 1. 输入 **n** ，新建磁盘
 2. 输入 **p** ，新建扩展分区
 3. 输入 **1-4的数字** ，使用第几个主分区（若冲突了就往后换）
 4. 直接回车，选择默认
 5. 设置分区大小,例：+1G（设置1GB大小，注意单位） 
 6. 输入 **w** ，保存配置并退出（若操作有误，输入q,不保存退出）
  ```cs
  [root@buer ~]# fdisk /dev/sda
  WARNING: DOS-compatible mode is deprecated. It's strongly recommended to
           switch off the mode (command 'c') and change display units to
           sectors (command 'u').

  Command (m for help): n
  Command action
     e   extended
     p   primary partition (1-4)
  p
  Partition number (1-4): 3
  First cylinder (3264-3524, default 3264): 
  Using default value 3264
  Last cylinder, +cylinders or +size{K,M,G} (3264-3524, default 3524): +1G

  Command (m for help): w
  The partition table has been altered!

  Calling ioctl() to re-read partition table.

  WARNING: Re-reading the partition table failed with error 16: 设备或资源忙.
  The kernel still uses the old table. The new table will be used at
  the next reboot or after you run partprobe(8) or kpartx(8)
  Syncing disks.
  ```
  
 - 使用 **fdisk -l** 查看新的分区（此时df -h不显示）
  ```cs
  [root@buer ~]# fdisk -l

  Disk /dev/sda: 29.0 GB, 28991029248 bytes
  255 heads, 63 sectors/track, 3524 cylinders
  Units = cylinders of 16065 * 512 = 8225280 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disk identifier: 0x0001812f

     Device Boot      Start         End      Blocks   Id  System
  /dev/sda1   *           1          64      512000   83  Linux
  Partition 1 does not end on cylinder boundary.
  /dev/sda2              64        3264    25701376   8e  Linux LVM
  /dev/sda3            3264        3395     1055937+  83  Linux
  # /dev/sda3是新建的分区
  ······
  ```
 - 格式化分区（可根据需求决定文件系统的格式,这里以ext3为例子）
  ```cs
  [root@buer ~]# mkfs.ext3 /dev/sda3
  mke2fs 1.41.12 (17-May-2010)
  文件系统标签=
  操作系统:Linux
  块大小=1024 (log=0)
  分块大小=1024 (log=0)
  Stride=0 blocks, Stripe width=0 blocks
  51200 inodes, 204492 blocks
  10224 blocks (5.00%) reserved for the super user
  第一个数据块=1
  Maximum filesystem blocks=67371008
  25 block groups
  8192 blocks per group, 8192 fragments per group
  2048 inodes per group
  Superblock backups stored on blocks: 
    8193, 24577, 40961, 57345, 73729

  正在写入inode表: 完成                            
  Creating journal (4096 blocks): 完成
  Writing superblocks and filesystem accounting information: 完成

  This filesystem will be automatically checked every 27 mounts or
  180 days, whichever comes first.  Use tune2fs -c or -i to override.
  ```
  
## 挂载分区

 - 使用  **mount**   [分区]   [用户]挂载分区 
  ```cs
  [root@buer ~]# mount /dev/sda3 /home/bueryouth
  ```
  
 - 使用 **df -h** 查看当前分区挂载情况
  ```cs
  [root@buer ~]# df -h
  Filesystem            Size  Used Avail Use% Mounted on
  /dev/mapper/vg_buer-lv_root
                         22G  4.5G   17G  22% /
  tmpfs                 931M   72K  931M   1% /dev/shm
  /dev/sda1             477M   41M  411M   9% /boot
  /dev/sda3             194M  5.6M  178M   4% /home/bueryouth 
  ```
 - 设置开机自动挂载
```cs
[root@buer ~]# echo '/dev/sda3 /home/bueryouth ext3 defaults 0 0' >> /etc/fstab
```
 - 或者手动编辑
 ```cs
 [root@buer ~]# vi /etc/fstab
 ```
  
推荐Linux命令网站：[Linux命令大全(手册)](https://www.linuxcool.com/)
