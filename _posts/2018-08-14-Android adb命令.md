---
layout: post
title: Android Adb命令
date: 2016-08-14 21:30
categories: Android
tags: 工具
---

当我们触摸屏幕，一个事件从产生到派发至View的过程：
>* 首先**驱动**，从硬件到Linux Kernel层的设备驱动产生输入事件，这些事件最终会写入到设备文件/dev/input/eventX中完成驱动层的上报（一般的手机触摸屏一般以60次/秒左右的频率上报触摸事件）；   
>* 再到**框架FrameWork层**，InputManager中的InputReader线程会通过EventHub的getEvent方法从/dev/input/目录下的设备文件中持续读取事件，然后将读取到的事件放至消息队列中，WindowManagerService中InputDispatcher线程则循环从消息队列中取出原始事件RawEvent进行加工，并找到Window Manager中的Focus Window，然后通过进程间通信将其发送至当前焦点所在的窗口；
>* 最终到当前**焦点所在的窗口**，通常是Activity，再从View树的树根ViewRootImpl开始分发到具体View。

所有的RawEvent事件都会被封装成MotionEvent对象，该对象代表了Touch事件的坐标、动作等基本信息。  
一般情况下，每一个Touch事件，总是以ACTION_DOWN事件开始，中间穿插着一些ACTION_MOVE事件（取决于是否有手势的移动），然后以ACTION_UP事件结束。  
ACTION_DOWN-----LongClick------ACTION_UP------Click

对事件的处理有三类：
>分发：public boolean dispatchTouchEvent(MotionEvent ev)  
>拦截：public boolean onInterceptTouchEvent(MotionEvent ev)  
>消费：public boolean onTouchEvent(MotionEvent ev)和OnTouchListener接口中boolean onTouch(View v, MotionEvent event)  

以上三类函数非常重要，贯穿整个事件分发的流程   

##  1、安装apk，直接将文件拖到.bat命令上安装
```
rem 直接将文件拖到这个命令上即可安装
@echo off
title 安装手机软件
color 0A
echo 正准备安装
echo "%~f1"
cd "%~dp0"
cd ..
adb wait-for-device
adb install -r "%~f1"
echo 约5秒后自动退出
ping -n 5 127.1 > nul
exit
```

## 2、卸载apk  
```
rem 卸载手机软件，输入1 2 3 或者包名即可
@echo off
title 卸载手机软件
color 0A

:start
cls
echo ---------------请输入卸载的选择项---------------
echo ==1==    写入1或等待4秒，默认手机管家
echo ==2==    写入2，system,列出包名，copy
echo ==3==    写入3，data，由于权限，估计弄不出来
echo ==4==    写入4，输入包名
echo.

choice /C 1234 /D 1 /T 4 /M "请选择"
if %errorlevel%==1 (
    set apktype=com.tencent.qqpimsecure 
    goto uninstall) else if %errorlevel%==2 (
    set apktype=system
    goto applist) else if %errorlevel%==3 (
    set apktype=data
    goto applist) else (
    SET /P apktype=请输入包名后回车:
    goto uninstall)

:applist
cls
echo %apktype%/app文件列表
echo ----------------------------------
adb shell ls %apktype%/app
echo ----------------------------------
set /P INPUT=请输入包名:
set apktype=%input%
goto uninstall

:uninstall
cls
echo 准备卸载%apktype%
adb uninstall %apktype%
echo 约6秒后自动退出
ping -n 6 127.1 > nul
exit  
```

## 3、文件操作   
一些常用文件操作adb命令：

* **截图**
```  
rem 截图
@echo off
title 截图
color 0A
adb wait-for-device
set picTitle=%date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%_%time:~9,2%
adb shell screencap -p | sed 's/\r$//' > %picTitle%.png
```  
* sd卡导入到电脑当前文件夹
``` 
adb pull /sdcard/log.txt .
``` 
* 电脑文件导入到sd卡目录
``` 
adb push log.txt /sdcard/
``` 
* 电脑文件导入到sd卡目录
``` 
adb push log.txt /sdcard/
``` 

MOVE事件只要不划出View边界是能处理Click和LongClick的，长按的检测时长是500ms（默认值，系统可以修改），Click没有时间限制，View能同时处理LongClick和Click。 


<br />
一般来说，super.xx是系统默认的处理结果，我们可以根据这个结果来判断一些结果。