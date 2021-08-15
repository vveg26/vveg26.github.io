---
layout:     post
title:      通过宝塔面板一键部署wordpress
subtitle:   通过freenom白嫖来的域名绑定自己的wordpress
date:       2021-7-19
author:     veg
header-img: img/jc_bgphoto.jpg
catalog: true
tags:
    - 服务器
---
1购买没有被墙的服务器，用cmd ping一下是否被墙（之后用ssh工具链接）

2.freenom申请顶级域名（域名有效期12个月，到期15天内可续费//TODO）

https://my.freenom.com/clientarea.php

申请完的域名如下

![https://i.imgur.com/mS2W9Mu.png](https://i.imgur.com/mS2W9Mu.png)

3.在cloudflare上实现域名解析（PS：将服务器ip绑定到二级域名上）

网址：[https://www.cloudflare.com/](https://www.cloudflare.com/)

添加站点

![https://i.imgur.com/4wWVnRy.png](https://i.imgur.com/4wWVnRy.png)

添加cloudflare名称服务器到freenom里的域名里

![https://i.imgur.com/emPAg8T.png](https://i.imgur.com/emPAg8T.png)

![https://i.imgur.com/Koa7erX.png](https://i.imgur.com/Koa7erX.png)

cloudflare里添加二级域名

![https://i.imgur.com/Koa7erX.png](https://i.imgur.com/Koa7erX.png)

（上面内容详细自行搜索相关资料）

================================================================

================================================================

4.vps装 ubuntu18或centos7系统（用xshell输入命令）

5.安装宝塔界面

ssh命令

ubuntu：

```fsharp
wget -O [install.sh](http://install.sh/) [http://download.bt.cn/install/install-ubuntu_6.0.sh](http://download.bt.cn/install/install-ubuntu_6.0.sh) && sudo bash [install.sh](http://install.sh/)
```

自行官网搜索

7安装bbr加速服务四合一脚本（加速v2ray）

```fsharp
wget -N --no-check-certificate "[https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh](https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh)" && chmod +x [tcp.sh](http://tcp.sh/) && ./tcp.sh
```
8登录宝塔面板，并安装推荐的LNMP

并在宝塔上一键部署wordpress(应用商店中一键部署）

//TODO 此处省略一下

9.设置伪静态为wordpress，申请SSL证书

我们现在需要登录你安装好的宝塔或者aapanel申请，添加网站，并且申请ssl证书。开启强制HTTPS

![https://i.imgur.com/b3WoX4g.png](https://i.imgur.com/b3WoX4g.png)

10.安装完成，访问自己网站吧

## PS：
想要富强魔法的话，可以参考这篇文章
这篇文章提供了宝塔面板和xray面板共存的方法
https://www.v2rayssr.com/xray_v2ui_bt.html


11.freenom自动续期（看下一篇）