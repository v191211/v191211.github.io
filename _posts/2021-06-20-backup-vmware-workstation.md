---
layout: post
title: 备份 VMware Workstation 虚拟机的最佳做法
categories: 技术
tags: ["VMware Workstation"]
---

* 目录
{:toc .toc}

## 操作步骤

1. 确保虚拟机电源处于已关闭状态。
2. 找到虚拟机文件夹。
   1. 右键虚拟机 > Settings，或者选择虚拟机 点击菜单栏中的 VM > Settings。
   2. 点击 Options。
   3. 点击 Advanced，在 File locations 下面有 Configuration 和 Log，Configuration 就是虚拟机文件夹的位置。
3. 复制虚拟机文件夹，粘贴到要存储备份文件夹的位置。
4. 复制虚拟机的硬盘文件，粘贴到要存储备份虚拟机硬盘文件的位置。
5. 点击菜单栏中的 File > Open，找到刚才备份的文件夹位置，选中 vmx 文件。
6. 右键虚拟机 > Settings，或者选择虚拟机 点击菜单栏中的 VM > Settings。
7. 选中 Hard Disk > Remove。
8. Add > Hard Disk > Next > SCSI > Next > Use an existing virtual disk > Next > 找到刚才备份虚拟机硬盘文件的位置，选中 vmdk 文件 > Finish。
9. 启动时会询问是移动还是复制的虚拟机，选复制的选项。

---

参考：

[备份 VMware Workstation 虚拟机的最佳做法 (2006202)](https://kb.vmware.com/s/article/2006202?lang=zh_CN)

[找出虚拟机文件的位置 (1003880)](https://kb.vmware.com/s/article/1003880)

