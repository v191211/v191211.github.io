---
layout: post
title: 备份和恢复 Windows10 驱动
categories: 技术
tags: ["Windows10 驱动"]
---

* 目录
{:toc .toc}
重新安装的 Windows 需要安装驱动，而有些驱动厂家可能已经不再提供了。

因此，重新安装系统前最好备份一份驱动。之后根据需要进行恢复。

该教程为你展示如何备份和恢复 Windows10 驱动。

> 你必须以管理员身份登录才能够进行备份和恢复驱动的操作。

## 命令行方式备份驱动

> 更多 `dism /export-driver` 命令使用细节，参考[DISM 驱动服务命令行选项](https://docs.microsoft.com/en-us/windows-hardware/manufacture/desktop/dism-driver-servicing-command-line-options-s14)。
>
> 更多 `PnPUtil` 命令使用详情，查看[PnPUtil 命令语法](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/pnputil-command-syntax)。

1. 以管理员身份打开命令提示符。

2. 输入下面两个命令中的任何一个，按回车键（如下图）。

   ```bash
   dism /online /export-driver /destination:"full path of folder"
   ```

   或者

   ```bash
   pnputil /export-driver * "full path of folder"
   ```

   > 用你想要保存驱动备份的文件夹全路径替换掉命令行中的 full path of folder（例如 "F:\Drivers Backup"）。
   >
   > 如果这个文件夹不存在，你需要先创建它然后再执行命令。
   >
   > 例如： 
   >
   > ```bash
   > dism /online /export-driver /destination:"F:\Drivers Backup"
   > ```
   
   ![备份驱动命令]({{ site.url }}/assets/images/posts/Backup_drivers_command.png "备份驱动命令")




3. 导入完成后可以关闭命令提示符。

4. 现在，驱动已经被导出到之前指定的文件夹中（本例中指定的文件夹位置是 "F:\Drivers Backup"），如下图。

   ![备份的驱动]({{ site.url }}/assets/images/posts/Drivers_Backup.png "备份的驱动")


## PowerShell 中备份驱动

> 更多 `Export-WindowsDriver` 命令使用方法，查看[Export-WindowsDriver](https://docs.microsoft.com/en-us/powershell/module/dism/export-windowsdriver?view=win10-ps)。

1. 以管理员身份打开 PowerShell。

2. 输入下面命令，按回车键（如下图）。

   ```bash
   Export-WindowsDriver -Online -Destination "full path of folder"
   ```

   > 用你想要保存驱动备份的文件夹全路径替换掉命令行中的 full path of folder（例如 "F:\Drivers Backup"）。
   >
   > 如果这个文件夹不存在，你需要先创建它然后再执行命令。
   >
   > 例如： 
   >
   > ```bash
   > Export-WindowsDriver -Online -Destination "F:\Drivers Backup"
   > ```

   ![使用PowerShell备份驱动]({{ site.url }}/assets/images/posts/Backup_drivers_PowerShell.png "使用PowerShell备份驱动")

3. 导入完成后可以关闭 PowerShell。

4. 现在，驱动已经被导出到之前指定的文件夹中（本例中指定的文件夹位置是 "F:\Drivers Backup"），如下图。

   ![备份的驱动]({{ site.url }}/assets/images/posts/Drivers_Backup.png "备份的驱动")

## 从设备管理器中恢复备份的驱动

1. 打开设备管理器。

2. 在你想要恢复驱动的设备上点击右键（例如："Intel(R) RealSense(TM) 3D Camera (Front F200) Depth"），然后选择 更新驱动（如下图）。

   ![从设备管理器恢复驱动(图1)]({{ site.url }}/assets/images/posts/Restore_Drivers-1.png "从设备管理器恢复驱动(图1)")

3. 点击手动安装驱动（如下图）。

   ![从设备管理器恢复驱动(图2)]({{ site.url }}/assets/images/posts/Restore_Drivers-2.png "从设备管理器恢复驱动(图2)")
   
4. 按照下面步骤选择你第一步或第二步中存放备份文件的文件夹（本例中是 "F:\Drivers Backup"）如下图。

   1. 点击浏览按钮。

   2. 选择存放备份的文件夹（例如： "F:\Drivers Backup"）。

   3. 点击确定。

   4. 勾选中包含子文件夹选项。

   5. 点击下一步。

      ![从设备管理器恢复驱动(图3)]({{ site.url }}/assets/images/posts/Restore_Drivers-3.png "从设备管理器恢复驱动(图3)")
   
5. 设备管理器会搜索并安装驱动（如下图）。

   ![从设备管理器恢复驱动(图4)]({{ site.url }}/assets/images/posts/Restore_Drivers-4.png "从设备管理器恢复驱动(图4)")

6. 恢复完驱动后，可以关闭设备管理器。

## 命令提示符中恢复驱动

> 更多 `PnPUtil` 命令的使用细节，参考：[PnPUtil 命令语法](https://docs.microsoft.com/en-us/windows-hardware/drivers/devtest/pnputil-command-syntax)

1. 以管理员身份打开命令提示符。

2. 输入下面命令，按回车键（如下图）。

   ```bash
   pnputil /add-driver "full path of folder\*.inf" /subdirs /install /reboot
   ```

   > 用备份驱动文件夹的路径替换掉上面命令行中的 "full path of folder"（本例中保存驱动的文件夹路径为："E:\Drivers Backup"），例如：
   >
   > ```bash
   > pnputil /add-driver "E:\Drivers Backup\*.inf" /subdirs /install /reboot
   > ```

   > 命令提示符中的 /reboot 选项的意思是如果恢复驱动时需要重启电脑，会自动进行重启。
   >
   > 因此确保已经保存好重要的数据再执行这条命令。

3. 导入（恢复）完成后，可以关闭命令提示符。

   ![从命令提示符恢复驱动(图1)]({{ site.url }}/assets/images/posts/pnputil-1.png "从命令提示符恢复驱动(图1)")

   ![从命令提示符恢复驱动(图2)]({{ site.url }}/assets/images/posts/pnputil-2.png "从命令提示符恢复驱动(图2)")



## 使用批处理文件(BAT)备份和恢复驱动

1. 点击下载下面的 BAT 文件。

   [Backup_or_Restore_Device_Drivers.bat]({{ site.url }}/assets/attachments/posts/Backup_or_Restore_Device_Drivers.bat)
   
2. 保存 .bat 文件到指定位置。

3. 右键以管理员身份运行 .bat 文件。

4. 如果提示用户账户控制（UAC），点击确定按钮。

5. 输入数字执行相应的操作（如下图）。

   ![批处理备份恢复驱动(图1)]({{ site.url }}/assets/images/posts/backup_1.jpg "批处理备份恢复驱动(图1)")
   
6. 选择你要保存驱动的位置或用于恢复驱动的文件夹位置（如下图）。

   ![批处理备份恢复驱动(图2)]({{ site.url }}/assets/images/posts/backup_2.jpg "批处理备份恢复驱动(图2)")


---

原文：

[Windows 10 中备份和恢复驱动](https://www.tenforums.com/tutorials/68426-backup-restore-device-drivers-windows-10-a.html?__cf_chl_jschl_tk__=902b09e017c3e8505666fd449df6e88d35e4b455-1586331189-0-Ac0NqielX0rjxU4-NjXDgvIX2iknQNyXieJJmZb-Z73t0a9y-91p6TlHBI0aRI8nuueVFP2HwBpXIpcusketMHVG5iTxIs2G-MIiyVbh62uE2BEGsiPLxOUgPoz_GQhQgJH1EiFXr2iSpyDLkXH8KxS4WCfZYQkZbVkKfOh69RNdAnlRoRxl_adPBCHIiBGGxISG67WXw18rT73ON93Vs68FGzkcA-lkBgPApEx4dEhyeSR6W9LNnM5AvUaXnyHhlK4lRScQ2MyjE0ND__2HbvJ9ikTw4PI_I_fpJAkMu27eTIWCpt9ukn4CC3ec--6T1ieUP6uWA6Kg7Kzj423JsA_TatI65Pv7l8T5mvvkI3OD)

