---
title:  "[Docker]주피터 노트북 원격 접속"
categories: Docker
tag: [linux, Docker]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: jupyter notebook
comments: true
date: 2024-01-22
toc_sticky: true
---
# 개요
원격 서버 환경에서 주피터를 이용해 개발을 하기 위해 이번 포스팅을 작성하겠습니다.

## Container Install
주피터 노트북을 실행시키기 위해서 우선 컨테이너부터 설치를 해줘야합니다. 컨테이너는 우선 간편하게 tensorflow docker에 존재하는데, gpu와 jupyter를 모두 설치되어 있는 컨테이너를 갖고와서 실행하겠습니다. 하기에 docker hub에서 원하는 tensorflow 버전을 설치하시면 됩니다. 만약 컨테이너에서 따로 jupyter나 tensorflow를 설치하실거라면 python 컨테이너를 pull하여 직접 환경을 세팅하시면 됩니다. 이 부분에 대해서는 추후에 Dockerfile에 대해 공부하며 함께 정리해보겠습니다.   
[tensorflow Docker](https://www.tensorflow.org/install/docker?hl=ko)   
   
우선 image를 pull 하겠습니다.    
```bash
docker pull tensorflow/tensorflow:latest-gpu-jupyter
```

이제 이미지를 설치했으니 다음 단계로 넘어가보겠습니다.   

## Create Container and Connect Remote Server
우선 외부 환경에서 해당 컨테이너로 접속을 하려면 포트를 열어줘 ip:port를 통해 접속을 해야합니다. 그럼 우선 구조가 어떤식인지에 대해 간략하게 알아보겠습니다.   
<img src="../../../assets/images/Docker/2024-01-22-jupyternotebook/jupyter 1.png" alt="Network Architecture" style="zoom:80%;" />    
외부에서 접속하는 우리는 USER라고 생각하시면 되고, 원격 서버에 tensorflow 개발환경과 jupyter가 설치된 Container로 접속하는 것이 목표입니다.   

보통 jupyter Conatiner를 열 때 포트는 8888로 설정을 많이 합니다. 따라서 8888포트로 여는 것을 기준으로 설명하겠습니다.   
```bash
docker run -it --gpus all -p 8888:30000 IMAGE[:TAG]
```
이제 상기의 Docker Container를 실행하는 CLI을 상기의 이미지와 대조하며 설명하겠습니다.   
우선 -p 옵션은 Container의 포트를 개방한다는 의미가 되며 앞의 8888포트는 Container의 내부 포트를 의미합니다. 즉, 상기의 이미지에서 UNIX SOCKET에 해당합니다. 우리의 OS환경인 Ubuntu와의 연결이 필요합니다.   

Ubuntu의 포트 30000과 컨테이너 간의 연결이 -p 8888:30000을 의미합니다. 즉, 상기의 이미지에서 보면 UNIX SOCKET의 Ubuntu방향이 30000이 되고 Container쪽이 8888이 된다는 의미입니다.    
이제 Conatiner와 우리의 환경인 OS간의 연결은 완료가 되었습니다. 이 때, 만약 원격 접속이 아닌 경우에는 localhost:30000을 통해 jupyter를 접속하실 수 있으실 겁니다.   

이제 Ubuntu와 DHCP간의 포트인 TCP 포트를 연결할 차례입니다. 이 부분은 DHCP를 통해 기존에는 자동할당을 통해 IP가 바뀌면서 할당이 되었을텐데, 저희는 고정IP설정을 통해, 사설 IP가 고정되어 있는 상태입니다. 그럼 이 사설 IP인 192.168.xx.xx:30000과 컨테이너 8888포트와 연결이 된 부분이 TCP PORT가 됩니다.    

마지막으로 외부 IP인 61.xx.xx.xx에 내가 연결하고자하는 포트를 포트포워딩으로 열어 연결하시면됩니다. 포트포워딩을 외부 포트 7777과 내부 포트 30000으로 설정을 해주면 61.xx.xx.xx:7777을 통해 컨테이너 환경과 연결이 된다는 뜻이 됩니다. 하기에 jupyter notebook을 실행시키는 CLI를 알려드리겠습니다.   
```bash
docker run \
        -d \
        --gpus all \
        -p 8888:30000 \
        --name tensorflow_gpu \
        tensorflow/tensorflow:latest-gpu-jupyter \
        jupyter notebook \
        --allow-root \
        --ip 0.0.0.0 \
        --NotebookApp.token='' \
        --no-browser
```
