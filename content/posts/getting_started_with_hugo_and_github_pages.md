+++
date = '2025-08-09T10:23:10+08:00'
draft = false
title = 'Hugo 简单上手及部署'
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

以子模块的方式添加主题到 `theme` 目录

```sh
git submodule add  --depth 1 -b main https://github.com/nunocoracao/blowfish.git themes/blowfish
```

删除生成的`hugo.toml`,并复制主题提供的配置文件模板

```sh
rm hugo.toml
cp -r themes/blowfish/config/ .
```

顺便也copy一下`.gitignore`

```sh
cp themes/blowfish/.gitignore .
```

如果你在使用vscode可以把`.vscode`添加到文件里.

修改 `config/_default/hugo.toml` ,删除 `theme = "blowfish"`前的`# `.

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

## Github Pages 部署

```sh
mkdir -p .github/workflows
```

创建`.github/workflows/hugo.tonl`

```yml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# Sets permissions of the GITHUB_TOKEN to allow deployment to GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

# Allow only one concurrent deployment, skipping runs queued between the run in-progress and latest queued.
# However, do NOT cancel in-progress runs as we want to allow these production deployments to complete.
concurrency:
  group: "pages"
  cancel-in-progress: false

# Default to bash
defaults:
  run:
    # GitHub-hosted runners automatically enable `set -eo pipefail` for Bash shells.
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      DART_SASS_VERSION: 1.89.2
      HUGO_VERSION: 0.148.2
      HUGO_ENVIRONMENT: production
      TZ: America/Los_Angeles
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb
          sudo dpkg -i ${{ runner.temp }}/hugo.deb
      - name: Install Dart Sass
        run: |
          wget -O ${{ runner.temp }}/dart-sass.tar.gz https://github.com/sass/dart-sass/releases/download/${DART_SASS_VERSION}/dart-sass-${DART_SASS_VERSION}-linux-x64.tar.gz
          tar -xf ${{ runner.temp }}/dart-sass.tar.gz --directory ${{ runner.temp }}
          mv ${{ runner.temp }}/dart-sass/ /usr/local/bin
          echo "/usr/local/bin/dart-sass" >> $GITHUB_PATH
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v5
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Cache Restore
        id: cache-restore
        uses: actions/cache/restore@v4
        with:
          path: |
            ${{ runner.temp }}/hugo_cache
          key: hugo-${{ github.run_id }}
          restore-keys:
            hugo-
      - name: Configure Git
        run: git config core.quotepath false
      - name: Build with Hugo
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/" \
            --cacheDir "${{ runner.temp }}/hugo_cache"
      - name: Cache Save
        id: cache-save
        uses: actions/cache/save@v4
        with:
          path: |
            ${{ runner.temp }}/hugo_cache
          key: ${{ steps.cache-restore.outputs.cache-primary-key }}
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  # Deployment job
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

新建一个`你的GitHub 用户名.github.io`的repo

然后将本地repo push 到你新建的repo.

在GitHub Repo 设置里找到Pages,选择GitHub action,等待完成后即可在你的GitHub用户名.github.io查看你部署的网站.

## Cloudflare Pages 部署

[查看Pages 官方文档](https://developers.cloudflare.com/pages/framework-guides/deploy-a-hugo-site/
)详细了解.
只需要将本地推送到GitHub后在cloudflare新建一个pages服务即可,无需其他操作.
绑定域名可查看cloudflare的文档.

### 主题配置

请参考[blowfish 官方文档](https://blowfish.page/zh-cn/).
