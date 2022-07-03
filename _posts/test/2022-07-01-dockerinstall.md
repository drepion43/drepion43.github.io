---
title:  "Ubuntu Docker 설치"
categories: test
tag: [linux, ubuntu,docker]
toc: true
author_profile: false
sidebar:
    nav: "docs"
---


## 1. Install

[https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/)
docker 공식 홈페이지의 설치 순서대로 docker 설치


## 2. Nvidia-docker install
```python
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list

sudo apt-get update

sudo apt-get install -y nvidia-docker2

sudo systemctl restart docker

#test
sudo docker run --rm --gpus all nvidia/cuda:11.0-base nvidia-smi
```
### GPU 사용 docker를 위해 설치


## 3. docker Command

```python
# 이미지 다운
docker pull IMAGE[:TAG]

# 이미지 확인
docker images

# 컨테이너 실행
docker run -it IMAGE[:TAG]

# 컨테이너 확인
docker ps -a

# GPU 사용 컨테이너 실행
docker run -it --gpus all IMAGE[:TAG]
nvidia-docker run -it IMAGE[:TAG]
docker run --runtime=nvidia -it IMAGE[:TAG]

# GPU+ volume 지정
docker run -it --gpus all -v ~/sample(나의 로컬):/workspace(torch지역) IMAGE[:TAG]

# GPU + jupyter notebook
docker run -it --gpus all -p 8888:8888 IMAGE[:TAG]
후 token 번호로 입장

# GPU + jupyter notebook bash
docker run -it --gpus all -p 8888:8888 IMAGE[:TAG] bash

```

```python
# docker image commit
docker commit {CONTAINER ID} {IMAGE NAME}[:TAG]

# 컨테이너 중지/ 재시작
docker stop/start {CONTAINER ID}

# status=exited인 컨테이너 삭제
docker rm ${docker ps -a -q -f status=exited}
# image name으로 컨테이너 삭제
docker rm ${docker ps -a -q -f ancestor=docker ps -a -q -f ancestor=IMAGE NAME

# docker 이미지 삭제
docker rmi {IMAGE ID}
docker rmi {IMAGE NAME}[:TAG]


docker ps -a 로 컨테이너 확인 후
docker exec -it {CONTAINER ID} bash로 다시 접속

# 실행중인 컨테이너에 동시 접속
docker attach -it {CONATINER ID}

# DOCKER Hub에 이미지 업로드
docker login --username={MYDOCKEHUB_USERNAME}
docker tag{CONTAINER ID}{MYDOCKEHUB_USERNAME}/{REPOSITORY_NAME}[:TAG]
docker push {MYDOCKEHUB_USERNAME}/{REPOSITORY_NAME}

```


## 4. Docker Image PULL

[https://hub.docker.com/](https://hub.docker.com/)
원하는 image를 찾아 pull


## 5. Docker 컨테이너 내부에 sudo 권한 주기
```python
# docker sudo 권한 부여
sudo usermod -aG docker $USER

#Docker 컨테이너에서 sudo install
apt-get update && apt-get install -y sudo

```