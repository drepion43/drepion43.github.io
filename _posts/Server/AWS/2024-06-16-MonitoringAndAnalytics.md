---
title:  "[AWS]Monitoring and Analytics"
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

# Monitoring and Analytics 소개

## 목표
① AWS 환경의 모니터링 방법을 요약할 수 있습니다.    
② Amazon CloudWatch의 이점을 설명할 수 있습니다.   
③ AWS CloudTrail의 이점을 설명할 수 있습니다.    
④ AWS Trusted Advisor의 이점을 설명할 수 있습니다.   

## Amazon CloudWatch
애플리케이션 운영에서는 사용자가 확인할 수 있도록 하는 **가시성**이 필요합니다.   
AWS에서는 이런 가시성확보를 위한 **Amazon CloudWatch** 기능을 제공해줍니다. Amazon CloudWatch은 **AWS 인프라**와 **AWS에서 실행하는 애플리케이션을 실시간으로 모니터링**할 수 있습니다.   
CloudWatch에서는 CloudWatch 임계값을 설정하여 **경보**라는 기능을 통해 사용자에게 알람을 보낼 수 있습니다.    
CloudWatch는 대시보드 기능을 통해 거의 **실시간으로 지표나 지표의 변동 상황을 그래프로 표시하는 웹 화면으로 제공**해줍니다.   

### Amazon CloudWatch
Amazon CloudWatch는 **다양한 지표를 모니터링 및 관리하고 해당 지표의 데이터를 기반으로 경보 작업을 구성할 수 있는 웹 서비스**입니다. 지표를 사용하여 **리소스의 데이터 포인트**를 나타냅니다.   

### CloudWatch 경보
CloudWatch를 사용하면 지표의 값이 **미리 정의된 임계값을 상회 또는 하회할 경우** 자동으로 작업을 수행하는 경보를 생성할 수 있습니다.    

### CloudWatch 대시보드
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-16-MonitoringAndAnalytics/cloudwatch dashboard.png" alt="cloudwatch dashboard" style="zoom:80%;" />    
CloudWatch 대시보드 기능을 사용하면 단일 위치에서 리소스에 대한 모든 지표에 액세스할 수 있습니다.    
별도의 대시보드를 사용자 지정할 수도 있습니다.   

## AWS CloudTrail
AWS CloudTrail은 포괄적인 API 감사 도구입니다.   
AWS에 대한 **모든 요청은 CloudTrail 엔진에 기록**이 됩니다.   
CloudTrail은 **로그를 안전한 S3 버킷에 무제한으로 저장**할 수 있습니다. 또한 S3 저장소 잠금 같은 변조 방지 기능을 사용하여 이러한 중요 보안 감사 로그의 절대적인 출처를 제시할 수 있습니다.   

### AWS CloudTrail
AWS CloudTrail은 계정에 대한 API 호출을 기록합니다. CloudTrail은 누군가 남긴 이동 경로(또는 작업 로그)의 ‘추적’으로 생각할 수 있습니다.   
CloudTrail을 사용하여 애플리케이션 및 리소스에 대한 사용자 활동 및 API 호출의 전체 내역을 볼 수 있습니다.   
CloudTrail을 사용하여 애플리케이션 및 리소스에 대한 사용자 활동 및 API 호출의 전체 내역을 볼 수 있습니다.   

#### AWS CloudTrail 이벤트 예시
커피숍 점주가 AWS 관리 콘솔의 AWS Identity and Access Management(IAM) 섹션을 탐색한다고 가정해 보겠습니다. 점주는 Mary라는 이름의 새 IAM 사용자가 생성된 것을 발견하지만 이 사용자를 만든 사람, 시기 또는 방법은 알 수 없습니다.   
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-16-MonitoringAndAnalytics/CloudTrail.png" alt="CloudTrail" style="zoom:80%;" />    
점주는 CloudTrail 이벤트 기록 섹션에서 IAM의 ‘CreateUser’ API 작업에 대한 이벤트만 표시하도록 필터를 적용합니다. 그 결과 Mary라는 IAM 사용자를 생성한 API 호출에 대한 이벤트를 찾았습니다. 이 이벤트 레코드는 발생한 작업에 대한 전체 세부 정보를 제공합니다.   

### CloudTrail Insights
CloudTrail에서 **CloudTrail Insights를 활성화**할 수도 있습니다.   
CloudTrail Insights는 **CloudTrail이 AWS 계정에서 비정상적인 API 활동을 자동으로 감지**할 수 있습니다.    

## AWS Trusted Advisor
AWS에서는 **AWS Trusted Advisor라고 하는 자동화된 조언 도구**가 있습니다. 사용자는 AWS 계정에서 5대 핵심 원칙을 기준으로 리소스를 평가해주는 이 서비스를 사용할 수 있죠. 이 다섯 가지 핵심 원칙은 **비용 최적화, 성능, 보안성, 내결함성, 그리고 서비스 한도**입니다.   
Trusted Advisor는 고객 계정에서 AWS 모범 사례를 기반으로 **각 핵심 원칙에 대한 일련의 검사를 실시간으로 실행**해줍니다. 이 결과는 결과는 **AWS 콘솔에서 여러분께서 직접 확인**하실 수 있습니다.

### AWS Trusted Advisor
AWS Trusted Advisor는 **AWS 환경을 검사하고 AWS 모범 사례에 따라 실시간 권장 사항을 제시하는 웹 서비스**입니다. Trusted Advisor는 비용 최적화, 성능, 보안, 내결함성, 서비스 한도라는 5개 범주에서 결과를 AWS 모범 사례와 비교합니다.   

#### AWS Trusted Advisor 대시보드
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-16-MonitoringAndAnalytics/Trusted Advisor Dashboard.jpg" alt="Trusted Advisor Dashboard" style="zoom:80%;" />    
AWS 관리 콘솔에서 Trusted Advisor 대시보드에 액세스하면 비용 최적화, 성능, 보안, 내결함성, 서비스 한도 범주에서 완료된 검사를 검토할 수 있습니다.   
   

① 녹색 체크 표시는 문제가 감지되지 않은 항목 수를 나타냅니다.   
② 주황색 삼각형은 권장 조사 항목 수를 나타냅니다.   
③ 빨간색 원은 권장 조치 수를 나타냅니다.