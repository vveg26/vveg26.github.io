---

layout: post
title: Chrome和Edge开启多线程下载
subtitle: Chrome和Edge开启多线程下载
date: 2022-4-2
author: veg
header-img: img/home-bg.jpg
catalog: true
tags:
 - 浏览器
 - 多线程下载

---
## 简介

大家在使用 Chrome、Edge 浏览器的时候，下载一些小文件往往都会直接用默认下载器，但是下载大文件的时候，一般都会搭配其他 专业多线程下载工具 使用，但是嫌麻烦...
最近我发现 Chrome、Edge 浏览器有个多线程下载选项，默认是关闭的，我试了下发现挺好用。

## 开启教程

- Chrome 浏览器，地址栏输入并回车：chrome://flags/#enable-parallel-downloading
- Edge 新版浏览器，地址栏输入并回车：edge://flags/#enable-parallel-downloading

就会看到如下图所示，将默认的 Default 改为 Enabled 即可！

![2022-04-02-13-39-53](https://raw.githubusercontent.com/vveg26/ImageHosting/main/images/2022-04-02-Chrome%E5%92%8Cedge%E5%A4%9A%E7%BA%BF%E7%A8%8B%E4%B8%8B%E8%BD%BD%E5%BC%80%E5%90%AF/2022-04-02-13-39-53.png)

然后重启 Chrome 浏览器再去下载个文件试试速度吧！

## 问题
### 为什么下载速度没有翻倍？
两种可能性：
- 该文件不允许多线程下载！
    例如，网站服务器限制了同一时间一个 IP 只能建立 1 个下载连接。
    该文件没有显示文件总大小！
- 部分网站下载文件时，不会显示文件总大小，这样的话就没办法多线程下载了（其他任何多线程下载工具都不行），没有文件总大小，就没办法等份分割文件进行并行下载。
