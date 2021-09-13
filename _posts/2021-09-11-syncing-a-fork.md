---
layout: post
title: "同步 fork"
categories: ["技术"]
tags: ["Git"]
---

* 目录
{:toc .toc}




从上游同步 fork 必须在 Git 中配置指向上游远程仓库的地址。[^1]

1. 打开 Git Bash

2. 切换到项目目录

3. 从上游仓库 fetch 分支及各分支相应的提交，提交到分支 **BRANCHNAME** 的提交会存储在本地分支 **upstream/BRANCHNAME** 中

   ```bash
   $ git fetch upstream
   > remote: Counting objects: 75, done.
   > remote: Compressing objects: 100% (53/53), done.
   > remote: Total 62 (delta 27), reused 44 (delta 9)
   > Unpacking objects: 100% (62/62), done.
   > From https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY
   >  * [new branch]      main     -> upstream/main
   ```

4. 切换到本地分支，示例中使用的是 **main**

   ```bash
   $ git checkout main
   > Switched to branch 'main'
   ```

5. 合并上游分支改变到本地分支，而不会丢失本地的修改，示例中使用的是 upstream/main

   ```bash
   $ git merge upstream/main
   > Updating a422352..5fdff0f
   > Fast-forward
   >  README                    |    9 -------
   >  README.md                 |    7 ++++++
   >  2 files changed, 7 insertions(+), 9 deletions(-)
   >  delete mode 100644 README
   >  create mode 100644 README.md
   ```

   如果你的本地分支没有能够引起冲突的提交，Git 会使用 "fast-forward"：

   ```bash
   $ git merge upstream/main
   > Updating 34e91da..16c56ad
   > Fast-forward
   >  README.md                 |    5 +++--
   >  1 file changed, 3 insertions(+), 2 deletions(-)
   ```

注：同步 fork 只会更新本地仓库，更新远程代码库必须使用 push 命令。








[^1]: [同步 fork](https://docs.github.com/en/github/collaborating-with-pull-requests/working-with-forks/syncing-a-fork){:target="_blank"}

