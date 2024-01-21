---
title:  "[Ubuntu]CUDA 설치"
categories: Linux
tag: [linux, ubuntu,cuda]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: CUDA 설치
comments: true
date: 2022-07-01
toc_sticky: true
---

해당 설치방법은 Ubunutu 20.04버전입니다. 22.04버전으로 다시 업데이트를 진행해서 게시를 했습니다.
## 1. INSTALL Nvidia Graphics Driver
```bash
ubuntu-drivers devices

원하는 드라이버로 설치
sudo apt install nvidia-driver-460
or
권장하는 드라이버로 설치
sudo ubuntu-drivers autoinstall 

sudo reboot
```

```bash
nvidia-smi
```
### 설치 확인


## 2.  CUDA INSTALL
```bash
sudo rm -rf /usr/local/cuda*
./bashrc
```
### CUDA 관련된 파일 다 삭제


https://developer.nvidia.com/cuda-toolkit-archive
### 자신의 GPU에 알맞은 CUDA VERSION을 찾아서 설치

```bash
sudo sh -c "echo 'export PATH=$PATH:/usr/local/cuda-11.4/bin' >> /etc/profile"
sudo sh -c "echo 'export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-11.4/lib64' >> /etc/profile"
sudo sh -c "echo 'export CUDADIR=/usr/local/cuda-11.4' >> /etc/profile"
source /etc/profile

and
gedit ~/.bashrc
export PATH=$PATH:/usr/local/cuda-xx.x/bin
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda-xx.x/lib64
export CUDADIR=/usr/local/cuda-xx.x
```

```bash
nvcc -V
```
### CUDA 설치 확인

## 3. Cudnn INSTALL

https://developer.nvidia.com/cudnn
### CUDA와 알맞은 Cudnn 버전 설치


### COPY FILE
```bash
tar xvzf cudnn-11.2-linux-x64-v8.1.0.77.tgz

sudo cp cuda/include/cudnn* /usr/local/cuda/include

sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64

sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

### Symbolic Link
```bash
sudo ln -sf /usr/local/cuda-xx.x/targets/x86_64-linux/lib/libcudnn_adv_train.so.x.x.x /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_adv_train.so.8
sudo ln -sf /usr/local/cuda-xx.x/targets/x86_64-linux/lib/libcudnn_ops_infer.so.x.x.x  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_ops_infer.so.8
sudo ln -sf /usr/local/cuda-xx.x/targets/x86_64-linux/lib/libcudnn_cnn_train.so.x.x.x  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_train.so.8
sudo ln -sf /usr/local/cuda-xx.x/targets/x86_64-linux/lib/libcudnn_adv_infer.so.x.x.x  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_adv_infer.so.8
sudo ln -sf /usr/local/cuda-xx.x/targets/x86_64-linux/lib/libcudnn_ops_train.so.x.x.x  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_ops_train.so.8
sudo ln -sf /usr/local/cuda-xx.x/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.x.x.x /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn_cnn_infer.so.8
sudo ln -sf /usr/local/cuda-xx.x/targets/x86_64-linux/lib/libcudnn.so.x.x.x  /usr/local/cuda-11.2/targets/x86_64-linux/lib/libcudnn.so.8

```
