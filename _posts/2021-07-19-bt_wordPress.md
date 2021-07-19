---
layout:     post
title:      宝塔面板搭建wordpress
subtitle:   宝塔面板+wordpress+某面板+vps+反代+cdn
date:       2021-7-19
author:     veg
header-img: img/jc_bgphoto.jpg
catalog: true
tags:
    - Tor
    - Sniffing
    - V2ray
    - v2-ui
---
<!-- wp:paragraph -->
<p>1购买没有被墙的服务器，用cmd ping一下是否被墙（之后用ssh工具链接）</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>2.freenom申请顶级域名（域名有效期12个月，到期15天内可续费//TODO）</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>https://my.freenom.com/clientarea.php</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>申请完的域名如下</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/mS2W9Mu.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>3.在cloudflare上实现域名解析（PS：将服务器ip绑定到二级域名上）</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>网址：<a href="https://www.cloudflare.com/"></a><a href="https://www.cloudflare.com/">https://www.cloudflare.com/</a></p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>添加站点</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/4wWVnRy.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7915295e-ecbb-4aa5-a280-00101fe1367d/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>添加cloudflare名称服务器到freenom里的域名里</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/emPAg8T.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bc465248-e7da-40a9-8978-c76cbd70d7dd/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/Koa7erX.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7a29366a-149c-478b-9e94-93754962f8d2/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>cloudflare里添加二级域名</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/Koa7erX.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>（上面内容详细自行搜索相关资料）</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>================================================================</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>================================================================</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>4.vps装 ubuntu18或centos7系统（用xshell输入命令）</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>5.安装宝塔界面</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>ssh命令</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>centos：</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">yum install -y wget&amp;&amp; wget -O install.sh &lt;http://download.bt.cn/install/install_6.0.sh&amp;&amp;> sh install.sh
</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">      ubuntu：
</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">wget -O install.sh &lt;http://download.bt.cn/install/install-ubuntu_6.0.sh&amp;&amp;> sudo bash install.sh
</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>6.安装v2-ui面板（一键安装和升级）</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">bash &lt;(curl -Ls &lt;https://blog.sprov.xyz/v2-ui.sh>)
</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>7安装bbr加速服务（加速v2ray）</p>
<!-- /wp:paragraph -->

<!-- wp:syntaxhighlighter/code -->
<pre class="wp-block-syntaxhighlighter-code">#bbr 4 in 1 脚本
wget -N --no-check-certificate "&lt;https://raw.githubusercontent.com/chiakge/Linux-NetSpeed/master/tcp.sh>" &amp;&amp; chmod +x tcp.sh &amp;&amp; ./tcp.sh
</pre>
<!-- /wp:syntaxhighlighter/code -->

<!-- wp:paragraph -->
<p>8.在宝塔面板（ip:8888)安全给v2-ui面板 放行65432端口</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>并在宝塔上一键部署wordpresS</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>9.申请SSL证书</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>我们现在需要登录你安装好的宝塔或者aapanel申请，添加网站，并且申请ssl证书。开启强制HTTPS</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/b3WoX4g.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/75102d6b-03ea-4a72-ac00-86193027082e/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>之后，到网站设置中，复制你的证书路径。如图：</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>10.登录v2-ui面板(ip:65432）</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>*修改登录用户名和密码以及面板数据（修改完的路径为ip:/45454/azurychu)重启v2ui</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/aUB5nB6.png" alt=""/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>创建节点</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>，配置为vless+ws，tls不打开</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/0qLxv1N.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0c04f59c-0d53-4a41-9a34-34df8bacd2f5/TGG8HU8768PDEN50RNC.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>11Nigx反代，让你的小鸡用上443端口</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>进入宝塔界面设置伪静态</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/PDYHx41.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4ef38f8f-1e77-4a3f-9bba-9a874965e790/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>在配置文件中添加代码</p>
<!-- /wp:paragraph -->
```fsharp
location ^~ /bobo {
    proxy_pass http://127.0.0.1:45454/bobo;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
}
location /Date2021 {
        proxy_redirect off;
        proxy_pass http://127.0.0.1:54321;
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

<!-- wp:paragraph -->
<p>在软件商店里重载并重启nigx服务</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>去v2-ui里面生成链接,在客户端里配置(<a href="http://xn--jc-tz2c54t3kqcw9d.yinweiqi.ga">路径变为jc.yinweiqi.ga</a>)</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>在客户端里配置时需要修改文件</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/Jbpw5pf.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2c279868-6a17-4770-a3fd-39e57ddb30af/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>开启CDN加速，点亮小云朵或者使用CF优选IP（PS：cloudflare的CDN慢的要死，可使用cloudflare优选ip）</p>
<!-- /wp:paragraph -->

<!-- wp:paragraph -->
<p>开启CDN加速</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/kst1tfj.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/019190b0-56d5-4712-8c9e-8fabc4049cc2/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>优选IP，使用优选IP软件，将选好的IP放入，over</p>
<!-- /wp:paragraph -->

<!-- wp:image {"sizeSlug":"large"} -->
<figure class="wp-block-image size-large"><img src="https://i.imgur.com/bUEwkNO.png" alt="https://s3-us-west-2.amazonaws.com/secure.notion-static.com/33f70a24-8408-475d-a8d2-4922bb90338c/Untitled.png"/></figure>
<!-- /wp:image -->

<!-- wp:paragraph -->
<p>续费域名（freenom自动邮件）</p>
<!-- /wp:paragraph -->