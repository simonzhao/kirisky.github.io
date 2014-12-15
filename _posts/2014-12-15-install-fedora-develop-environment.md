---
layout: post
title:  "Fedora开发环境的安装"
date:   2014-11-27 11:00:00
categories: Pages
tags: PHP
---
最近想以Fedora来进行开发使用，所以折腾了很久，虽然还是有很多问题，但从中也学习到了很多。    
很多人会问，为什么非要纠结于是否使用fedora作为开发环境，其实优缺点都很多。那么我就来简单的说下优缺点，以Fedora为例。    

优点        
1. 丰富的软件资源。因为有开源这种种族天赋，所以可以通过添加repo来使软件仓库包罗万象。
2. 高度自由。只有想不到，没有做不到。 
3. 在linux下，有神的编辑器和编辑器之神等着你来用。
4. 让你有更多的机会熟悉linux，学习到更多好的软件思想。      
    
缺点
1. 有门槛。没有windows那么友好的图形界面，很多东西都需要自己通过命令行来进行操作。      
2. 很多地方需要配置。虽然linux里面高度的灵活可定制，但通常页需要自身对其进行一定的配置才能达到自己的需求。      
3. 权限是把双刃剑。    
    
接下来，简单给大家描述一下安装linux开发环境的过程，希望可以对需要的人有所帮助。     

步骤一 安装linux
安装linux有很多种方式，但我比较喜欢使用VirtualBox来进行虚拟安装linux(没有用MB，所以不做讨论)。
1. 下载安装[VirtualBox]，对应格平台的各版本，具体安装步骤可以参见[VirtualBox]官方文档。
2. 新建虚拟机，然后使用各分发版linux的镜像进行安装。

步骤二 添加repo
repo就是yum的资源库，可以通过添加repo来使自己系统中的资源更加丰富。国内比较好的镜像有[aliyun]、[网易]、[搜狐]，至于如何添加repo，可以看这三个镜像站中对应分发版的help。
记得添加好后自己的repo后，更新缓存，更新linux(yum update)。     

步骤三 安装常用的软件
新装的linux，其实还是有很多常用的软件没有的。如vim、wget、gcc、g++等等(以fedora20为例)。

步骤四 配置
各种配置因需要而异，如web开发除了php、mysql、nginx等常用软件之外，还要记得安全方便的配置。如selinux是enforce模式，可能nginx、apache、ssh等服务在使用中会出现permission denied问题。
具体可自行搜索selinux、iptables的使用文档。也可以使用图形化配置工具来配置这些安全功能。如果使用ssh、samba等服务，还是对这些服务进行配置。

以上就是安装linux的大概过程。以下是一些服务的配置方法总结的归纳，具体配置过程可以直接点击连接进行相关文章。

草稿1


---

[VirtualBox]: http://www.virtualbox.org
[aliyun]: http://mirrors.aliyun.com
[网易]: http://mirrors.163.com
[搜狐]: http://mirrors.sohu.com
