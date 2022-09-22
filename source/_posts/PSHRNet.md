---
title: ✨Deep-High-Resolution Representation Learning for Cross-Resolution Person Re-identification
tags: [行人重识别,论文]
categories: [行人重识别]
date: 2021-10-7

---

<div align="center">
  
[**<front size=5>Journal of IEEE TIP (SCI-Q1 Top)</front>**](https://ieeexplore.ieee.org/document/9591273/authors#citations)

<center>
G. Zhang, 👉Y. Ge👈, Z. Dong, H. Wang, Y. Zheng and S. Chen
</center>

![](https://img.gejiba.com/images/de85252fec31e625fecf220fa7b686b8.png)

</div>

## <a id="contents-">Contents 🗒</a>
- [<a id="contents-">Contents 🗒</a>](#contents-)
- [<a id="introduction-">Introduction 🗒</a>](#introduction-)
- [<a id="usage-">Usage 🔧</a>](#usage-)
- [<a id="result-">Results 🏆</a>](#results-)
- [<a id="acknowledgements-">Acknowledgements 👍</a>](#acknowledgements-)

## <a id="introduction-">Introduction 🗒</a>

We propose a Deep High-Resolution Pseudo-Siamese Framework (PS-HRNet) to solve the cross-resolution person re-ID problem. Specifically, in order to restore the resolution of low-resolution images and make reasonable use of different channel information of feature maps, we introduce and innovate VDSR module with channel attention (CA) mechanism, named as VDSR-CA. Then we reform the HRNet by designing a novel representation head to extract discriminating features, named as HRNet-ReID. In addition, a pseudo-siamese framework is constructed to reduce the difference of feature distributions between low-resolution images and high-resolution images. The experimental results on five cross-resolution person datasets verify the effectiveness of our proposed approach. Compared with the state-of-the-art methods, our proposed PS-HRNet improves 3.4%, 6.2%, 2.5%,1.1% and 4.2% at Rank-1 on MLR-Market-1501, MLR-CUHK03, MLR-VIPeR, MLR-DukeMTMC-reID, and CAVIAR datasets, respectively.

## <a id="usage-">Usage 🔧</a>

We use apex (A PyTorch Extension) a Pytorch extension with NVIDIA-maintained utilities to streamline mixed precision and distributed training. Some of the code here will be included in upstream Pytorch eventually. The intention of Apex is to make up-to-date utilities available to users as quickly as possible.Installation instructions can be found here: https://github.com/NVIDIA/apex#quick-start.

We display the process of the algorithm as an ipynb file, you can use jupyter notebook to view and run it.

You may need HRNet-W32-C ImageNet pretrained models or learn more about HRNet: https://github.com/HRNet/HRNet-Image-Classification.git.

Wanna know more detail of the first phase？ Check this：https://github.com/NUISTGY/Person-re-identification-based-on-HRNet
## <a id="result-">Results 🏆</a>

<div align="center">

![](https://img.gejiba.com/images/6670ce1bd1696c28e0fedd4fbefd676f.png)

</div>

## <a id="acknowledgements-">Acknowledgements 👍</a>

- This code is built on [HRNet-Image-Classification](https://github.com/HRNet/HRNet-Image-Classification) and [Person_reID_baseline_pytorch](https://github.com/layumi/Person_reID_baseline_pytorch). We thank the authors for sharing their codes. To the great spirit of open source!
- Thank [Z.Dong](https://github.com/dzc2000) and [H.Wang](https://github.com/Rockdow), they are the most important contributors to the related work of the experiment. If you have any questions in the process of testing, you can send them by email or pose issues.
- Thanks for the right to use the GPU workstation provided by Nanyang Technological University.
