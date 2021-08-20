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
关键词：v2-ui(vmess+websocket+tls)，bbr4加速，宝塔面板，wordpress

## 服务器购买

1. 购买没有被墙的服务器（这里推荐便宜的厂商racknerd,DigtalOeason,Buyvm),创建一个机子
2. 用本机ping一下服务器ip，需要ping的通过
3. 用ssh工具连接自己的服务器，开始操作

## 域名的解析绑定

1. 这里使用的是freenom免费提供一年的域名 ⇒ [freenom网址](https://www.freenom.com/) 
2. 在freenom申请顶级域名（域名有效期12个月，到期15天内可续费，自动续费请查看）
    - 申请完的域名如下

    ![https://i.imgur.com/mS2W9Mu.png](https://i.imgur.com/mS2W9Mu.png)

3. 在cloudflare上实现域名解析（将服务器ip绑定到二级域名上）⇒[cloudflare](https://www.cloudflare.com/)
    - 添加站点为刚刚申请的顶级域名

    ![https://i.imgur.com/4wWVnRy.png](https://i.imgur.com/4wWVnRy.png)

    - 添加cloudflare名称服务器到freenom里的域名里

    ![https://i.imgur.com/emPAg8T.png](https://i.imgur.com/emPAg8T.png)

    ![https://i.imgur.com/Koa7erX.png](https://i.imgur.com/Koa7erX.png)

- cloudflare里添加二级域名DNS解析

[https://imgur.com/qmS6CSX](https://i.imgur.com/qmS6CSX.png)

（上面内容详细自行搜索相关资料）

================================================================

================================================================

## 安装宝塔面板

1. vps装 ubuntu18
2. 安装宝塔界面

    ubuntu：

    ```fsharp
    **wget -O [install.sh](http://install.sh/) [http://download.bt.cn/install/install-ubuntu_6.0.sh](http://download.bt.cn/install/install-ubuntu_6.0.sh) && sudo bash [install.sh](http://install.sh/)**
    ```

    其他自行官网搜索

3. 登录宝塔面板,安装推荐的LNMP

    并在宝塔上一键部署wordpress(应用商店中一键部署）

## 安装x-ui面板

1. 安装x-ui面板（一键安装和升级）

    ```fsharp
    bash <(curl -Ls https://blog.sprov.xyz/v2-ui.sh)
    ```

2. 安装bbr加速服务四合一脚本（加速v2ray）

    ```fsharp
    wget -N --no-check-certificate "[https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh](https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh)" && chmod +x [tcp.sh](http://tcp.sh/) && ./tcp.sh
    ```

3. 在宝塔面板安全给x-ui面板 放行54321端口
4. 登录x-ui面板，更改相关的配置
    - 修改登录用户名和密码以及面板数据（修改完的路径为ip:/45454/azurychu)重启v2ui

    ![https://i.imgur.com/aUB5nB6.png](https://i.imgur.com/aUB5nB6.png)

    - 创建节点

        配置为vless+ws，tls不打开（vmess同，sniffing关闭可访问洋葱）

        ![https://i.imgur.com/0qLxv1N.png](https://i.imgur.com/0qLxv1N.png)

## Nigx反代

1. 设置伪静态为wordpress，申请SSL证书

    我们现在需要登录你安装好的宝塔或者aapanel申请，添加网站，并且申请ssl证书。开启强制HTTPS

![https://i.imgur.com/b3WoX4g.png](https://i.imgur.com/b3WoX4g.png)

1. Nigx反代，让你的小鸡用上443端口

    进入宝塔界面设置伪静态

    ![https://i.imgur.com/PDYHx41.png](https://i.imgur.com/PDYHx41.png)

    1. 在配置文件中添加代码

    ```fsharp
    location ^~ /azurychu {
        proxy_pass http://127.0.0.1:45454/azurychu; 
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
    location /mis {
            proxy_redirect off;
            proxy_pass http://127.0.0.1:34176;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            proxy_set_header Host $http_host;
            proxy_read_timeout 300s;
            # Show realip in v2ray access.log
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      }
    ```

![https://i.imgur.com/UkS7o8k.png](https://i.imgur.com/UkS7o8k.png)

1. 在软件商店里重载并重启nigx服务

## 客户端配置

1. 在客户端里配置时需要修改文件
2. 去v2-ui里面生成链接,在客户端里配置([路径变为jc.yinweiqi.ga](http://xn--jc-tz2c54t3kqcw9d.yinweiqi.ga/))

![https://i.imgur.com/Jbpw5pf.png](https://i.imgur.com/Jbpw5pf.png)

1. 开启CDN加速，点亮小云朵或者使用CF优选IP（PS：cloudflare的CDN慢的要死，可使用cloudflare优选ip）

    开启CDN加速

    ![https://i.imgur.com/kst1tfj.png](https://i.imgur.com/kst1tfj.png)

    优选IP，使用优选IP软件，将选好的IP放入，over

    ![https://i.imgur.com/bUEwkNO.png](https://i.imgur.com/bUEwkNO.png)

## 续费域名（freenom自动邮件）