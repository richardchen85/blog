---
title: nginx安装、配置及支持https不完全手册
date: 2018-04-14 21:23:14
updated: 2019-05-15 13:52:29
comments: 1
categories: 服务器
tags: []
permalink: /post/nginx-install-config-supoort-https.html
---

`nginx` 已经成为 `web` 开发者不可或缺的工具，虽然平时我们用到的服务器，运维都已经帮我们安装配置好了 `nginx` ，但作为开发人员，掌握如何搭建和配置服务器环境，能让我们的工作更加得心应手。

<!--more-->

## 安装 nginx

`nginx` 安装方式有两种，一种是直接通过包管理工具（如：`yum` 或 `apt-get`）安装，另一种方式是通过编译源码安装。这里我们选择自由度更高（当然也更麻烦）的编译源码安装。

首先是安装依赖：

`nginx` 安装前需要安装几个依赖：`zlib` 、 `zlib-devel`、`openssl`、`openssl-devel`、`pcre` 和 `pcre-devel`，大多数主机都默认安装了，如果没有安装的话，可以通过自带的包管理工具 `yum` 或 `apt-get` 安装。

这里我们下载 `nginx-1.12.2` 最新的稳定版本。

```bash
# 下载
curl -O http://nginx.org/download/nginx-1.12.2.tar.gz

# 解压
tar -zxf nginx-1.12.2.tar.gz

# 转到目录
cd nginx-1.12.2

# 创建安装目录
mkdir -p /export/servers/nginx

# 执行配置命令
# --prefix 表示要安装的路径
# --with 表示要安装哪些模块，这里我们安装 http_ssl_module 用于支持 https
# 注意：要用 https 需要依赖 OpenSSL 库
# 具体查看： http://nginx.org/en/docs/configure.html
./configure --prefix=/export/servers/nginx --with-http_ssl_module

# 执行 make 安装
make && make install
```

到此，安装就已经完成了，你可以到 `/export/servers/nginx/` 目录看到安装好的 `nginx` 的文件。

## 配置

### nginx 自启动

安装完成过后，首先我们要配置的就是让 `nginx` 随系统自动启动：

```bash
# 编辑 /etc/rc.local 文件（这里需要 root 权限）
sudo vi /etc/rc.local
# 在文件中添加如下内容
/export/servers/nginx/sbin/nginx
# 然后保存退出
```

> 注意：从 centos7 开始，开机时默认将不执行 /etc/rc.local，如果想要让它执行，必须更改该文件的权限 `chmod +x /etc/rc.d/rc.local`。当然也可以通过增加 systemctl service 的方式来实现。

因为我们将 `nginx` 安装到了 `/export/servers/nginx/` 目录，所以每次要执行 `nginx` 命令都要写全路径。所以最好是建立一个 `nginx` 软链接到 `/usr/local/bin/` 目录：

```bash
ln -s /export/servers/nginx/sbin/nginx /usr/local/bin/nginx
```

这样，我们可以直接运行 `nginx -s stop` 或 `nginx -s reload` 来停止或者重启 `nginx` 了。

### nginx 支持多网站多域名

支持多个网站，我们只需要在 `nginx` 配置文件增加 `server` 节点就可以实现。配置文件在安装目录中 `/export/servers/nginx/conf/nginx.conf`（这就是为什么我喜欢指定`nginx` 的安装目录了，因为默认安装时各种文件会放在不同的目录中）。

```bash
cd /export/servers/nginx/conf/

# 修改 nginx.conf
vi nginx.conf
# 在 http 节点最后增加一行配置
# 这里表示将 domains 目录下所有以 .conf 结尾的文件都包含进配置文件中
include domains/*.conf;
# 保存退出

# 创建一个网站配置文件
mkdir domains
cd domains
touch server1.conf
```

在 `server1.conf` 文件中增加一个 server 节点配置 比如

```conf
server {
  listen 80; # 监听80端口
  server_name abc.com; # 网站域名，多个域名用空格隔开
  root /export/wwwroot/abc.com/; # 网站根目录

  # nginx 日志存放位置
  access_log /export/servers/nginx/logs/abc.com/access.log;
  error_log  /export/servers/nginx/logs/abc.com/error.log;
}
```

如果我们的网站是 `java` 或者 `nodejs` 写的，那么我们就需要通过配置 `nginx` 代理将请求代理到我们的 `java` 或者 `nodejs` 服务了：

```conf
server {
  listen 80;
  server_name abc.com;
  # 匹配所有请求
  location / {
    # 代理请求，假如我们的应用监听了 8080 端口
    proxy_pass http://127.0.0.1:8080;
  }
}
```

