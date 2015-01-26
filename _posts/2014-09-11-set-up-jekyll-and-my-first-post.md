---
layout: post
title:  "通过pages与jekyll搭建Blog"
date:   2014-09-11 15:49:04
categories: Pages
tags: [Jekyll]
---

将github.io的blog搭好了。记录一下整个搭建的过程。	
首先通过[automatic generator]的教程来自动生成一个模板，然后安装[jekyll][using jekyll]。    
然后查看一下[jekyll]\([中文版][jekyll中文版]\)的教程，了解layout、include与posts的关系，参照[webfrogs.me]进行了一些修改。	

觉得在github.io中搭建blog，是一个很有趣的体验。以后将使用[markdown]\([中文版][markdown中文版]\)来写博客，想想还有些小激动呢！	

注: 因为Gem可能被墙了，推荐大家使用[AliyunRubyGems镜像]和[TaobaoRubyGems镜像]。	

{% highlight ruby linenos %}
#以淘宝镜像为例
gem source -a http://ruby.taobao.org 
{% endhighlight %}      
    
    
---

相关资料:       

> [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门]
    
---

[搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门]: http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html
[automatic generator]: https://help.github.com/articles/creating-pages-with-the-automatic-generator
[using jekyll]: https://help.github.com/articles/using-jekyll-with-pages
[jekyll]: http://jekyllrb.com/
[jekyll中文版]: http://jekyllcn.com/
[webfrogs.me]: http://webfrogs.me/
[markdown]: http://daringfireball.net/projects/markdown/syntax
[markdown中文版]: https://github.com/othree/markdown-syntax-zhtw/blob/master/syntax.md
[AliyunRubyGems镜像]: http://mirrors.aliyun.com/help/rubygems
[TaobaoRubyGems镜像]: http://ruby.taobao.org/
