---
layout: post
title: SVN stuck due to previous operation has not finished
categories: 技术
tags: SVN
---

* 目录
{:toc .toc}

下载 [SQLite](http://www.sqlite.org/download.html)

执行下面命令：

$ `sqlite3.exe <svn_project_path>/.svn/wc.db "delete from work_queue"`

原文：[Subversion stuck due to “previous operation has not finished”?](https://stackoverflow.com/a/22717607/4612522)

