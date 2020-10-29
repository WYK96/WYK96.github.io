---
title: 文章写作trick
tags: [工具使用]
style: fill
color: dark
description: 完成paper过程中使用到的一些trick.
---

**查找近义词**

https://www.thesaurus.com/browse/synonym

**data set & dataset**

dataset 和 data set 的含义有微妙的区别。data 和 set 本来是两个词，一般来说用 the data set(s) 是不会错的，也很正规，注意一定要加冠词，来指出特定的数据集，dataset 作为一个新词相对于 data set 的抽象层次提升了，用来指具有数据集属性的东西，比如网上各种公开数据集，可以叫public datasets。写论文时用 data set 比较保险。

**空格问题**

左括号前要加一个空格，如： we tested our pipeline (see Fig.1).

双层括号之间不需要加空格，如： we tested our pipeline ((a) in Fig.1).

**图片注释问题**

通常会在图片中标注编号（如(a) (b) (c)），然后在文字中作相应的解释。此时文字中说明时编号后面不再需要加冒号：

```
# 错误示范
Figure 1: Comparision of results. (a): method1; (b): method2; (c): ours.
# 正确示范
Figure 1: Comparision of results. (a) method1; (b) method2; (c) ours.
```

如果是说第几行或者左边右边，则需要加上冒号：

```
Figure 1: Comparision of results. Top row: method1; bottom row: ours.
Figure 1: Comparision of results. Left: method1; middle: ours; right: ours.
```