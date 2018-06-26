---
date: 2016-11-29 13:16
status: public
tags: Python,AES
title: 使用cryptography进行AES的cbc模式加密
---

[cryptography](https://github.com/pyca/cryptography) 是一个python加密库

> cryptography is a package designed to expose cryptographic primitives and recipes to Python developers. [文档 cryptography.io](https://cryptography.io/)

> 以前使用过很多不同的加密库，但pyhton界貌似没有一个统一的库。
但需要用到RSA加密时用到一个库，需要AES时又要装另一个库，这对于库的使用和项目管理变得很不友好！

`直到有一天发现了伯乐在线的一篇文章，决定了以后加密就用cryptography这个库了`
[Cryptography：用于加密的Python库](http://hao.jobbole.com/cryptography/)

但是由于对加密这块内容的了解不深，在看 cryptography 文档的时候 比较痛苦。因为 cryptography更像是一个提供了基础的加密相关的封装。具体的一些实现需要你多研究他的文档，以及对加密算法的了解。

## AES的cbc模式加密
`在需要实现AES在cbc模式下的加密时折腾了点时间，下面贴出实现代码。`

```python
import os

from cryptography.hazmat.primitives import padding
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes
from cryptography.hazmat.backends import default_backend


class AESCrypto(object):

    AES_CBC_KEY = os.urandom(32)
    AES_CBC_IV = os.urandom(16)

    @classmethod
    def encrypt(cls, data, mode='cbc'):
        func_name = '{}_encrypt'.format(mode)
        func = getattr(cls, func_name)
        return func(data)

    @classmethod
    def decrypt(cls, data, mode='cbc'):
        func_name = '{}_decrypt'.format(mode)
        func = getattr(cls, func_name)
        return func(data)

    @staticmethod
    def pkcs7_padding(data):
        if not isinstance(data, bytes):
            data = data.encode()

        padder = padding.PKCS7(algorithms.AES.block_size).padder()

        padded_data = padder.update(data) + padder.finalize()

        return padded_data

    @classmethod
    def cbc_encrypt(cls, data):
        if not isinstance(data, bytes):
            data = data.encode()

        cipher = Cipher(algorithms.AES(cls.AES_CBC_KEY),
                        modes.CBC(cls.AES_CBC_IV),
                        backend=default_backend())
        encryptor = cipher.encryptor()

        padded_data = encryptor.update(cls.pkcs7_padding(data))

        return padded_data

    @classmethod
    def cbc_decrypt(cls, data):
        if not isinstance(data, bytes):
            data = data.encode()

        cipher = Cipher(algorithms.AES(cls.AES_CBC_KEY),
                        modes.CBC(cls.AES_CBC_IV),
                        backend=default_backend())
        decryptor = cipher.decryptor()

        uppaded_data = cls.pkcs7_unpadding(decryptor.update(data))

        uppaded_data = uppaded_data.decode()
        return uppaded_data

    @staticmethod
    def pkcs7_unpadding(padded_data):
        unpadder = padding.PKCS7(algorithms.AES.block_size).unpadder()
        data = unpadder.update(padded_data)

        try:
            uppadded_data = data + unpadder.finalize()
        except ValueError:
            raise Exception('无效的加密信息!')
        else:
            return uppadded_data

```

`执行完美!`
![](/_image/aes-cbc/11-03-11.jpg)

代码已放在我的github项目中了 
[cryptokit](https://github.com/istommao/cryptokit)

---------
* 以后会在需要RSA时 实现出相应代码，对加密这块今后会写相关的学习笔记 欢迎交流互相学习!