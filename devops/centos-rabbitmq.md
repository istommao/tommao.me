---
date: 2016-12-06 17:06
status: public
tags: rabbitmq
title: Centos下rabbitmq安装配置
---

## 安装依赖
```shell
yum install erlang
```
`rabbitmq官网下载` http://www.rabbitmq.com/install-rpm.html

```shell
wget http://www.rabbitmq.com/releases/rabbitmq-server/v3.5.5/rabbitmq-server-3.5.5-3.noarch.rpm

yum install rabbitmq-server-3.5.5-3.noarch.rpm
```
`加入开机启动`
```shell
chkconfig rabbitmq-server on
```
`启动`
```shell
service rabbitmq-server start
```


## 常用命令
### 启动服务
```shell
rabbitmq-server start &
```
### 停止服务
```shell
rabbitmq-server stop &
```
### 查看队列中的数据
```shell
rabbitmqctl list_queues
```
### 查看节点状态
```shell
rabbitmqctl status
```
### 查看分布式节点状态
```shell
rabbitmqctl cluster_status
```
### 停止应用
```shell
rabbitmqctl stop_app
```
### 启动应用
```shell
rabbitmqctl start_app
```
## 重设应用
```shell
rabbitmqctl reset
```
### 加入分布式节点
```shell
rabbitmqctl cluster 节点标识
```
### 具体参考
```shell
rabbitmqctl –help
```

### 查看未确认消息
```shell
rabbitmqctl list_queues -p / name
```

### 添加修改用户
```shell
rabbitmqctl delete_user  guest
rabbitmqctl add_user admin 123456
rabbitmqctl set_user_tags admin administrator
```

### 添加权限
```shell
rabbitmqctl set_permissions [-p vhost] {user} {conf} {write} {read}
```

### 修改密码
```shell
rabbitmqctl change_password guest 123456
```
