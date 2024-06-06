---
title:  "[AWS]Networking"
categories: AWS
tag: [AWS, Cloud]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: AWS Cloud Practitioner Essentials
comments: true
date: 2024-06-01
toc_sticky: true
---


# 출처
앞으로 정리할 내용은 [AWS Cloud Practitioner Essentials (Korean) (Na) (한국어 강의)](https://explore.skillbuilder.aws/learn/course/13522/play/107682/aws-cloud-practitioner-essentials-korean-na-hangug-eo-gang-ui)을 기반임을 밝힙니다.   

# Networking 소개

## 목표
① 네트워킹의 기본 개념을 설명할 수 있습니다.   
② 퍼블릭 네트워킹 리소스와 프라이빗 네트워킹 리소스의 차이점을 설명할 수 있습니다.   
③ 실제 시나리오를 사용하여 가상 프라이빗 게이트웨이를 설명할 수 있습니다. 
④ 실제 시나리오를 사용하여 Virtual Private Network(VPN)를 설명할 수 있습니다.   
⑤ AWS Direct Connect의 이점을 설명할 수 있습니다.    
⑥ 하이브리드 배포의 이점을 설명할 수 있습니다.    
⑦ IT 전략에서 사용되는 보안 계층을 설명할 수 있습니다.   
⑧ 고객이 AWS 글로벌 네트워크와 상호 작용하기 위해 사용하는 서비스를 설명할 수 있습니다.


## AWS와 연결
VPC(Virtual Private Cloud)는 **AWS에서 사용할 수 있는 가상의 격리된 프라이빗 네트워크**입니다. VPC를 사용하면 AWS 리소스용 프라이빗 IP 범위를 정의하실 수가 있고요, VPC에 EC2 인스턴스나 ELB와 같은 요소를 배치할 수 있습니다.   
인터넷 게이트웨이(IGW)를 통해 **VPC에 연결해 누구든 리소스에 접근**할 수 있습니다. 하지만, 내부 프라이빗 리소스 즉 공개적으로 접근해서는 안 되는 리소스에 대한 방지도 필요하게됩니다. 이를 위해서는 IGW가 아닌 공개 인터넷이 아닌 승인된 네트워크에서 오는 사람은 허용하는 **프라이빗 게이트웨이**가 필요합니다.    
프라이빗 게이트웨이를 사용해도 결국 많은 사람들이 쓰는 인터넷망의 회선을 이용하기 때문에 속도 저하가 발생하게됩니다. AWS에서는 이를 막기위해 전용 회선인 **AWS Direct Connect**를 지원합니다. **AWS Direct Connect**는 물리적인 회선을 제공합니다.   

### Amazon Virtual Private Cloud(Amazon VPC)
Amazon VPC를 사용하여 AWS 클라우드의 격리된 섹션을 프로비저닝할 수 있습니다. 이 격리된 섹션에서는 사용자가 정의한 가상 네트워크에서 리소스를 시작할 수 있습니다. 한 Virtual Private Cloud(VPC) 내에서 여러 서브넷으로 리소스를 구성할 수 있습니다. 서브넷은 리소스(예: Amazon EC2 인스턴스)를 포함할 수 있는 VPC 섹션입니다.   

### IGW(인터넷 게이트웨이)
인터넷의 퍼블릭 트래픽이 VPC에 액세스하도록 허용하려면 인터넷 게이트웨이를 VPC에 연결합니다.   
인터넷 게이트웨이는 VPC와 인터넷 간의 연결입니다. 인터넷 게이트웨이가 없으면 아무도 VPC 내의 리소스에 액세스할 수 없습니다.   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/IGW.png" alt="IGW" style="zoom:80%;" />    

### 가상 프라이빗 게이트웨이
VPC 내의 프라이빗 리소스에 액세스하려면 가상 프라이빗 게이트웨이를 사용할 수 있습니다.   
보디가드는 주변의 다른 모든 요청으로부터 인터넷 트래픽을 암호화(또는 보호)하는 가상 프라이빗 네트워크(VPN) 연결과 같습니다. 가상 프라이빗 게이트웨이는 보호된 인터넷 트래픽이 VPC로 들어오도록 허용하는 구성 요소입니다.   
가상 프라이빗 게이트웨이를 사용하면 VPC와 프라이빗 네트워크(예: 온프레미스 데이터 센터 또는 회사 내부 네트워크) 간에 가상 프라이빗 네트워크(VPN) 연결을 설정할 수 있습니다. 가상 프라이빗 게이트웨이는 승인된 네트워크에서 나오는 트래픽만 VPC로 들어가도록 허용합니다.   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/PGW.png" alt="PGW" style="zoom:80%;" />    

### AWS Direct Connect
AWS Direct Connect는 데이터 센터와 VPC 간에 비공개 전용 연결을 설정하는 서비스입니다.   
AWS Direct Connect가 제공하는 프라이빗 연결은 네트워크 비용을 절감하고 네트워크를 통과할 수 있는 대역폭을 늘리는 데 도움이 됩니다.   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/AWS Direct Connect.png" alt="AWS Direct Connect" style="zoom:80%;" />    

## 서브넷 및 네트워크 액세스 제어 목록
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/Network 1.png" alt="Network 1" style="zoom:80%;" />    
VPC는 명시적 허가 없이는 아무것도 출입할 수 없는 강화된 요새라고 생각하면 됩니다. VPC에는 VPC를 출입하는 트래픽만 허용하는 게이트웨이가 있습니다. 하지만 **게이트웨이는 경계**에만 적용됩니다.   
VPC에서 서브넷을 사용해야 하는 유일한 기술적인 이유는 **게이트웨이에 대한 액세스 관리**를 위해서 입니다. **퍼블릭 서브넷은 인터넷 게이트웨이에 액세스하여 인터넷으로 연결**할 수 있습니다. **프라이빗 서브넷**은 그렇지 않죠 이뿐 아니라 **서브넷은 트래픽 권한도 제어**합니다.   
서브넷 경계를 지나는 **모든 패킷은 네트워크 액세스 제어 목록**, 즉 **네트워크 ACL**에서 검사됩니다. **네트워크 ACL은 서브넷을 출입하는 트래픽을 검사**합니다. 네트워크 ACL은 **서브넷 경계를 지나는 패킷만 평가**합니다. 네트워크 ACL은 패킷이 특정 EC2 인스턴스에 도달할 수 있는지는 평가하지 않습니다. 따라서 같은 서브넷에 여러 인스턴스에 대한 접근시 포트로 관리되는데 이 때 **인스턴스 수준의 네트워크 보안**도 필요합니다. 따라서 이를 위해 AWS는 EC2 인스턴스가 시작되면 모두 **보안 그룹으로 이동하고 모든 포트와 IP에 대해 차단**됩니다. 따라서 직접 특정 유형의 트래픽을 수락하도록 보안 그룹을 수정해야 합니다.   
여기서 보안그룹과 네트워크 ACL은 같은 일을 하지만, **보안 그룹의 경우 상태 저장**, **네트워크 ACL은 상태를 비저장**합니다. 따라서 ACL은 계속해서 검사를 하게 됩니다.    
<br>   
보안 그룹은 이미 검사한 패킷에 대해서는 상태를 저장해서 해당 패킷이 EC2 인스턴스로 들어올 때, 보안그룹을 거치지 않습니다. 하지만, 네트워크 ACL은 상태 비저장이어서 항상 검사를 수행해야합니다.   

### 서브넷   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/subnet.png" alt="subnet" style="zoom:80%;" />    
서브넷은 보안 또는 운영 요구 사항에 따라 리소스를 그룹화할 수 있는 VPC 내의 한 섹션입니다. 크게 프라이빗 서브넷, 퍼블릭 서브넷이 있습니다.   
퍼블릭 서브넷에는 온라인 상점의 웹 사이트와 같이 누구나 액세스할 수 있어야 하는 리소스가 포함됩니다.   
프라이빗 서브넷에는 고객의 개인 정보 및 주문 내역이 포함된 데이터베이스와 같이 프라이빗 네트워크를 통해서만 액세스할 수 있는 리소스가 포함됩니다.    
**VPC 내에서 서브넷은 서로 통신**할 수 있습니다.    

### VPC의 네트워크 트래픽
패킷은 **인터넷이나 네트워크를 통해 전송되는 데이터의 단위**입니다.   
패킷은 인터넷 게이트웨이를 통해 VPC로 들어갑니다. 패킷이 서브넷으로 들어가거나 서브넷에서 나오려면 **먼저 권한을 확인**해야 합니다. 이러한 사용 권한은 패킷을 보낸 사람과 패킷이 서브넷의 리소스와 통신하려는 방법을 나타냅니다.   

서브넷의 패킷 권한을 확인하는 VPC 구성 요소는 **네트워크 액세스 제어 목록(ACL)**입니다.   

### 네트워크 ACL
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/NACL.png" alt="NACL" style="zoom:80%;" />    
네트워크 ACL은 **서브넷 수준에서 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽**입니다.   
각 AWS 계정에는 기본 네트워크 ACL이 포함됩니다. VPC를 구성할 때 계정의 기본 네트워크 ACL을 사용하거나 사용자 지정 네트워크 ACL을 생성할 수 있습니다.    
계정의 기본 네트워크 ACL은 **기본적으로 모든 인바운드 및 아웃바운드 트래픽을 허용**하지만 **사용자가 자체 규칙을 추가하여 수정**할 수 있습니다.   
<br>
<span style='color:blue'>**스테이트리스 패킷 필터링**</span>   
네트워크 ACL은 스테이트리스 패킷 필터링을 수행합니다. ACL은 아무것도 기억하지 않고 각 방향(인바운드 및 아웃바운드)으로 서브넷 경계를 통과하는 패킷만 확인합니다.    
네트워크 ACL은 규칙 목록에 따라 패킷 응답을 확인하여 허용 또는 거부 여부를 결정합니다.   

### 보안그룹
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/Security Group.png" alt="Security Group" style="zoom:80%;" />    
보안 그룹은 Amazon EC2 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상 방화벽입니다.   
보안 그룹은 **패킷에서 Amazon EC2 인스턴스에 대한 권한을 확인하는 VPC 구성 요소**입니다.   
기본적으로 보안 그룹은 **모든 인바운드 트래픽을 거부하고 모든 아웃바운드 트래픽을 허용**합니다.   
동일한 VPC 내에 여러 Amazon EC2 인스턴스가 있는 경우 동일한 보안 그룹에 연결하거나 각 인스턴스마다 서로 다른 보안 그룹을 사용할 수 있습니다.   
<br>
<span style='color:blue'>**스테이트풀 패킷 필터링**</span>   
보안 그룹은 스테이트풀 패킷 필터링을 수행합니다. 들어오는 패킷에 대한 이전 결정을 기억합니다.   
보안 그룹은 인바운드 보안 그룹 규칙에 관계없이 응답이 진행하도록 허용합니다.   
네트워크 ACL과 보안 그룹을 모두 사용하면 VPC에서 트래픽에 대한 사용자 지정 규칙을 구성할 수 있습니다.   

<br>
정리해보겠습니다.   
프라이빗 서브넷은 **고객의 개인 정보가 포함된 데이터베이스를 격리**합니다.   
가상 프라이빗 게이트웨이은 **VPC와 사내 네트워크 간 VPN 연결을 생성**합니다.   
퍼블릭 서브넷은 고객 대상 웹 사이트를 지원합니다.   
AWS Direct Connect은 **온프레미스 데이터 센터와 VPC 간에 전용 연결을 설정**합니다.   

## 글로벌 네트워킹
Route 53은 AWS의 Domain Name Service(DNS)입니다.   
DNS는 번역 서비스인데, 웹 사이트의 이름을 컴퓨터가 이해할 수 있는 주소인 IP address, 즉 인터넷 프로토콜로 번역해주는 것입니다.    
Route 53는 트래픽을 지연 시간, 지리적 위치, 지역 근접성, 가중치 기반 라운드 로빈 같은 다양한 라우팅 정책을 사용하여 서로 다른 엔드포인트로 보낼 수 있습니다. 

### 도메인 이름 시스템(DNS)
DNS는 번역 서비스인데, 웹 사이트의 이름을 컴퓨터가 이해할 수 있는 주소인 IP address, 즉 인터넷 프로토콜로 번역해줍니다.   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/DNS.png" alt="DNS" style="zoom:80%;" />    

### Amazon Route 53
Amazon Route 53은 **DNS 웹 서비스**입니다. AWS에서 호스팅되는 인터넷 애플리케이션으로 라우팅할 수 있는 신뢰할 수 있는 방법을 제공합니다.   
Route 53의 또 다른 기능에는 도메인 이름의 **DNS 레코드를 관리하는 기능**도 있습니다.   
추가로 웹 사이트를 호스팅할 때, 지리적으로 가까운 위치인 **CDN**을 이용하여 더 빠르게 호스팅할 수 있도록 할 수 있습니다.   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-01-Networking/Route53(CDN).png" alt="Route53(CDN)" style="zoom:80%;" />    
① 고객이 AnyCompany의 웹 사이트로 이동하여 애플리케이션에서 데이터를 요청합니다.    
② Amazon Route 53은 DNS 확인을 사용하여 AnyCompany.com의 IP 주소인 192.0.2.0을 식별합니다. 이 정보는 고객에게 다시 전송됩니다.    
③ 고객의 요청은 Amazon CloudFront를 통해 가장 가까운 엣지 로케이션으로 전송됩니다.    
④ Amazon CloudFront는 수신 패킷을 Amazon EC2 인스턴스로 전송하는 Application Load Balancer에 연결됩니다.

