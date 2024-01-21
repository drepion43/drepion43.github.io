---
title:  "[Ubuntu]계정 생성"
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

# 계정생성
Ubuntu에서 계정을 생성하는 법은 크게 2가지가 있습니다. 이 두가지는 adduser와 useradd가 있습니다.   
① adduser 명령어를 통해 계정을 생성할 경우 기본 계정 정보, 홈 디렉터리, 쉘 설정 등 한 번에 진행되면서 생성됩니다.   
② useradd 명령어는 계정만 생성이 되고 비밀번호 설정이나 홈 디렉터리 등 기타 부가적인 부분들은 따로 진행을 해야 한다. 한번 각각 실행해보면서 알아보겠습니다.   

## adduser
adduser를 사용할시, 계정을 생성하면서 group 추가와 user 추가, 홈 디렉터리 생성, 비밀번호 설정 등 모든 과정이 한 번에 진행됩니다.   
<img src="../../../assets/images/Linux/2024-01-20-newaccount/adduser 1.JPG" alt="adduser 1" style="zoom:80%;" />    
상기와 같이 쉽게 생성이 됩니다.   

## useradd
상기에서 설명했다시피, useradd는 계정을 생성하고 추가적으로 더 설정을 해줘야 합니다. 크게 7가지로 나눌 수 있습니다.   

① 우선 계정만 생성을 해보겠습니다.   
<img src="../../../assets/images/Linux/2024-01-20-newaccount/useradd 1.JPG" alt="useradd 1" style="zoom:80%;" />    

② 이제 계정을 만들었으니 비밀번호를 설정해보겠습니다.   
```bash
passwd uncle
```

③ 이제 홈 디렉터리가 없으니 생성하겠습니다.   
<img src="../../../assets/images/Linux/2024-01-20-newaccount/useradd 2.JPG" alt="useradd 2" style="zoom:80%;" />    
<img src="../../../assets/images/Linux/2024-01-20-newaccount/useradd 3.JPG" alt="useradd 3" style="zoom:80%;" />    

④ 이제 홈디렉터리에 권한을 부여하겠습니다. 현재 root 권한으로 만들었기 때문에 소유가 root이기 때문에 생서한 계정으로의 권한을 부여해줘야합니다.   
<img src="../../../assets/images/Linux/2024-01-20-newaccount/useradd 4.JPG" alt="useradd 4" style="zoom:80%;" />    
```bash
chown -R uncle:uncle /home/uncle
```
이제 해당 디렉토의 소유주가 uncle로 바뀐 것을 확인할 수 있습니다.   

⑤ 생성한 계정이 가입될 그룹을 생성하고 추가해주겠습니다.   
```bash
groupadd customer
usermod -G customer uncle
usermod -G customer captin
```   
<img src="../../../assets/images/Linux/2024-01-20-newaccount/useradd 5.JPG" alt="useradd 5" style="zoom:80%;" />    
   
⑥ 이제 기본 쉘을 지정하겠습니다. 현재 기본 쉘로 dash 쉘을 사용하고 있는 것을 확인할 수 있다. bash 쉘을 사용하고 싶다면 하기와같이 진행하시면 됩니다.   
<img src="../../../assets/images/Linux/2024-01-20-newaccount/useradd 6.JPG" alt="useradd 6" style="zoom:80%;" />    
   
⑦ 이제 계정으로 접속을 해보시면 끝이납니다.   
```bash
su -uncle # uncle 계정으로 접속
su -captin # captin 계정으로 접속
```

## 계정 삭제
이때까지 계정 생성에 대해 알아보았습니다. 그럼 생성을 했으면 삭제도 필요하니 삭제에 대해 알아보겠습니다.   
계정 삭제도 ① 계정만 삭제 ② 계정과 홈디렉토리 삭제 이렇게 2가지 방법이 있습니다.   

① 계정만 삭제하겠습니다  
```bash
sudo userdel [계정이름]
```   
계정은 삭제되었으나 홈 디렉터리는 그대로 남아있는 것을 확인할 수 있습니다.   
계정의 삭제 여부는 /etc/passwd 파일을 통해서도 확인할 수 있습니다.   

② 계정과 홈디렉토리 모두 삭제하겠습니다.   
똑같이 userdel 명령어인데, 추가적인 option을 넣어주면 됩니다.   
```bash
sudo userdel -rf [계정이름]
-r : 계정 삭제시 홈 디렉터리를 포함한 모든 정보를 삭제
-f : 강제 삭제
```   
