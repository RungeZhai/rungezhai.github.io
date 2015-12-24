---
layout: post
title: "Octopress Q&amp;A"
date: 2015-12-22 22:36:52 +0800
comments: true
categories: Octopress
---

{% raw %}

关于Octopress的配置在[这个博客](http://shengmingzhiqing.com/)中有详细的教程, 这里我只是做一些很必要的配置或者开发中遇到的各种问题的总结. 会一直持续更新.

<!--more-->

###文章在首页显示摘要

一般情况下我们想让文章在主页只显示摘要, 而不是全部显示, 这种情况下, 我们只需要在文章开头的摘要后面空行加入`<!--more-->`.

###Markdown文件拓展名修改

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

###修改默认Markdown解释器

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

###在主页侧边栏显示文章分类

* 在`plugins`目录下新建`category_list_tag.rb`文件, 内容如下:

```
module Jekyll 
  class CategoryListTag < Liquid::Tag 
    def render(context) 
      html = "" 
      categories = context.registers[:site].categories.keys 
      categories.sort.each do |category| 
        posts_in_category = context.registers[:site].categories[category].size 
        category_dir = context.registers[:site].config['category_dir'] 
        category_url = File.join(category_dir, category.gsub(/_|\P{Word}/, '-').gsub(/-{2,}/, '-').downcase) 
        html << "<li class='category'><a href='/#{category_url}/'>#{category} (#{posts_in_category})</a></li>\n" 
      end 
      html 
    end 
  end 
end
	
Liquid::Template.register_tag('category_list', Jekyll::CategoryListTag)

```

* `source/include/custom/asides`目录下新建`category_list.html`文件, 内容如下:

```
<section>
  <h1>Categories</h1>
  <ul id="categories">
    {% category_list %}
  </ul>
</section>

```

* 修改`_config.yml`文件

	找到以`default_asides:`开头的一行, 添加`custom/asides/category_list.html`. 
	到了这里, 重新生成部署已经可以看到结果了, 但仔细看会发现, 分类的名字中的大写字母都被变成了小写, 如果想改成本来的大小写, 参考下一步.
	
* 修改`Jekyll`, 取消自动小写分类名称
	找到`Jekyll`的源代码目录, 我这里是`/Users/liuge/.rvm/gems/ruby-1.9.3-p551/gems/jekyll-2.5.3/lib/jekyll` 修改目录下的`post.rb`文件, 找到如下内容, 删除其中的`downcase`.

```
self.categories = self.data.pluralized_array('category', 'categories').map {|c| c.to_s.downcase}

```

> 如果遇到`Error:  Pygments can't parse unknown language: </p>`这种问题, 一般是文章中包含HTML代码等非常规文字, 修改`plugins/pygments_code.rb`文件, 将`raise "Pygments can't parse unknown language: #{lang}."`替换成`raise "Pygments can't parse unknown language: #{lang}#{code}."` 然后再执行 `rake generate` 就可以看到产生问题的位置. 实际使用中, 我发现产生问题的位置都是在文章中贴的HTML代码块或者其他非常规文字的位置, 解决方法是代码结束的"```"(triple-backtick)前后都需要留一个空行. 顺便说一句, Pygments是语法高亮插件.

###在文章中显示特殊字符

有一些字符是Octopress得保留字符, 比如`{{, ^ ...`等. 若要在文中显示这些字符

{% endraw %}

需要将包含这些字符的文字放在 `{{ "{% raw " }}%}` 和 `{{ "{% endraw " }}%}`之间.

{% raw %}




{% endraw %}

<br>

Reference

[improve-octopress-advanced-tweaks-tips](http://www.narga.net/improve-octopress-advanced-tweaks-tips/)

[Octopress CDN Config](http://i.rexdf.org/blog/2014/09/26/octopressbo-ke-geng-xin-ri-zhi/)


