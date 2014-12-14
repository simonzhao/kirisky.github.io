---
layout: post
title:  "解决Fedora20中Sublime无法输入中文的问题"
date:   2014-12-07 19:49:04
categories: Pages
tags: Sublime
---
在fedora20中使用sublime，不论是2还是3都会出现无法输入中文的问题。以下为解决该问题的办法。   
&ensp;      
    
---
    
步骤1.    
----------------------------------
>安装前，查看是否安装了gcc、gcc-g++、kernel-header等依赖。      
        
>安装sublime，注意一定要将sumblie目录的权限设置为775或者777，不然会导致无法启动中文输入法的问题。       
        
>安装fcitx-pinyin，注意安装的时候要看是不是有装qt、x11、fcitx-gtk2、fcitx-gtk3等依赖库是否安装，一般yum会自动安装这些依赖。     

步骤2.   
----------------------------------
>将以下代码保存为sublime_imfix.c
>
{% highlight c++ lineno %}

/*
sublime-imfix.c
Use LD_PRELOAD to interpose some function to fix sublime input method support for linux.
By Cjacker Huang <jianzhong.huang at i-soft.com.cn>
 
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
LD_PRELOAD=./libsublime-imfix.so sublime_text
*/
#include <gtk/gtk.h>
#include <gdk/gdkx.h>
typedef GdkSegment GdkRegionBox;
 
struct _GdkRegion
{
  long size;
  long numRects;
  GdkRegionBox *rects;
  GdkRegionBox extents;
};
 
GtkIMContext *local_context;
 
void
gdk_region_get_clipbox (const GdkRegion *region,
            GdkRectangle    *rectangle)
{
  g_return_if_fail (region != NULL);
  g_return_if_fail (rectangle != NULL);
 
  rectangle->x = region->extents.x1;
  rectangle->y = region->extents.y1;
  rectangle->width = region->extents.x2 - region->extents.x1;
  rectangle->height = region->extents.y2 - region->extents.y1;
  GdkRectangle rect;
  rect.x = rectangle->x;
  rect.y = rectangle->y;
  rect.width = 0;
  rect.height = rectangle->height; 
  //The caret width is 2; 
  //Maybe sometimes we will make a mistake, but for most of the time, it should be the caret.
  if(rectangle->width == 2 && GTK_IS_IM_CONTEXT(local_context)) {
        gtk_im_context_set_cursor_location(local_context, rectangle);
  }
}
 
//this is needed, for example, if you input something in file dialog and return back the edit area
//context will lost, so here we set it again.
 
static GdkFilterReturn event_filter (GdkXEvent *xevent, GdkEvent *event, gpointer im_context)
{
    XEvent *xev = (XEvent *)xevent;
    if(xev->type == KeyRelease && GTK_IS_IM_CONTEXT(im_context)) {
       GdkWindow * win = g_object_get_data(G_OBJECT(im_context),"window");
       if(GDK_IS_WINDOW(win))
         gtk_im_context_set_client_window(im_context, win);
    }
    return GDK_FILTER_CONTINUE;
}
 
void gtk_im_context_set_client_window (GtkIMContext *context,
          GdkWindow    *window)
{
  GtkIMContextClass *klass;
  g_return_if_fail (GTK_IS_IM_CONTEXT (context));
  klass = GTK_IM_CONTEXT_GET_CLASS (context);
  if (klass->set_client_window)
    klass->set_client_window (context, window);
 
  if(!GDK_IS_WINDOW (window))
    return;
  g_object_set_data(G_OBJECT(context),"window",window);
  int width = gdk_window_get_width(window);
  int height = gdk_window_get_height(window);
  if(width != 0 && height !=0) {
    gtk_im_context_focus_in(context);
    local_context = context;
  }
  gdk_window_add_filter (window, event_filter, context); 
}

{% endhighlight %}  
    
步骤3.        
-------------------------------------------
>将sublime_imfix.c编译为sublime_imfix.so
{% highlight c++ linenos %}
gcc -shared -o libsublime-imfix.so sublime_imfix.c  `pkg-config --libs --cflags gtk+-2.0` -fPIC
{% endhighlight %}  
    
步骤4.      
----------------------------------------
>启动sublime，有两种启动sublime的方式。      

   >>###方法1.######
     
   >>直接通过命令来启动
   {% highlight bash linenos %}
   // 当前位置为sublime目录下
   LD_PRELOAD=./libsublime-imfix.so ./sublime_text 
   {% endhighlight %}   
    
   >>###方法2.######
     
   >>通过桌面launcher来启动(毕竟sublime在桌面环境中使用),在桌面总创建一个launcher，将其command选项设置为    

   {% highlight bash linenos %}
    bash -c 'LD_PRELOAD=/sublime所在目录/libsublime-imfix.so /sublime所在目录/sublime_text' %F 
   {% endhighlight %}   

   >>或是将sublime.desktop文件进行编辑，将Exec更改为上面的bash语句。类似如下代码:       

   {% highlight bash linenos %}
    [Desktop Entry]
    Version=1.0
    Type=Application
    Name=sublime
    Comment=
    Exec=bash -c 'LD_PRELOAD=/sublime所在目录/libsublime-imfix.so /sublime所在目录/sublime_text' -n
    Icon=sublime.icon
    Path=
    Terminal=false
    StartupNotify=false
  
   {% endhighlight %}
&ensp; 
     
--- 
参考: [完美解决Linux下SublimeText中文输入]    
&ensp;&ensp;&ensp;&ensp;&ensp;[警惕UNIX下的LD_PRELOAD环境变量]    



[完美解决Linux下SublimeText中文输入]: http://my.oschina.net/tsl0922/blog/113495 "OSChina"    
[警惕UNIX下的LD_PRELOAD环境变量]: http://blog.csdn.net/haoel/article/details/1602108 "CoolShell陈皓"   
