---
layout: post
title: 客户端使用多个 SSH Keys
categories: 技术
tags: ["SSH Keys"]
---

* 目录
{:toc .toc}
## 生成 SSH Key

参考：[生成 SSH Key]({{ site.url }}/2021/06/22/generating-a-new-ssh-key/)



## 创建配置文件管理 SSH Keys

假设**你公司的 GitHub 账户用户名是 abc1234**，**你个人的 GitHub 账户用户名是 jack1234**。

假设你创建了两个 RSA Keys，分别命名为 **id_rsa_company** 和 **id_rsa_personal**。

你在 **~/.ssh  目录下创建 config 文件**，文件内容如下：

```bash
# Company account
Host company
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_company

# Personal account
Host personal
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_rsa_personal
```

当你要克隆公司 GitHub 账号上的仓库（仓库名为 demo），仓库的 URL 大概是这样：

```bash
Repo URL: git@github.com:abc1234/demo.git
```

你需要在克隆的时候修改上面的仓库 URL 地址为：

```bash
git@company:abc1234/demo.git
```

> **注意 github.com 是如何被替换为我们定义在配置文件中的 "company" 别名的。**

同样，你必须根据配置文件中的别名修改个人账户仓库的 URL 地址。

---

参考：

[客户端使用多个 SSH Keys 最佳实践](https://stackoverflow.com/a/38454037/4612522)

