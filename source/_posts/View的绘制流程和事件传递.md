---
title: View的绘制流程和事件传递
date: 2016-08-06 14:28:20
tags:
---

[View](https://developer.android.com/reference/android/view/View.html)在Android中代表了基础的UI构建组件，一个View在屏幕上占有一个矩形的显示区域，负责图形的绘制和事件的处理。
View的绘制有三大主要流程，measre(测量), layout(布局),draw(绘制)，以及在自定义View必要的一些方法，如构造方法，onAttach 、onVisiblityChanged、onDetach等



|类别 | 方法 | 描述|
|-----|------|-----|
|Creation | Constructors |1.使用代码创建,2.从xml布局文件中inflate；在构造方法中还会解析和应用一些在layout布局文件中定义的属性|
|     | onFinishInflate() |在view和它所有的子view从xml中inflate之后调用 | 
|Layout| onMeasure(int, int) | 被调用来决定当前view及其子view需要的size大小|
| | onLayout(boolean, int, int, int, int) | |
| | onSizeChanged(int, int, int, int) | |
|Drawing | onDraw(Canvas) | |
|Event processing | onKeyDown(int, keyEvent) | |
| | onKeyUp(int, KeyEvent) | |
| | onTrackballEvent(MotionEvent) | |
| | onTouchEvent(MotionEvent) | |
|Focus | onFocusChanged(boolean, int, Rect) | |
| | onWindowFocusChanged(bllean) | |
|Attaching | onAttachedToWindow() |当view被添加到windows时调用 |
| | onDetachedFromWindow() |当view从windows移除时调用 |
| | onWindowVisibilityChanged(int) |当包含这个view的windows的visibility被改变是调用 |


> 参考资料:
[公共技术点之 View 绘制流程](http://a.codekk.com/detail/Android/lightSky/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20View%20%E7%BB%98%E5%88%B6%E6%B5%81%E7%A8%8B),对于改文章后面的requestLayout和invalidate方法调用时的绘制过程的说法，经验证调用requestLayout方法会触发view重新绘制的三大流程，measure、layout、draw,而调用invalidate时仅会出发draw来重绘当前view
详细可见该文章，[Android View 深度分析requestLayout、invalidate与postInvalidate](http://www.jianshu.com/p/effe9b4333de)。




