---
title: numpy数组和python列表的区别
tags: [python,numpy]

categories: [python]
date: 2020-3-21
---

1. numpy数组创建时是固定大小，python数组（list）是动态的。更改ndarray的大小将创建一个新数组并删除原来的数组。

2. 元素类型区别。
3. NumPy数组中的元素都需要具有相同的数据类型，因此在内存中的大小相同。

4. python的List可以存放不同类型的元素。
5. 例外情况：Python的原生数组里包含了NumPy的对象的时候，这种情况下就允许不同大小元素的数组。

6. 数学操作执行效率高于原生python
7. 越来越多的基于Python的科学和数学软件包使用NumPy数组