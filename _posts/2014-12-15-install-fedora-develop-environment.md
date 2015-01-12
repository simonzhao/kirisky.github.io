---
layout: post
title: "Fedora开发环境的安装"
date: 2014-12-15 23:00:00
categories: Pages
tags: 开发
---
最近想以Fedora来进行开发使用，所以折腾了很久，虽然还是有很多问题，但从中也学习到了很多。             
         
---
很多人会问，为什么非要纠结于是否使用fedora作为开发环境，其实优缺点都很多。那么我就来简单的说下优缺点，以Fedora为例。    

###优点#####       
> 1. 丰富的软件资源。因为有开源这种种族天赋，所以可以通过添加repo来使软件仓库包罗万象。
> 2. 高度自由。只有想不到，没有做不到。 
> 3. 在linux下，有神的编辑器和编辑器之神等着你来用。
> 4. 让你有更多的机会熟悉linux，学习到更多好的软件思想。      
    
###缺点#####
> 1. 有门槛。没有windows那么友好的图形界面，很多东西都需要自己通过命令行来进行操作。      
> 2. 很多地方需要配置。虽然linux里面高度的灵活可定制，但通常页需要自身对其进行一定的配置才能达到自己的需求。      
> 3. 权限是把双刃剑。    
                
---      
接下来，简单给大家描述一下安装linux开发环境的过程，希望可以对需要的人有所帮助。     

###步骤一 安装Linux#####      
     
>安装linux有很多种方式，但我比较喜欢使用VirtualBox来进行虚拟安装(没有用MB，所以不做讨论)。        
>>1. 下载安装[VirtualBox]，对应格平台的各版本，具体安装步骤可以参见[VirtualBox]官方文档。       
>>2. 新建虚拟机，然后使用各分发版linux的镜像进行安装。      

###步骤二 添加repo#####     
    
>repo就是yum的资源库，可以通过添加repo来使自己系统中的资源更加丰富。    
>国内比较好的镜像有[aliyun]、[网易]、[搜狐]，至于如何添加repo，可以看这三个镜像站中对应分发版的help。      
>记得添加好后自己的repo后，更新缓存，更新linux。     

###步骤三 安装常用的软件#####       
    
>新装的linux，其实还是有很多常用的软件没有的。如vim、wget、gcc、g++等等(fedora20)。

###步骤四 配置#####     
>各种配置因需要而异，如web开发除了php、mysql、nginx等常用软件之外，还要记得安全方便的配置。     
>如selinux是enforce模式，可能nginx、apache、ssh等服务在使用中会出现permission denied问题。具体可自行搜索selinux、iptables的使用文档。也可以使用图形化配置工具来配置这些安全功能。       
>如果使用ssh、samba等服务，还是对这些服务进行配置。

以上就是安装linux的大概过程。       
         
---
以下是一些服务的配置方法总结的归纳，具体配置过程可以直接点击连接进行相关文章。
    
###NMP安装#####     
    
安装Nginx、MySql和PHP的步骤大体如下：   
    
步骤1.     
      
>安装MySql，在Fedora中，直接使用yum安装MySql是直接安装MariaDB，也就是MySql的一个分发版。由于与MySql的渊源(具体情况请Google，在此不展开)，使用起来基本一样，但还是有差别。所以，一般会选择安装MySql。       
>
>安装的方法，可以去MySql官网查看，这里推荐repo的方式安装MySql。去官网下载一个MySql的repo，然后字节通过yum进行安装。         
    
步骤2.           
    
>安装PHP与PHP-FPM。在Fedora中，php是默认安装的，如果不喜欢当前的方式或者版本，可以yum remove掉后，自行安装。     
>
>PHP-FPM是FastCGI管理器，专门用于PHP。安装它的主要目的是用于Nginx中。     
    
步骤3.      
    
>安装Nginx，安装过程与配置没什么可说的，与apache类似，没有相关经验的人也可以通过文档来进行安装。        
>
>需要注意的是权限的问题及Iptables与selinux的问题。Fedora默认是不开放http及https端口，而且SELinux也是enforing的状态，所以安装后直接查看localhost会出现forbidden的状态，log中会显示permission denied, 需要将selinux改成Permissive，并且开放防火墙的http、https端口。       
    

###SSH#####     
    
>Fedora默认安装ssh，所以直接启动ssh服务以及在iptables中开启ssh后就可以使用ssh服务了。但默认配置的ssh服务使用起来不太方便，所以需要在/etc/ssh/目录中配置一下。      
>ssh_config是用来配置客户端，sshd_config是用来配置服务端。  

###Samba#####       

>Fedora默认安装samba，samba服务可以通过/etc/samba/目录中的smb.conf进行配置，直接执行smbd来进行服务的启动。      
>samba是需要手动添加账户和配置开放区域的，而不是像ssh使用系统账户及其所在区。
        
###Mount#####       

>一般使用Linux来进行开发，很多时候出于个方面的原因(如安全问题、配置环境的复杂度等等)不能直接配置出与服务器相同的本地环境，那么直接使用mount来挂在测试机，直接使用本地的开发配置对测试机中的代码进行编写。
>
{% highlight shell lineno %}
mount src_dir dest_dir -t cifs -o rw,username=xxxx,passwd=xxxx
{% endhighlight %}  
        
###GIT#####     
     
>这个版本控制程序，我向大家已经如雷灌耳，再次不再展开，值得注意的是权限问题。       
>
>因为开发的时候难免需要更改目录及文件的权限，但默认的配置会将权限的修改也作为更改显示在status中，所以需要配置当前程序目录下的.git/config文件，将其中的[core]下的filemode选项设置为false，这样就不会记录目录及文件权限的修改了。        
>
>还有可以使用当前程序目录下的.gitignore忽略不希望上上传的目录及文件。

###Sublime#####     
>Sublime是个很好用的文本编辑器，最初作为扩展vim而发展。但在fedora中Sublime会出现一些问题，比如[中文输入法的支持问题]以及图标显示不全的问题。目前并没有招接完美的解决办法，希望能解决该问题的同学指点。

---

[VirtualBox]: http://www.virtualbox.org
[aliyun]: http://mirrors.aliyun.com
[网易]: http://mirrors.163.com
[搜狐]: http://mirrors.sohu.com
[中文输入法的支持问题]: http://kirisky.github.io/pages/2014/12/07/about-sublime-chinese-input-on-fedora-20.html
