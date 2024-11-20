---
date: 2024-11-19T14:25:56+08:00
draft: false
title: hugo blog - PaperMod
tags:
  - hugo
  - blog
---
hugo blog & github page & workglow 部署

## 部署
```bash
brew install hugo
hugo new site hugo -f --format yaml
```


## 主题
```bash
cd hugo
git init .
git submodule add --depth=1 https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod
```

## 文章
```bash
hugo new posts/<title>.md
```



## 搜索
add to hugo.yaml
```yaml
outputs:
    home:
        - HTML
        - RSS
        - JSON # is necessary
```

在 `content` 目录下创建 `search.md`，内容如下：
```
---
title: "search"
layout: "search"
summary: "search"
placeholder: ""
---
```


## config.yaml
```yaml
baseURL: https://tripse.github.io/
languageCode: zh-CN
title: tripse blog
theme: PaperMod


params:
    homeInfoParams:
        Content: '希望对你有所帮助' # 描述
    SocialIcons:
        - name: github
          url: 'https://github.com/Tripse'
        - name: bilibili
          url: 'https://space.bilibili.com/324542959'

    ShowBreadCrumbs: true # 面包屑导航
    ShowPostNavLinks: true # 文章前后页按钮
    ShowCodeCopyButtons: true # 显示复制代码按钮
    showtoc: true # 在每篇文章开头显示目录

menu:
  main:
    - identifier: tags
      name: tags
      url: /tags/
      weight: 30
    - identifier: search # 需做额外配置，具体参考下文
      name: search
      url: /search/
      weight: 40

outputs:
    home:
        - HTML
        - RSS
        - JSON # is necessary

```

## github 
### pages
```bash
git remote add origin git@github.com:tripse/tripse.github.io.git
git add .
git branch -M main
git push origin
git push --set-upstream origin main --force
```

### workflow
```yaml
# Sample workflow for building and deploying a Hugo site to GitHub Pages
name: Deploy Hugo site to Pages

on:
  # Runs on pushes targeting the default branch
  push:
    branches:
      - main # 分支名称默认为main

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
    shell: bash

jobs:
  # Build job
  build:
    runs-on: ubuntu-latest
    env:
      HUGO_VERSION: 0.138.0 # Hugo版本号
    steps:
      - name: Install Hugo CLI
        run: |
          wget -O ${{ runner.temp }}/hugo.deb https://github.com/gohugoio/hugo/releases/download/v${HUGO_VERSION}/hugo_extended_${HUGO_VERSION}_linux-amd64.deb \
          && sudo dpkg -i ${{ runner.temp }}/hugo.deb          
      - name: Install Dart Sass
        run: sudo snap install dart-sass
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: recursive
          fetch-depth: 0
      - name: Setup Pages
        id: pages
        uses: actions/configure-pages@v4
      - name: Install Node.js dependencies
        run: "[[ -f package-lock.json || -f npm-shrinkwrap.json ]] && npm ci || true"
      - name: Build with Hugo
        env:
          # For maximum backward compatibility with Hugo modules
          HUGO_ENVIRONMENT: production
          HUGO_ENV: production
        run: |
          hugo \
            --gc \
            --minify \
            --baseURL "${{ steps.pages.outputs.base_url }}/"          
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v2
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
        uses: actions/deploy-pages@v3

```

重新 push
```bash
git add .
git commit -m 'Add workflows'
git push origin
```

## 维护
```bash
hugo new posts/<name>.md
hugo
git add .
git commit -m '[描述信息]'
git push origin
```

## references 
- https://www.cnblogs.com/liumylay/articles/17936667.html
- https://www.elegantcrazy.com/posts/blog/build-blog-with-github-pages-hugo-and-papermod/
