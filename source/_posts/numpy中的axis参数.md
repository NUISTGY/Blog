---
title:  numpy之sum函数的axis参数
tags: [python,numpy]

categories: [python]
date: 2020-3-21
---

# 结论：

没有axis参数表示全部相加，axis＝0表示按列相加，axis＝1表示按照行的方向。验证如下：
![axis&sum](https://5b0988e595225.cdn.sohucs.com/images/20171124/e90ccf3e7386468fa64c8b4bc0e60e5e.png)

# 推广：
axis=i表示对array的第i个维度**变化**的方向进行操作！