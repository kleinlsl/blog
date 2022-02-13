---
title: VMware 虚拟机给Ubuntu根分区扩容   #文章页面上的显示名称，一般是中文
date: 2020-11-13 09:14:16 #文章生成时间，一般不改，当然也可以任意修改用
---

> 用VMware安装了Ubuntu service 18.04 ，刚开始给了30G磁盘空间，使用发现空间不够了，想对磁盘进行扩容。但是直接在VMware点击扩展后发现Ubuntu容量并没有变化，原因是需要在Ubuntu里面进行处理：对分区进行合并。

<!--more-->

# 1、改变虚拟磁盘大小

* 关闭Ubuntu
* 右键--->设置--->硬盘---->实用工具--->扩展 
* 填入扩展后的分区大小
* 确定

# 2、查看磁盘使用情况

* 开启Ubuntu

* 输入：`df -h`

  ```shell
  klein@klein:~$ df -h
  Filesystem      Size  Used Avail Use% Mounted on
  udev            955M     0  955M   0% /dev
  tmpfs           198M  1.3M  196M   1% /run
  /dev/sda2        30G   14G   15G  48% /
  tmpfs           986M     0  986M   0% /dev/shm
  tmpfs           5.0M     0  5.0M   0% /run/lock
  tmpfs           986M     0  986M   0% /sys/fs/cgroup
  /dev/loop0       98M   98M     0 100% /snap/core/10185
  /dev/loop1      205M  205M     0 100% /snap/microk8s/1710
  /dev/loop2       98M   98M     0 100% /snap/core/10126
  /dev/loop3      205M  205M     0 100% /snap/microk8s/1769
  tmpfs           198M     0  198M   0% /run/user/1000
  ```

  

# 3、在Ubuntu shell进行分区扩容操作

```shell
klein@klein:~$ sudo fdisk /dev/sda

Welcome to fdisk (util-linux 2.31.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.
```

* 打印查看分区划分情况

  ```shell
  Command (m for help): p
  Disk /dev/sda: 40 GiB, 42949672960 bytes, 83886080 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: gpt
  Disk identifier: BB1002CF-D749-4A1F-8DF7-EF0945D4DF0E
  
  Device     Start      End  Sectors Size Type
  /dev/sda1   2048     4095     2048   1M BIOS boot
  /dev/sda2   4096 83886046 83881951  40G Linux filesystem
  ```

* 删除需要扩容的分区，这里是2，完成后不要执行 w 写入

  ```shell
  Command (m for help): d
  Partition number (1,2, default 2): 2
  
  
  Command (m for help): p
  Disk /dev/sda: 100 GiB, 107374182400 bytes, 209715200 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: gpt
  Disk identifier: 0976B291-839E-463D-BD05-936253587234
  
  Device     Start   End Sectors Size Type
  /dev/sda1   2048  4095    2048   1M BIOS boot
  ```

* 创建新分区

  ```shell
  # 创建新分区 ，First和Last sector直接回车默认值，因为我是把剩下所有空闲的空间全部分配到扩容的新分区内
  # 若是部分分配，请在Last sector输入对应的值
  Command (m for help): n
  Partition number (2-128, default 2):
  First sector (4096-209715166, default 4096):
  Last sector, +sectors or +size{K,M,G,T,P} (4096-209715166, default 209715166):
  
  Created a new partition 2 of type 'Linux filesystem' and of size 100 GiB.
  Partition '#2' contains a ext4 signature.
  
  # No
  Do you want to remove the signature? [Y]es/[N]o: n
  ```

* 查看新分区

  ```shell
  Command (m for help): p
  
  Disk /dev/sda: 100 GiB, 107374182400 bytes, 209715200 sectors
  Units: sectors of 1 * 512 = 512 bytes
  Sector size (logical/physical): 512 bytes / 512 bytes
  I/O size (minimum/optimal): 512 bytes / 512 bytes
  Disklabel type: gpt
  Disk identifier: 0976B291-839E-463D-BD05-936253587234
  
  Device     Start       End   Sectors  Size Type
  /dev/sda1   2048      4095      2048    1M BIOS boot
  /dev/sda2   4096 209715166 209711071  100G Linux filesystem
  ```

* 确认无误后 w 写入操作

  ```
  Command (m for help): w
  
  The partition table has been altered.
  Syncing disks.
  ```

* 需要重启

  ```
  klein@klein:~$ sudo reboot
  ```

# 4、执行扩容操作

```shell
# 执行扩容操作
klein@klein:~$ sudo resize2fs /dev/sda2
```

