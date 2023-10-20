---
layout: post
title: "创建 tar.gz 时忽略文件和目录"
categories: ["技术"]
tags: ["Linux 命令", "tar"]
---

* 目录
{:toc .toc}
## 快速使用[^1]

```bash
$ tar --exclude='./folder' --exclude='./upload/folder2' -zcvf /backup/filename.tgz .
```

## 1. 概述

在 Linux 和 类 Unix 系统中，tar 命令用于打包文件。[^2]

它能打包成多种格式，像是 .tar, .tar.gz, .cpio, .tar.bz2, .zip, .rar 等。

打包成 .tar.gz 格式时使用的是 gzip 算法，打包成 .tar.bz2 格式时使用 bzip 算法。

这篇教程介绍了打包 .tar.gz 文件时以不同的方式排除 1 个或多个文件、目录。

## 2. 环境准备

使用 mkdir 命令创建名称为 parent_directory 的目录，目录中存放教程要用到的文件和目录。

```bash
mkdir parent_directory
```

进入到该目录。

```bash
cd parent_directory
```

使用 touch 命令创建 3 个文件：

```bash
touch file1.txt file2.txt file3.txt
```

创建 3 个目录：

```bash
mkdir folder1 folder2 folder3
```

执行完上面的命令，的目录结构如下：

```bash
ls
file1.txt file3.txt folder2
file2.txt folder1	folder3
```

## 3. 使用 -exclude 选项

tar -exclude 选项基础语法：

```bash
tar --exclude="pattern" [options] [archive_name] [path]
```

打包 .tar.gz 时使用 -exclude 选项跳过文件或目录。

```bash
tar --exclude='file1.txt' -zcvf backup.tar.gz .
./
./folder3/
./file3.txt
./folder1/
./file2.txt
./folder2/
tar: .: file changed as we read it
```

一起来分析一下这个命令：

* -z 使用 gzip 算法压缩文件和目录
* -c 创建新的打包文件
* -v 显示打包文件和目录的过程
* -f 给打包的文件起一个名字
* --exclude 打包时排除 file1.txt 文件

最后的 . 代表当前工作目录，里面包含了我们需要打包的文件。

**排除目录时，不能用正斜线 / 结尾。**

**我们看到消息 "file changed as we read it"，是因为 backup.tar.gz 文件和被打包的文件在同一目录中。**

使用 -v 选项可以看到上面命令执行时跳过了 file1.txt 文件。

使用以下命令列出 backup.tar.gz 文件中的内容而无需解压。

```bash
tar -tf backup.tar.gz
./
./folder3/
./file3.txt
./folder1/
./file2.txt
./folder2/
```

-t 选项列出打包文件中的内容。

### 3.1 排除多个文件和目录

使用多个 --exclude 选项排除多个文件或目录。

```bash
tar --exclude='file1.txt' --exclude='folder1' -zcvf backup.tar.gz .
./
./folder3/
./file3.txt
./file2.txt
./folder2/
tar: .: file changed as we read it
```

也可以用下面这种方式：

```bash
tar --exclude={"file1.txt", "file2.txt"} -zcvf backup.tar.gz .
file3.txt
folder3
folder2
folder1
```

花括号这种方式在 Bash 方法中可能会有问题， --exclude 选项可能无法正确工作。

### 3.2 排除指定扩展名文件

通过匹配正则表达式可以排除指定扩展名文件：

```bash
tar --exclude='*.txt' -zcvf backup.tar.gz .
./
./folder3/
./folder1/
./folder2/
tar: .: file changed as we read it
```

上面的命令跳过所有扩展名是 .txt 的文件。

同样因为 backup.tar.gz 文件和被打包的文件在同一目录中，返回了 "file changed as we read it" 消息。

tar 命令内置了一些选项可以忽略一些自动生成的文件，下面是一些选项和他们的行为：

* --exclude-backups 排除所有 backup 和 lock 文件
* --exclude-cashes 排除所有有 CACHEDIR.TAG 标签的目录，不包括标签本身
* --exclude-vcs 排除所有版本控制文件
* --exclude-vcs-ignores 排除所有版本控制系统中忽略的文件，例如在 .gitignore 中列出的文件、目录以及扩展名都将被排除，包括 .gitignore 文件本身也会被排除。

## 4. 使用排除文件

使用 tar 命令打包或解压文件时，可以提供一个文件，文件中包含需要排除的文件、目录。这个文件叫做排除文件。

下面是打包时使用排除文件忽略指定文件。

首先，创建一个名称为 exclude_file.txt 的排除文件：

```bash
touch exclude_file.txt
```

接着添加需要被排除的文件或目录名，以行进行分割。

```bash
file1.txt
folder3
file2.txt
```

排除包含在 exclude_file.txt 文件中的项目。

```bash
tar -zcvf backup.tar.gz -X exclude_file.txt .
./
./folder1/
./folder2/
./exclude_file.txt
./file3.txt
tar: .: file changed as we read it
```

**使用 -X 选项接收一个排除文件。**它是 --exclude-from 选项的简写形式：

```bash
tar -zcvf backup.tar.gz --exclude-from="exclude_file.txt" .
```

排除文件中同样可以使用正则表达式。

改变一些 exclude_file.txt 文件内容：

```bash
*.txt
```

再次打包文件：

```bash
tar -zcfv backup.tar.gz --exclude-from="exclude_file.txt" .
./
./folder3/
./folder2/
./folder1/
```

可以看到上面的命令跳过了扩展名是 .txt 的文件。

## 5. 总结

这篇文章介绍了打包 .tar.gz 文件时排除一些指定文件或目录的方法。

打包小型目录时 --exclude 选项很实用，而打包大型目录时使用排除文件则很方便。



[^1]: [tar 命令打包时忽略某些文件、目录](https://stackoverflow.com/questions/984204/shell-command-to-tar-directory-excluding-certain-files-folders){:target="_blank"}

[^2]: [打包 tar.gz 文件时排除指定文件和目录](https://www.baeldung.com/linux/tar-exclude-files-directories){:target="_blank"}