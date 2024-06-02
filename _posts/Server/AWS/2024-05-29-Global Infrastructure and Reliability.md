---
title:  "[AWS]Global Infrastructure and Reliability"
categories: AWS
tag: [AWS, Cloud]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: AWS Cloud Practitioner Essentials
comments: true
date: 2024-05-29
toc_sticky: true
---


# 출처
앞으로 정리할 내용은 [AWS Cloud Practitioner Essentials (Korean) (Na) (한국어 강의)](https://explore.skillbuilder.aws/learn/course/13522/play/107682/aws-cloud-practitioner-essentials-korean-na-hangug-eo-gang-ui)을 기반임을 밝힙니다.   

# Global Infrastructure and Reliability 소개

## 목표
① AWS 글로벌 인프라의 이점을 요약할 수 있습니다.   
② 가용 영역의 기본 개념을 설명할 수 있습니다.   
③ AWS CloudFront 및 엣지 로케이션의 이점을 설명할 수 있습니다.   
④ Amazon 서비스를 프로비저닝할 수 있는 다양한 방법을 비교할 수 있습니다.   
<br>
AWS는 AWS 글로벌 인프라를 구축해 고가용성과 내결함성을 가졌습니다. 즉, 리전(지리적 위치)를 사용해 다양한 곳에 데이터센터를 두는 방법을 채택했습니다.   

## 글로벌 인프라
AWS가 등장한 후 여러분의 회사와 같은 기업은 애플리케이션을 자사의 소유가 아닌 **데이터 센터에서 실행**할 수 있게 되었습니다.    
AWS는 대규모 그룹의 자체 **데이터 센터를 각 리전(지역) 구축**하여 재난 발생에 대비합니다. 즉, 각 리전에는 애플리케이션을 실행하는 데 필요한 컴퓨팅, 스토리지 및 기타 모든 서비스가 구비된 다양한 데이터 센터가 존재합니다. 각 리전은 고속섬유 광네트워크를 통해 **다른 리전과 연결**되어있습니다. 각 리전은 **다른 리전과 격리**되어 있으며 사용자가 명시하지 않는 이상 **다른 리전으로 데이터 이동은 불가**합니다. 리전 선택에는 4가지 비지니스 요구사항이 있습니다.    
① 규정 준수입니다. $\rightarrow$ 해당 데이터가 선택한 리전을 벗어나면 안되는 조건에 대한 것입니다.   
② 근접성입니다. $\rightarrow$ 고객들이 해당 리전에 산다면 그 리전에서의 실행을 고려해야 합니다. 다른 리전에서 실행시 **지연시간**이 발생하기 때문입니다.   
③ 가용성입니다. $\rightarrow$ 가장 가까운 리전에서 원하는 AWS 기능 중 일부가 제공되지 않기도 합니다. 그럼 그 기능을 제공하는 리전에서 사용을 해야합니다.    
④ 가격입니다. $\rightarrow$ 리전에 있는 하드웨어가 동일하더라도 일부 지역은 운영 비용이 더 비쌉니다.    

### 리전 선택
서비스, 데이터 및 애플리케이션에 적합한 리전을 결정할 때 하기의 비지니스 요구사항을 고려해야합니다.   

<br>
<span style='color:blue'>데이터 커버넌스 및 법적 요구 사항 준수</span>    
회사와 위치에 따라 **특정 영역에서 데이터를 실행**해야 할 수도 있습니다.   
모든 회사에 위치 기반 데이터 규정이 있는 것은 아니므로 다른 세 가지 요소에 더 집중해야 할 수도 있습니다.   

<br>
<span style='color:blue'>고객과의 근접성</span>    
**고객과 가까운 리전을 선택**하면 고객에게 콘텐츠를 **더 빠르게 제공**하는 데 도움이 됩니다.   

<br>
<span style='color:blue'>리전 내에서 사용 가능한 서비스</span>    
경우에 따라 고객에게 제공하려는 기능이 가장 가까운 리전에 없을 수도 있습니다. 즉, AWS에서 모든 지역에 똑같은 기능을 제공하고 있지는 않습니다. 원한느 기능을 제공하고 있는 리전을 선택해야합니다. 즉, 개발자는 **이미 서비스를 제공하는 리전 중 하나에서 이 플랫폼을 실행**해야 합니다.   

<br>
<span style='color:blue'>요금</span>    
각 리전마다의 요금이 다르게 부과됩니다.    

### 가용 영역
AWS에서는 단일 데이터 센터 또는 데이터 센터의 그룹을 가용 영역 이제 **Availability Zone**이라고 합니다. 각 가용 영역에는 예비 전원과 네트워킹을 구비한 하나 이상의 **독립적인 데이터 센터**가 있습니다. 즉, 각 AWS 리전은 특정 지역 내에 **서로 격리**되어 있고 다시 **각각의 리전은 물리적으로 분리된 여러 가용 영역으로 구성**됩니다.   
<br>
<img src="../../../assets/images/AWS Cloud Practitioner/2024-05-29-Global Infrastructure and Reliability/available zone.png" alt="available zone" style="zoom:80%;" />    
가용 영역은 **리전 내의 단일 데이터 센터 또는 데이터 센터 그룹**입니다. 간격은 어느정도 가까우지만, 리전의 한 부분에서 재해가 발생할 경우 여러 가용 영역이 영향을 받을 가능성을 줄일 만큼 멀리 떨어져 있습니다.   

## 엣지 로케이션
리전을 선택할 때의 주요 기준 중 하나는 **고객과의 근접성**이지만, 고객이 AWS 리전과 멀리 떨어져 있을 수도 있습니다. 이 경우 전 세계 고객과 더 가까운 곳에 **데이터 복사본을 캐싱**하는 작업은 **콘텐츠 전송 네트워크(CDN)**라고 합니다.    
**Amazon CloudFront**는 데이터, 동영상, 애플리케이션, API를 전 세계 고객에게 짧은 지연 시간으로 빠르게 전달해 주는 서비스입니다. Amazon CloudFront는 전 세계에 있는 **엣지 로케이션**을 이용해 사용자가 어떤 위치에 있든 통신 속도를 높입니다. 엣지 로케이션은 **리전과 구분**되며, 리전에 있는 콘텐츠를 전 세계 엣지 로케이션 모음에 푸시할 수 있습니다. AWS 엣지 로케이션은 CloudFront뿐만 아니라, **Amazon Route 53**라고 하는 DNS도 실행합니다.   
**AWS Outposts**는 사용자의 데이터 센터 내부에 정상적으로 작동하는 소형 리전을 기본적으로 설치하는 곳입니다. AWS가 소유하고 운영하며 AWS의 모든 기능을 사용하지만 **사용자의 건물에 격리**됩니다. 즉, 많은 고객에게 필요한 솔루션은 아니지만, **기업 내 자체 건물에 머물러야하는 경우**에 적합합니다.   

<br>
정리해보겠습니다.   
리전은 지리적으로 격리된 영역으로 리전을 통해 기업 운영에 필요한 서비스를 액세스할 수 있습니다.   
리전에는 가용 영역이 있어 물리적으로 수십 킬로미터 떨어진 여러 건물에서 애플리케이션을 실행하면서도 논리적 통합을 유지할 수 있습니다.   
AWS 엣지 로케이션은 Amazon CloudFront를 실행하여 고객이 전 세계 어디에 있더라도 콘텐츠를 고객과 가까운 곳으로 가져올 수 있습니다.   

## AWS 리소스를 프로비저닝하는 방법
AWS의 모든것들은 **API를 통해 서비스**가 동작하며 구성할 수 있습니다. 즉, AWS Management Console, AWS 명령줄 인터페이스, AWS 소프트웨어 개발 키트, 또는 AWS CloudFormation과 같은 도구를 이용하여 **AWS API로의 전송 요청**을 만들 수 있으며 **AWS 리소스를 생성하고 관리**할 수 있습니다.   

### AWS Management Console
AWS Management Console은 브라우저 기반입니다. AWS 리소스를 **시각적**이면서도 이해하기 쉬운 형태로 관리할 수 있으며 **버튼으로 인스턴스를 생성**할 수 있습니다. 하지만, 이 경우 2번째 동일한 인스턴스를 만들 때, 사람이 직접하기 때문에 오류가 발생할 수 있습니다. 이를 위해 AWS 명령줄 인터페이스 즉, **CLI를 통해 AWS API를 호출하는 방법**도 있습니다.   

<br>
<span style='color:blue'>AWS Management Console</span>    
<img src="../../../assets/images/AWS Cloud Practitioner/2024-05-29-Global Infrastructure and Reliability/AWS Management Console.png" alt="AWS Management Console" style="zoom:80%;" />    
AWS Management Console은 Amazon 서비스 액세스 및 관리를 위한 웹 기반 인터페이스입니다.   

<br>
<span style='color:blue'>AWS Command Line Interface</span>    
<img src="../../../assets/images/AWS Cloud Practitioner/2024-05-29-Global Infrastructure and Reliability/AWS CLI.png" alt="AWS CLI" style="zoom:80%;" />    
명령을 작성하고 해당 명령을 실행하는 것으로 EC2 인스턴스를 시작할 수가 있습니다. AWS CLI를 사용하면 스크립트를 통해 서비스 및 애플리케이션이 수행하는 작업을 자동화할 수 있습니다.   

<br>
<span style='color:blue'>소프트웨어 개발 키트(SDK)</span>    
<img src="../../../assets/images/AWS Cloud Practitioner/2024-05-29-Global Infrastructure and Reliability/AWS SDK.png" alt="AWS SDK" style="zoom:80%;" />    
SDK를 사용하면 프로그래밍 언어 또는 플랫폼용으로 설계된 API를 통해 AWS 서비스를 보다 간편하게 사용할 수 있습니다. SDK를 통해 AWS 서비스를 기존 애플리케이션과 함께 사용하거나 AWS에서 실행할 완전히 새로운 애플리케이션을 생성할 수 있습니다. SDK는 C++, Java 등의 언어에서 제공합니다.   

### AWS Elastic Beanstalk
**AWS Elastic Beanstalk**은 **Amazon EC2를 기반으로 하는 환경을 프로비저닝**할 수 있게 지원하는 서비스입니다. Beanstalk 서비스에 애플리케이션 코드 그리고 원하는 구성을 제공하면 자동으로 환경이 구축됩니다.   
즉, 사용자가 코드 및 구성 설정을 제공하면 **Elastic Beanstalk**이 다음 작업을 수행하는 데 **필요한 리소스를 배포**합니다.   
① 용량 조정   
② 로드 밸런싱   
③ 자동 조정   
④ 애플리케이션 상태 모니터링

### AWS CloudFormation
우리가 **자동화되고 반복 가능한 배포**를 만드는 데 도움이 되는 또 다른 서비스는 **AWS CloudFormation**입니다. JSON 혹은 YAML 텍스트 기반 문서의 템플릿을 이용하여 AWS 리소스를 정의할 수 있습니다.   
CloudFormation을 이용하시면 원하는 항목을 사용자가 정의할 수가 있고 추후에 CloudFormation 엔진이라고 하는 것이 API 호출과 관련한 세부 사항들을 모두 처리해서 필요한 모든 것들을 구축해 줍니다.   
CloudFormation은 스토리지, 데이터베이스, 분석, 기계 학습 등의 굉장히 다양한 AWS 리소스를 지원합니다.   
동일한 CloudFormation 템플릿을 여러 계정이나 또 여러 리전에서 동시에 우리가 실행한다면 여러 계정 또는 리전에서 동일한 환경이 생성됩니다.   

<br>
정리해보겠습니다.   
**AWS Management Console**은 **사용자가 학습하기에 좋고 시각적 정보 확인**을 할 때는 굉장히 유용합니다. 하지만, 수동이라는 단접이 있습니다.   
CLI를 사용해서 터미널로 AWS와 상호작용하도록 스크립트를 작성하실 수가 있습니다. 혹은 SDK를 사용해서 AWS와의 상호작용에 대한 프로그램을 작성할 수도 있고 AWS Elastic Beanstalk 혹은 AWS CloudFormation과 같은 관리 도구를 사용하여 자동화를 구축할 수 있습니다.   
