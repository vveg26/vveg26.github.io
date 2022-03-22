---
layout:     post
title:      Xray配置2 Vless
subtitle:   vless+ws+tls+ngnix
date:       2022-1-7
author:     veg
header-img: img/home-bg.jpg
catalog: true
tags:
    - Xray
---

# Xray配置2 Vmess

“因为vmess xray和v2ray都支持，所以配置方面都一样，直接拿v2ray的来用”

## 服务器域名配置

1. VPS 一台，重置好主流的操作系统
2. 域名一个，解析到该VPS。
3. 自行开启 BBR 加速（这点很重要）
4. **以下的所有步骤请不要颠倒**

这些内容可以去看

[https://www.notion.so/azurychu/x-ui-wordpress-3656db5c88fa44acbf026175a4814fcd](https://www.notion.so/3656db5c88fa44acbf026175a4814fcd)

## ssl证书申请

因为xray的xtls协议需要证书，所以这里要进行证书申请

证书用acme.sh脚本

1. A-acme证书申请:（证书安装必须在nginx之前，不然nginx会占用80端口，懂得关闭nginx的可以无视）

```
apt-get install -y openssl cron socat curl unzip vim
```

1. 下面这个申请代码已经更新，email=后面这里要输入你的一个邮箱要输入一个邮箱(这个邮箱是要去zerossl注册一个账号，zerossl账号邮箱填入这里，否则会报错）

```
curl https://get.acme.sh | sh -s email=azurychu27@gmail.com
```

```
source ~/.bashrc
```

签发证书

```
.acme.sh/acme.sh --issue -d 你的域名 --standalone -k ec-256
```

![](https://raw.githubusercontent.com/vveg26/blog_photos/master/proxy/xray_config/xray1/xray1_1.png)

若能看到这些，则代表成功，签发成功！继续下一步

将密钥文件存入相应文件夹中 fullchainpath（公钥文件.crt)  keypath（密钥文件.key)

```
.acme.sh/acme.sh --installcert -d 你的域名 --fullchainpath /etc/ssl/private/你的域名.crt --keypath /etc/ssl/private/你的域名.key --ecc
```

```
chmod 755 /etc/ssl/private
#（记得cret和key要有755的权限，没有就手动添加，不然启动xray会报错23）
```

设置更新

```
acme.sh --upgrade --auto-upgrade
```

## Xray配置

1. 官方 脚本

```html
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install -u root
```

1. 随机生成一个uuid
2. 安装完毕以后，在VPS目录 vi /usr/local/etc/xray/config.json 找到 config,json 文件，贴入下面的配置文件

```go
{
  "inbounds": [
    {
      "port": 1112,
      "listen":"127.0.0.1",//只监听 127.0.0.1，避免除本机外的机器探测到开放了 1112 端口
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "98793a0c-f94d-4f75-8cc1-93c73dc0e86e",
            "alterId": 64
          }
        ]
      },
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
        "path": "/ray"
        }
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "settings": {}
    }
  ]
}
```

## Nginx配置

1. 安装nginx
   
    ubuntu或Debian系统

```jsx
apt install curl tar nginx -y
```

1. 启动nginx
   
    PS：在执行下面命令之前，最好重启一下VPS，以免上述的更新命令没有完成导致 Nginx 无法启动，有的搬瓦工机器需要人工重启——因为我遇见过一次）

```jsx
systemctl start nginx
```

1. 现在可以在浏览器中输入你的域名，看看是否可以访问到 Nginx 的欢迎页面

![](https://raw.githubusercontent.com/vveg26/blog_photos/master/proxy/xray_config/xray1/xray1_4.png)

1. 找到VPS目录 vim /etc/nginx/nginx.conf 文件，删除内容，将以下代码复制进去，把里面的域名修改为自己的，把注释中文都删掉，不然会报错

```html
user  root;
worker_processes  1;
#error_log  /etc/nginx/error.log warn;
#pid    /var/run/nginx.pid;
events {
    worker_connections  1024;
}
http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
    #access_log  /etc/nginx/access.log  main;
    sendfile        on;
    #tcp_nopush     on;
    keepalive_timeout  120;
    client_max_body_size 20m;
    #gzip  on;
server {
  listen 443 ssl;  # 监听443端口
  listen [::]:443 ssl;  # 验证证书

  ssl_certificate       /etc/ssl/private/zzz.vegvegveg.ml.crt;
  ssl_certificate_key   /etc/ssl/private/zzz.vegvegveg.ml.key;
  ssl_session_timeout 1d;
  ssl_session_cache shared:MozSSL:10m;
  ssl_session_tickets off;
  root /usr/share/nginx/html;

  ssl_protocols         TLSv1.2 TLSv1.3;
  ssl_ciphers           ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
  ssl_prefer_server_ciphers off;

  server_name           zzz.vegvegveg.ml; #当监听到xxxx.ml:443/ray时，转发到下面proxy_pass里
  location /ray {  # 与 V2Ray 配置中的 path 保持一致
    if ($http_upgrade != "websocket") { 
        return 404;  # WebSocket协商失败时返回404
    } 
    proxy_redirect off;
    proxy_pass http://127.0.0.1:1112;  # 假设WebSocket监听在环回地址的1112端口上;须与V2ray配置端口一致
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
    # Show real IP in v2ray access.log 
    proxy_set_header X-Real-IP $remote_addr; 
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}}
```

1. 设置 Nginx 开机启动，并重新启动 Nginx

```
systemctl enable nginx
systemctl restart nginx
```

1. 检验Xray配置文件

```
systemctl restart xray  #重启xray服务
systemctl status xray  #查看xray运行状态
```

1. 下载伪装网站及部署

默认的网站主程序文件夹在 /usr/share/nginx/html/ ，大家可以自行的替换里面的任何东西（整站程序）

```
rm -rf /usr/share/nginx/html/*
cd /usr/share/nginx/html/
wget https://github.com/V2RaySSR/Trojan/raw/master/web.zip
unzip web.zip
systemctl restart nginx
```

## SSL证书续签

1. 设置证书自动签

```
vi /etc/ssl/private/xray-cert-renew.sh
```

1. 将自己的域名填入，这是续签脚本

```
#!/bin/bash

.acme.sh/acme.sh --install-cert -d a-你的域名 --ecc --fullchain-file  /etc/ssl/private/你的域名.crt --key-file  /etc/ssl/private/你的域名.key
echo "Xray Certificates Renewed"

chmod +r /etc/ssl/private/你的域名.key
echo "Read Permission Granted for Private Key"

sudo systemctl restart xray
echo "Xray Restarted"
```

1. 保存后加权限

```
chmod +x /etc/ssl/private/xray-cert-renew.sh
```

1. 设置自动任务，自动执行脚本

```
crontab -e
```