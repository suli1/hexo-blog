---
title: android安全问题集锦
date: 2016-09-08 11:04:20
tags: [android, programming]
---


# 1.Android Accessibility导致的一些安全隐患(权限相关)
有些应用,比如1Password, 会要求accessibility permission(无障碍),会获得如下权限：检测您的窗口，检索窗口内容等
这样获取了这些权限的app能够从屏幕上读取文本，有时这些文本是一些敏感或隐私信息。    现在有些app在登录时提供了‘查看输入框密码’的功能，这样在你显示密码的时候，有accessiblility权限的应用就能读取到你的输入框中的信息了。

1.读取微信聊天的输入框的文本,在nexus 5x(android 6.0)上测试验证
![AccessibilityDemo](/images/accessibility.gif)

如何避免该问题，仅仅加入下面的代码段就ok了
```
ViewCompat.setImportantForAccessibility(your_view,ViewCompat.IMPORTANT_FOR_ACCESSIBILITY_NO);
```


参考链接:
[Security issues with Android Accessibility](https://android.jlelse.eu/android-accessibility-75fdc5810025#.laqbpz8dj)
[AccessibilityService on Android Developer](https://developer.android.com/reference/android/accessibilityservice/AccessibilityService.html)
[AccessibilityDemo on GitHub](https://github.com/bpr10/AccessibilityDemo)


# 2.网络安全
1. Https主要采用对称密钥，非对称密钥和CA(Certificate Authority)认证这三种技术来保证安全性  
[“HTTPS”安全在哪里？](http://bugly.qq.com/bbs/forum.php?mod=viewthread&tid=1074&extra=page%3D3)

2. 证书安全,由于Android设备太多，设备证书不统一，一般只有客户端验证服务器端证书，而服务器端在https层是不会验证客户端证书的，所以实际场景是一个单项的身份验证过程  
```
schemeRegistry.register(new Scheme("https", SSLSocketFactory.getSocketFactory(), 443));
```
利用google API来对https进行校验,1.签名CA是否合法,2.域名是否匹配3.是不是自签名证书,4.证书是否过期

3. 支付密码传输的安全性  
采用对称密钥(AES)和非对称密钥(RSA)进行双重加密



# 4.Android App安全体系
1. Android apk realease
1)关闭logcat,本地日志加密  
2)代码混淆, json注解，或使用protobuf  
3)apk包加固，防止反编译和重新打包  

2. 反调试模块,即进程被劫持
保护app不被调试有两种方法
* 一个是内核里有一个调试的debug选项关掉，但是作为app我们没办法改内核态的东西。
* 双进程实现反调试,fork一个daemon,并ptrace被保护的app进程

3. 反注入模块

4. WebView漏洞
1)js注入漏洞  
这个漏洞主要是通过getClass()的方法直接调用java. lang. Runtime接口执行系统命令，让任何js具有app相同的操作权限！
* android 4.2及其以上版本，google 已经fix该漏洞，只需要native 
interface上增加@addJavaInterface标示即可。
* 对于Android4.2以前的版本，则需要重写addJavaInterface，自己实现js调用和native调用的映射关系。这也是cordova的一个经典设计，值得学习一下  

2)WebView钓鱼漏洞  
* 检查WebView加载目标URL是否存在钓鱼欺骗等安全风险
* 对WebView关闭脚本环境

3)WebView跨域漏洞  
读取app/data数据库目录下的webviewCookiesChromium.db
* 针对 Android 4.1 以及以上系统，本身不存在这个跨域漏洞，没有针对性修改
* 对所有版本都 setAllowFileAccess（false），禁止从本地 html 加载页面

5. SQL注入

6. 键盘输入安全隐患

# 5.Android安全编程
1. 日志信息
2. Intent信息泄露,发送隐式Intent必须要指定接收方和权限,对于四大组件如果允许外部访问,还需要对传递的数据进行安全校验
3. 避免hardcode

# 6.本地文件数据加密
SQLite加密:
* 数据库字段级加密
* SQLite文件级加密


> 学习资料
[Android应用安全现状与解决方案](http://blog.csdn.net/yzzst/article/details/46471277)  


