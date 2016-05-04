---
layout: post
title: Let’s Encrypt!
---

关注已久的 [Let’s Encrypt](https://letsencrypt.org/) 进入Public Beta阶段了，于是准备弄一个SSL证书给blog试试。运行环境如下：

- AWS东京Region，EC2为t2.nano
- AMI为Bitnami提供的wordpress
- OS为Ubuntu Server 14.04.4 LTS
- Webserver为apache

用git下载客户端并安装依赖：

    git clone https://github.com/letsencrypt/letsencrypt
    cd letsencrypt
    ./letsencrypt-auto --help

申请证书时客户端需要使用80端口，因此先停掉apache：

    sudo /opt/bitnami/ctlscript.sh stop apache

申请证书：

    ./letsencrypt-auto certonly --standalone

按照屏幕提示依次输入联系email、同意Terms of Service、输入域名。

为apache配置SSL，编辑 `/opt/bitnami/apache2/conf/bitnami/bitnami.conf`

添加SSLCertificate文件：

```
<VirtualHost _default_:443>
    DocumentRoot "/opt/bitnami/apache2/htdocs"
    SSLEngine on
     
    SSLCertificateFile "/etc/letsencrypt/live/blog.zhiguang.me/cert.pem"
    SSLCertificateKeyFile "/etc/letsencrypt/live/blog.zhiguang.me/privkey.pem"
    SSLCertificateChainFile "/etc/letsencrypt/live/blog.zhiguang.me/fullchain.pem"
```

增加HSTS支持：

```
<VirtualHost _default_:443>
    DocumentRoot "/opt/bitnami/apache2/htdocs"
    SSLEngine on

    # HSTS for 1 year
    Header always set Strict-Transport-Security "max-age=31536000; includeSubDomains"
```

将所有HTTP流量301重定向到HTTPS：

```
<VirtualHost _default_:80>
    DocumentRoot "/opt/bitnami/apache2/htdocs"

    ## Rewrite all HTTP requests to HTTPS with 301 redirection
    RewriteEngine On
    RewriteCond %{HTTPS} !=on
    RewriteRule ^/(.*) https://%{SERVER_NAME}/$1 [L,R=301]
```

启动apache：

    sudo /opt/bitnami/ctlscript.sh start apache

由于Let’s Encrypt的证书有效期只有90天，因此编写一个脚本自动地renew证书：

`/home/bitnami/letsencrypt_renew.sh`

```bash
#!/bin/bash

sudo /opt/bitnami/ctlscript.sh stop apache
/home/bitnami/letsencrypt/letsencrypt-auto renew --force-renew
sudo /opt/bitnami/ctlscript.sh start apache
```

编辑crontab，每月1日自动调用脚本：

    0 0 1 * * /home/bitnami/letsencrypt_renew.sh >> /home/bitnami/letsencrypt_renew.log 2>&1

这样就大功告成了。使用 [SSL Labs](https://www.ssllabs.com/index.html) 测试一下证书的配置，Perfect！

![ssl-labs](/public/img/2016/ssl-labs.png)