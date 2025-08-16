+++
date = '2025-08-09T12:48:49+08:00'
draft = false
title = 'Git简单使用教程'
+++

git 的工作流程

git 三种状态

已提交（committed）、已修改（modified）及已预存（staged）

三个主要区域：Git 目录、工作目录（working directory）以及预存区（staging area）

基本 Git 工作流程大致如下：

  1. 你在你工作目录修改文件。
  2. 预存文件，将文件的快照新增到预存区。
  1. 做提交的动作，这会让存在预存区的档案快照永久地储存在 Git 目录中。


## 配置git

```sh
git config --global user.name "[name]"
```

对你的commit操作设置关联的用户名

```sh
git config --global user.email "[email address]"
```

对你的commit操作设置关联的邮箱地址

此外如果你需要将本地的提交上传到远程仓库,你还需要配置与远程仓库的身份验证.

## 使用 ssh 连接远程存储库(以 Github 为例)

### 生成新的 SSH 密钥

粘贴到终端运行,请将引号中的示例邮箱替换为你的邮箱.

```sh
ssh-keygen -t ed25519 -C "your_email@example.com"
```

当系统提示您“Enter a file in which to save the key（输入要保存密钥的文件）”时，可以按 Enter 键接受默认文件位置。

```
> Enter file in which to save the key (/c/Users/YOU/.ssh/id_ALGORITHM):[Press enter]
```

在提示符下，键入安全密码。不输入即为空密码.

```
> Enter passphrase (empty for no passphrase): [Type a passphrase]
> Enter same passphrase again: [Type passphrase again]
```

如果你没有修改文件的保存位置,那么这将在你主目录的`.ssh`文件夹下生成`id_ed25519`和`id_ed25519.pub`两个文件.
没有`.pub`扩展名的是私钥,带有`.pub`扩展名的是私钥.公钥用于数据加密,而私钥用于解密.私钥是所有者持有的,而公钥是公布给他人(比如GitHub).

### 将 SSH 密钥添加到 Github

运行下面的命令将公钥复制到剪切板,运行不会显示结果.

```sh
cat ~/.ssh/id_ed25519.pub | clip
```

> clip: 将输入复制到Windows的剪贴板

然后打开GitHub,在你的设置中找到SSH and GPG keys,点击New SSH key,将复制的公钥复制到密钥框中.title可以随意填写你想要的名字为

### 验证与 Github 的连接

```sh
ssh -T git@github.com
```
 如果看到"hello"欢迎,你就完成z和一部分了

 > 有部分机场校园网等锁22端口,请自行探索解决方法.


## 新建存储库

你可以在本地新建一个空仓库,这会在你当前目录下生成一个.git文件夹.

```sh
git init
```

此外,也可以在远程新建仓库clone到本地

```sh
git clone [url]
```

## 进行更改

修改文件后将文件进行快照(缓存区)处理用于版本控制

```sh
git add [file]
```

将文件快照永久地记录(commit 提交)在版本历史中

```sh
git commit -m "[descriptive message]"
```

## 同步更改

GitHub 现在默认的分支是main,将本地分支重命名为main

```sh
git branch -m main
```
添加远程存储库连接,记得选ssh协议

```sh
git remote add <url>
```

将所有本地分支提交上传到 GitHub.

```sh
git push
```

在第一次时可以
```sh
git push -u origin main
```

## Reference

1. [Pro Git book - 什么是Git ?](https://git-scm.com/book/zh/v2/%e8%b5%b7%e6%ad%a5-Git-%e6%98%af%e4%bb%80%e4%b9%88%ef%bc%9f)
2. [Git Cheat Sheets](https://training.github.com/downloads/zh_CN/github-git-cheat-sheet/)
3. [生成新的 SSH 密钥并将其添加到 ssh-agent](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)
4. [新增 SSH 密钥到 GitHub 帐户](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
