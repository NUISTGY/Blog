---
title: Matlab之二维插值：估测海底某曲面地形
tags: [Matlab,数学建模]
categories: [科学计算]
date: 2020-2-21

---

```Matlab
% 2维插值示例：估测海底某曲面地形
x = [129,140,103.5,88,185.5,195,105,157.5,107.5,77,81,162,162,117.5];
y = [7.5,141.5,23,147,22.5,137.5,85.5,-6.5,-81,3,56.5,-66.5,84,-33.5];
z = -[4,8,6,8,6,8,8,9,9,8,8,0,4,9];
xmm = minmax(x);
ymm = minmax(y);
xi = xmm(1):2.5:xmm(2);
yi = ymm(1):2.5:ymm(2);
z_interp = griddata(x,y,z,xi,yi','V4');     %griddata插值同interp2，但是对数据要求不严格
surf(xi,yi,z_interp);
```
**输出结果**

![ans](https://s3.bmp.ovh/imgs/2022/09/05/a671546bee229037.png "ans")

![ans](https://s3.bmp.ovh/imgs/2022/09/05/b516d0a759d6dc00.png "ans")
> PS:深度大于0的部分应手动舍弃，边值区域的二维插值会遇到问题。
