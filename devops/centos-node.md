---
date: 2016-12-06 09:53
status: public
title: Centos下node安装
---

## 安装

```shell
curl -sL https://rpm.nodesource.com/setup | bash -

# then install, as root
yum install -y nodejs
```

## 设置淘宝源

1.通过config命令
```shell
npm config set registry https://registry.npm.taobao.org 
npm info underscore （如果上面配置正确这个命令会有字符串response）
```

2.命令行指定
```shell
npm --registry https://registry.npm.taobao.org info underscore 
```

3.编辑 ~/.npmrc 加入下面内容
```shell
registry = https://registry.npm.taobao.org
```
