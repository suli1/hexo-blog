---
title: android消息处理机制详解
date: 2016-08-11 20:33:16
tags:
---

# 线程间的消息通信，主要是MainThread与WorkerThread间的消息传递
Handler, Looper, MessageQueue

Message是保存传递的消息的一个数据类,Message中保存post消息的目标Handler

ThreadLocal是一个线程内部的数据存储类，每个线程可存在自己线程的本地数据到ThreadLocal的table中，这样可在线程中访问自己线程的数据

MessageQueue是一个消息队列，内部使用单链表的数据结构实现

Handler 保存Looper和Callback, 通过Looper.MessageQueue发送消息

1.在ActivityThread中设置当前线程(UI线程)的Looper(MainLooper)到ThreadLocal<Looper>,
2.再启动loop循环从myLooper(MainLooper)的MessageQueue中读取数据,Message.Handler.dispatchMessage
3.这样post的Message就发送到对应线程的Looper.MessageQueue中,
4.通过Looper的回调就能在Handler对应的线程中处理回调结果。


# 进程间的消息通信

