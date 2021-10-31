---
title: deformation transfer for triangle meshes
tags: [论文阅读]
style: fill
color: dark
description: 非常经典的一篇paper.
---

# 《Deformation Transfer for Triangle Meshes》回顾


之前garment transfer的project中有使用到deformation gradient作为deformation representation，deformation gradient这个概念出自Sumner于04年在TOG上发表的一篇paper “Deformation Transfer for Triangle Meshes”，现进行回顾。


这个work的idea在于可以将某个mesh的deformation迁移至其它的mesh上，如下图，马的动作可以被迁移至骆驼上。

* 第一行：左侧为canonical space的source reference（记为source），右侧为不同pose下的deformed source。

* 第二行：左侧为cononical space的target reference（记为target），右侧为根据Source的deformation得到的deformed target，可以看出deformed target与deformed source的pose一致。

![alt text](/blog_resources/deformation_transfer/overview.PNG)


那么问题来了，怎么实现这样的transfer呢？这里先给出paper的pipeline：

**Step1:** 找一个transformation matrix，用于表示由source至deformed source的deformation，记为**deformation gradient**；

**Step2:** 建立一个source与target之间的mapping，以明确apply deformation时的correspondence；

**Step3:** 通过mapping建立的correspondence，将deformation gradient作用于canonical space的target；

**Step4:** 此时会发现deformed target会裂开，添加共点约束，确保shared vertices会transform至相同位置。


## 1 deformation gradient

deformation gradient用于描述两个相同拓扑mesh之间的线性变换，其encoding的内容为triangle的orientation、scale、rotation。由于仅根据三角形顶点坐标无法判断变形后face的方向是否正确，Sumner提出了引入第四个顶点，为deformation gradient添加方向信息：

$$ v_{4} = v_{1} + (v_{2} - v_{1}) / \sqrt{|v_{2} - v_{1}| \times v_{3} - v_{1}} $$

这里为什么要scale一下，是为了保证与edge的长度成比例。则transformation可定义为：

$$ Qv_{i} + d = \tilde{v}_{i}, i \in 1...4 $$

将$$ v_{1},...,v_{4} $$ 代入上式，可消除d，得到$$ QV = \tilde{V} $$，其中：

$$ V = [v_{2} - v_{1}, v_{3} - v_{1}, v_{4} - v_{1}] $$
$$ \tilde{V} = [\tilde{v}_{2} - \tilde{v}_{1}, \tilde{v}_{3} - \tilde{v}_{1}, \tilde{v}_{4} - \tilde{v}_{1}] $$

则deformation gradient（Q）可表示为：

$$ Q = \tilde{V}V^{-1} $$


## 2 Mapping


至此我们已经有了Q，并可以通过Q将source变形至deformed source，但是还有一个问题是如何将Q作用于target？这时便需要对source和target的triangle进行mapping，以确定source某triangle的deformation gradient对应至target的哪个triangle。

这个mapping是user specified，可以自行设定（如手动指定、找最近点等）。


## 3 Consistency Constraint

现在有了mapping的correspondence，我们可以找到source中每个triangle的deformation gradient对应至target的具体位置，并对target进行变形，但是如果直接这样做会出现下图的情况：


![alt text](/blog_resources/deformation_transfer/wo_cons.PNG)


* A: 将source->deformed source的deformation gradient作用于target，可以看出target裂开了，而且pose并没有变，只是triangle的orientation、scale、rotation发生了变化。这是因为deformation gradient并没有对translation进行encoding，故deformed target不包含translation。

* B: 既然A中的target没有translation，直接的想法是将source->deformed source的translation作用于target->deformed target，这样deformed target的pose变过来了，但是triangle是相互分裂的。原因很好理解：deformation gradient是作用于triangle的，对于vertices并没有添加约束，导致本该shared的vertices分开了。

* C: 原文在B的基础上添加了共点约束，使得deformed target的mesh是连续的。

接下来讲一下是怎么添加这个共点约束的。既然deformed target裂开了，那么我们就限制一下这个transformation，使得变换前后shared vertices都必须共点，根据上述描述，可以写出约束方程：

$$ T_{j}v_{i} + d_{j} = T_{k}v_{i} + d_{k}, \quad \forall i, \forall j,k \in p(v_{i}) $$

其中，$$ p(v_i) $$表示顶点i的一阶邻域中的顶点。这个式子怎么理解呢？看一下下面这张图就能明白：

![alt text](/blog_resources/deformation_transfer/cons.PNG)

既然我们要求triangle在进行transformation前后vertex是shared，那么对于这个vertex周围的所有triangle，它们的affine transformation对于顶点i的变换结果应该是相同的。

那么共点约束的目标方程可改写为：

$$ \underset{T_{1} + d, ..., T_{|T|} + d_{|T|}}{min} \sum_{j=1}^{|M|} || S_{S_{j}} - T_{t_{j}} {||}_{F}^{2} \\ s.t. \quad T_{j}v_{i} + d_{j} = T_{k}v_{i} + d_{k}, \quad \forall i, \forall j,k \in p(v_{i}) $$

这样便可求解出 $$T_{1} + d, ..., T_{|T|} + d_{|T|}$$。paper中还对这个解法进行了进一步优化，可以直接求解出deformed vertices，这里不再展开了。