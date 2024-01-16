---
title:  "[Ubuntu]설치 방법(듀얼 부팅)"
categories: Linux
tag: [linux, ubuntu]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: Ubuntu install
comments: true
date: 2022-07-01
toc_sticky: true
---

# Install

듀얼 부팅 사용법

# 1. 부팅 디스크
사용하지 않는 최소 16GB이상이 되는 USB하나를 준비해야합니다. 그리고 그 USB를 Ubuntu 부팅디스크로 만들어주겠습니다.   

<https://ubuntu.com/>    

상기의 Ubuntu 공식 홈페이지에 들어가 Download란을 클릭해주면 Ubuntu Desktop밑에 Get Ubuntu Desktop이 있을 것입니다. 여기서 들어가 원하는 Ubuntu 이미지 파일을 설치하시면 됩니다.   
저는 안전한 LTS버전인 22.04.3 LTS 버전을 설치하겠습니다. 보통 LTS 버전이 긴 시간 동안 지원해주는 버전이기도 하고 조금 더 안전해서 LTS 버전을 설치하시는 것을 추천드립니다. 그럼 amd.iso파일을 설치해주면 됩니다.    

## 부팅 디스크 세팅
<https://rufus.ie/downloads/>   
상기의 rufus파일을 설치하여 rufus-3.17.exe 프로그램 사용하여 부팅 디스크를 만들겠습니다.

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting Disk 1.JPG" alt="Step 1" style="zoom:80%;" />    
상기의 이미지에 내가 연결시킨 USB가 장치란을 클릭하면 뜰 것입니다. 해당 장치를 선택해주면됩니다.   
그리고 어떤 부팅디스크를 만들지 선택을 해주면, 이번 포스팅에서는 Ubuntu 22.04.3 LTS 버전의 부팅디스크를 만들 것이니, 방금 전에 설치한 amd.iso파일을 찾아 설치해줍니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting Disk 2.JPG" alt="Step 2" style="zoom:80%;" />    
파티션 방법은 현재에는 딱히 상관이 없으니, default값인 MBR로 선택을 하고, 대상 시스템은 UEFI나 BIOS를 선택하겠습니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting Disk 3.JPG" alt="Step 3" style="zoom:80%;" />    
파일시스템은 FAT32을 이용할 것이니 해당란이 선택되어있지 않다면, 선택해줍니다.   
그리고 시작버튼을 누르면 하기와 같이 나타날 것 입니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting Disk 4.JPG" alt="Step 4" style="zoom:80%;" />    
하기와 같이 iso로 쓸 것인지 DD로 쓸 것인지를 물어보는데, 저희는 iso파일로 쓸 것이니 확인 버튼을 누릅니다.    
그럼 추가적으로 현 USB에 저장된 내용이 다 날라간다고 뜨는데, 만약 사용중이던 USB라면, 해당 USB에 저장된 내용을 백업해두시는 걸 추천드리겠습니다.   
저는 새로운 USB라서 확인 버튼을 USB를 부팅디스크로 만들었습니다.    
약간의 시간을 기다리면 우분투 이미지로 된 부팅 디스크 생성이 완료됩니다. 

# 2. 할당할 디스크 파티션 생성

(윈도우키 + R)클릭 후 diskmgmt.msc 실행하면 디스크 관리창이 나타납니다.   

그림   
저는 현재 이전에 듀얼 부팅을 사용하던 Ubuntu에 700GB 정도를 할당시켰었습니다. 이미지에서 디스크 0에 736.43GB가 그 크기입니다. 현재 해당 크기의 볼륭을 삭제한 후 그대로 이 크기만큼 Ubuntu를 설치하겠습니다.    
<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Disk Partition.JPG" alt="Disk Partition" style="zoom:80%;" />    

# **3. 시작메뉴 > 복구 메뉴를 통해, 부팅 순서 변경하기**
우선 컴퓨터를 재부팅 후 복구 메뉴로 들어가야합니다. 이 때, USB를 꽂아서 USB에 설치된 우분투를 방금 파티션한 디스크에 설치를 하는 방식입니다.   
그럼 컵퓨터 재부팅시 만들었던 Ubuntu 부팅 디스크를 꽂아 다시 부팅하겠습니다.    
이때 각 조립 컴퓨터의 경우, 메인보드의 제조사마다 BIOS 진입하는 방법이 다르니, 이는 자신의 컴퓨터에 들어있는 메인보드 제조사에 맞는 방법을 찾아 진입하시면 됩니다.   

저는 메인보드가 MSI여서 Delete키를 부팅시에 누르면 BIOS 모드로 진입이 가능합니다.   
<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting step 1.jpg" alt="Booting step 1" style="zoom:80%;" />    
BIOS 모드로 진입했을 때의 이미지입니다.  

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting step 2.jpg" alt="Booting step 2" style="zoom:80%;" />    
이제 SETTING에서 부팅메뉴로 들어가겠습니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting step 3.jpg" alt="Booting step 3" style="zoom:80%;" />    
그런 후 부팅 순서를 변경해줄 것입니다. 상기의 파란색란을 선택하시고 Enter를 누르시면 하기와 같이 나옵니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting step 4.jpg" alt="Booting step 4" style="zoom:80%;" />    
하지만, 저의 경우 Default는 윈도우로 설정하고, BIOS모드로 진입해서 우분투로 OS를 변경하여 실행시킬 것이기 때문에 기존과 동일하게 WINDOWS BOOT MANAGER를 제일 상단인 가장 큰 우선순위로 그대로 두겠습니다. Ubunutu를 메인으로 한다면, 설치한 Ubunutu를 제일 위로 올리시면 됩니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/Booting step 5.jpg" alt="Booting step 5" style="zoom:80%;" />    
우선 제 디스크에 우분투가 설치가 안되어있으니, 설치하기 위해서 이전에 만들었던 부팅 디스크로 실행을 시키겠습니다.

