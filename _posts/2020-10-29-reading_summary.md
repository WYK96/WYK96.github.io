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