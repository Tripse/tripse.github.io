---
tags:
  - blog
  - hugo
---
references:
- https://www.cnblogs.com/liumylay/articles/17936667.html
- https://www.elegantcrazy.com/posts/blog/build-blog-with-github-pages-hugo-and-papermod/

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

## github pages
```bash
git remote add origin git@github.com:tripse/tripse.github.io.git
```