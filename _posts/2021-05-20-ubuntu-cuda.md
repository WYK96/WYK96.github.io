---
title: Ubuntu更改显卡驱动
tags: [操作系统]
style: fill
color: dark
description: Ubuntu更改显卡驱动.
---


***Step1*** 关闭图形界面:

```sh
sudo systemctl set-default multi-user.target
sudo reboot
```

***Step2*** 卸载原有驱动:

```sh
sudo apt-get --purge remove nvidia-*
sudo apt-get --purge remove "*nvidia*"
```

***Step3*** 安装cuda(以11.3为例):

```sh
sudo sh ./cuda_11.3.0_465.19.01_linux.run
```

***Step4*** 添加环境变量

```sh
vi ~/.bashrc

#文末添加如下语句：
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.3/lib64
export PATH=$PATH:/usr/local/cuda-11.3/bin
export CUDA_HOME=$CUDA_HOME:/usr/local/cuda-11.3

source ~/.bashrc
```

***Steop5*** 开启图形界面：
```sh
sudo systemctl set-default graphical.target
sudo reboot
```

至此，cuda安装完成。