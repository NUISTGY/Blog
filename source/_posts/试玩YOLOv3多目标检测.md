---
title:  试玩YOLOv3多目标检测
tags: [目标检测,深度学习,图像处理]
categories: [深度学习]
date: 2019-10-24

---
# YOLOv3简介

![](https://s1.ax1x.com/2022/09/05/vT2ltK.png)

[官网链接](https://pjreddie.com/darknet/yolo/)
[论文链接](https://pjreddie.com/media/files/papers/YOLOv3.pdf)

# 运行环境搭建
首先下载源码：
**源码：** <https://github.com/NUISTGY/TensorFlow2.0-Examples/tree/master/4-Object_Detection/YOLOV3>

接着安装依赖库和训练好的权重：
```
$ pip3 install -r ./docs/requirements.txt
$ wget https://pjreddie.com/media/files/yolov3.weights
```
将权重文件置于源码文件夹目录下即可。

# Quick Start
前面已经完成了所有的准备公作，运行以下命令即可进行目标检测：
```
$ python image_demo.py
$ python video_demo.py # if use camera, set video_path = 0
```
![运行python image_demo.py的结果](https://s1.ax1x.com/2022/09/05/vT2KTx.jpg)

# 尝试检测自己的图片或视频
想要识别自己的图片只需要做很小的改动即可。
打开`image_demo.py`:
![](https://s1.ax1x.com/2022/09/05/vT2J6H.png)
将22行image_path改为自己图片的地址即可，视频目标检测同理。

最后再贴一张目标比较多的照片：
![](https://s1.ax1x.com/2022/09/05/vT2Qk6.png)

