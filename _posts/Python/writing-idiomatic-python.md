---
date: 2016-12-09 16:43
status: public
title: 'Writing idiomatic python'
---

## 列表解析
`生成100个数字`
```python
[i for i in range(1, 101)]
```
## 数值交换

`bad usage in python`
```python
temp = a
a = b
b = temp
```
`recommend usage`
```python
b, a = a, b
```

## in的使用

```python
data = 1
dataset = {1, 2, 3}
data in dataset
```

![](~/16-50-28.jpg)

```python
key = 'name'
dataset = {'age': 25, 'name': 'python'}
key in dataset
```

![](~/16-49-26.jpg)

