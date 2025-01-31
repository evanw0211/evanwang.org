+++
title = "使用Zola部署静态博客"
date = 2025-01-31
draft = false

[taxonomies]
categories = ["tutorial"]
tags = ["zola",]
+++

## Install Zola

你可以用你喜欢的包管理器安装`zola`，因为我使用Arch Linux，所以我在这里使用`pacman`。

```sh
pacman -S zola
```

其他发行版(操作系统)详细的安装指南请移步[Zola 官方文档](https://www.getzola.org/documentation/getting-started/installation/)。

## Configuration Zola

初始化`Zola`

```sh
zola init my_site # 在当前目录下新建”my_site"文件夹，并在该文件夹内初始化
zola init #在当前文件夹init
```

运行init会有三个问题，默认即可，这些选择都可以在之后的`config.toml`中配置。

```
> What is the URL of your site? (https://example.com):
> Do you want to enable Sass compilation? [Y/n]:
> Do you want to enable syntax highlighting? [y/N]:
> Do you want to build a search index of the content? [y/N]:
```

这是默认的`config.toml`:

{% codeblock(name="./config.toml")%}

```toml
# The URL the site will be built for
base_url = "https://example.com"

# Whether to automatically compile all Sass files in the sass directory
compile_sass = true

# Whether to build a search index to be used later on by a JavaScript library
build_search_index = false

[markdown]
# Whether to do syntax highlighting
# Theme can be customised by setting the `highlight_theme` variable to a theme supported by Zola
highlight_code = false

[extra]
# Put all your custom variables here
```

{% end %}

构建网页

```sh
zola build
```

运行本地实施更新的服务器

```sh
zola serve
```

## 安装使用主题

这里我选择使用`serene`，这是在我浏览zola主题时发现的一款简洁、美观、功能丰富的zola主题。

安装serene主题，实际上就是把主题仓库clone到本地的themes目录下。

```sh
git submodule add -b latest https://github.com/isunjn/serene.git themes/serene
```

serene主题提供了`config.toml`示例，我们直接使用示例配置替换默认配置并根据注释和主题文档配置。

```sh
nv config.toml config.toml.bak
cp themes/serene/config.example.toml config.toml
```

之后请根据[isunjn/serene](https://github.com/isunjn/serene/)的README配置主题。

## 推送到 GitHub[^1]

添加远程存储库

```sh
git remote add origin <你的存储库地址>
```

```sh
git add .
git commit -m "init"
git push
```

## 部署Cloudflare Pages[^2]

{% note(header="Note") %}
以下内容假定你已经拥有并登录Cloudflare账号,且已将源文件push到远程仓库。
{% end %}

在 Account Home, 选择 *Workers & Pages* > *Create application* > *Pages* > *Connect to Git*.

选择你放置blog源文件的GitHub存储库, 在Set up builds and deployments 页面, 选择你要使用的branch，这里我们选择main。在*Framework preset* 选择zola。

| Configuration option | Value |
| --------------------- | -------------- |
|Production branch|main|
|Build command|zola build|
|Build directory|public|

在下面的*Environment variables (advanced)*中新建一个变量`ZOLA_VERSION`指定你的zola版本。

例如，现在的最新版本是`0.19.2`, `ZOLA_VERSION` = `0.19.2`。

点击*Save and Deploy*，开始你的第一次部署。在部署完成后，你可以在``<你的项目名>.pags.dev`` 查看你的blog。

你可以在`config.toml`中将base_url 的值指向这个子域名

例如：

```toml
# The URL the site will be built for
base_url = "https://my-zola-project.pages.dev"
```

绑定自定义域名请参考[Custom domains · Cloudflare Pages docs](https://developers.cloudflare.com/pages/configuration/custom-domains/)

## 部署到Github Pages

TODO

## Reference

[^1]:<https://github.com/git-guides> GitHub Git Guide
[^2]:<https://developers.cloudflare.com/pages/framework-guides/deploy-a-zola-site/> Cloudflare Pages docs
