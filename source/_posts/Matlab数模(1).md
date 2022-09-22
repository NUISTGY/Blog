---
title: Matlab之ineterp1插值
tags: [Matlab,数学建模]

categories: [科学计算]

date: 2020-2-21
---

```Matlab
%一维插值

x = 0 : pi/4 : 2 * pi;     %x的样本值
y = sin(x);                %y的样本值
xx = 0 : 0.5 : 2 * pi;     %目标的x值
%分段线性插值（默认）
y1 = interp1(x,y,xx);
subplot(2,2,1);plot(x,y,'o',xx,y1,'r');
title('分段线性插值')

%临近插值
y2 = interp1(x,y,xx,'nearnest');
subplot(2,2,2);plot(x,y,'o',xx,y2,'r');
title('邻近插值')

%球面线性插值
y3 = interp1(x,y,xx,'spline');
subplot(2,2,3);plot(x,y,'o',xx,y3,'r');
title('球面线性插值')

%三次多项式插值
y4 = interp1(x,y,xx,'PCHIP');
subplot(2,2,4);plot(x,y,'o',xx,y4,'r');
title('三次多项式插值')
```
**输出结果**

![ans](https://s3.bmp.ovh/imgs/2022/09/05/7f0cd8ca7c6c30fd.png "ans")
