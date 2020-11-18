---
title: "Matlab使用"
tags: [编程语言]
style: fill
color: success
description: Matlab使用技巧
---

**mat文件转CSV**

如果一个.mat文件中含有A、B两个变量：

```matlab
filename.mat
    -variableA
    -variableB
```

则可以通过如下方法保存为CSV格式的文件：

```matlab
FileData = load('filename.mat');
csvwrite('variableA.csv');
csvwrite('variableB.csv');
```