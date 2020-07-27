---
title: Learning an Intrinsic Garment Space for Interactive Authoring of Garment Animation
tags: [深度学习相关, 深度几何学习, 论文阅读]
style: fill
color: success
description: 通过学习内在服装参数空间的互动式服装动画创作方法.
---


![alt text](/blog_resources/intrinsic_garment_space/banner.png "banner")

这篇文章是Siggraph Asia 2019年的一篇文章，作者是[米哈游](https://www.mihayo.com/)公司的员工，点[这里](http://geometry.cs.ucl.ac.uk/projects/2019/garment_authoring/)可跳转到文章主页。

整篇文章是基于一个假设而展开的：服装形变由两个部分组成：人体的姿态参数(黄)以及服装的固有参数(蓝)

**人体姿态参数:** 即人体的关节运动信息，红色的点代表关节点。

**服装的固有参数:** 由以下三个部分组成：

 1. 模拟器参数(红)：模拟器通过对服装进行物理仿真，可以将服装穿着到人体之上，其设定会影响服装形变，如仿真时长、最大迭代次数；

 2. 服装材质参数(橙)：不同材质的服装具有不同的材质属性，进而影响到服装的摩擦力、弯曲程度等信息；
 
 3. 环境参数(灰)组成：由环境引起的服装形变，如重力、空气阻力等


{% include elements/figure.html image="/blog_resources/intrinsic_garment_space/assumption.png" caption="服装形变的划分" %}

下图给出了三组服装动画序列，同一组的服装具有相同的服装固有参数，而具有不同的人体姿态参数。不同组的服装具有不同的服装固有参数。可以看出，具有相同服装固有参数的服装在隐空间中学到的表达汇集在了同一个区域内，这样便可以区分出具有不同服装固有参数的服装。接下来，再根据给定姿态的人体，利用解码器将隐空间中的降维表示解码为具有不同固有参数的服装造型。


{% include elements/figure.html image="/blog_resources/intrinsic_garment_space/latent_space.png" caption="姿态无关的服装材质空间" %}

{% include elements/figure.html image="/blog_resources/intrinsic_garment_space/network_overview.png" caption="网络结构" %}