---
title:  "[AWS]Pricing and Support"
categories: AWS
tag: [AWS, Cloud]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: AWS Cloud Practitioner Essentials
comments: true
date: 2024-06-16
toc_sticky: true
---


# 출처
앞으로 정리할 내용은 [AWS Cloud Practitioner Essentials (Korean) (Na) (한국어 강의)](https://explore.skillbuilder.aws/learn/course/13522/play/107682/aws-cloud-practitioner-essentials-korean-na-hangug-eo-gang-ui)을 기반임을 밝힙니다.   

# Pricing and Support 소개

## 목표
① AWS 요금 및 지원 모델을 설명할 수 있습니다.    
② AWS 프리 티어를 설명할 수 있습니다.   
③ AWS Organizations 및 통합 결제의 주요 이점을 설명할 수 있습니다.    
④ AWS Budgets의 이점을 설명할 수 있습니다.   
④ AWS Cost Explorer의 이점을 설명할 수 있습니다.   
⑤ AWS 요금 계산기의 주요 이점을 설명할 수 있습니다.   
⑥ 다양한 AWS Support 플랜을 구별할 수 있습니다.   
⑦ AWS Marketplace의 이점을 설명할 수 있습니다.   


## AWS 프리 티어
AWS 서비스를 시험해 보고 싶을 때 **프리 티어**를 통해 서비스를 사용해 볼 수 있습니다. 어떤 제품을 염두에 두는지에 따라 프리 티어는 서비스를 테스트할 수 있는 세 가지 방법을 제공합니다.  
<br>
<span style='color:blue'>상시무료</span>   
모든 AWS 고객에게 서비스를 제공하며 프리 티어가 만료되지 않습니다.    
<span style='color:blue'>12개월 무료</span>   
AWS에 처음 가입한 그러니까 AWS에 계정을 만든 날로부터 12개월 동안 서비스를 사용해 볼 수 있습니다.   
<span style='color:blue'>평가판</span>   
일부 서비스에서는 단기 무료 평가판을 제공합니다. 특정 기간이 종료되면 평가판이 만료됩니다.   
<br>
서버리스 컴퓨팅 옵션인 AWS Lambda는 **매월 100만 건의 무료 호출을 제공**합니다.   
S3 또한 최대 5GB 스토리지까지 **12개월 동안 무료**로 사용할 수 있으며, 그 이후에는 비용이 발생합니다.   
Lightsail는 **최대 750시간을 사용할 수 있는 1개월 평가판을 제공**합니다.   


## AWS 요금 개념

### AWS 요금 적용 방식
<span style='color:blue'>실제 사용한 만큼만 지불</span>   
장기 계약 또는 복잡한 라이선스 없이 각 서비스에서 실제로 사용한 리소스의 양에 대해 정확히 지불합니다.   
<br>
<span style='color:blue'>예약하는 경우 비용이 감소</span>   
일부 서비스는 온디맨드 인스턴스 요금에 비해 상당한 할인을 제공하는 예약 옵션을 제공합니다.   
<br>
<span style='color:blue'>많이 사용할수록 볼륨 기반 할인이 적용되어 비용이 감소</span>   
일부 서비스는 계층화된 요금을 제공하므로 사용량이 증가함에 따라 점차 단위 비용이 낮아집니다.   
<br>

### AWS 요금 계산기
AWS 요금 계산기를 사용하면 AWS 서비스를 탐색하고 AWS 기반 사용 사례에 대한 비용을 추정할 수 있습니다.   
생성한 비용 추정을 저장하고 링크를 생성하여 다른 사람과 공유할 수 있습니다.   

### AWS 요금 예시

#### AWS Lambda
AWS Lambda의 경우 **함수 요청 수와 함수 실행 시간**을 기준으로 요금이 청구됩니다.   
AWS Lambda에서는 **매월 무료 요청 1백만 건과 최대 320만 초의 컴퓨팅 시간을 사용**할 수 있습니다.   
**Compute Savings Plan**에 가입하면 AWS Lambda 비용을 절감할 수 있으며 **1년 또는 3년** 기간 동안 일정 사용량을 약정하는 대신 컴퓨팅 비용이 할인됩니다.   
<br>
여러 AWS 리전에서 AWS Lambda를 사용했다면 청구서에서 **리전마다 항목별 요금**을 볼 수 있습니다.   

#### Amazon EC2
Amazon EC2를 사용하면 **인스턴스가 실행되는 동안 사용한 컴퓨팅 시간에 대해서만 비용**을 지불합니다.   
**스팟 인스턴스**를 사용하여 Amazon EC2 비용을 **최대 90% 의 비용을 절감**할 수 있습니다. 또한, Savings Plans 및 예약 인스턴스으로도 절감할 수 있습니다.   

#### Amazon S3
- 스토리지
    - 사용한 스토리지에 대해서만 요금을 지불
    - 객체의 크기, 스토리지 클래스, 해당 월에 각 객체를 저장한 기간에 따라 Amazon S3 버킷에 객체를 저장하는 요금이 청구
- 요청 및 데이터 검색
    - Amazon S3 객체 및 버킷에 수행한 요청에 대해 비용을 지불
- 데이터 전송
    - **다른 Amazon S3 버킷 간에 데이터를 전송하거나 Amazon S3에서 동일한 AWS 리전의 다른 서비스로 데이터를 전송하는 데 드는 비용은 없음**
    - Amazon S3에서 송수신한 데이터에 대해서는 비용을 지불
    - 단, 인터넷에서 Amazon S3로 전송되는 데이터 또는 Amazon CloudFront로 전송되는 데이터, Amazon S3 버킷과 동일한 AWS 리전에 있는 Amazon EC2 인스턴스로 전송되는 데이터도 비용이 없음
- 관리 및 복제
    - 계정의 Amazon S3 버킷에서 활성화한 스토리지 관리 기능에 대해 비용을 지불(Amazon S3 인벤토리, 분석, 객체 태그 지정이 포함)

## 통합 결제
AWS에서는 **AWS Organizations를 사용해서 여러 계정을 한꺼번에 관리**할 수 있습니다. AWS Organizations의 유용한 기능 중 하나는 **통합 결제**가 있습니다. 매월 말에 AWS 비용을 단일 계정마다 결제하는 것이 아니라 해당 **청구 내용을 조직 소유자가 소유한 단일 청구서로 통합해서 관리**할 수 있습니다. 각 계정에 대한 비용을 한 곳으로 모으는 것이 AWS Organizations의 통합 결제 기능입니다.   
조직 수준에서 AWS 리소스 사용량을 통합해서 관리할 수 있습니다. AWS는 대량 할인 요금을 제공하기 때문에 **각 개별 계정이 사용량이 작다고 할지라도 조직의 모든 계정을 집계한다면 대량 할인 요금을 적용** 받을 수 있습니다.    
Savings Plan을 사용 중인 경우 혹은 EC2용 예약 인스턴스를 사용하는 경우에는 이것을 조직의 AWS 계정 전체에서 공유할 수 있습니다. 또한, AWS Organization의 통합 결제 기능이 **무료**입니다.   

### 통합 결제
중앙 위치에서 여러 AWS 계정을 관리할 수 있습니다. AWS Organizations의 통합 결제 기능을 통해 **조직의 모든 AWS 계정에 대한 단일 청구서**를 받을 수 있습니다. 기본적으로 조직에 허용되는 **최대 계정 수는 4개**이지만, 필요한 경우 AWS Support에 문의하여 할당량을 늘릴 수 있습니다.   
조직의 계정 전체에서 **대량 할인 요금, Savings Plans 및 예약 인스턴스를 공유**할 수 있습니다.   

## AWS Budgets
비용 또는 사용량을 초과하거나 사용량이 예산 금액을 **초과할 것으로 예상되면 알림**을 받게 됩니다. 즉, 리소스에 배정된 예산 금액을 초과하려는 경우 **사전 알림**을 받을 수 있습니다.   

### AWS Budgets
AWS Budgets에서 예산을 생성하여 서비스 사용, 서비스 **비용 및 인스턴스 예약을 계획**할 수 있습니다. AWS Budgets에서는 정보가 **하루에 3번 업데이트**됩니다.    
AWS Budgets에서 사용량이 예산 금액을 초과하거나 초과할 것으로 예상되면 알려주는 **사용자 지정 알림**을 설정할 수 있습니다.    

## AWS Cost Explorer
AWS는 **가변 비용 모델**입니다.   
AWS Cost Explorer을 통해 AWS에서 **지출하는 비용을 시각적으로 확인하고 분석**할 수 있습니다.   
그룹핑 방법 중에 하나는 **태그별로 그룹화**하여 쉽게 시각화할 수 있습니다.   

## AWS Support 플랜
AWS에서는 **AWS Basic Support가 무료**로 제공됩니다. 모든 고객은 고객 서비스, 설명서, 백서, 지원 포럼, AWS Trusted Advisor, AWS Personal Health Dashboard에 대한 하루 24시간 기준 7일간, 즉 연중 무휴 (24/7)와 같은 지원 기능에 접근할 수 있습니다.   
다음 플랜은 **AWS Developer Support**입니다. Basic Support 수준의 모든 지원 항목을 포함하며 질문이 있는 경우 고객 지원팀에 직접 이메일을 보내면 **24시간 이내에 응답**을 받을 수 있습니다. 또한, 시스템에 장애가 있는 경우에는 **12시간 이내**에 답변을 받을 수 있습니다.    
이어서 **AWS Business Support**이 있습니다. 이는 AWS Developer Support기능에 더해 **Trusted Advisor**가 추가됩니다. AWS 지원 팀에 **전화로 직접 문의**할 수 있고, 지원팀은 운영 시스템에 장애가 있는 경우 **4시간 이내**, 운영 시스템이 **다운된 경우 1시간 이내에 응답**을 합니다. 또한 **인프라 이벤트 관리에 대한 액세스를 제공**합니다.   
**AWS로 마이그레이션**하는 기업의 경우 **AWS Enterprise On-Ramp**을 권장합니다. 전 플랜의 모든 지원 항목 외에 비즈니스 주요 워크로드에 대한 **30분 응답 시간** 및 **TAM**(Technical Account Manager) 인력풀에서 지원이 제공됩니다.   
미션 크리티컬 워크로드를 실행하는 기업의 경우 **AWS Enterprise Support**를 권장합니다. 미션 크리티컬 워크로드에 대한 **15분 응답 시간** 및 **전담 TAM이 제공**됩니다. 또한 **사전 검토, 워크숍 및 심층 분석에 대한 액세스**도 포함됩니다. TAM은 **인프라 이벤트 관리, Well-Architected 검토, 운영 검토를 제공**합니다.    
<br>
Enterprise On-Ramp에서는 TAM과의 계약에 **응답 시간에 대한 속도 제한**이 있는 반면, AWS Enterprise Support에서는 **응답 시간에 대한 속도 제한이 적용되지 않습니다**.   
TAM과 함께 작업하며 **Well-Architected Framework**를 사용하여 아키텍처를 검토합니다. 아키텍처가 운영 우수성, 보안, 신뢰성, 성능 효율성, 비용 최적화, 지속 가능성 등 여섯 가지 Well-Architected Framework 핵심 요소에 따라 검토합니다.   

### AWS Support
크게 5가지의 Support 플랜이 존재합니다.   
① Basic   
② Developer   
③ Business   
④ Enterprise On-Ramp   
⑤ Enterprise   

#### Basic Support
Basic Support는 모든 **AWS 고객에게 무료**로 제공됩니다. Basic Support에서는 **제한된 AWS Trusted Advisor 검사에 액세스**할 수 있습니다. **AWS Personal Health Dashboard**을 사용해 사용자에게 영향을 줄 수 있는 이벤트가 발생할 때 알림을 줄 수 있습니다.   
<br>
Basic 외의 Support 플랜은 **월 단위로 비용**을 지불하며 장기 계약이 필요하지 않습니다.

#### Developer Support
**모범 사례 지침**, **클라이언트 측 진단 도구**, **AWS 제품, 기능 및 서비스를 함께 사용하는 방법에 대한 지침으로 구성된 빌딩 블록 아키텍처 지원** 합니다.   

#### Business Support
**특정 요구 사항을 가장 잘 지원할 수 있는 AWS 제품, 기능 및 서비스를 식별하기 위한 사용 사례 지침**, **모든 AWS Trusted Advisor 검사**, **일반적인 운영 체제 및 애플리케이션 스택 구성 요소와 같은 서드 파티 소프트웨어에 대한 제한된 지원** 합니다.

#### Enterprise On-Ramp Support
**사전 예방적 지침을 제공하고 프로그램 및 AWS 전문가에 대한 액세스를 조율하는 TAM(Technical Account Manager) 풀**, **비용 최적화 워크숍(연 1회)**, **결제 및 계정 지원을 위한 컨시어지 지원 팀**, **Trusted Advisor 및 Health API/Dashboard를 통해 비용 및 성능을 모니터링하는 도구**를 지원합니다.   
<br>
TAM에서도 **자문 검토 및 아키텍처 지침(연 1회)**, **인프라 이벤트 관리 지원(연 1회)**, **지원 자동화 워크플로**, **비즈니스 크리티컬 워크로드에 대한 30분 이하 응답 시간**을 지원합니다.   

#### Enterprise Support
**사전 예방적 지침을 제공하고 프로그램 및 AWS 전문가에 대한 액세스를 조율하는 전담 Technical Account Manager**, **결제 및 계정 지원을 위한 컨시어지 지원 팀**, **운영 검토 및 상태 모니터링 도구**, **혁신 추진을 위한 교육 및 게임 데이**, **Trusted Advisor 및 Health API/Dashboard를 통해 비용 및 성능을 모니터링하는 도구**를 지원합니다.   
<br>
TAM에서도 **자문 검토 및 아키텍처 지침**, **인프라 이벤트 관리 지원**, **비용 최적화 워크숍 및 도구**, **지원 자동화 워크플로**, **비즈니스 크리티컬 워크로드에 대한 15분 이하 응답 시간**을 지원합니다.   

### TAM
TAM은 AWS 측 주 연락 창구입니다. 회사가 **Enterprise Support** 또는 **Enterprise On-Ramp**를 구독하는 경우 TAM은 전체 AWS 서비스에 걸쳐 클라우드 여정에 대한 교육, 역량 강화, 개선을 제공합니다.   
TAM은 전문 엔지니어링 지침을 제공하고, AWS 서비스를 효율적으로 통합하는 솔루션을 설계하도록 돕고, 비용 효율적이고 복원력 있는 아키텍처를 구축하도록 지원하고, AWS 프로그램 및 광범위한 전문가 커뮤니티에 대한 직접 액세스를 제공합니다.   

## AWS Marketplace
AWS Marketplace는 **Independent Software Vendor(ISV)의 소프트웨어 리스팅 수천 개가 포함된 디지털 카탈로그**입니다.    
AWS Marketplace의 각 리스팅에 대해 요금 옵션, 사용 가능한 지원 및 다른 AWS 고객의 리뷰 등 자세한 정보에 액세스할 수 있습니다.   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-16-Pricing and Support/Marketplace.png" alt="Marketplace" style="zoom:80%;" />    
AWS Marketplace는 인프라 소프트웨어, DevOps, 데이터 제품, 전문 서비스, 비즈니스 애플리케이션, 기계 학습, 사물 인터넷(IoT) 같은 여러 범주의 제품을 제공합니다. 각 범주에서 하위 범주의 상품 리스팅을 탐색하여 검색 범위를 좁힐 수 있습니다. 예를 들어 DevOps 범주의 하위 범주에는 애플리케이션 개발, 모니터링 및 테스트 같은 영역이 포함됩니다.

