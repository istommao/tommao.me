---
date: 2016-12-06 15:46
status: public
tags: Python
title: Python装饰器
---

![](~/16-49-28.jpg)

## 什么是装饰器
> Python中的装饰器本质上就是在不改变函数本身的情况下包装一个函数成为另一个函数的语法糖

## 装饰器简单示例
```python
def wrapper(func):
    def inner(*args, **kwargs):
        print(func.__name__, *args, **kwargs)
        return func(*args, **kwargs)
    return inner

@wrapper
def print_func(words):
    return words

print_func('Hello decorator!')
```

![](~/15-55-13.jpg)

## 装饰器有什么用？
`看起来好像很不错，那么装饰器有什么用呢?`
> 在我看来本质上就是减少代码重复(Don't repeat yourself)，让代码的可读性更好!

### 需要缓存的地方使用示例

```python
def get_article_detail(uid):
    article = ORM.get_article(uid)

    if article:
        cache.incr('key')
    return article
```
`这样在访问文章的时候，每次都要在函数内累加浏览量，如果换成装饰方式呢？`

```python
def increase_page_view(func):
    def wrapper(*args, **kwargs):
        obj = func(*args, **kwargs)
        if obj:
            cache.incr(obj.id)
        return obj

@increase_page_view
def get_article_detail(uid):
    return ORM.get_article(uid)
```

`这样原来的获取文章详情的函数，只关心获取文章，而累加浏览量的操作放到具体的装饰器函数中，提高代码的可读性`