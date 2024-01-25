---
title:  "[Centos]Off-line Docker 설치"
categories: Docker
tag: [linux, centos, Docker]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Docker 설치
comments: true
date: 2023-11-23
toc_sticky: true
---

## 개요
보통 도커를 설치한다고 하면 [https://docs.docker.com/get-docker/](https://docs.docker.com/get-docker/) 해당 docker 공식 문서를 통해 설치하는 방법은 다들 알고 계신 분들이 많을거라고 생각합니다.   
하지만, 추후에 애플리케이션을 서비스화 하거나 서비스 중인 애플리케이션의 개발을 이용하는 경우에는 외부와의 통신이 막혀있는 폐쇄망 서버(Off-line Server)인 경우가 많습니다. 이런 경우에도 도커를 쉽게 설치할 수 있는 방법에 대해 설명하려고 합니다. 이번 방법은 Centos-7.x버전을 기준으로 설명하겠습니다.

## Install
우선 도커 바이너리 파일을 인터넷이 가능한 환경에서 설치를 합니다. 하단에 해당 링크 주소입니다.   
[Docker Binary File](https://download.docker.com/linux/static/stable/x86_64/)  
여기서 설치하고 싶은 도커 바이너리 파일의 버전을 찾아서 우선 설치합니다. 
설치한 후 해당 tar 파일을 폐쇄망 서버로 전송해줍니다. 이 때, FTP, SFTP, SCP 등 폐쇄망 서버와 통신이 가능한 서버에서 폐쇄망으로 tar 파일을 전송해주면 됩니다.   

그 후, 이동 시킨 tar 파일을 내가 원하는 디렉토리에 넣어준 후 해당 tar 파일의 압축을 해제 시켜줍니다.   
```bash
tar -xvzf docker*.tar
```
이렇게 압축을 풀고나면, containerd, containerd-shim, containerd-shim-runc-v2, ctr, docker, dockerd, docker-init, docker-proxy, runc의 파일들이 생성됩니다. 이제 이 해당 파일들을 모두 복사시켜줄 겁니다.   
```bash
sudo cp * /usr/bin
```

이렇게 다 복사를 시켜준 후 이제 docker설정해주는 명령를 수행하겠습니다.   
```bash
sudo dockerd &
```

이제 docker가 백그라운드에서 실행이 되고 있을 겁니다. 
하지만 dockerd & 를 통해 백그라운드로 실행한 도커의 경우 해당 터미널이 종료되면 도커 engine 또한 down되는 현상이 발생합니다. 그래서 애플리케이션의 서비스의 경우 이런 경우들에 대해 쉽게 서버 엔진을 실행시키기 위해 service나 systemctl의 cli를 통해 관리를 합니다. 이제 실행된 docker를 프로세스를 죽인 후 systemctl을 통해 동작하도록 설정해보겠습니다.   
```bash
ps aux | grep docker
kill -9 docker*
```
상기의 CLI를 통해 docker 엔진을 껐습니다. 이제 system control에 docker.service를 추가해보겠습니다.   

```bash
vi /etc/systemd/system/docker.service
```
```bash
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com

[Service]
ExecStart=/usr/bin/dockerd
Restart=always
StartLimitInterval=0
RestartSec=3
ExecStop=/bin/kill -s QUIT $MAINPID

[Install]
WantedBy=multi-user.target
```
```bash
// 상기의 내용을 저장한 후 하기의 CLI 설정
systemctl daemon-reload
systemctl restart docker
// 부팅시 자동을 활성화 시켜주기
systemctl enable docker
```

이렇게 도커 설치를 완료할 수 있습니다. 추가적으로 docker에 sudo 권한 그룹을 묶어보겠습니다.
```bash
sudo usermod -aG docker $USER
id $USER
sudo systemctl restart docker
docker info
```   
docker group에 현재 사용중인 계정 또한 포함시켰습니다.   
만약 상기대로 진행했어도 되지않는다면, /var/run/docker.sock의 파일 권한을 한번 변경해보시면 해결이 되실겁니다.

이제 docker-compose를 설치해보겠습니다.    
[Docker-compose](https://github.com/docker/compose/releases)에서 자신의 환경에 맞는 docker-compose를 설치해줍니다. 그런 후 이전과 동일하게 scp, sftp 등을 통해 서버로 옮겨주겠습니다.   
```bash
mv docker-compose-Linux-x86_64 docker-compose // 이름 변경

sudo mv docker-compose /usr/bin/  // 디렉토리 이동
sudo chmod +x /usr/bin/docker-compose
systemctl restart docker
```
docker-compose 또한 설치를 완료 했습니다. 
