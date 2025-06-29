---
created: 2024-12-12 09:17
updated: 2025-06-10 09:54
dg-publish: true
tags:
  - garden/🌱Seedling
---

## 问题描述

![40971D89-A7F4-4711-BFE6-4011BE405E12_1_105_c.jpeg](https://picbed.guoyingwei.top/2024/12/40971D89-A7F4-4711-BFE6-4011BE405E12_1_105_c.jpeg)

Error mounting /dev/sdb1 at /run/media/xxy/My Book: Filesystem type exfat not configured in kernel.


## 解决方法

### CentOS
CENTOS 默认支持FAT32格式，不支持exfat，NTFS的处理办法（需联网）

exfat

1.安装Nux Dextop库：
yum install -y http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm  (也可先下载好文件再安装)
2.安装exfat支持库文件
yum install exfat-utils fuse-exfat

NTFS:
yum -y install epel-release
yum -y install ntfs-3g

### Ubuntu

原因：在ubuntu下，由于版权的原因，默认不支持exfat格式的u盘，不过可以很方便就能添加对exfat的支持：

对于ubuntu 14.04版本，直接运行下面的命令就可以了： 

sudo apt-get install exfat-utils

安装完之后重启生效。

对于ubuntu 12.04 ~ 13.10的版本，分别： 

sudo add-apt-repository ppa:relan/exfat   
sudo apt-get update   
sudo apt-get install exfat-utils fuse-exfat

同样，重启电脑生效。