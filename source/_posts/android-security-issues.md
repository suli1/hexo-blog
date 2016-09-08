---
title: android安全问题集锦
date: 2016-09-08 11:04:20
tags:
---


# Android Accessibility导致的一些安全隐患
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



