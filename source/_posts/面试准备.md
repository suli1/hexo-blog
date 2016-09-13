---
title: Android面试准备
date: 2016-09-06 10:54:55
tags: [android]
---
已经进行2年多的Android应用开发工作，现在又要开始找新工作了，记录一下自己这次找工作的准备工作，以做后用。

# Java及Android面试常用知识
[国内一线互联网公司内部面试题库](https://github.com/JackyAndroid/AndroidInterview-Q-A)

> Java  

## Java中一些重要的关键字synchroized,volatile, final, transient
synchroized,要看该关键字修饰的对象的作用域  
主要有两层作用域,  
    1.作用于对象实例,在多线程同步时仅在当前对象中起作用;  
2.作用于类,包括1,在该类的所有对象实例中起作用。

volatile:保证被修饰的变量每次读取的值都是最新的,一种简单的同步机制  

final: 可修饰变量，方法，类   

transient:被修饰的字段不会被序列化或反序列化  

## Java中的异常处理制
try catch finally  
Throwable

## Java中Object的意义
Class Object is the root of the class hierarchy. Every class has Object as a superclass. All objects, including arrays, implement the methods of this class.

## 多态  
Java 中实现多态的方式:接口实现，继承父类进行方法重载，同一个类中进行方法重载  
好处:  
1.可替换性  
2.可扩充性  
3.灵活性  
5.简化性  


## [排序算法](http://blog.csdn.net/qy1387/article/details/7752973)
冒泡排序，快速排序，堆排序

```
/**
 * 直接插入排序
 */
public static void insertSort(int[] data) {
    int tmp = 0;
    for (int i = 1; i < data.length; i++) {
        int j = i - 1;
        tmp = data[i];
        for (; j >= 0 && tmp < data[j]; j--) {
            data[j + 1] = data[j]; // 将大于tmp的值整体后移一个单位
        }
        data[j + 1] = tmp;
    }
}

```


```
/**
 * 冒泡排序
 */
public static void bubbleSort(int[] data) {
    for (int i = 0; i < data.length - 1; i++) {
        for (int j = data.length - 1; j > i; j--) {
            if (data[j] < data[j - 1]) {
                int tmp = data[j]; // switch
                data[j] = data[j - 1];
                data[j - 1] = tmp;
            }
        }
    }
}

```

```
/**
 * 希尔排序
 */
public static void shellSort(int[] data) {
    double d1 = data.length;
    int temp = 0;

    while (true) {
        d1 = Math.ceil(d1 / 2);
        int d = (int) d1;
        for (int x = 0; x < d; x++) {

            for (int i = x + d; i < data.length; i += d) {
                int j = i - d;
                temp = data[i];
                for (; j >= 0 && temp < data[j]; j -= d) {
                    data[j + d] = data[j];
                }
                data[j + d] = temp;
            }
        }

        System.out.println();
        for (int i : data) {
            System.out.print(i + ",");
        }

        if (d == 1) {
            break;
        }
    }
}


```

```
/**
 * 快速排序
 */
    public static void quickSort(int[] data) {
        if (data.length > 0) {
            _quickSort(data, 0, data.length - 1);
        }
    }

private static int getMiddle(int[] data, int low, int high) {
    int tmp = data[low]; // 数组的第一个作为中轴
    while (low < high) {
        while (low < high && data[high] >= tmp) {
            high--;
        }

        data[low] = data[high];     // 比中轴小的记录移到低端
        while (low < high && data[low] <= tmp) {
            low++;
        }
        data[high] = data[low];
    }
    data[low] = tmp;
    return low;
}

private static void _quickSort(int[] data, int low, int high) {
    if (low < high) {
        int middle = getMiddle(data, low, high); // 将data数组一份为二
        _quickSort(data, low, middle - 1);  // 对低字表进行递归排序
        _quickSort(data, middle + 1, high); // 对高字表进行递归排序
    }
}

```

## java虚拟机垃圾回收机制
分代回收,定时GC
[理解Java垃圾回收机制](http://jayfeng.com/2016/03/11/%E7%90%86%E8%A7%A3Java%E5%9E%83%E5%9C%BE%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6/)


## 进程和线程的区别及并发编程  
> java并发编程实战.pdf  

线程属于进程的一个执行单元,进程是操作系统的一个执行单元
进程有独立的地址空间，由操作系统管理，而线程只是进程中的不通过执行路径，多线程间共享内存，它们有自己的堆栈和局部变量，但线程之间没有独立的地址空间，一个线程死掉就等于整个进程死掉  
线程的阻塞  
1.sleep():允许线程在指定时间内进入阻塞状态，不能得到CPU时间片  
2.suspend()和resume():配套使用  
3.yield():放弃当前CPU时间片，使线程进入等待状态，随时可再分配到时间片  
4.wait()和notify():配套使用，允许指定时间进入阻塞状态，超出指定时间或使用notify()时可重写进入可执行状态  


## StringBuffer和StringBuilder的区别  
StringBuffer是线程安全的，StringBuilder非线程安全，速度要比StringBuffer稍快点，一般情况下可使用StringBuilder以提高性能


## HashMap和HashTable, ArrayMap和HashMap, SparseArrayMap

> Android  

## Activity LaunchMode
- standard 
每次都从新创建一个新的Activity
- singleTop  
创建新的Activity，如果需要创建的Activity在栈顶则使用栈顶的Activity,会调用onNewIntent
- singleTask
如果在当前栈中发现存在需要创建的Activity则重用该栈中的Activity,并将在它上面的Activity出栈
- singleInstance
与singleTask类似，但Activity会在单独的一个栈中

## Activity的创建过程
[startActivity源码分析](http://blog.csdn.net/luoshengyang/article/details/6703247)
[startActivity](http://gityuan.com/2016/03/12/start-activity/)


## 进程间通信

## view的事件传递机制

## Message的消息通信机制

## 动画,及属性动画的原理

## DataBinding


# Android应用性能优化

[Android Performance Patterns](https://www.youtube.com/playlist?list=PLOU2XLYxmsIKEOXh5TwZEv89aofHzNCiu):2015年Google在youtube上发布的Android性能优化视频。
[(中文)Android性能模式 第四季](http://v.youku.com/v_show/id_XMTUyMTM0MzgyNA==.html?f=26946827)
[Android性能优化典范中文版文档](http://hukai.me/):对官方视频翻译整理的文档


## gradle编译速度优化
1.开启gradle的守护进程,见[The Gradle Daemon](https://docs.gradle.org/2.14.1/userguide/gradle_daemon.html)  
在USE\_HOME/.gradle/gradle.properties中添加如下代码
```
org.gradle.daemon=true
```

gradle.properties
```
# Specifies the JVM arguments used for the daemon process.
# The setting is particularly useful for tweaking memory settings.
# Default value: -Xmx10248m -XX:MaxPermSize=256m
org.gradle.jvmargs=-Xmx6144m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8

# When configured, Gradle will run in incubating parallel mode.
# This option should only be used with decoupled projects. More details, visit
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:decoupled_projects
org.gradle.parallel=true

# Enables new incubating mode that makes Gradle selective when configuring projects.
# Only relevant projects are configured which results in faster builds for large multi-projects.
# http://www.gradle.org/docs/current/userguide/multi_project_builds.html#sec:configuration_on_demand
org.gradle.configureondemand=true
```

2.如果使用了productFlavors,可以使用命令进行打包当前需要的apk包，忽略其他的apk包，这样可以减少一些编译打包时间  
编译打包Product的Debug包,这样仅生成包app-production-deubg.apk, app-production-debug-unaligned.apk
```
$./gradlew assembleProductionDebug 
```
3.添加gradle代码，忽略当前不需要执行的单元测试


## 官方性能优化的文章

## 优化电量
延长电池的使用时间可以考虑从如下方面着手(一般应用都会有如下功能):
- 执行代码
- 数据传输(网络的上传和下载，使用Wi-Fi, 2G, 3G, 4G, 蓝牙)
- 追踪位置
- 使用传感器(加速度计、陀螺仪)
- 渲染图像(使用GPU或CPU)
- 唤醒以执行各种任务(Alarm,WakeLock,Receiver)

## 布局和图形的优化
      通过测试可以了解到Activity从onCreate()到onResume()介绍之间90%以上的时间会消耗在setContentView上，所有应该尽量减少布局展开所花费的时间，为了达到这一目的一般基于同样的原则：减少创建对象数或延时创建对象。  
可以在如下方面做一些改善:
- 减少布局层次(使用RelativeLayout或<merge\/>标签
- 使用<include \/>重用布局()
- 使用ViewStub延迟加载
- 尽可能对每一个view只使用一次findViewById的调用

优化布局的工具:hierarchyviewer, layoutopt, lint

使用OpenGl ES进行图形的渲染，压缩

## 减少Apk包的体积
- 图片资源压缩
- 去除无用的资源和代码

## 内存优化、内存泄露、OOM分析

[Bitmpa 内存优化](http://blog.csdn.net/u010687392/article/details/50721437)
[LeakCanary](https://github.com/square/leakcanary)
[一些会产生内存泄露的地方](http://blog.nimbledroid.com/2016/05/23/memory-leaks.html)
[Fixing Memory Leaks in Android Studio](http://www.theshiftingbit.com/Fixing-Memory-Leaks-in-Android-Studio)

## 提高app启动速度
[加速应用的启动速度](http://blog.csdn.net/u010687392/article/details/50518343)


# 单元测试编写
小创博客-[单元测试](http://www.chriszou.com/?from=jianshuhome)



# 一些开源库的原理深入剖析

1.Retrofit2, 2.Dagger2, 3.RxJava, 4.Picasso or Fresco


# Android中的设计模式
[android design patterns](https://github.com/suli1/android_design_patterns_analysis)


# 当前比较火热的技术
热修复, 插件化, 换肤 



- <input type="checkbox" onclick="return false;" checked/> View的事件体系与工作原理
- <input type="checkbox" onclick="return false;" checked/> Android的消息机制
- <input type="checkbox" onclick="return false;" checked/> android architecture


