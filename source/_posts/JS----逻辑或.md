---
title:  JS学习之路--逻辑或
tags: [编程,学习,JS,前端]
categories: [JS]
date: 2022-10-19

---

## ||

**关于逻辑或有一点需要声明**

在某些代码中会出现：
```javascript
function test(n){
    n = n || '2';
    console.log(n);
}

test('1')   //输出 1
test()      //输出 2
```

它的语法为：
``` x || y ```
- 如果 x 是Truthy则返回x
- 如果 x 是Falsy则返回y
  
**但是不推荐这种用法！**
