---
title: 一些Android小技巧
date: 2016-08-22 22:30:49
updated: 2016-08-23 09:30:00
categories: 编程
tags: [Android, Programming]
---

# 一些Android小技巧

1. [在Release版本下查看log](#1)
2. [PNG优化](#2)
3. [TCP抓包](#3)
4. [查看签名](#4)
5. [单例模式](#5)
6. [多进程Application](#6)
7. [ListView 的局部刷新](#7)
8. [系统日志中的几个重要TAG](#8)
9. [计算程序运行时间](#9)
10. [图片缓存大小](#10)
11. [ClipPadding](#11)
12. [强大的dumpsys](#12)
13. [bugreport命令](#13)
14. [ListView的滑动与点击事件冲突](#14)
15. [Context到底是什么](#15)
16. [禁止EditText输入](#16)


#### 参考资料
* [你应该知道的那些Android小经验](http://jayfeng.com/2016/03/18/%E4%BD%A0%E5%BA%94%E8%AF%A5%E7%9F%A5%E9%81%93%E7%9A%84%E9%82%A3%E4%BA%9BAndroid%E5%B0%8F%E7%BB%8F%E9%AA%8C/ )

## <h2 id = 1 >1.在Release版本下查看log</h2>
因为实现里用了Log.isLoggable(TAG,Log.VERBOSE)做了判断，
LessCode的LogLess中也参考了这种机制：[LogLess](https://github.com/openproject/LessCode/blob/master/lesscode-core/src/main/java/com/jayfeng/lesscode/core/LogLess.java)。

使用这种方法就可以在Release版本也能做到查看应用的打印日志了，
系统重启后设置失效。

```
adb shell setprop log.tag.SQLiteLog V

```


## <h2 id = 2 >2.PNG优化</h2>
APK打包会自动对PNG进行无损压缩，如果自行无损压缩是无效的。

当然进行有损压缩是可以的：https://tinypng.com/
tinypng压缩率一般可超过70%

## <h2 id = 3 >3.Tcpdump抓包</h2>
抓包命令, http://www.strazzere.com/android/tcpdump

```
adb shell  /data/local/tcpdump -i any -p -s 0 -w /sdcard/capture.pcap

```

## <h2 id = 4 >4.查看签名</h2>
```
keytool -list -v -keystore release.jks

```

## <h2 id = 5>5.单例模式</h2>
最佳写法

```

public class Singleton {
    private Singleton() {
    }
                        
    private static class SingletonLoader {
        private static final Singleton INSTANCE = new Singleton();
    }
                                                
    public static Singleton getInstance() {
        return SingletonLoader.INSTANCE;
    }
}

```

## <h2 id = 6>6.多进程Application</h2>
当Android app有多个进程时，Application会执行多次，可以通过pid和applicationId来判断哪些方法只执行一次，避免浪费资源

> [android:process 的坑，你懂吗？](http://www.rogerblog.cn/2016/03/17/android-proess/)

```
String processName = getProcessName(this, android.os.Process.myPid());

// 判断进程名，保证只有主进程运行
if (!TextUtils.isEmpty(processName) && processName.equals(this.getPackageName())) {
        // 主进程初始化逻辑
            ....
}

```
* 获取当前进程名称

方案1

```
public static String getProcessName(Context cxt, int pid) {  
    ActivityManager am = (ActivityManager) cxt.getSystemService(Context.ACTIVITY_SERVICE);  
    List<RunningAppProcessInfo> runningApps = am.getRunningAppProcesses();  
    if (runningApps == null) {  
        return null;  
    }  
    for (RunningAppProcessInfo procInfo : runningApps) {  
        if (procInfo.pid == pid) {  
            return procInfo.processName;  
        }  
    }  
    return null;  
}  

```


方案2，效率更高
```
public static String getProcessName() {
    try {
        File file = new File("/proc/" + android.os.Process.myPid() + "/" + "cmdline");
        BufferedReader mBufferedReader = new BufferedReader(new FileReader(file));
        String processName = mBufferedReader.readLine().trim();
        mBufferedReader.close();
        return processName;
    } catch (Exception e) {
        e.printStackTrace();
        return null;
    }
}
```

## <h2 id = 7>7.ListView 的局部刷新</h2>
有的列表可能notifyDataSetChanged()代价有点高，最好能局部刷新。
局部刷新的重点是，找到要更新的那项的View，然后再根据业务逻辑更新数据即可。


```
private void updateItem(int index) {
    int  visiblePosition = listView.getFirstVisiblePosition();
    if (index - visiblePosition >= 0) {
    // 得到要更新的item的view
    View view = listView.getChildAt(index - visiblePosition);

    // 更新界面（示例参考）
    // TextView nameView = ViewLess.$(view, R.id.name);
    // nameView.setText("update " + index);
    // 更新列表数据（示例参考）
    // list.get(index).setName("Update " + index);
    }
}
```
- 强调一下，最后那个列表数据别忘记更新， 不然数据源不变，一滚动可能又还原了。


## <h2 id = 8>8.系统日志中的几个重要TAG</h2>

```
// 查看Activity跳转
adb logcat -v time | grep ActivityManager
// 查看崩溃信息
adb logcat -v time | grep AndroidRuntime
// 查看Dalvik信息，比如GC
adb logcat -v time | grep "D\/Dalvik"
// 查看art信息，比如GC
adb logcat -v time | grep "I\/art"

```

## <h2 id = 9>9.计算程序运行时间</h2>
为了计算一段代码运行时间，一般的做法是，在代码的前面加个startTime，在代码的后面把当前时间减去startTime，这个时间差就是运行时间。
这里提供一种写起来更方便的方法，完全无时间逻辑，只是加一个打印log就够了。


```
// 测试setContentView()的时间
Log.d("TAG", "Start");
setContentView(R.layout.activity_http);
Log.d("TAG", "End");
```
日志过滤出来，运行命令“adb logcat -v time | grep TAG”：
通过-v time参数，可以比较日志的时间来计算代码的运行时间

```
03-18 14:47:25.477 D/TAG     (14600): Start
03-18 14:47:25.478 D/TAG     (14600): End
```

### <h2 id = 10>10.图片缓存大小</h2>
现在很多图片库需要给图片设置一个最大缓存，但是这个值设置多少合适呢？
高端机和低端机的配置显然应该不同，可以考虑设置一个动态值。
建议设置为应用可用内存的1/8:

```
int memoryCache = (int) (Runtime.getRuntime().maxMemory() / 8);

```



### <h2 id = 11>11.ClipPadding</h2>

这个不多说，ListView的ClipPadding设为false，就能为ListView设置各种padding而不会出现丑陋的滑动“禁区”了。


### <h2 id = 12>12.强大的dumpsys</h2>
dumpsys可以查看系统服务和状态，非常强大，可通过如下查看所有支持的子命令：

```
dumpsys | grep "DUMP OF SERVICE"
```


子命令 | 备注
---|---
activity | 显示所有的activities信息
cpuinfo | 显示CPU信息
window | 显示键盘，窗口和他们的关系
meminfo | 内存信息（meminfo $package_name or $pid 使用包名或者进程id显示内存信息）
alarm   | 显示Alarm信息
statusbar   | 显示状态栏相关的信息（找出广告通知属于哪个应用）
usagestats  | 每个界面启动的时间


### <h2 id = 13>13.bugreport命令</h2>
很多人都用过adb logcat，但是如果想要更详细的信息，logcat则无能为力。
所以大多数手机厂商测试更多的是用adb bugreport来抓log给开发人员分析。

```
// 除了log，还包括启动后的系统状态,包括进程列表，内存信息，VM信息等等
// 而且不像logcat是一直打印的，bugreport命令输出到当前时间就停止结束了。
adb bugreport > main.log

```
### <h2 id = 14>14.ListView的滑动与点击事件冲突</h2>
[ListView事件冲突解决](http://blog.csdn.net/singwhatiwanna/article/details/8863232)

```
public class NestedHorizontalListView extends HorizontalListView {
    private final static int TOUCH_STATE_REST = 0;
    private final static int TOUCH_STATE_SCROLLING = 1;
    private final static int TOUCH_SLOP = 10;

    private int mTouchState;
    private float mLastMotionX;

    public NestedHorizontalListView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
//      return super.onInterceptTouchEvent(ev);

        final int action = ev.getAction();
        if ((action == MotionEvent.ACTION_MOVE) && (mTouchState != TOUCH_STATE_REST)) {
            return true;
        }

    final float x = ev.getX();
//  final float y = ev.getY();
    switch (action) {
        case MotionEvent.ACTION_MOVE:
            final int xDiff = (int) Math.abs(mLastMotionX - x);
            if (xDiff > TOUCH_SLOP) {
                mTouchState = TOUCH_STATE_SCROLLING;
            }
            break;
        case MotionEvent.ACTION_DOWN:
            mLastMotionX = x;
//          mLastMotionY = y;
            mTouchState = mFlingTracker.isFinished() ? TOUCH_STATE_REST : TOUCH_STATE_SCROLLING;
            break;
        case MotionEvent.ACTION_CANCEL:
        case MotionEvent.ACTION_UP:
            mTouchState = TOUCH_STATE_REST;
            break;
        }

        return mTouchState != TOUCH_STATE_REST;
    }
}

```
### <h2 id = 15>15.Context到底是什么</h2>
[Context](http://www.jianshu.com/p/94e0f9ab3f1d)的中文翻译为：语境; 上下文; 背景; 环境，在开发中我们经常说称之为“上下文”，那么这个“上下文”到底是指什么意思呢？在语文中，我们可以理解为语境，在程序中，我们可以理解为当前对象在程序中所处的一个环境，一个与系统交互的过程。比如微信聊天，此时的“环境”是指聊天的界面以及相关的数据请求与传输，Context在加载资源、启动Activity、获取系统服务、创建View等操作都要参与。  
> 实现Context的子类的关系图如下  
![实现Context的子类的关系图](/images/android_context.png "实现Context的子类的关系图")


### <h2 id = 16>16.禁止EditText输入</h2>
> void [setKeyListener](https://developer.android.com/reference/android/widget/TextView.html#setKeyListener(android.text.method.KeyListener)) (KeyListener input)  
Sets the key listener to be used with this TextView. *This can be null to disallow user input.* Note that this method has significant and subtle interactions with soft keyboards and other input method: see KeyListener.getContentType() for important details. Calling this method will replace the current content type of the text view with the content type returned by the key listener.

```
editText.setKeyListener(null);
```

