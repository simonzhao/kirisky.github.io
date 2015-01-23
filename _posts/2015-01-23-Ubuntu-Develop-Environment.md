---
layout: post
title:  "Ubuntu的开发环境安装"
date:   2015-01-21 01:24:00
categories: Pages
tags: 重构
---

Ubuntu以精美的桌面著称，也有debian系列丰富的资源库，所以作为开发环境是非常合适的。  
下面就来记录在Ubuntu中配置开发环境的过程。
    
---     
    
###安装操作系统######

> 安装的过程就不说了，去Ubuntu官网下载镜像，然后通过Virtualbox或者VM来进行安装。         
> 然后添加163或者aliyun的源，这个完全凭个人喜欢来。

###更新Linux######

> 安装后当然想将系统更新到最新，那么在Ubuntu中使用
{% highlight shell %}
    // 更新软件列表信息
    sudo apt-get update
    // 更新已经安装的软件
    sudo apt-get upgrade
    // 会更改旧的配置文件及依赖包，一般升级系统用这个
    sudo apt-get dist-upgrade
{% endhighlight %}

###配置免密码登录SSH与Push GIT######

> 下面只说最基本的方法  
> SSH与GIt面密码登录与PUSH，先用ssh-keygen生成公钥和私钥，然后将公钥添加到目标服务器中~/.ssh/authorized_keys中。    
> 然后将.ssh文件夹的权限更改为700，否则设置无法生效。这个没有深究，估计是跟ssh配置有关。   
> 然后将.ssh文件夹中的authorized_keys更改为600，否则也会让免密码登录无法生效。  
> 如果以上还无法生效，那么要看一下ssh的配置文件中是否有打开公钥认证。
{% highlight %}
    RSAAuthentication yes
    PubkeyAuthentication yes
{% endhighlight %}
> 不论Ubuntu还是fedora，ssh配置文件一般都在/etc/ssh/目录下。并且一般linux都是开启以上选项的。

###配置免密码对Github进行PUSH

> 首先是将本地机器的公钥添加进Github    
> 然后以ssh方式将项目clone下来。如果使用https进行clone，就无法使用免密码push的方式了。

###安装PHP CodeSniffer######

> 在PHP官网中有两种安装cs的方法，一种使用pear，另一种使用pyrus。  
> 使用pyrus安装，会让Ubuntu桌面程序产生错误。所以我选择使用pear安装。   
> 但在ubuntu14中，默认安装的cs是老版本，那怎么办呢？ 直接更新pear！     
{% highlight ruby %}
    pear upgrade-all
{% endhighlight %}
> 然后直接按照官网中的方法安装cs
{% highlight ruby %}
    pear install PHP_CodeSniffer
{% endhighlight %}

---     

该文章会不断更新，将使用新的与踩过的坑记录下来。

---
 
