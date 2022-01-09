---
layout:     post
title:      Sniffing问题导致无法连接Tor
subtitle:   本文鉴于tor和v2ray因sniffing冲突 连接不上的问题
date:       2022-1-7
author:     veg
header-img: img/home-bg.jpg
catalog: true
tags:
    - Proxy
---
# Sniffing问题导致无法连接Tor

本文鉴于tor和v2ray因sniffing冲突 连接不上的问题

## Tor的sniffing问题

<aside>
💡 这是sniffing的问题，关闭sniffing即可。v2ray有一个sniffing功能，它可以检测http和tls流量中的域名并把它提取出来交给vps解析，然后把这些流量的数据包的目的地址重写为解析所得的地址。其本意是解决dns污染的问题，但因为tor连接用了一些不寻常的方式(比如域名和ip不匹配等)，所以此功能反而会使连接失败。

</aside>

解决方法：v2ray的sniffing开关会导致与tor冲突，要服务端与客户端的sniffing都关闭才行

## 实现方案

### 1.客户端关闭sniffing（流量监测）

    这里使用的是V2RAYN客户端，直接关闭

![](https://raw.githubusercontent.com/vveg26/blog_photos/master/proxy/question/tor/tor_q3.png)

### 2.服务端关闭sniffing（因人而异）

     2.1  v2-ui面板 直接关闭

![](https://raw.githubusercontent.com/vveg26/blog_photos/master/proxy/question/tor/tor_q1.png)

2.2 其他

  在相应的配置文件中修改

例：233boy一键脚本

解决方案（自行在配置文件中修改）

![](https://raw.githubusercontent.com/vveg26/blog_photos/master/proxy/question/tor/tor_q2.png)

找相应的配置文件修改就可