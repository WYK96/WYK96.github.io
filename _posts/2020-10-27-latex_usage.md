---
title: Latex使用
tags: [工具使用]
style: fill
color: dark
description: Latex使用过程中的一些trick.
---

**png转eps**

```cmd
cd WORKING_DIRECTORY
bmeps -c source_figure.png converted_figure.eps
```

**图片错版**

将图片往上挪，没必要紧跟在段落后面，但是不要太前了（图文间隔不要超过一页）。

**向量**

单字符的变量使用\vec{a}表示, $\vec{a}$
多字符的变量使用\overrightarrow{var}表示, $\overrightarrow{var}$