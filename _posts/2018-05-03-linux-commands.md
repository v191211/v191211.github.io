---
layout: post
title: "Linux 命令"
categories: ["技术"]
tags: ["Linux命令"]
---

* 目录
{:toc .toc}
## grep 命令[^1]



**使用 grep 命令查找包含指定文本的文件**

```bash
$ grep -rnw '/path/to/somewhere/' -e 'pattern'
```

> **-r** 或 **-R** 表示递归查找。
> **-n** 展示行号。
> **-w** 完全匹配（匹配整个单词）。
> **-l** （小写 L）只显示文件名，而不是结果本身。
> **i** 忽略大小写（可选）。

和标签 **--exclude**、**--include**、**--exclude-dir** 一起使用可以更有效的查找：

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



## netstat 命令

**查看指定端口占用情况**

```bash
$ netstat -anp | grep :<port_number>
```



## SSH 登录后自动切换目录[^2]

1. 登录 Linux。

2. 编辑 ~/.bash_profile 文件。

3. 在文件末尾追加命令：

   ```bash
   $ cd /登陆后/自动切换的目录/路径
   ```

4. 保存并退出。

5. 登出 Linux。

6. 重新登录后会切换到配置的路径。



[^1]: [Linux 中查找包含指定文本的所有文件](https://stackoverflow.com/a/16956844/4612522){:target="_blank"}
[^2]: [SSH 登录后自动切换目录](https://newbedev.com/how-can-i-automatically-change-directory-on-ssh-login){:target="_blank"}

