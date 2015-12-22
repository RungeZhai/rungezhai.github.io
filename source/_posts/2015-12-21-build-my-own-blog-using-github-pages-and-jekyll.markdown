---
layout: post
title: "Build My Own Blog Using GitHub Pages and Jekyll"
date: 2015-12-21 21:50:38 +0800
comments: true
categories: 
---

{% raw %}

今天终于使用 GitHub Pages 和 Jekyll 搭建起了自己的博客, 本来一直想自己租个服务器用 WordPress 自己搭建一个个人博客网站, 但是一直没有行动, 一拖再拖. 最后发现本末倒置了, 工具不重要, 最主要的是想积累一些自己的东西, 有自己的一点空间, 分享自己的想法, 如果能帮助别人, 那就最好不过了.

<!--More-->

所以今天抽出时间先把整个环境搭起来, 先保证其可用性, 至于其他的, 比如美观什么的, 那是以后慢慢优化的事了.

本来准备用 Octopress, 但是根据文档一步一步配置, 发现过程太复杂. 我还是更喜欢直接用 Jekyll, 用 Jekyll 搭建博客甚至不需要在本地安装任何额外的程序, 因为 GitHub 本就支持 Jekyll, 所以只要按照 Jekyll 的要求组织文件, 然后 push 到 GitHub 上就行了. 当然这有一个问题, 就是 GitHub 发布有延时, 所以 push 成功后需要等一段时间才能看到更新, 这样就不好调试了, 所以最好是在本机也装一个 Jekyll, 在本机调试好以后再提交到 GitHub. 只要本机的 Jekyll 和 GitHub Pages 使用的 Jekyll 是同一个版本, 那本地的结果和服务器的结果就是一样的.

##新建Repository

在 GitHub 网站为 Blog 新建 Repository, 我这里取名 blog. 详细不再说了, 我这个新建的 Repository 的链接是

```
https://github.com/RungeZhai/blog.git
```

后面会用到.

在本机新建文件夹并进入该文件夹

```bash
mkdir blog
cd blog
```

初始化 git 并关联上面新建的 Repository

```bash
git init
git remote add origin https://github.com/RungeZhai/blog.git
```

新建 gh-pages 分支, 因为 GitHub 规定, 只有该分支中的页面, 才会自动生成网页文件

```bash
git checkout --orphan branch gh-pages
```

新建一个 index.html 文件, 并输入内容

```html
<!DOCTYPE html>
<html lang=“en-ca”>
<head>
<meta charset=“utf-8”>
<title>Hello, World!</title>
</head>
<body>
<h1>H1</h1>
</body>
</html>
```

提交修改

```bash
git add .
git commit -m 'add index.html'
git push -u origin gh-pages
```

此时在浏览器中输入如下格式的链接

```
<GitHub Username>.github.io/<Repository Name>
```

我这里是rungezhai.github.io/blog. 应该会看到一个空白页, 而不会看到 Hello, World!. 因为上面说了, GitHub 发布需要时间, 只要没报错, 说明到目前为止一切正常. 我等了足足两个小时才看到久违的 Hello, World!.

##增加 Jekyll 所需的文件

Jekyll 的目录结构是下面这样的:

```
.
|
|-- _config.yml
|
|-- _includes
|       |
|       |-- head.html
|       |-- ...
|
|-- _layouts
|       |
|       |-- page.html
|       |-- post.html
|       |-- ...
|
|-- _posts
|       |
|       |-- 2015-09-23-blog.md
|       |-- ...
|
|-- _site
|     |--CNAME
|     |--index.html
|     |--...
|
|-- index.html
```

###新建 _config.yml

`_config.yml`是 Jekyll 的配置文件, 这里新增如下两行内容

```
markdown: redcarpet
baseurl: /blog
```

特别注意格式, 冒号后面要有空格. 第一行, redcarpet 是 GitHub 的 markdown 解析器. 第二行, baseurl 是 上面提到的链接格式处显示的文字, 默认是 repository 的名字.

