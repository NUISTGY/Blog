---
title:  JS学习之路--常量
tags: [编程,学习,JS,前端]
categories: [JS]
date: 2022-10-18

---

## const⛔

- const 修饰的叫常量，只能赋值一次

```javascript
const b = 300;
b = 400 // error
```

- const 并不意味着它的引用内容不可修改，例如：

```javascript
const c = [1,2,3];
c[2] = 4; // ok
c = [5,6]; // error
```