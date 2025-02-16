---
title:  "[Docker]Failed to initialize NVML"
categories: Docker
tag: [linux, Docker, Nvidia]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: jupyter notebook
comments: true
date: 2025-01-23
toc_sticky: true
---

# 개요
이번절에서는 제가 현업에서 발생했던 신기한 현상 문제에 대한 해결법을 설명하겠습니다. 다들 GPU가 달린 온프레스미스 서버를 이용할 때, 가상환경을 이용하는 경우가 많습니다. 그 중 저는 도커를 이용했었는데, AI 모델 학습을 위해 컨테이너를 만들 당시에는 컨테이너에서 GPU가 잘 연결된 것을 확인할 수 있으며, AI 모델 학습시 GPU를 잘 이용하는 것을 확인할 수 있었습니다.    
하지만, 몇시간 또는 며칠 후 컨테이너 내부에서 GPU를 잡지 못하고 있는 현상이 발생하였습니다. 여기서 재밌는 점은 그 전에 돌려놓았던 AI 모델 학습은 여전히 GPU를 잘 이용하고 있었습니다.      
```bash
nvidia-smi

Failed to initialize NVML: Unknown Error
``` 
이 경우, 컨테이너 내부에서만 발생하며, 호스트 머신에서는 이상이 없었습니다. 따라서 도커 컨테이너를 재시작하면 다시 GPU를 잘잡고 있지만, 또 며칠 후면 다시 상기와 같은 현상이 발생했습니다. 처음에는 컨테이너의 CUDA버전과 호스트머신의 CUDA 버전 호환문제일거라 예측하여 버전도 맞추고 다른 여러가지 방법을 시도해보았지만, 해결이 되지 않았었습니다. 이번절에서는 이런 현상을 해결한 방법에 대해 설명해보겠습니다.   

<br>
<a href="https://github.com/NVIDIA/nvidia-docker/issues/1730" target="_blank">Failed to initialize NVML</a> 해당 링크의 해결법을 참고하여 수행했습니다.   

## 원인
원인은 Host에서 주기적으로 daemon-reload를 수행하고 있는데, 컨테이너에서는 systemd를 이용하여 cgroups을 관리합니다. 이 때 <sap style='color:red'>daemon-reload가 수행될 경우 NVIDIA GPU를 참조하는 Unit파일들을 reload할 때, 컨테이너에서는 GPU 참조에 대한 권한을 잃어 버리게 됩니다.</span> 이런 원인이라고 의심이 될 때, Host에서 systemctl daemon-reload을 수행한 후, 다시 컨테이너로 돌아가서 GPU를 잡고 있는지 확인해보면 됩니다. 만약 못잡고 있다면 하기의 Solution을 따라 수행하시면 됩니다.   

## 해결
가장 쉬운 방법은 docker의 daemon.json파일 수정을 통해 컨테이너에서 cgroups을 비활성화하는 방법입니다.   
즉, daemon.json에 하기를 추가해주시면 됩니다.   
```bash
vi etc/docker/daemon.json 

"exec-opts": ["native.cgroupdriver=cgroupfs"] 
```

상기의 exec-opts를 추가한 후 docker를 재시작해주시면 됩니다.   
```bash
service docker restart
```

상기의 해결책으로 현상에 대해 빠르게 고치시길 기원합니다.   