###_layout

该目录用来存放模板文件, 在该目录下新建 default.html 文件, 内容如下:

```
<!DOCTYPE html>
<html>
<head>
　　<meta http-equiv="content-type" content="text/html; charset=utf-8" />
　　<title>{{ page.title }}</title>
</head>
<body>

　　{{ content }}

</body>
</html>
```

Jekyll使用Liquid模板语言，{{ page.title }}表示文章标题，{{ content }}表示文章内容，更多模板变量请参考官方文档。

###创建文章

新建目录_posts, 进入该目录, 创建第一篇文章. 文章就是普通的文本文件, 文件名假定为2012-08-25-hello-world.html. 注意, 文件名必须为"年-月-日-文章标题.后缀名"的格式. 如果网页代码采用 html 格式, 后缀名为 html; 如果采用 markdown 格式, 后缀名为md. 在该文件中输入以下内容

```
---
layout: default
title: 你好，世界
---
<h2>{{ page.title }}</h2>
<p>我的第一篇文章</p>
<p>{{ page.date | date_to_string }}</p>
```

每篇文章的头部, 必须有一个 yaml 文件头, 用来设置一些元数据. 它用三根短划线"—", 标记开始和结束, 里面每一行设置一种元数据. "layout: default"，表示该文章的模板使用 _layouts 目录下的 default.html 文件; "title: 你好, 世界”, 表示该文章的标题是"你好, 世界"，如果不设置这个值, 默认使用嵌入文件名(2012-08-25-hello-world.html)的标题, 即"hello world"。 在 yaml 文件头后面, 就是文章的正式内容, 里面可以使用模板变量. {{ page.title }}就是文件头中设置的"你好，世界"，{{ page.date }}则是嵌入文件名的日期（也可以在文件头重新定义 date 变量）, "| date_to_string"表示将 page.date 变量转化成人类可读的格式。

###首页

首页就是我们上面新建的 index.html, 现在把它修改成如下:

```
---
layout: default
title: 我的Blog
---

<h2>{{ page.title }}</h2>
<p>最新文章</p>
<ul>
　　{% for post in site.posts %}
　　　　<li>{{ post.date | date_to_string }} <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
　　{% endfor %}
</ul>
```

它的Yaml文件头表示, 首页使用default模板, 标题为"我的Blog". 然后，首页使用了{% for post in site.posts %}, 表示对所有帖子进行一个遍历. 这里要注意的是, Liquid模板语言规定, 输出内容使用两层大括号, 单纯的命令使用一层大括号. 至于{{site.baseurl}}就是_config.yml中设置的baseurl变量.

###发布

上面已经提到了, 其实就是提交

```bash
git add .
git commit -m 'Init Jekyll'
git push -u origin gh-pages
```

然后在浏览器里输入上面提到的链接, 就可以看到自己的博客了!!! 呃… 还是"Hello, World!“? 可能需要时间, 耐心等几分钟.

##安装 Jekyll

为了本机立即查看效果, 我们需要在本机安装Jekyll. 为了保证本机的 Jekyll 和 GitHub Pages 使用的 Jekyll 是同一个版本, 我们直接使用 GitHub 的 Jekyll:

```bash
sudo gem install github-pages
```

然后进入上文的Repository目录, 执行下面的命令:

```bash
jekyll serve --watch --baseurl ""
```

应该会看到类似下面的提示:

```
Configuration file: .../blog/_config.yml
Source: .../blog
Destination: .../blog/_site
Generating...
done.
Auto-regeneration: enabled for '.../blog'
Configuration file: .../blog/_config.yml
Server address: http://127.0.0.1:4000/
Server running... press ctrl-c to stop.
```

在浏览器中输入 http://127.0.0.1:4000/ 就可以看到我们的成果了.

注: 文中部分内容抄袭的阮一峰大神的, [附上链接](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)

{% endraw %}
