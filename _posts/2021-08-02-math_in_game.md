---
title: 游戏中的三维数学
tags: [基础知识学习]
style: fill
color: dark
description: 游戏中所需的三维数学.
---

# 矩阵旋转


顺时针：

**Step1**: 沿水平轴旋转 $$ matrix[row, col] $$ -> $$ matrix[N-row-1, col] $$

**Step2**: 沿对角线旋转 $$ matrix[row, col] $$ -> $$ matrix[col, row] $$

$$ matrix[row, col] $$ -> $$ matrix[N-row-1, col] $$ -> $$ matrix[col, N-row-1] $$


逆时针：

**Step1**: 沿树直轴旋转 $$ matrix[row, col] $$ -> $$ matrix[row, N-col-1]

**Step2**: 沿对角线旋转 $$ matrix[row, col] $$ -> $$ matrix[col, row] $$

$$ matrix[row, col] $$ -> $$ matrix[row, N-col-1] $$ -> $$ matrix[N-col-1, row] $$
