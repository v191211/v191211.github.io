---
layout: post
title: "使用 grep 命令查找包含指定文本的所有文件"
categories: ["技术"]
tags: ["Linux 命令", "grep"]
---

* 目录
{:toc .toc}

```bash
$ grep -rnw '/path/to/somewhere/' -e 'pattern'
```

> **-r** 或 **-R** 表示递归查找。
> **-n** 展示行号。
> **-w** 完全匹配（匹配整个单词）。
> **-l** （小写 L）只显示文件名，而不是结果本身。
> **i** 忽略大小写（可选）。

和标签 **--exclude**、**--include**、**--exclude-dir** 一起使用可以更有效的查找：[^1]

只搜索扩展名为 .c 或 .h 的文件：

```bash
$ grep --include=\*.{c,h} -rnw '/path/to/somewhere/' -e "pattern"
```

排除所有扩展名为 .o 的文件：

```bash
$ grep --exclude=\*.o -rnw '/path/to/somewhere/' -e "pattern"
```

通过 **--exclude-dir** 参数可以排除指定的目录。
例如，下面的命令将排除目录 dir1/、dir2/ 和所有 *.dst/ 目录。

```bash
$ grep --exclude-dir={dir1,dir2,*.dst} -rnw '/path/to/somewhere/' -e "pattern"
```

也可以从根目录开始查找：

```bash
$ grep -Ril "text-to-find-here" /
```

这些命令能够出色的完成任务，同样能够帮你完成类似的搜索。
更多选项可以查看 man 命令手册 **man grep**



[^1]: [Linux 中查找包含指定文本的所有文件](https://stackoverflow.com/a/16956844/4612522){:target="_blank"}
