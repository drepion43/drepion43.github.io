---
title:  "[Ubuntu]CUDA INSTALL"
categories: Linux
tag: [linux, ubuntu]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: CUDA Install
comments: true
date: 2024-01-20
toc_sticky: true
---

# CUDA 설치확인
Ubuntu에서 CUDA 설치를 확인하기 위해서 nvcc -V 나 nvcc --version을 입력했을 때, "Command 'nvcc' not found, but can be installed with"과 같은 에러 메시지가 나오면 정상적으로 CUDA 설치나 연동이 정상적으로 되지 않았다는 얘기입니다. 이런 메시지가 뜨는 경우는 크게 2가지가 있습니다.   

① **CUDA 설치를 하지 않은 경우**입니다.   
② CUDA 버전과 현재 내 GPU(그래픽카드) 버전이 호환되지 않는 버전일 경우입니다.  

②의 경우에는 nvcc -V에서는 에러 메시지가 나타나지만, CUDA를 이용하는 대표적인 tensorflow나 pytorch에서는 연결이 되어 정상적으로 사용을 할 수 있는 경우도 있습니다. 그럼 이번 포스팅에서는 CUDA를 설치하지 않았다는 가정하에 CUDA를 설치하는 과정에 대해 설명하겠습니다.   

## 1. NVIDIA DRIVER Install
먼저 내 GPU(그래픽 카드)에 호환되는 NVIDIA-DRIVER 설치가 가능한 버전들을 확인해야합니다.    
```bash
ubuntu-drivers devices
```   
상기의 명령어를 치면 하기와 같이 내 GPU에 설치가능한 NVIDIA-DRIVER 목록들이 나타납니다.    
<img src="../../../assets/images/Linux/2024-01-20-cudainstall/nvidia driver 1.jpg" alt="nvidia driver 1" style="zoom:80%;" />    
내가 원하는 특정 버전을 설치할거면 해당 버전의 NVIDIA-DRIVER를 설치하시면 됩니다. 해당 특정 버전의 확인은 하기의 링크에서 나의 GPU 장비를 검색하면 거기에 맞는 NVIDIA-DRIVER 버전이 나타납니다.   
[NVIDIA-DRIVER 버전 확인](https://www.nvidia.co.kr/Download/index.aspx?lang=kr)   
```bash
sudo apt-get install nvidia-driver-535
```    
하지만 그냥 호환되는 어떤 버전이든지 상관이 없다면 자동 설치를 진행하시면 됩니다.   
```bash
sudo ubuntu-drivers autoinstall
```     
상기의 이미지 옆에 란에 **recommended**가 적혀있는 버전이 보이실 겁니다. autoinstall을 하시면 그 버전이 설치되실 겁니다.    

그 후 한번 rebooting을 시켜주겠습니다.   
```bash
sudo reboot
```     

## 2. CUDA TOOLKIT INSTALL
상기의 내용대로 잘 따라오셨다면, NVIDIA-DRIVER가 잘 설치되셨을 겁니다. 그것을 확인하기 위해 하기의 CLI을 치시면, 하기의 이미지처럼 나타나실 겁니다.   
```bash
nvidia-smi
```     
<img src="../../../assets/images/Linux/2024-01-20-cudainstall/nvidia driver 2.jpg" alt="nvidia driver 2" style="zoom:80%;" />    
상기의 이미지에서 빨간색으로 블록 쳐진 부분이 보이실 겁니다. 이 부분이 현 내 <span style='color:red'>**GPU와 호환되는 최대 설치 가능한 CUDA 버전**</span>입니다. 즉, 저의 경우 CUDA 12.2 버전을 넘어가는 버전을 설치시, 제 GPU와 CUDA가 호환이 되지 않는다는 의미입니다.   

이제 CUDA TOOLKIT을 설치해보겠습니다. 하기의 링크에 따라 들어가시면 내 버전과 호환되는 원하는 버전의 CUDA를 찾아 설치하시면 됩니다.   
[CUDA Toolkit Install](https://developer.nvidia.com/cuda-toolkit-archive)   

참고로 현재 내 OS가 어떤건지 까먹으신 분들을 위해 하기의 OS확인과 Architecture확인 CLI를 남겨두겠습니다.   
```bash
cat /etc/*-release # OS 확인
uname -m # Architecture 확인
```     
<img src="../../../assets/images/Linux/2024-01-20-cudainstall/cuda toolkit 1.JPG" alt="cuda toolkit 1" style="zoom:80%;" />    

그럼 이제 CUDA Toolkit Install 링크에 들어가 설치를 진행해보겠습니다.   
자신에게 알맞은 OS와 Architecture를 선택하시면 됩니다.   
<img src="../../../assets/images/Linux/2024-01-20-cudainstall/cuda toolkit 2.JPG" alt="cuda toolkit 2" style="zoom:80%;" />    

그럼 이제 호환되는 버전인지 확인해보겠습니다. 제가 설치한 NVIDIA-DRIVER와 같은 버전인 535가 빨간색 블록안에 적혀있는 것을 하기의 이미지를 통해 확인할 수 있습니다.   
<img src="../../../assets/images/Linux/2024-01-20-cudainstall/architecture and os.jpg" alt="architecture and os" style="zoom:80%;" />    

저는 현재 외부에서 접속한 형태이므로, local이 아닌, network를 통해 설치를 진행하겠습니다. 설치는 알려주는 CLI를 그대로 따라치시면 되십니다.   
<img src="../../../assets/images/Linux/2024-01-20-cudainstall/cuda toolkit 3.JPG" alt="cuda toolkit 3" style="zoom:80%;" />    



## CUDA UNINSTALL
만약 현 CUDA가 설치되어 있는데, 버전이 맞지 않거나 버전을 다운그레이드를 하고 싶으시다면, 우선 이미 설치했던 CUDA를 삭제하셔야 합니다.   
```bash
sudo apt-get purge nvidia*
sudo apt-get autoremove
sudo apt-get autoclean
sudo rm -rf /usr/local/cuda*
```    
상기의 CLI를 통해 CUDA를 삭제 후, /usr/local/에 들어가셔서 CUDA가 삭제된 것을 확인해보시며 됩니다. 그런 후 다시 CUDA를 재설치 하시면 됩니다.    
