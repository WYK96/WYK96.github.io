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