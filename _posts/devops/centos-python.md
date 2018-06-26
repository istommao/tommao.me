---
date: 2016-12-05 16:34
status: public
tags: 运维
title: Centos下Python生产环境配置
---

## pyenv 安装

```shell
git clone https://github.com/yyuu/pyenv.git ~/.pyenv
```

`vim ~/.bashrc`
```shell
export PYENV_ROOT="$HOME/.pyenv"
export PATH="$PYENV_ROOT/bin:$PATH"
eval "$(pyenv init -)"
```

`生效的三种方式`
- 重新打开一个session
- `source ~/.bashrc`
- `exec $SHELL -l`

## 安装Python3.5.0版本

```shell
pyenv install 3.5.0 -v
```

## pyenv-virtualenv

`创建python虚拟环境`
```shell
pyenv virtualenv 3.5.0 project_env
```

`具体使用请看`
https://github.com/yyuu/pyenv-virtualenv
