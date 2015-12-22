---
layout: post
title: "Octopress Q&amp;A"
date: 2015-12-22 22:36:52 +0800
comments: true
categories: 
---


关于Octopress的配置在[这个博客](http://shengmingzhiqing.com/)中有详细的教程, 这里我只是做一些很必要的配置的总结. 会一直持续更新.

<!--more-->

**文章在首页显示摘要**

一般情况下我们想让文章在主页只显示摘要, 而不是全部显示, 这种情况下, 我们只需要在文章开头的摘要后面空行加入`<!--more-->`.

**Markdown文件拓展名修改**

默认的MarkDown文件名是`.markdown`. 我们想把它改成`.md`, 这样更简洁. 方法是修改 `rakefile` 文件, 将下面两行

```
new_post_ext    = "markdown"  # default new post file extension when using the new_post task
new_page_ext    = "markdown"  # default new page file extension when using the new_page task
```

替换成

```
new_post_ext    = "md"  # 默认新日志文件后缀
new_page_ext    = "md"  # 默认新页面文件后缀
```

**修改默认Markdown解释器**

默认的Markdown解释器是`rdiscount`, 可以把它改成GitHub推荐的`kramdown`. 打开`Gemfile`, 在最后新增下面一行

```
gem 'kramdown'
```

然后在Terminal执行`bundle install`安装这个依赖项(很可能已经安装过了).

再打开`_config.yml`, 找到`markdown: rdiscount`, 将其中的`rdiscout`改成`kramdown`. 并将下面的删除或注释掉(yml文件的行注释是前面加`#`).

```
rdiscount:
  extensions:
    - autolink
    - footnotes
    - smart
```
