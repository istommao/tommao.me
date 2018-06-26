---
date: 2016-12-08 13:57
status: public
title: Centos下supervisor配置
---

## 安装

```shell
pip install supervisord
```

## gunicorn 配置文件
`server.conf`
```shell
[program:gunicorn_name]
command=/path/gunicorn server.wsgi:application  -c /path/gunicorn.conf
directory=/path/project

autostart=true
autorestart=true
stdout_logfile=/path/gunicorn.out.log
stdout_logfile_maxbytes=10MB
stdout_logfile_backups=10
stderr_logfile=/path/gunicorn.err.log
stderr_logfile_maxbytes=10MB
stderr_logfile_backups=10
stopsignal=QUIT
killasgroup=true

user=user
```
`gunicorn.conf`
```shell
bind = 'unix:/tmp/gunicorn-name.sock'
backlog = 2048
timeout = 30
preload = True
pidfile = '/tmp/gunicorn-name.pid'
workers = 2
errorlog = '/path/gunicorn-error.log'
loglevel = 'info'
accesslog = '/path/gunicorn-access.log'
access_log_format='%({X-Real-IP}i)s %(l)s %(u)s %(q)s %(t)s "%(r)s %(b)s" "%(s)s" "%(f)s" "%(a)s"'
proc_name = 'gunicorn-name'
keep_alive = 2
max_requests = 1000
```
`%({X-Real-IP}i)s` 需要nginx中做以下配置
```shell
proxy_set_header X-Real-IP $remote_addr;
```

## celery 配置文件
`celeryd.conf`
```shell
[program:celery]
directory=/path/project/
command=/path/celery worker --app=server.celery --loglevel=INFO

numprocs=1
stdout_logfile=/path/celery.out.log
stderr_logfile=/path/celery.err.log
stdout_logfile_maxbytes=10MB
stderr_logfile_maxbytes=10MB
autostart=true
autorestart=true
killasgroup=true
stopsignal=QUIT
user=user

[program:celery_beat]
directory=/path/project/

command=/path/celery beat --app=server.celery --loglevel=INFO
autostart=true
autorestart=true
stdout_logfile=/path/celery_beat.out.log
stdout_logfile_maxbytes=10MB
stderr_logfile=/path/celery_beat.err.log
stderr_logfile_maxbytes=10MB

user=user
```
