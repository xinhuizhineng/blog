---
title: "Git常用命令总结"           # 文章标题
author: "常香玉"              # 文章作者
description : "Git常用命令总结"    # 文章描述信息
lastmod: 2023-11-26        # 文章修改日期
date: 2023-11-26T14:36:18+08:00
tags : [                    # 文章所属标签
    "git",
]
categories : [              # 文章所属标签
    "git",
]

---

## 1. 全局与局部配置 (git config)

`git config` 命令的作用是配置 Git 的相关信息。

### 用户名与邮箱配置
**全局配置** (适用于所有项目):
Mac 下可通过终端输入 `cat ~/.gitconfig` 查看。
`git config --global user.name "name"`
`git config --global user.email "email"`

**单个仓库配置** (仅适用于当前项目): 进入项目根目录，输入 cat .git/config 查看。
`git config user.name "name"
git config user.email "email"`

### 查看配置

查看所有配置信息
` git config --list`
查看当前配置的用户名
`git config user.name`
查看当前配置的用户邮箱
`git config user.email `

### 定义命令别名 (Alias)

为了提高效率，可以给常用命令设置简写：
`
git config --global alias.st status   # git st = git status
git config --global alias.co checkout # git co = git checkout
git config --global alias.ci commit   # git ci = git commit
git config --global alias.br branch   # git br = git branch`

## 2. 初始化与远程连接 (git init)
`git init` 用于在当前目录初始化仓库，生成 `.git` 目录。
初始化仓库
`git init`

连接远程项目 (origin 后接仓库地址)
`git remote add origin <git项目链接>`

从远程项目拉取文件 (首次拉取建议加 --rebase)
`git pull --rebase origin master`

推送本地仓库到 Github (首次推送需 -u 绑定上游分支)
`git push -u origin master`

## 3. 查看文件状态 (git status)
红色表示工作区修改未提交到暂存区，绿色表示已提交到暂存区。

`#标准显示
git status
#极简方式显示
git status -s`

状态标识说明：

1. 本地新增的文件
2. 文件的一个新拷贝
3. 本地删除的文件
4. 修改过的文件 (红色：未暂存；绿色：已暂存)
5. 文件名被修改
6. 文件类型被修改
7. 文件未合并 (需解决冲突)
8. 未被 Git 管理的文件
已被修改但未暂存的文件，可用 `git checkout -- fileName` 撤销更改。

## 4. 添加到暂存区 (git add)
`#把所有修改的信息添加到暂存区
git add .
#仅添加被跟踪文件中被修改或删除的信息 (不处理新文件)
git add -u
#添加所有变化 (修改、删除、新增)
git add -A`
已提交到暂存区的文件，可以通过 `git reset HEAD -- fileName` 撤销提交（移出暂存区）。

## 5. 提交代码 (git commit)
将暂存区的修改提交到本地仓库，生成 commit-id。

`#提交暂存区修改
git commit -m "message"
#跳过 git add，直接将工作区修改提交 (不包含新文件)
git commit -a -m "message"
#修改最后一次提交 (用于漏掉文件或修改 commit 信息)
git commit --amend`

## 6. 拉取与获取 (git pull & git fetch)
### git pull
获取远程更新并与本地分支合并。逻辑上等于 git fetch + git merge。

`
#格式: git pull <远程主机名> <远程分支名>:<本地分支名>
#取回远程 dev 分支与本地 master 合并
git pull origin dev:master
#取回远程 dev 分支与当前分支合并
git pull origin dev`
### git fetch
仅将远程更新取回本地，不自动合并。

`
#获取远程 master 分支代码
git fetch origin
#获取远程 master 代码到本地新建的 test 分支
git fetch origin master:test`

## 7. 推送代码 (git push)

`#将本地 master 分支推送到远程
git push origin master
#删除远程 dev 分支
git push origin --delete dev`

## 8. 分支管理 (git branch)

`#查看本地分支
git branch
#查看本地和远程分支
git branch -a
#新建分支
git branch test
#重命名分支 (test 改为 dev)
git branch -m test dev
#删除分支 (本地)
git branch -d dev`

## 9. 切换与撤销 (git checkout)

`# 切换到指定 tag
git checkout v1.0.0
#基于 v1.0.0 创建并切换到 test 分支
git checkout -b test v1.0.0
#放弃工作区所有修改 (慎用)
git checkout .
#放弃指定文件的修改
git checkout -- fileName`

## 10. 标签管理 (git tag)

`# 查看已有标签
git tag
#给最新 commit 打标签
git tag v1.0.0
#给指定 commit id 打标签
git tag v0.9.0 <commit-id>`

## 11. 查看日志 (git log)

`# 查看详细历史
git log
#单行显示 (简洁)
git log --oneline
#查看特定作者
git log --author="name"
#图形化显示分支合并历史
git log --graph
#显示前 n 条
git log -n 5
#按日期筛选
git log --after="2018-10-1"
git log --before="2018-10-7"`

## 12. 版本回退 (git reset)
`git reset` 用于撤销暂存区修改或本地仓库提交。

### 撤销暂存区 (git add 后):

`git reset HEAD fileName
git reset HEAD .`
### 撤销本地提交 (git commit 后):

- --soft: 仅重置 HEAD 指针，保留暂存区和工作区内容 (相当于撤销 commit 命令)。
- --mixed (默认): 重置 HEAD 指针和暂存区，保留工作区内容。
- --hard: 重置 HEAD、暂存区和工作区，所有修改丢失。
`#回退到上一个版本
git reset --soft HEAD~1
#回退到指定 commit-id
git reset --hard a1b2c3d`

## 13. 远程仓库管理 (git remote)

`#查看远程仓库名称
git remote
#查看详细信息 (fetch/push 地址)
git remote -v
#添加远程仓库
git remote add origin <url>
#删除远程关联
git remote remove origin
#修改远程仓库地址
git remote set-url origin <new-url>
#清理远程已删除分支的本地缓存
git remote update origin --prune`

## 14. 合并 (git merge)

`# 将 dev 分支合并到当前分支
git merge dev`

## 15. 暂存更改 (git stash)
当需要切换分支但不想提交当前工作时使用。

`#保存当前进度
git stash
#查看保存列表
git stash list
#恢复最近进度并删除记录
git stash pop
#恢复指定进度 (不删除记录)
git stash apply stash@{0}
#删除指定进度
git stash drop stash@{0}
#清空所有进度
git stash clear`

## 16. 忽略文件 (.gitignore)
在项目根目录创建 `.gitignore` 文件，列出不需要 Git 管理的文件或目录（如系统文件、编译产物、密钥等）。GitHub 提供了标准模板。

## 17. 实战：新建项目并提交
1. 初始化本地仓库: `git init`
2. 添加文件到暂存区: `git add .`
3. 提交到本地仓库: `git commit -m "first commit"`
4. 关联远程仓库: `git remote add origin https://github.com/user/repo.git`
5. 推送到远程: `git push -u origin master`

## 18. SSH 秘钥配置
1. 检查现有秘钥:
`cd ~/.ssh
ls`
2. 生成新秘钥:
`ssh-keygen -t rsa -C "your_email@example.com"`
一路回车使用默认值即可。
3. 获取公钥:
`cat ~/.ssh/id_rsa.pub`
复制输出内容配置到 GitHub/GitLab 的 SSH Keys 中。

## 19. 常见问题与修复
Push 失败 (本地与远程分支不一致)
通常发生在远程
