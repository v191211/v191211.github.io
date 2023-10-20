---
layout: post
title: "定位 JS 的三种方式"
categories: ["技术"]
tags: ["JS"]
---

* 目录
{:toc .toc}
## 通过 Initiator 定位 JS

请求是由 JS 发出的。

例如某些用户登录场景：

请求服务器之前密码已经被 JS 加密，也就是密码由 JS 进行加密后才触发请求服务器动作。

可以通过开发者工具中的 NetWork --> Initiator 查看是哪个 JS 文件中触发了请求服务器的动作，定位 JS。

然后查询发送请求时的关键字，找到相关 JS 代码。

## 通过 Search 关键字定位 JS

通过开发者工具中的 Search（快捷键：Ctrl + Shift + F）搜索请求参数 Key 定位 JS。

## 通过 Event Listeners 定位 JS

通过开发者工具中的 Elements --> Event Listeners 找到相应的监听事件，定位 JS。
