---
date: 2016-11-29 13:07
status: public
title: HTTP2折腾记
---


`2016-08-23`
> 折腾了近一天终于把Nginx配置http2成功了！
其实原因已经知道了，是openssl的版本太低导致的未启用http2, OpenSSL 1.0.2后才能配置成功!
Note that accepting HTTP/2 connections over TLS requires the “Application-Layer Protocol Negotiation” (ALPN) TLS extension support, which is available only since OpenSSL version 1.0.2. Using the “Next Protocol Negotiation” (NPN) TLS extension for this purpose (available since OpenSSL version 1.0.1) is not guaranteed.

[相关介绍文章](https://iyaozhen.com/nginx-http2-conf.html)

## Let's Encrypt 证书安装
[相关文章](https://blog.kuoruan.com/71.html)

## 在Nginx中配置HTTP2 相关文章
- [https://luolei.org/update-http2-nginx](https://luolei.org/update-http2-nginx)
- [https://www.mf8.biz/71/?spm=5176.100239.blogcont7171.10.FWQjW9](https://www.mf8.biz/71/?spm=5176.100239.blogcont7171.10.FWQjW9)
- [https://ye11ow.gitbooks.io/http2-explained/content](https://ye11ow.gitbooks.io/http2-explained/content)
- [https://iyaozhen.com/nginx-http2-conf.html](https://iyaozhen.com/nginx-http2-conf.html)
- [https://www.nginx.com/blog/supporting-http2-google-chrome-users/](https://www.nginx.com/blog/supporting-http2-google-chrome-users/)

## Nginx源码安装
[传送门](http://opensgalaxy.com/2015/07/20/nginx%E6%BA%90%E7%A0%81%E5%AE%89%E8%A3%85/)

### ubuntu依赖包
```bash
apt-get install build-essential
apt-get install libtool
```

### RHEL、Centos依赖包

```bash
yum -y install gcc automake autoconf libtool make
```

#### g++安装
```bash
yum install gcc gcc-c++
```

> 需要先装pcre, zlib,  pcre为了重写rewrite, zlib为了gzip压缩.
选定源码目录(可以是任何目录)
cd /usr/local/src


### 安装PCRE库
```bash
ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/ 下载最新的 PCRE 源码包：
cd /usr/local/src
wget ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre/pcre-8.37.tar.gz 
tar -zxvf pcre-8.37.tar.gz
cd pcre-8.37

./configure
make
make install
```

### 安装zlib库
```bash
http://zlib.net/zlib-1.2.8.tar.gz 下载最新的 zlib 源码包：
cd /usr/local/src
wget http://zlib.net/zlib-1.2.8.tar.gz
tar -zxvf zlib-1.2.8.tar.gz
cd zlib-1.2.8
./configure
make
make install
```

### 安装ssl(如果没有的话)
```bash
cd /usr/local/src
wget http://www.openssl.org/source/openssl-1.0.1c.tar.gz
tar -zxvf openssl-1.0.1c.tar.gz
```

### 安装nginx
```bash
cd /usr/local/src
wget http://nginx.org/download/nginx-1.8.0.tar.gz
tar -zxvf nginx-1.8.0.tar.gz
cd nginx-1.8.0

./configure --sbin-path=/usr/local/nginx/nginx \
--conf-path=/usr/local/nginx/nginx.conf \
--pid-path=/usr/local/nginx/nginx.pid \
--with-http_ssl_module \
--with-pcre=/usr/local/src/pcre-8.37 \
--with-zlib=/usr/local/src/zlib-1.2.8 \
--with-openssl=/usr/local/src/openssl-1.0.1c


make
make install
```