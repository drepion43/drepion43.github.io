---
title:  "[Ubuntu]내 데스크탑 서버로 만들기"
categories: Linux
tag: [linux, ubuntu]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: 포트포워딩과 ssh 설치
comments: true
date: 2024-01-16
toc_sticky: true
---
# Ubuntu 설치(Linux 계열)
저는 Ubuntu를 이용할 것이기 때문에 Ubuntu를 이용하고 싶으신 분들은 하기의 링크에 듀얼 부팅 설치하는 법을 정리했으니 먼저 이를 수행해주시면 감사하겠습니다. 만약 Ubuntu가 아닌 Centos, Debian등을 이용하고 싶으신 분들은 iso파일만 자신이 사용하고 싶은 OS를 설치하여 부팅디스크를 만들고, 해당 OS설치하는 법만 다를뿐, 큰 틀인 듀얼 부팅 설정방법은 동일하니 참고하시기에도 좋으실 것 같습니다.   
[DUAL BOOTING UBUNTU INSTALL](https://drepion43.github.io/linux/ubuntuInstall/)

# 포트포워딩
우선 cmd창을 켠 후 ipconfig를 통해 이더넷인 나의 IP주소와 공유기의 IP주소를 알아옵니다. 그럼 인터넷창에 공유기 IP주소를 입력하면 하기와 같은 창이 뜨며, 공유기 설정으로 들어갈 수 있습니다.   
저는 SKT BroadBand 인터넷을 사용하고 있어 SKT기주으로 설명을 드리겠습니다. 저의 사설 IP는 192.168.45.3으로 되어있으며, 하기의 설정창으로 들어가기 위해서는 인터넷에 192.168.45.1로 검색을 해주면 되었습니다.   
<img src="../../../assets/images/Linux/2024-01-16-ssh Connect/portforwarding 1.JPG" alt="portforwarding 1" style="zoom:80%;" />    

그 후 설정창에서 포트포워딩을 클릭하여 포트포워딩할 내 컴퓨터의 IP주소와 오픈할 외부 포트와 내부 포트를 입력해줍니다. 외부 포트의 경우 100이하는 보통 어떤 포트를 사용하고 있기 때문에 그 이후의 사용안하는 포트를 자유롭게 설정해주시면 됩니다. **다만, 외부 접속을 이용하실 거면, 외부 포트와 내부 포트 모두 22로 설정하셔야합니다.**  
<img src="../../../assets/images/Linux/2024-01-16-ssh Connect/portforwarding 2.JPG" alt="portforwarding 2" style="zoom:80%;" />    

그럼 포트포워딩은 완료가 됩니다.   
다음으로 듀얼부팅을 킨 Ubuntu에 외부에서 접속하기 위해 Ubunutu에 ssh를 설정해보겠습니다.
# SSH
이전 듀얼 부팅으로 Ubuntu를 설치하고 간단한 필요 패키지들을 설치했습니다. 이제는 제 데스크탑을 서버와 같이 만들기 위해 외부에서도 접속 가능하게 만들어보겠습니다.   
우선 외부에서의 SSH를 허용해야하기 때문에 SSH접속을 허용해보도록 하겠습니다. 보통 외뷔 ip에서의 접속까지 허용을 해주려면 포트포워딩의 포트는 **22번 포트**로 해주시는게 좋습니다.    

그럼 이제 SSH를 설정해보겠습니다. 우선 안전하게 update와 openssh-server를 설치해줍니다.   
```bash
sudo apt-get update
sudo apt-get install openssh-server
```   
<img src="../../../assets/images/Linux/2024-01-16-ssh Connect/ssh 1.jpg" alt="ssh 1" style="zoom:80%;" />    
상기와 같이 나오면 설치가 잘 완료되었으면 이제 SSH 서비스가 자동으로 시작됩니다. 이 서비스가 잘 실행되고 있는지 확인하기 위해 system control을 이용하여 status를 확인해보겠습니다.   

```bash
sudo systemctl ssh status
```
<img src="../../../assets/images/Linux/2024-01-16-ssh Connect/ssh 2.jpg" alt="ssh 2" style="zoom:80%;" />    
상기와 같이 active(running)상태이면 현재 ssh 서비스가 잘 구동되고 있다는 뜻입니다.   

Ubuntu는 기본적으로 UFW라고 불리는 방화벽 시스템이 있는데, 이 방화벽 시스템에서 SSH 포트를 열어줘야합니다.   
```bash
sudo ufw allow ssh
```   
<img src="../../../assets/images/Linux/2024-01-16-ssh Connect/ssh 3.jpg" alt="ssh 3" style="zoom:80%;" />    
UFW에 대한 규칙이 새롭게 변경되어 설정되었습니다.   
이제 SSH가 준비 되었고, 접속 준비가 완료되었습니다. 이제 접속을 해보겠습니다.   

<img src="../../../assets/images/Linux/2024-01-16-ssh Connect/ssh 4.JPG" alt="ssh 4" style="zoom:80%;" />    
```bash
ssh 계정@x.x.x.x
```
같은 사설 ip대역에서는 저렇게 외부 접속이 잘 허용되는 것을 확인할 수 있습니다. yes를 통해 key gen을 생성하고 해당 컴퓨터에서의 접속을 허용해주게 됩니다.   

<img src="../../../assets/images/Linux/2024-01-16-ssh Connect/ssh 5.JPG" alt="ssh 5" style="zoom:80%;" />    
이제 포트포워딩한 사설 ip가 아닌 공용 iP를 통해 접속을 해보겠습니다. 잘 접속이 되는 것을 상기에서 확인할 수 있습니다.   

이제 내 컴퓨터를 서버로 만들기를 성공했습니다. 내 컴퓨터를 우분투로 켜놓기만 한다면, 어디서든 서버처럼 사용을 할 수 있게 되었습니다.