---
title: AMASS环境配置
tags: [人体运动数据集]
style: fill
color: primary
description: AMASS——大规模人体运动数据集.
---

![alt text](/blog_resources/config_amass/square_with_avatar.png "Overview")

[AMASS](https://amass.is.tue.mpg.de/)是一种人体运动数据集，很适合作为深度学习的训练样本，以下为配置流程。

***Step1***  -> 首先，要确保当前的操作系统为Ubuntu等**非Windows**系统，我在Windows上尝试配了很多次都因为很多奇怪的bug失败了，本次环境配置所使用的操作系统为Ubuntu 18.04.4 LTS。下载[AMASS工程文件](https://github.com/nghorbani/amass)。

***Step2*** -> 安装[anaconda](https://www.anaconda.com/)，创建虚拟环境:

```python
# 创建虚拟环境
# 虚拟环境的名称为AMASS
# python版本为3.7
sudo  conda create -n AMASS python=3.7
# 激活虚拟环境
conda activate AMASS
```

***Step3*** -> 安装依赖库:

```python
# 安装configer，human_body_prior，amass
pip install git+https://github.com/nghorbani/configer
pip install git+https://github.com/nghorbani/human_body_prior
pip install git+https://github.com/nghorbani/amass
```

***Step4*** -> 下载DMPL的model—[DMPLs for AMASS](https://smpl.is.tue.mpg.de/downloads)，以及SMPLH的model—[Extended SMPLH model for AMASS](https://mano.is.tue.mpg.de/downloads)，以上两个网站均需要注册才能够下载。

model的文件组织结构如下所示：

```python
- DMPL/SMPL
    - male                         # 男性
        - model.npz
    - female                       # 女性
        - model.npz
    - neutral                      # 中性
        - model.npz
```

可以看出，每种性别类型都对应一个model.npz文件，该文件存储了不同性别模型的相关信息，在生成给定pose及shape的人体时，需要使用到这个model。

***Step5*** -> 找到在 ***Step1*** 中下载的AMASS工程文件，在其根目录创建一个名称为body_models的文件夹，将 ***Step4*** 下载的两种model复制到该文件夹下：

```python
- amass-master                      # amass根目录
    - ...
    - body_models                   # 新创建的文件夹
        - dmpls                     # 用于存放DMPL的model
            - male
                - model.npz
            - female
                - model.npz
            - neutral
                - model.npz
        - smplh                     # 用于存放SMPLH的model
            - male
                - model.npz
            - female
                - model.npz
            - neutral
                - model.npz
```

***Step6*** -> 安装Jupyter Notebook：

```python
# 安装Jupyter Notebook
sudo pip install jupyter 
# 配置环境变量
vim ~/.bashrc
# 在文件最后输入如下两行
PATH=~/.local/bin:$PATH
export PATH
# 退出vim，输入如下命令，使环境变量生效
source ~/.bashrc
# 启动Jupyter Notebook
jupyter notebook --ip=0.0.0.0 --port=8000
```

***Step7*** -> 开始使用工程。找到amass-master/notebooks，底下有01-AMASS_Visualization.ipynb, 02-AMASS_DNN.ipynb, 03-AMASS_Visualization_Advanced.ipynb三个教程，跟着一步一步做就成。


至此，AMASS环境配置完成。