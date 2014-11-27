---
layout: post
title:  "Web生命周期"
date:   2014-11-27 11:00:00
categories: Pages
tags: PHP
---

这两天遇见被人问Web请求生命周期的问题，但一直没有仔细考虑，今天总结一下。
所谓Web请求的生命周期，就当在浏览器中输入地址后点击跳转，到请求的页面加载完毕后，完整显示在浏览器中的过程。 

当然，在请求之前，服务端是要情动好http server服务(nginx、apache等等)的，而请求端这边也要启动客户端才可以(各种浏览器)。  

准备    
启动http服务，也就是在服务端创建socket描述符，然后绑定80端口后，开始开始监听。
而请求方这边，启动浏览器输入地址后，开始连接服务器\[[1]\]。

Web生命周期     
1. 用户在浏览器中输入URI(URI是抽象的资源标志，具体的如URL、URN\[[2]\])，然  
&ensp;&ensp;&ensp;&ensp;后客户端会向发服务端发送一个http头。   
2. 服务器接受请求会，开始解释http头，然后通过地址来定位资源。   
3. 将资源处理好后(比如启动服务，处理数据，获取图片)，将资源封装好后，返回   
&ensp;&ensp;&ensp;&ensp;给请求端。      
4. 请求端解释response信息，然后显示给用户。     
注：因为Web软软可以看作一种新式的C/S软件，所以每次的Web请求都可以看成一  
&ensp;&ensp;&ensp;&ensp;种对资源的请求，可以通过Get、Post、Put、Delete进行资源操作\[[3]\]。    
&ensp;&ensp;&ensp;&ensp;Web请求是短连接，所以一次请求后，连接就断开了。 


[1]: http://product.china-pub.com/196770
[2]: http://www.cnblogs.com/gaojing/archive/2012/02/04/2413626.html
[3]: http://www.ruanyifeng.com/blog/2011/09/restful.html
