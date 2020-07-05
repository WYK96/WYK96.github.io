---
title: SMPL环境配置
tags: [人体表示方法]
style: fill
color: dark
description: SMPL——参数化人体表示方法.
---

[SMPL](https://smpl.is.tue.mpg.de/en)是一种参数化的人体模型表示方法，通过shape和pose两种参数驱动人体变形。在配置工程环境时，踩了很多坑，以下为配置流程。

***Step1***  -> 首先，要确保当前的操作系统为Ubuntu等**非Windows**系统，我在Windows上尝试配了很多次都因为很多奇怪的bug失败了，本次环境配置所使用的操作系统为Ubuntu 18.04.4 LTS。

***Step2*** -> 安装[anaconda](https://www.anaconda.com/)，创建虚拟环境:

```python
# 创建虚拟环境
# 虚拟环境的名称为SMPL
# python版本为2.7
sudo  conda create -n SMPL python=2.7
# 激活虚拟环境
conda activate SMPL
```

***Step3*** -> 安装依赖库:

```python
# 安装numpy
sudo conda install numpy
# 安装scipy
sudo conda install scipy
# 安装chumpy
sudo pip install chumpy
# 安装opencv
sudo pip install opencv-python
```

***Step4*** -> 安装opendr，这里需要注意，直接通过`sudo pip install opendr`安装会报如下错误：

<img src="/blog_resources/config_smpl/opendr_error.png" width="95%">


此时，需要针对opendr安装相关依赖：

```python
# 安装losmesa, lgl, lglu
sudo apt install libosmesa6-dev
sudo apt-get install build-essential
sudo apt-get install libgl1-mesa-dev
sudo apt-get install libglu1-mesa-dev
sudo apt-get install freeglut3-dev
# 安装opendr
sudo pip install opendr
```

***Step5*** -> 从[SMPL官网](https://smpl.is.tue.mpg.de/downloads)下载 *SMPL for Python Users*这个工程，下在前需要在网站上注册账户。

***Step6*** -> 打开下载的工程，找到工程目录下的*smpl_webuser/hello_world/*，运行*hello_smpl.py*。如果一切顺利，可以得到输出的*hello_smpl.obj*人体模型。接下来，运行*render_smpl.py*渲染人体模型：

<img src="/blog_resources/config_smpl/render_result.png" width="95%">

至此，SMPL环境配置完成。