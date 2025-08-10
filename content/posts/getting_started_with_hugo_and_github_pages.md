+++
date = '2025-08-09T10:23:10+08:00'
draft = false
title = 'Getting Started with Hugo and Github Pages'
+++

## 安装Hugo 与 git

根据 hugo 官方文档提示,请使用 Powershell 或 git bash 运行命令.

> PowerShell 和 Windows PowerShell 是不同的应用程序 。

Windows Powershell 是自带的Powershell 老版本,新的powershell 7.x 可以使用winget安装

```sh
winget install Microsoft.Powershell
```

### 使用包管理器安装 Hugo 和 Git

如果你已经安装了git,那你仅仅需要安装hugo

debian

```sh
apt install git
apt install hugo
```

Windows

```sh
winget install git.git
winget install Hugo.Hugo.Extended
```

检查Hugo是否成功安装

```sh
hugo version
```

## 初始化Hugo repo

在当前目录的新建一个目录(请将YourFolderName改为你想要的名字),并生成目录结构.

```sh
hugo new site YourFolderName
```

完成后你新建目录的内容大概是这样:

```
my-site/
├── archetypes/
│   └── default.md
├── assets/
├── content/
├── data/
├── i18n/
├── layouts/
├── static/
├── themes/
└── hugo.toml
```

在你新建的目录初始化git repo

```sh
cd evanwang.org
git init
```



```sh
git submodule add -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

删除生成的`hugo.toml`,并复制主题提供的配置文件模板

```sh
rm hugo.toml
cp -r themes/blowfish/config/ .
```

修改 config/_default/hugo.toml ,取消第五行的注释

```toml
# -- Site Configuration --
# Refer to the theme docs for more details about each of these parameters.
# https://blowfish.page/docs/getting-started/

theme = "blowfish" # UNCOMMENT THIS LINE
# baseURL = "https://your_domain.com/"
defaultContentLanguage = "en"
```

在本地预览查看一下效果,现在是空白的:

```sh
hugo server
```

### 主题配置

主页布局

background
