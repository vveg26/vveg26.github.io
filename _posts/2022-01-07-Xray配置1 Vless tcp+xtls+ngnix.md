---
layout:     post
title:      Xray配置1 Vless
subtitle:   vless+xtls
date:       2022-1-7
author:     veg
header-img: img/home-bg.jpg
catalog: true
tags:
    - Proxy
---

# Xray配置1 Vless

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

## Xray配置（面板和官方二选一）

### 1. x-ui面板配置

1. 直接安装x-ui面板

```go
bash <(curl -Ls https://raw.githubusercontent.com/sprov065/x-ui/master/install.sh)
```

1. 更改x-ui面板的各项配置（公钥，私钥），让x-ui面板也用上Https协议啦，签上证书

![](https://raw.githubusercontent.com/vveg26/blog_photos/master/proxy/xray_config/xray1/xray1_2.png)

1. 更改用户名密码，x-ui面板监听端口，保证其更加安全，这里不做演示
2. 点击入站列表⇒ 添加一个节点

![](https://raw.githubusercontent.com/vveg26/blog_photos/master/proxy/xray_config/xray1/xray1_3.png)

 5.导出配置，在客户端自己导入，完工

### xray官方配置

1. 官方 脚本

```html
bash -c "$(curl -L https://github.com/XTLS/Xray-install/raw/main/install-release.sh)" @ install -u root
```

1. 随机生成一个uuid
2. 安装完毕以后，在VPS目录 vi /usr/local/etc/xray/config.json 找到 config,json 文件，贴入下面的配置文件（这里主要是用xray监听443端口，看是不是vless协议，然后回落到ngnix走正常服务或者走vless协议翻墙）

```go
{
    "log": {
        "loglevel": "warning"
    }, 
    "inbounds": [
        {
            "listen": "0.0.0.0", 
            "port": 443,   //监听的端口
            "protocol": "vless", 
            "settings": {
                "clients": [
                    {
                        "id": "20ee81ac-70bf-4e66-a43d-84acc509325a", //此处为你的UUID
                        "level": 0, 
                        "email": "v2rayssr@v2rayssr.com",
                        "flow":"xtls-rprx-direct"
                    }
                ], 
                "decryption": "none", 
                "fallbacks": [
                    {
                        "dest": 33222  //默认回落端口，如果不是vless协议就走这里
                    }, 
                    {
                        "alpn": "h2", 
                        "dest": 33223  //https回落端口，回落到ngnix，如果不是vless协议就走这里
                    }
                ]
            }, 
            "streamSettings": {
                "network": "tcp", 
                "security": "xtls", 
                "xtlsSettings": {
                    "serverName": "zzz.vegvegveg.ml",  //你的域名
                    "alpn": [
                        "h2", 
                        "http/1.1"
                    ], 
                    "certificates": [
                        {
                            "certificateFile": "/etc/ssl/private/zzz.vegvegveg.ml.crt",
                            "keyFile": "/etc/ssl/private/zzz.vegvegveg.ml.key" //证书路径
                        }
                    ]
                }
            }
        }
    ], 
    "outbounds": [
        {
            "protocol": "freedom", 
            "settings": { }
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

1. 找到VPS目录 vim /etc/nginx/nginx.conf 文件，删除内容，将以下代码复制进去，把里面的域名修改为自己的，主要是把回落到的33222，33223，80端口都转向静态网页

```go
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
server { //xray回落到33222端口，监听33222端口，如果有就到静态主页
    listen 127.0.0.1:33222;
    server_name  zzz.vegvegveg.ml;   //你的域名
    root /usr/share/nginx/html;
    index index.php index.html index.htm; 
}
 server { //监听33223端口，回落到静态主页
    listen 127.0.0.1:33223 http2;
    server_name  zzz.vegvegveg.ml;  //你的域名
    root /usr/share/nginx/html;
    index index.php index.html index.htm;
}
server {
    listen       0.0.0.0:80;
    server_name  zzz.vegvegveg.ml; //你的域名
    return 301 https://zzz.vegvegveg.ml$request_uri;
}
}
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

2. 将自己的域名填入，这是续签脚本

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

1. 在里面输入以下代码，意思为每月1日自动申请证书

```
0 1 1 * *   bash /etc/ssl/private/xray-cert-renew.sh
```