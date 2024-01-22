---
title:  "[Ubuntu]패키지 설치/업데이트 오류"
categories: Linux
tag: [linux, ubuntu]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 에러 해결
comments: true
date: 2024-01-20
toc_sticky: true
---

## 패키지 업데이터 오류
```bash
다음 패키지의 의존성이 맞지 않습니다:
nvidia-kernel-common-535 : 충돌: nvidia-kernel-common
nvidia-kernel-common-545 : 충돌: nvidia-kernel-common
E: 오류, pkgProblemResolver::Resolve가 망가졌습니다. 고정 패키지때문에 발생할 수도 있습니다.
```
CUDA 설치시 패키지 설치시 상기와 같은 에러가 발생했습니다. 현 에러 메시지를 보면 nvidia-kernel-common-535 nvidia-kernel-common-545의 충돌로 인해 패키지가 오류가 발생했습니다. 보통 의존성이 맞지 않는 패키지가 있다면 이렇게 어떤 패키지인지 보여주며 해당 패키지를 설치하시면 해결이 됩니다.   
```bash
apt list --installed | grep [프로그램명]
```
상기의 CLI를 통해 의존성 패키지가 설치되어있는지 확인을 하고 되어있지 않으시다면, 설치를 해주시면 해결하실 수 있으실 겁니다.   

저의 경우에는 설치 확인을 해보니 하기와 같이 잘 설치되어있으며, nvidia-kernel-common-545은 설치가 되어있지 않은 상황입니다.   
<img src="../../../assets/images/Linux/2024-01-20-Package Error/nvidia error 1.jpg" alt="nvidia error 1" style="zoom:80%;" />    

저의 에러와 같은 에러가 발생한 분이 올린 문의를 찾아보니 하기의 CLI를 통해 설치를 진행하면 된다고 해서 하기와같이 진행하니 설치가 잘 완료되었습니다.   
[Nvidia-kerenel Error](https://forums.developer.nvidia.com/t/driver-in-unknown-state-after-attempting-to-install-cuda-ubuntu-22-04/217106)
```bash
sudo apt-get install nvidia-kernel-common-535
sudo apt-get install nvidia-kernel-source-535
```