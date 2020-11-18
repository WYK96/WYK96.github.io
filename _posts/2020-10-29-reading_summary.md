---
title: 论文阅读纪要
tags: [论文阅读]
style: fill
color: danger
description: 一两句话整理出不同论文的要点.
---

**TailorNet**

Summary: 将服装变形解耦为高频成分及低频成分，使用拉普拉斯算子对顶点做平滑，得到低频成分，然后使用原顶点坐标减去低频成分得到高频成分。使用MLP对高频特征进行融合，然后与低频成分重组，得到目标体型及姿态下的服装变形。合成出的服装具有丰富的褶皱细节，且方法相较于PBS快1000倍。

![alt text](/blog_resources/reading_summary/TailorNet.png)

**Mesh-Based Autoencoders for Localized Deformation Component Analysis**

Summary: 在变形梯度之上引入的卷积的概念，使得网络能够获得更好的特征表示，卷积核的定义为当前点与其一阶领域的加权平均；此外，在Loss中引入稀疏惩罚项，使得网络能够捕获到局部变形。方法可以应用于降维、变形成分分析、重构、形状合成等任务中。

![alt text](/blog_resources/reading_summary/convMesh.png)

**Automatic Unpaired Shape Deformation Transfer**

Summary: 给定一组源体型下不同姿态的人体，可生成与源体型姿态一致的目标人体。方法的核心在于提出了VAE-Cycle-GAN，从而允许在不同拓扑的人体间进行deformation transfer。此外，提出了源网格与目标网格视觉相似度的衡量方法。

![alt text](/blog_resources/reading_summary/auto_trans.png)

**Data-Driven Elastic Models for Cloth: Modeling and Measurement**

tog12年的paper，给出了不同材质布料的成分组成，ARCSim仿真时应该用得到。

![alt text](/blog_resources/reading_summary/material_parameters.png)

**Learning-based Animation of Clothing for Virtual Try-On**

Summary:一种基于学习的虚拟试衣方法。将服装看作是相对于人体shape和pose的函数，使用相较于T-pose服装的偏移量去表示服装。方法首先通过MLP学习一个从shape到低频服装变形的映射，然后学习一个从shape+pose到高频服装变形的映射，可以合成出在时间上连续且细节丰富的服装动画。缺点也很明显，目前只能做一种服装、一种材质参数。

![alt text](/blog_resources/reading_summary/learning_based.png)

**DRAPE: DRessing Any PErson**

Summary:相关性学习方法中比较经典的一篇文章，可以将给定款式的服装快速穿着到不同体型和姿态的人体之上。该方法使用变形梯度表示服装变形，然后通过PCA将服装变形解耦为体型相关的变形和姿态相关的变形，然后根据输入的体型和姿态参数对上述两种变形成分进行重组。

![alt text](/blog_resources/reading_summary/DRAPE.png)

**Learning an Intrinsic Garment Space for Interactive Authoring of Garment Animation**

Summary:服装动画创作过程中，设计师需要逐帧地指定不同运动人体对应的服装变形，这会带来高昂的时间和经济成本。该工作提出了一种半自动化的服装动画设计方法，设计师只需要指定一段运动序列中少量的关键帧，方法便可以自动补全其它帧的服装运动，从而大幅降低了服装动画创作所需的时间。

![alt text](/blog_resources/reading_summary/authoring.png)

**DeepWrinkles: Accurate and Realistic Clothing Modeling**

![alt text](/blog_resources/reading_summary/DeepWrinkles.png)

**PIFu: Pixel-Aligned Implicit Function for High-Resolution Clothed Human Digitization**

Summary:可以通过单视角图片重建出完整人体及服装的工作，方法分为几何重建和材质重建两个部分。几何重建阶段，网络首先通过encoder对每个像素点的特征进行编码，并根据相机参数估算坐标点的深度值，然后以特征向量和深度值为输入使用MLP预测像素点在网格内的概率；材质重建阶段，网络以单张图片及几何重建子网络编码的特征向量为输入，使用MLP预测像素点对应的RGB信息。方法相较于之前的工作效果更好，并且由于方法学习的是关于的像素点是否在网格内的函数，在存储上比较友好。缺点在于对于几何细节的重建效果不够理想，且重建效果依赖于较好的相机参数。

*需要注意的是，方法在三维坐标点的采样策略上做了一些trick，直接在bounding box中均匀采样往往不能得到理想的结果，因为我们关注的是物体表面附近的信息；作者在三维模型的表面附近进行采样，每个顶点在$xyz$三个方向上进行抖动，抖动的距离符合高斯分布$\mathcal N(0, \sigma)$。作者将均匀采样和高斯采样两种方式相结合，可以取得更好的重建效果。

![alt text](/blog_resources/reading_summary/PIFu.png)

****