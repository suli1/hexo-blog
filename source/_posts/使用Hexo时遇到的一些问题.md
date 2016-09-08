---
title: Hexo使用时遇到的一些问题
date: 2016-07-05 23:07:27
tags:
---

记录在使用Hexo写博客时遇到的一些问题

<!-- more -->



# 执行hexo deploy, ERROR Deployer not found: git
解决: npm install hexo-deployer-git --save

# 如何在首页显示文章概要，而不要显示全文
1. 在博客文档中加入<!-- more -->，则博客就会自动显示<!-- more --> 之前的内容
2. nexT的解决方法 [点这里](http://theme-next.iissnan.com/faqs.html#%E9%A6%96%E9%A1%B5%E6%98%BE%E7%A4%BA%E6%96%87%E7%AB%A0%E6%91%98%E5%BD%95)


# 在window上使用遇到的一些问题
windows hexo version
```
hexo: 3.2.2
hexo-cli: 1.0.2
os: Windows_NT 10.0.10586 win32 x64
http_parser: 2.7.0
node: 4.5.0
v8: 4.5.103.37
uv: 1.9.1
zlib: 1.2.8
ares: 1.10.1-DEV
icu: 56.1
modules: 46
openssl: 1.0.2h
```

## hexo server命令可能没有
安装hexo-server模块
```
npm install hexo-server
```

## 本地预览时出现Cannot GET /


