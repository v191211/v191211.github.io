---
layout: post
title: "Linux 查找删除大文件，释放磁盘空间"
categories: ["技术"]
tags: ["Linux"]
---

* 目录
{:toc .toc}
## 查看磁盘占用情况[^1]

```bash
df -h
```

## 查找大文件

```bash
du -h / --max-depth=1 | sort -hr | head -n 10
# --max-depth=1 只展示第一个层级目录或文件
# sort -hr, h: 以人类可阅读方式展示, r: 倒叙排列
# head -n 10 展示10条
```

## 按文件大小删除

```bash
# 测试
# 生成指定大小的文件
# of=文件名
# bs后面的单位可以是 G、M等
# seek 偏移量，例如：2，意思就是产生的文件大小是 800M * 2
dd if=/dev/zero of=test-big1 bs=800M count=0 seek=2

# 查找指定大小范围的文件
# . 当前目录
# + 表示大于，- 表示小于，无符号表示等于
find . -type f -size +100M

# 删除
# {} \; 必须有，而且 {} 和 \; 之间必须有空格
find . -type f -size +500M -exec rm -rf {} \;
```

## 按时间和名称删除

```bash
# -mtime 参数中 0 表示修改时间在 24 小时以内， +X 表示修改时间距今天超过 X 天，-X 表示距今天少于 X 天，无符号表示等于
find . -mtime +10 -name "*.gz" -exec rm -rf {} \;
```

[^1]:[Linux磁盘空间100% 查找并删除大文件](https://blog.csdn.net/CL_YD/article/details/79458092)