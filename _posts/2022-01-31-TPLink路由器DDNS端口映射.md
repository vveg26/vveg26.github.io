---
layout:     post
title:      通过TPlink实现外网访问家庭设备
subtitle:   通过TPlink实现外网访问家庭设备
date:       2022-1-31
author:     veg
header-img: img/home-bg.jpg
catalog: true
tags:
    - TPlink
    - 日常
---

# 通过TPlink实现外网访问家庭设备

关键词：DDNS，公网IP，端口映射

## 名词解释

### DNS

IP地址转换为相应的域名地址

如：将182.61.200.6转换为www.baidu.com

182.61.200.6就是IP地址 www.baidu.com 就是域名

再如一个访问网站的流程，如下：

打开浏览器->地址栏输入www.baidu.com->DNS服务器根据域名查询IP地址为182.61.200.6->浏览器和IP为服务器建立tcp连接->数据交互

### DDNS

因为公网IP数量不够，所以有动态公网IP，就是ip会自动变换

### 端口映射

端口映射功能是将一台主机的假IP地址映射成一个真IP地址，当用户访问提供映射端口主机的某个端口时，服务器将请求转到内部提供这种特定服务的主机；利用端口映射功能还可以将一台真IP地址机器的多个端口映射成内部不同机器上的不同端口。

端口映射就是将内网中的主机的一个端口映射到外网主机的一个端口，提供相应的服务。当用户访问外网IP的这个端口时，服务器自动将请求映射到对应局域网内部的机器上。

IP地址为192.168.1.100的设备开放一个端口,如192.168.1.100:8080

另一个192.168.1.101设备开放一个端口80，通过端口映射两个就能通信



## 实现流程

1. 需要一个公网IP，如果没有公网IP，就算有域名，也无法实现外网访问内网（需要内网穿透）

2. TPlink上设置DDNS为一个域名，即将路由器网关IP绑定到域名上
   
   ![](https://raw.githubusercontent.com/vveg26/blog_photos/master/Da/Tplink20220123212454.png)

3. TPlink上打开虚拟服务器功能（端口映射）设置所需要的服务器端口与路由器的端口
   
   ![](https://raw.githubusercontent.com/vveg26/blog_photos/master/Da/Tplink20220123212727.png)

4. 访问域名：路由器端口号   就可以访问了