# 4. Ubuntu 설치
상기의 순서대로 진행을 했다면, 이제 만들었던 부팅 디스크로 실행이 될 것 입니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 1.jpg" alt="ubuntu install 1" style="zoom:80%;" />    
시작된 부팅 디스크에서 상기의 파란색란인 내 컴퓨터의 디스크로 설치를 하기 위해서 Install Ubuntu를 선택해줍니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 2.jpg" alt="ubuntu install 2" style="zoom:80%;" />    
<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 3.jpg" alt="ubuntu install 3" style="zoom:80%;" />    
Install Ubuntu를 선택하면 상기의 이미지처럼 GRUB이 뜨는데, 여기서도 Install Ubuntu를 선택해줍니다. 만약, 한글로 이용하고싶다면, 옆의 언어에서 Korean을 찾으시면 되실 것입니다. 저는 예전에 한글 지원이 잘 안되는 버전에서 한글이 깨지는 현상이 있어 선호하진 않았습니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 4.jpg" alt="ubuntu install 4" style="zoom:80%;" />    
이제 언어를 선택 후, Continue를 누르시면 상기와 같은 창이 나타날 것 입니다. 이 때, Minimal Install은 정말 간단하게 사용하며, 빠르게 설치를 하실 때 주로 선택하시면 되는데, 저는 일반 설치를 선택하겠습니다. 그리고 파란색란의 경우에는 무선랜카드나 엔피디아 그래픽 카드를 사용하는 환경이라면 체크하는 것을 추천드립니다. 저는 GPU 이용을 위해 선택 후 Continue를 했습니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 5.jpg" alt="ubuntu install 5" style="zoom:80%;" />    
이제 매우 중요한 내 디스크 어디에 우분투를 설치할지를 결정하는 부분입니다. 저희는 디스크 파티션을 토해 설치할 곳을 미리 포맷을 해두었습니다. 그리고 **듀얼부팅의 경우에는 꼭 Something Else(기타)란을 체크**하시고 넘어가시기 부탁드립니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 6.JPG" alt="ubuntu install 6" style="zoom:80%;" />    
저희가 포맷했었던 디스크의 크기나 나옵니다. 해당 free space를 더블 클릭을 해주겠습니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 7.jpg" alt="ubuntu install 7" style="zoom:80%;" />    
스왑 파티션이 필요하다면, 설정을 해줍니다. 만약 현재 컴퓨터의 램 공간이 충분하다면 안만들으셔도 되지만, 저는 RAN의 크기가 여유롭지 않기 때문에 스왑 파티션을 생성해주었습니다. 1GB = 1024MB라고 생각을 하시고 필요한 만큼 스왑 파티션을 만드시면 됩니다. 파티션의 종류는 **Logical**, 파티션 위치는 **Beginning**으로 설정하겠습니다.    

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 8.jpg" alt="ubuntu install 8" style="zoom:80%;" />    
이제 제가 사용할 디스크를 이용해보겠습니다. 운영체제를 설치할 파티션의 경우에는 파티션의 종류는 **Logical이 아닌 주**로 설정을 해주어야합니다. 남은 용량 전체를 이용하여 만들 것이며, 파티션의 종류는 **주**, 파티션의 위치는 **Beginning**, 용도는 **EXT4**로, 마운트 위치는 **/**로 설정하겠습니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 9.jpg" alt="ubuntu install 9" style="zoom:80%;" />    
제가 할당 시킨 공간이 이렇게 나타날 것 입니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 10.jpg" alt="ubuntu install 10" style="zoom:80%;" />    
가장 중요한 부분입니다. 보통 sdb는 SDD를 나타내며, sda는 HDD를 나타냅니다. 기본적으로 부트로더 설치할 장치는 /dev/sda로 잡히며, 내가 현재 설치한 운영체제가 SSD에 있다면 /dev/sdb로 설정해야합니다. 그렇게 설정을 하지 않고 설치시 GRUB 부트로더가 나오지 않게됩니다. 따라서, 듀얼 부팅의 경우 저는 Ubuntu를 HDD인 /dev/sda에 설치하기 때문에 부트로더는 **/dev/sda3**로 설정을 해줘야합니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 11.jpg" alt="ubuntu install 11" style="zoom:80%;" />    
다음으로 넘겨주면 이제 지역을 선택해주면 됩니다. 저는 한국인이니 서울이니 서울을 선택했습니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 12.jpg" alt="ubuntu install 12" style="zoom:80%;" />    
마지막 과정인 계정을 생성하면 됩니다. 우선 사용자 이름 생성을 해주고 비밀번호도 설정을 해주면 됩니다.   

<img src="../../../assets/images/Linux/2022-07-01-ubuntu/ubuntu install 13.jpg" alt="ubuntu install 13" style="zoom:80%;" />    
자 이제 설치를 기다리면 Ubuntu 듀얼 부팅 설치가 끝이 납니다.   