然后运行命令 `nginx -s reload` 重新加载一下 `nginx` 配置，配置就生效了。更多 `nginx` 的配置信息可以查看文档：[http://nginx.org/en/docs/](http://nginx.org/en/docs/)

### nginx 日志按日期分割

`nginx` 可以为每一个 `server` 节点指定日志文件，但是久而久之，这个文件会越来越大。通常的处理方法是将日志文件按日期分割，并且定期删除旧的日志文件。

假设我们 `nginx` 日志文件目录结构如下：

```conf
# 每个网站单独一个目录，来存放各自的日志文件
/export/server/nginx/logs/
 ├ abc.com
 │ ├ access.log
 │ └ error.log
 └ def.com
   ├ access.log
   └ error.log
```

首先来看如何将日志文件按日期分割：

创建一个 `sh` 文件：`touch nginxLogRotate.sh`。文件内容如下：

```bash
#!/bin/bash

# 到 nginx 日志目录
cd /export/servers/nginx/logs/

# 获取昨天的日期组成字符串 YYYY-mm-dd
YESTERDAY=$(date -d "yesterday" +%Y-%m-%d)

# 复制文件的函数 rotate src dist
function rotate() {
  local src=$1
  local dist=$2
  if test -f ${src}; then
    mv ${src} ${dist}
  else
    echo "can not find: " ${src}
  fi
}

# 遍历当前目录
for dir in $(ls .)
do
  # 复制每个目录下的日志文件并以日期命名
  if test -d ${dir}; then
    rotate ${dir}/access.log ${dir}/access_${YESTERDAY}.log
    rotate ${dir}/error.log ${dir}/error_${YESTERDAY}.log
    # 删除 7 天以前的日志文件
    find $dir -mtime +7 -type f -name '*.log' -exec rm -f {} \;
  fi
done

# 重新加载日志文件
kill -USR1 $(cat nginx.pid)
```

将 `nginxLogRotate.sh` 添加执行权限：`chmod +x nginxLogRotate.sh`，这样我们每运行一次这个文件就会分割一次日志文件。所以我们需要让服务器每天零时能够自动运行这个文件，以达到按天分割日志，并清除 7 天前旧文件的目的。

这时候我们需要用到 `linux` 的计划任务配置：`cron`。`windows` 的定时计划任务我们很熟悉了，`cron` 也有类似的功能：指定某个时间执行特定的任务，这里我们需要让 `cron` 每天零时执行 `nginxLogRotate.sh` 这个文件。

`cron` 有个配置文件： `/etc/crontab`，我们可以在这个配置文件里配置我们的任务：

```bash
vi /etc/crontab

# 内容如下：
# 表示在每天 00:01 时执行
1 0 * * * root /export/servers/nginx/logs/nginxLogRotate.sh
# 保存退出

# 重启 cron
crond restart
```

## 让 nginx 支持 https

支持 `https` 也比较简单，主要包含两部分：签署第三方可信任的 `SSL` 证书 和 配置 `HTTPS`，
我们在安装 `nginx` 的时候已经开启了 `https` 模块，所以只需要配置就行了，现在我们来看看如何签署第三方可信任的 `SSL` 证书。

第三方证书有很多机构可以提供，这里以 [Let’s Encrypt](https://letsencrypt.org/) 推荐的[certbot](https://certbot.eff.org) 为例。

### 安装 certbot

`certbot` 依赖 `python`，所以先确认您已经安装了 `python` 2.7 或者 3.4+。并且需要先安装 `epel-release`（可以通过包管理工具安装）。

`certbot` 官方首页（ [https://certbot.eff.org/](https://certbot.eff.org/) ）提供了如何在不同系统下安装 `certbot` 的教程，这里不做详细介绍。需要注意一点是，因为我们的 `nginx` 是自己编译安装的，所以我们在第一个下拉选项框里应该选择 `None of the above` 而不是 `nginx`。

```bash
# 先停掉 nginx
nginx -s stop

# 生成证书
# 这里需要注意一下，因为 let's encrypt 会验证域名的所有权，你必须将域名先解析到你的主机上
certbot certonly --standalone --email xxx@xx.com -d abc.com

# 查看证书
cd /etc/letsencrypt/live/abc.com/
ls
```

接下来配置 nginx 的 server1.conf 配置文件，然后重启 `nginx`：`nginx -s reload`。

```conf
server {
  # 添加以下几行
  listen 443 ssl;

  ssl_certificate /etc/letsencrypt/live/abc.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/abc.com/privkey.pem;
}
```

### 自动续签证书

因为 `Let's Encrypt` 的证书只能免费使用 90 天，过期后必须运行命令 `certbot renew` 重新续签。为了省心，我们再次使用 `linux` 的计划任务配置 `cron`。

```bash
vi /etc/crontab

# 表示每天 0 点和 12 都执行一次命令
0 0,12 * * * root certbot renew
# 保存退出

# 重启 cron
crond restart
```

以上就是关于如何安装 `nginx`，如果配置 `nginx` 多网站多域名，以及支持 `http` 的过程，若本文有什么遗漏或者失误，会及时更新到本文。

## 参考资料

- [http://nginx.org/en/docs/](http://nginx.org/en/docs/)
- [https://letsencrypt.org/getting-started/](https://letsencrypt.org/getting-started/)
- [https://certbot.eff.org/](https://certbot.eff.org/)
