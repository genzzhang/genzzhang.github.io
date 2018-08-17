---
layout: post
title: Android Adb命令
date: 2018-08-14 21:30
categories: Android
tags: 技巧
---

window中，在当前目录，**shift+右击**，选择在此处打开命令窗口，直接调出cmd。
adb命令运行的文件在android-sdk\platform-tools中，如下：
> adb.exe   
> AdbWinApi.dll  
> AdbWinUsbApi.dll


## 1、使用指令备忘   
一些常用的调试优化命令：

* **.bat打印log到当前目录文件**
```
cd  %~dp0
@echo off
echo 正在检查设备连接...
for /f "skip=1 tokens=1" %%i in ('adb devices') do set d=%%i
if "%d%"=="" (
    echo 未检测到设备连接 & pause
    exit
)
echo 设备%d%已连接，开始采集log，操作手机重现bug后按ctrl+c结束
set logTitle=%date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%_%time:~9,2%
adb logcat -v time > log_%logTitle%.txt  
``` 
* **查看running activities**
```  
adb shell dumpsys activity activities | sed -ne '/Running activities/,/Run #0/p'
```  
* **查看top activity**
```  
adb shell dumpsys activity top
``` 
* **查看内存**
```  
adb shell dumpsys meminfo com.tentcent.qqpimsecure
``` 
* **startActivity/Broadcast**  
```  
adb shell am start -n "com.tencent.qqpimsecure/com.tencent.server.fore.QuickLoadActivity" 
-a android.intent.action.MAIN -c android.intent.category.LAUNCHER  
adb shell am broadcast -a xxx
```  
* **ANR**，/data/anr/traces.txt,一般直接进入anr目录不需要权限
```  
adb shell
su
cd /data/anr
ls
cat traces.txt > /sdcard/traces.txt
exit
adb pull /sdcard/traces.txt .
```  
* **卡顿**
``` 
1）adb shell dumpsys SurfaceFlinger 
SurfaceFlinger服务是Android的系统服务，负责管理Android系统的显示帧缓冲信息。
Android应用程序通过调用SurfaceFlinger服务将Surface渲染到显示屏。 
命令获取最近127帧的数据。  
&nbsp 			
2）adb shell dumpsys gfxinfo    
gfxinfo，GPU呈现模式分析，Android 4.1引入的一个新功能。
在开发者选项中开启强制进行GPU渲染，获取最新128帧的绘制信息。
详细包括每一帧绘制的Draw，Process，Execute三个过程的耗时，
如果这三个时间总和超过16.6ms即认为是发生了卡顿。  
&nbsp
3）利用UI线程的Looper打印的日志    
Looper.loop()中，dispatchMessage前后会打印日志
Looper.getMainLooper().setMessageLogging(new Printer())
打印UI线程的堆栈信息
	StringBuilder sb = new StringBuilder();
	StackTraceElement[] stackTrace = Looper.getMainLooper().getThread().getStackTrace();
	for (StackTraceElement s : stackTrace) {
	    sb.append(s.toString() + "\n");
	}
	Log.e("TAG", sb.toString());
&nbsp
4）利用Choreographer.FrameCallback监控卡顿  
Choreographer.getInstance().postFrameCallback(new Choreographer.FrameCallback())
Android系统每隔16ms发出VSYNC信号，来通知界面进行重绘、渲染，
每一次同步的周期为16.6ms，代表一帧的刷新频率。
当每一帧被渲染时会触发回调FrameCallback，
FrameCallback回调doFrame(long frameTimeNanos)函数。
``` 

## 2、文件操作    
一些常用文件操作adb命令：

* **截图**
```  
rem 截图
cd  %~dp0
@echo off
title 截图
color 0A
adb wait-for-device
set picTitle=%date:~0,4%%date:~5,2%%date:~8,2%_%time:~0,2%%time:~3,2%%time:~6,2%_%time:~9,2%
adb shell screencap -p | sed 's/\r$//' > %picTitle%.png
echo.
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

##  3、安装apk，直接将文件拖到.bat命令上安装
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

## 4、卸载apk  
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