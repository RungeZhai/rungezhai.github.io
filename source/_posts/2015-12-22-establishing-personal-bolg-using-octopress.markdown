---
layout: post
title: "Establishing Personal Bolg Using Octopress"
date: 2015-12-22 21:40:00 +0800
comments: true
categories: Octopress
---

{% raw %}

本来是想直接用Jekyll进行个人博客搭建的, 但是搭建完以后发现, 生成的页面太原始了, 对于markdown的代码高亮基本就没有, 只是和其他文字字体不一样, 一点都不明显. 况且我对于Web开发并不熟, 自己调整页面布局什么的又是无底洞了. 所以虽然配置过程很痛苦, 最后还是转向了Octopress.

<!--More-->

首先说明, 本文的环境是Mac OSX.

本来配置过程并不复杂, 就几句命令, 但是其背后是各种依赖, 每一步都是坑, 先贴一下其中用到的Bundle的依赖, 让大家见识一下:

```
Using rake 10.4.2
Using RedCloth 4.2.9
Using blankslate 2.1.2.4
Using chunky_png 1.3.4
Using fast-stemmer 1.0.2
Using classifier-reborn 2.0.3
Using coffee-script-source 1.9.1.1
Using execjs 2.6.0
Using coffee-script 2.4.1
Using colorator 0.1
Using multi_json 1.11.2
Using sass 3.4.18
Using compass-core 1.0.3
Using compass-import-once 1.0.5
Using rb-fsevent 0.9.6
Using ffi 1.9.10
Using rb-inotify 0.9.5
Using compass 1.0.3
Using tilt 2.0.1
Using haml 4.0.7
Using jekyll-coffeescript 1.0.1
Using jekyll-gist 1.3.4
Using jekyll-paginate 1.1.0
Using jekyll-sass-converter 1.3.0
Using listen 3.0.3
Using jekyll-watch 1.3.0
Using kramdown 1.8.0
Using liquid 2.6.3
Using mercenary 0.3.5
Using posix-spawn 0.3.11
Using yajl-ruby 1.2.1
Using pygments.rb 0.6.3
Using redcarpet 3.3.2
Using safe_yaml 1.0.4
Using parslet 1.5.0
Using toml 0.1.2
Using jekyll 2.5.3
Using jekyll-sitemap 0.9.0
Using octopress-hooks 2.6.1
Using octopress-date-format 2.0.2
Using rack 1.6.4
Using rack-protection 1.5.3
Using rdiscount 2.1.8
Using sass-globbing 1.0.0
Using sinatra 1.4.6
Using stringex 1.4.0
Using bundler 1.10.6
```

一屏幕都显示不开有没有!!!

###配置Brew

* 安装

HomeBrew 是一个非常有用的软件包管理系统, 一般情况下, Mac机器已经安装过Brew了. 可以使用`brew --version`查看brew的版本. 如果机器没有安装Brew, 可以运行下面命令安装
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
或者想卸载重装, 运行下面命令卸载
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"
```
按照提示一步一步来即可, 期间可能会遇到`Permission denied`什么的, 那么就在命令前面加上`sudo`(super do)再执行一遍喽.

* 诊断Brew

运行`brew doctor`来检查Brew的状况. 
如果出现如`Unbrewed *** were found`这种提示, 可以忽略, 如果实在不放心, 那就按照提示把它们统统删掉(不放心可以删之前先备份).
如果出现`Homebrew's sbin was not found in your PATH but you have installed...`这种提示, 在命令行运行
`export PATH="/usr/local/sbin:$PATH"`.
总之, 到最后如果完全没有问题的话, 运行`brew doctor`会出现`Your system is ready to brew.`

###新建Github Repository
新建一个repository, 命名规则为`[your_username].github.io`. 把其中的`[your_username]`换成你自己在Github 上的用户名, 我这里是rungezhai.github.io. 记下这个repository的链接, https或者SSH都可以, 下面会用到.

###本地安装Octopress


* 安装Ruby

首先使用RVM安装Ruby, Octopress使用的ruby最好是1.9.3版本, Mac10.10以后的版本自带的Ruby版本太高, 最好不要超过2.0.0

执行下面的命令安装RVM

```
curl -L https://get.rvm.io | bash -s stable --ruby
```

执行下面的命令安装ruby

```
rvm install 2.0.0
rvm use 2.0.0
rvm rubygems latest

```

* 把Octopress克隆到本地

```
git clone git://github.com/imathis/octopress.git rungezhai.github.io
cd rungezhai.github.io
```

最后一个参数`rungezhai.github.io`是克隆到的目录, 我为了命名统一, 克隆到了和Github repository同样名字的目录下.

然后安装所需的依赖项


```
sudo gem install bundler
bundle install
```
期间可能会出现如下类似错误

```
An error occurred while installing tilt (2.0.1), and Bundler
cannot continue.
Make sure that `gem install tilt -v '2.0.1'` succeeds before
bundling.
```

没有办法, 只能根据提示执行相应的命令, 但要在前面加sudo, 以上面为例:

```
sudo gem install tilt -v '2.0.1'
```

最后应该会出现如下提示, 说明安装成功:

```
Bundle complete! 13 Gemfile dependencies, 47 gems now installed.
Use `bundle show [gemname]` to see where a bundled gem is installed.

```

* 安装默认主题

```
rake install
```
本地安装完毕. 顺便说一句, 所谓 rake 就是 ruby make 的缩写.

* 预览

```
rake preview
```
在浏览器中输入:`http://localhost:4000/`即可查看网站, 现在应该是一个黑色背景的空白页面. 命令行中按Ctrl+C结束预览.


###发布

* 设置Github pages

```
rake setup_github_pages
```

期间会有一个问句, 把上面记下的那个repository地址复制进去即可.

* 生成页面并部署

```
rake generate
rake deploy
```

上面的命令可以简化为

```
rake gen_deploy
```

* 将结果提交到Github

上面的命令其实已经将结果(_deploy目录下的所有内容)提交到了main分支, 但我们的所有文件按照Octopress要求, 应该提交到source分支上, soure分支已经在上面的命令中在本地新建了, 所以我们只需要提交即可:

```
git add .
git commit -m 'comment here'
git push origin source
```

如果出现提示`Updates were rejected because the remote contains work that you do...`可以先执行下面的命令再执行上面三条

```
git branch --unset-upstream
```

* 查看最终结果

浏览器中地址栏输入`http://[your_username].github.io/`即可. 如果连接失败, 那可能要等一段时间, Github部署也是需要一段时间的.


###新建文章

```
rake new_post["Post Title"]
```

`Post Title`是想要新建的文章的标题, 运行上面的命令会在 octopress/source/_posts 目录下新增 yyyy-mm-dd-Post-Title.markdown 文件, 默认是使用的markdown, 如果想直接用html的话, 把拓展名改成html即可, 打开文件会发现文件已经有了几行文字, 这些是文章的metadata, 不要删除这段信息, 在下面写自己的文章就行了. 写完之后执行上面的`生成页面并部署`和`将结果提交到Github`两步即可.


参考

http://shengmingzhiqing.com/blog/setup-octopress-with-github-pages.html/
https://github.com/imathis/octopress/issues/121
http://stackoverflow.com/questions/12940626/github-error-message-permission-denied-publickey
https://github.com/Homebrew/homebrew/issues/30180
http://linfan.info/blog/2012/02/25/homebrew-installation-and-usage/
http://anandmanisankar.com/posts/set-up-blog-jekyll-github-pages/
http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
http://stackoverflow.com/questions/24102498/escaping-double-curly-braces-inside-a-markdown-code-block-in-jekyll

{% endraw %}
