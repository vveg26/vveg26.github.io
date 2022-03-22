---
layout:     post
title:     V2rayN最新版使用clash配置文件
subtitle:   V2rayN最新版使用clash配置文件
date:       2022-3-22
author:     veg
header-img: img/home-bg.jpg
catalog: true
tags:
    - V2rayN
---

## 最新版V2rayN
### 添加订阅
1. 点击订阅->添加订阅
2. 直接将clash订阅复制进去
3. 点击确定
![2022-03-22-17-14-39](https://raw.githubusercontent.com/vveg26/blog_photos/main/images/2022-03-22-v2rayN%E6%9C%80%E6%96%B0%E7%89%88%E4%BD%BF%E7%94%A8clash%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/2022-03-22-17-14-39.png)

### 更新订阅
1. 点击订阅->更新订阅
2. 双击服务器
3. core内核选择clash
4. 点击确定
![2022-03-22-17-19-31](https://raw.githubusercontent.com/vveg26/blog_photos/main/images/2022-03-22-v2rayN%E6%9C%80%E6%96%B0%E7%89%88%E4%BD%BF%E7%94%A8clash%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/2022-03-22-17-19-31.png)

### 端口问题
端口问题延用配置文件中的端口
配置文件编辑
![2022-03-22-22-01-16](https://raw.githubusercontent.com/vveg26/blog_photos/main/images/2022-03-22-v2rayN%E6%9C%80%E6%96%B0%E7%89%88%E4%BD%BF%E7%94%A8clash%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/2022-03-22-22-01-16.png)

配置文件中的端口设置在此

![2022-03-22-22-00-13](https://raw.githubusercontent.com/vveg26/blog_photos/main/images/2022-03-22-v2rayN%E6%9C%80%E6%96%B0%E7%89%88%E4%BD%BF%E7%94%A8clash%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/2022-03-22-22-00-13.png)

```
    port: 7890
    socks-port: 7891
    allow-lan: true
    mode: Rule
    log-level: info
    external-controller: :9090
```

port 端口
socks-port socks端口
external-controller 外部端口

详细的clash设置可打开这个网页

 https://clash.razord.top/#/proxies

详细clash参数配置可以参考以前的博客