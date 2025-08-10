+++
date = '2025-08-09T12:48:49+08:00'
draft = true
title = 'Git简单使用教程'
+++

首先,我认为大部分人不需要这篇教程.
你有很多其他更好的教程可以参考学习.
https://docs.github.com/en/authentication/connecting-to-github-with-ssh


这篇是我教我亲爱的用git的一个记录,也给新接触计算机的一些同学参考.

首先我们打开终端,Windows映入眼帘的可能是

```
PowerShell 7.5.2
PS C:\Users\evanw>
```

PS: Powershell 运行的shell
C:\Users\evanw> : 当前所在的目录

Linux的可能是:

```
evanw@navi:~$
```

evan@navi : navi主机上的evanw用户
冒号后面的 ~ 指当前所在的目录,~有特殊含义,代指当前用户的主目录.也就是evanw的主目录.
而$是在提示我们正在输入命令

好了,我们知道这些后想要新建一个文件夹给我们做实验该这么做呢?
我们可以使用`mkdir`新建一个文件夹.

```
evanw@navi:~$ mkdir test
evanw@navi:~$
```

```
PS C:\Users\evanw> mkdir test1

    Directory: C:\Users\evanw

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d----            2025/8/9    13:46                test1

PS C:\Users\evanw>
```

Linux环境的可能比较疑惑,我回车怎么什么都没有显示?那它创建成功了吗?
别急,我们将要接受下一个命令:`ls`

```
evanw@navi:~$ ls
test
evanw@navi:~$
```

你可以发现这个test是有颜色的,说明你成功创建了一个文件夹.

那我怎么将当前目录切换到我们新建的test目录呢?
我们使用`cd`去改变当前的目录.

```
evanw@navi:~$ cd test
evanw@navi:~/test$
```

可以看的冒号后的~已经变成了~/test
你也可以输入pwd来手动打印当前的目录:

```
evanw@navi:~/test$ pwd
/home/evanw/test
evanw@navi:~/test$
```

那我们想返回到上一级目录怎么办呢?

```
evanw@navi:~/test$ cd ..
evanw@navi:~$
```

差不多我们已经知道基本的导航了.

我们要使用 git 将这个test文件夹作为一个存储库

> 安装 git 过程不在这里阐述

```
evanw@navi:~/test$ git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /home/evanw/test/.git/
```

将"hello"添加到README.nd

```
evanw@navi:~/test$ echo "hello" >> README.md
evanw@navi:~/test$ ls
README.md
evanw@navi:~/test$ cat README.md
hello
evanw@navi:~/test$
```

这里出现了一个没见过的`cat`,可以打印文件的内容.

```
evanw@navi:~/test$ git add README.md
evanw@navi:~/test$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   README.md

evanw@navi:~/test$
```

```
evanw@navi:~/test$ git commit -m "first commit"
Author identity unknown

*** Please tell me who you are.

Run

  git config --global user.email "you@example.com"
  git config --global user.name "Your Name"

to set your account's default identity.
Omit --global to set the identity only in this repository.

fatal: empty ident name (for <evanw@navi.localdomain>) not allowed
```

```
evanw@navi:~/test$ git config --global user.email "evanwang0211@gmail.com"
evanw@navi:~/test$   git config --global user.name "Evan Wang"
evanw@navi:~/test$ git commit -m "first commit"
[master (root-commit) 30c9954] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
```

```
evanw@navi:~/test$ git branch -M main
```

```
evanw@navi:~/test$ git push -u origin main
Enter passphrase for key '/home/evanw/.ssh/id_ed25519':
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 220 bytes | 220.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:evanw0211/test.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```





