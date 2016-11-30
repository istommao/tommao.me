---
date: 2016-11-29 13:13
status: public
title: Python内建函数
---

Python内建模块
## sys.getrefcount 获取对象引用计数

```python
import sys
words = 'Hello world!'
sys.getrefcount(words)
```
## sys.getrecursionlimit 获取递归最大层数
```python
import sys
sys.getrecursionlimit()
```

## sys.getdefaultencoding 获取默认编码
```python
import sys
sys.getdefaultencoding()
```

## sys.getsizeof 获取对象的字节大小
```python
import sys
words = 'Hello world!'
sys.getsizeof(words)
```