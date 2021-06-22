---
layout: post
title: 生成 SSH Key
categories: 技术
tags: ["SSH Key"]
---

* 目录
{:toc .toc}


1. 打开 Git Bash。

2. 粘贴下面代码，替换为自己的邮箱。

   ```bash
   $ ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

   > 注意：如果使用的系统版本较低，不支持 Ed25519 算法，使用下面代码：
   >
   > ```bash
   > $ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   > ```
   >
   > 

   这样就使用提供的邮箱创建了一个新的 key。

   ```bash
   > Generating public/private ed25519 key pair.
   ```

3. 提示”输入一个路径保存 key“时，直接按回车键，会将 key 保存到默认位置。

   ```bash
   > Enter a file in which to save the key (/c/Users/you/.ssh/id_ed25519):[Press enter]
   ```

4. 输入密码，直接回车表示不设置密码。

   ```bash
   > Enter passphrase (empty for no passphrase): [Type a passphrase]
   > Enter same passphrase again: [Type passphrase again]
   ```



---

参考：

[生成 ssh key 并添加到 ssh-agent](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

