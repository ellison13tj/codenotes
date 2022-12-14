<!--
 * @Author: ellison13tj@gmail.com
 * @Date: 2022-12-14 17:04:44
 * @LastEditTime: 2022-12-14 21:33:39
 * @LastEditors: ellison13tj@gmail.com
 * @FilePath: /docs/git/git-remote-warehouse-related-commands.md
 * @Description:
-->

# Git 远程仓库相关命令

## 远程仓库配置

```bash
-- 配置验证信息
ssh-keygen -t rsa -C "注册Github的邮箱"

-- 如果出现生成路径乱码执行如下命令
chcp 65001
```

> - 然后一路回车。最后根据保存路径去寻找 .ssh 文件
> - 打开.ssh 文件中的 id_rsa.pub, 复制里面的 key
> - 然后打开 GitHub
> - 点击 setting
> - 点击 SSH and GPG keys
> - 点击 New SSH key
> - Title 随便写
> - Key 为 id_rsa.pub 里的 key
> - 点击 Add SSH key

```bash
## 验证是否成功
ssh -T git@github.com

## 输出以下表示成功
Hi WongJay! You’ve successfully authenticated, but GitHub does not provide shell access.

## 第一次添加远程仓库地址
git remote add origin git@git.zhlh6.cn:nigthlife/test.git

## 添加远程仓库地址
git remote set-url --add origin git@git.zhlh6.cn:nigthlife/test.git

## 删除
git remote set-url --delete git@git.zhlh6.cn:nigthlife/test.git

## 推送代码到远程
git push <远程主机名> <本地分支名>:<远程分支名>

origin: 远程主机名
master: 本地分支名
## 将本地的master分支推送到origin主机的master分支，
## 如果远程master分支不存在则会被创建
git push origin master

## 强制推送
git push -f origin master

## 拉取远程仓库代码
git pull <远程主机名> <本地分支名>:<远程分支名>
origin: 远程主机名
master: 远程分支名
## 将远程origin主机中的master分支中的代码拉取到本地master分支
## 如果本地master分支不存在则会被创建
git pull origin master
或
git pull origin master:branchName

## 显示以下表示成功
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 273 bytes | 91.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote:
remote: Create a pull request for 'master' on GitHub by visiting:
remote:      https://github.com/nigthlife/test/pull/new/master
remote:
To git.zhlh6.cn:nigthlife/test.git
 * [new branch]      master -> master
Branch 'master' set up to track remote branch 'master' from 'origin'.
```

## 推送代码到远端

> - git add 需要推送的文件
> - git commit -m '本次推送时，备注的信息'
> - git push 远程仓库名 分支名

## 其他命令

```bash
## 查看当前有哪些远程仓库与实际链接地址
git remote -v

## 查看当前分支所属
git remote -vv

## 删除远程仓库
git remote rm 远程仓库名

## 将所有本地分支都推送到origin主机。
git push --all origin
```
