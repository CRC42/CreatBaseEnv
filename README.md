# CreatBaseEnv

创建ubuntu+pytorch环境的笔记

## 一、概述

大致的流程为：

安装ubuntu双系统、配置ubuntu基础功能、安装显卡驱动、安装cudatoolkit、使用Anaconda配置python虚拟环境、根据项目需求安装pytorch及依赖。

（需求：nvidia显卡）

## 二、ubuntu双系统安装及基础配置

（tips: 快捷键 ctrl + alt + T 打开命令行终端）

### 1. 为什么安装双系统：

大部分的项目代码都在linux下运行，如果想要在windows系统下运行，往往需要对代码进行一些改动（例如代码内路径分隔符），避免这些问题的方法就是装个linux系统。

而虚拟机无法调用电脑的显卡，因此如果有调用显卡进行训练/测试的需求，就需要装双系统。

### 2. 安装步骤：

下载ubuntu系统镜像(18.04版本比较常见)、用U盘制作安装盘、进bios关闭安全启动/改变启动顺序、安装。

具体操作参照教程：

https://www.bilibili.com/video/BV1554y1n7zv

### 3. 如果遇到无wifi适配器的bug：

手机用线连接电脑，开启设置中的“USB共享手机网络”，就可以上网了，之后可以参考一些教程安装无线网卡驱动（不行的话就每次都连接手机使用）。

### 4. 换源：

参考教程：

https://zhuanlan.zhihu.com/p/473782645

换源之后需要更新软件包，命令为：

```
sudo apt update
sudo apt upgrade
```

### 5. 安装中文输入法：

参考教程：

[ubuntu18.04下安装中文输入法 - 吾码的博客 - 博客园](https://www.cnblogs.com/51ma/p/12868504.html)

### 三、显卡驱动与CUDA安装

### 1. 查看推荐驱动：

使用：

```
ubuntu-drivers devices
```

查看推荐的驱动(标有recommended的版本)。

### 2. 安装驱动：

参考教程：

[ubuntu18.04“软件与更新”中无附加驱动问题（已解决）_hongyiWeng的博客-CSDN博客](https://blog.csdn.net/hongyiWeng/article/details/121084076)

在 软件与更新-附加驱动 中选择推荐版本的驱动，然后应用更改。

### 3. 安装CUDA：

#### 根据代码要求选择对应版本的CUDA：

如果指定了pytorch版本，在 https://pytorch.org/get-started/previous-versions/ 查看pytorch对应的CUDA版本。

以pytorch1.7.1为例，在网页搜索1.7.1，结果为：

![](https://github.com/CRC42/CreatBaseEnv/blob/main/pic/cudaversion.png)

可以看到有四个版本可以选择。

以CUDA11.0为例，

搜索cuda toolkit 11.0：

![](https://github.com/CRC42/CreatBaseEnv/blob/main/pic/cuda.png)

这样选择：

![](https://github.com/CRC42/CreatBaseEnv/blob/main/pic/choose.png)

会生成两段命令。

首先复制第一行wget命令，粘贴到ubuntu命令行中运行。

下载结束之后，不要使用官网的第二行命令，运行这两段命令：

```
sudo chmod +x cuda_11.0.2_450.51.05_linux.run
sudo ./cuda_11.0.2_450.51.05_linux.run
```

然后按照 [Ubuntu 20.04安装CUDA 11.0、cuDNN 8.0.5 | 开发之路](http://www.linuxchn.com/ubuntu-20-04%E5%AE%89%E8%A3%85cuda-11-0%E3%80%81cudnn-8-0-5/) 中的说明进行选择，并配置环境变量。

配置结束之后，使用 nvcc -V 命令进行检验。

## 四、配置python虚拟环境

#### 1. 安装anaconda：

参考教程：

[Ubuntu18.04 安装 Anaconda3_梦dancing的博客-CSDN博客_ubuntu18安装anaconda](https://blog.csdn.net/qq_15192373/article/details/81091098)

### 2. 创建环境：

按照项目代码要求，创建相应版本的虚拟环境：

```
cuda create -n envname python=3.x
```

#### 3. 安装pytorch

在前文所述的 https://pytorch.org/get-started/previous-versions/ 中查看安装命令：

以1.7.1为例，首先：

```
conda activate envname
```

然后：

```
pip install torch==1.7.1+cu110 torchvision==0.8.2+cu110 torchaudio==0.7.2 -f https://download.pytorch.org/whl/torch_stable.html
```

#### 4. 安装其他的依赖：

根据项目代码要求进行安装，如果提供了文件requirements.txt，则

```
pip install -r requirements.txt
```
