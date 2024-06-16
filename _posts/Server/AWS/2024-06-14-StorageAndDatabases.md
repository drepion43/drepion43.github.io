---
title:  "[AWS]Storage and Databases"
categories: AWS
tag: [AWS, Cloud]
toc: true
author_profile: false
sidebar:
    nav: "docs"
use_math: true
excerpt: AWS Cloud Practitioner Essentials
comments: true
date: 2024-06-14
toc_sticky: true
---


# 출처
앞으로 정리할 내용은 [AWS Cloud Practitioner Essentials (Korean) (Na) (한국어 강의)](https://explore.skillbuilder.aws/learn/course/13522/play/107682/aws-cloud-practitioner-essentials-korean-na-hangug-eo-gang-ui)을 기반임을 밝힙니다.   

# Storage and Databases 소개

## 목표
① 스토리지 및 데이터베이스의 기본 개념을 요약할 수 있습니다.   
② Amazon Elastic Block Store(Amazon EBS)의 이점을 설명할 수 있습니다.   
③ Amazon Simple Storage Service(Amazon S3)의 이점을 설명할 수 있습니다.    
④ Amazon Elastic File System(Amazon EFS)의 이점을 설명할 수 있습니다.   
⑤ 다양한 스토리지 솔루션을 요약할 수 있습니다.   
⑥ Amazon Relational Database Service(Amazon RDS)의 이점을 설명할 수 있습니다.    
⑦ Amazon DynamoDB의 이점을 설명할 수 있습니다.   
⑧ 다양한 데이터베이스 서비스를 요약할 수 있습니다.

## 인스턴스 스토어 및 EBS(Elastic Block Store)
AWS EC2를 사용하면 CPU, 메모리, 네트워크, 스토리지에 액세스할 수 있습니다. 이번에는 **스토리지**에 대해 알아보겠습니다.   
대부분 스토리지는 블록 수준의 스토리지를 이용합니다. 파일들을 바이트 단위로 저장하며, 파일이 수정될시 해당 부분에만 덮어쓰기가 진행됩니다.   
EC2 인스턴스에도 하드디스크가 있습니다. EC2 인스턴스를 시작할 때, 인스턴스의 유형에 따라 인스턴스 스토어 볼륨이라고 하는 로컬 저장소를 제공합니다. 이 볼륨은 EC2 인스턴스가 실제로 실행되는 호스트에 물리적으로 연결이 되게 되고 일반 하드 디스크처럼 사용할 수 있습니다. 여기서 이 볼륨은 **물리적으로 직접 연결**이 되어있어 **EC2 인스턴스를 중지하거나 종료하면 인스턴스 스토어 볼륨에 작성한 모든 데이터가 삭제**됩니다. 그 이유는 EC2 인스턴스를 재시작하면 **다른 호스트에서 시작**될 수 있기 때문입니다. 즉, 휘발이 안되어야하는 중요한 데이터는 **인스턴스 스토어 볼륨에서 작성하면 안된다**입니다.    
**EBS**는 EBS 볼륨이라고 하는 **가상 하드 드라이브**를 만들고 그 드라이브를 **EC2 인스턴스에 연결**하여 **데이터가 휘발되는 것을 방지**할 수 있습니다.   

### 인스턴스 스토어
인스턴스 스토어는 물리적으로 EC2 인스턴스의 호스트 컴퓨터에 연결되어 있고, 따라서 인스턴스와 수명이 동일한 디스크 스토리지입니다. 인스턴스가 종료되면 인스턴스 스토어의 데이터가 손실됩니다.   

### EBS
EBS는 Amazon EC2 인스턴스에서 사용할 수 있는 블록 수준 스토리지 볼륨을 제공하는 서비스입니다. Amazon EC2 인스턴스를 중지 또는 종료하더라도 연결된 EBS 볼륨의 모든 데이터를 사용할 수 있습니다.   
EBS 볼륨을 생성하려면 구성(예: 볼륨 크기 및 유형)을 정의하고 볼륨을 프로비저닝합니다. EBS 볼륨을 생성한 다음 볼륨을 Amazon EC2 인스턴스에 연결할 수 있습니다.   
EBS 스냅샷은 **증분 백업**입니다. 따라서 이후의 백업에서는 가장 최근의 스냅샷 이후 변경된 데이터 블록만 저장됩니다.    

## S3(Simple Storage Service)
AWS의 S3는 스토리지 서비스입니다. S3는 **거의 무한대의 데이터를 저장하고 검색할 수 있는 데이터 저장소**입니다. **데이터를 객체로 저장**하고, **객체를 파일 디렉토리가 아닌 버킷에 저장**합니다. 업로드할 수 있는 최대 객체 크기는 **5TB**입니다.

### 객체 스토리지
<img src="../../../assets/images/AWS Cloud Practitioner/2024-06-14-StorageAndDatabases/object storage.png" alt="object storage" style="zoom:80%;" />    
객체 스토리지에서 각 객체는 데이터, 메타데이터, 키로 구성됩니다.   

### Amazon Simple Storage Service(Amazon S3)
S3는 객체 수준 스토리지를 제공하는 서비스입니다. S3는 데이터를 버킷에 객체로 저장합니다.   
S3에 저장할 수 있는 객체의 최대 파일 크기는 5TB입니다.   
S3에 파일을 업로드할 때 권한을 설정하여 파일에 대한 표시 여부 및 액세스를 제어할 수 있습니다.   
S3 버전 관리 기능을 사용하여 시간 경과에 따른 객체 변경 사항을 추적할 수도 있습니다.   

### Amazon S3 스토리지 클래스
S3에서는 사용한 만큼만 비용을 지불합니다. S3 스토리지 클래스를 선택할 때 **데이터를 검색할 빈도**와 **필요한 데이터 가용성**을 고려해야합니다.   
<br>
이번에는 S3 스토리지 클래스에 대해 알아보겠습니다.   
<span style='color:blue'>**① S3 standard**</span>    
**자주 액세스하는 데이터용으로 설계**   
**최소 3개의 가용 영역에 데이터를 저장**   
**객체에 대한 고가용성을 제공**   
   

<span style='color:blue'>**② S3 Standard-IA**</span>   
**자주 액세스하지 않는 데이터에 이상적**   
**S3 Standard와 비슷하지만 스토리지 가격은 더 저렴하고 검색 가격은 더 높음**   
**고가용성이 요구되는 데이터에 이상적**   
**최소 3개의 가용 영역에 데이터를 저장**   
   

<span style='color:blue'>**③ S3 One Zone-Infrequently Access(S3 One Zone-IA)**</span>   
**단일 가용 영역에 데이터를 저장**   
**S3 Standard-IA보다 낮은 스토리지 가격**   
따라서 **스토리지 비용을 절감하려는 경우** 또는 **가용 영역 장애가 발생할 경우 데이터를 손쉽게 재현할 수 있는 경우**에 용이함    
   

<span style='color:blue'>**④ S3 Intelligent-Tiering**</span>   
**액세스 패턴을 알 수 없거나 자주 변화하는 데이터에 이상적**   
**객체당 소량의 월별 모니터링 및 자동화 요금을 부과**   
객체의 액세스 패턴을 모니터링 하며, 30일간 객체를 액세스하지 않을 시, 자주 사용하지 않는 S3 Standard-IA로 이동시킴, 다시 이 객체를 액세스하면 자주 사용하는 액세스 계층인 S3 Standard로 이동시킴.   
   

<span style='color:blue'>**⑤ S3 Glacier Instant Retrieval**</span>    
**즉각적인 액세스가 필요한 아카이브 데이터에 적합**   
**몇 밀리초 만에 객체 검색 가능**   
보관된 객체를 얼마나 빨리 검색해야 하는지 고려할 때 용이   
   

<span style='color:blue'>**⑥ S3 Glacier Flexible Retrieval**</span>    
**데이터 보관용으로 설계된 저비용 스토리지**   
**객체를 몇 분에서 몇 시간 이내에 검색**   
데이터 보관에 이상적인 저비용 스토리지 클래스    
   

<span style='color:blue'>**⑦ S3 Glacier Flexible Retrieval**</span>   
**가장 저렴한 객체 스토리지 클래스로 보관에 적합**   
**12시간 이내에 객체를 검색**   
일 년에 한두 번 액세스되는 데이터의 장기 보존 및 디지털 보존을 지원   
AWS 클라우드에서 비용이 가장 저렴한 스토리지   
모든 객체가 최소한 3개의 지리적으로 분산된 가용 영역에 복제되고 저장    
   

<span style='color:blue'>**⑧ S3 Outposts**</span>   
**Amazon S3 Outposts에 S3 버킷을 생성**   
**AWS Outposts에서 더 쉽게 데이터를 검색, 저장 및 액세스**   
온프레미스 AWS Outposts 환경에 객체 스토리지를 제공   
여러 개의 디바이스와 서버에 데이터를 내구성이 높고 중복되게 저장하도록 설계   

### EBS와 S3 비교
- EBS
    - 최대 16TB
    - EC2 인스턴스가 종료되도 비휘발성
    - 기본적으로 SSD
    - HDD도 제공

- S3
    - 무제한 스토리지
    - 최대 5TB의 개별 객체
    - 한 번쓰기/ 여러번 읽기 : WORN
    - 내구성 99.999%

<span style='color:red'>**완성 객체를 사용하거나 변경 횟수가 적다면 S3를 사용해야 한다는 뜻입니다. 복잡한 읽기, 쓰기, 변경 기능을 수행한다면 승자는 당연히 EBS가 됩니다.**</span>   

## Amazon Elastic File System(Amazon EFS)
EFS는 **관리형 파일 시스템**입니다.   
기존 온프레미스의 경우, 현재 저장 중인 데이터 양을 스토리지가 계속적으로 감당할 수 있는지를 확인해야하며, 데이터에 대한 백업이 수행되었는지 데이터가 중복으로 저장되었는지를 확인하고 이 데이터를 호스팅하는 모든 서버를 관리해야합니다.   
하지만, EFS는 AWS에서 이 모든 작업을 맡길 수 있습니다.   
그럼 EBS와 EFS의 차이에 대해 알아보겠습니다.   
EBS는 EC2 인스턴스에 연결이 되고 가용 영역 수준의 리소이며, 같은 AZ에 있어야 합니다. 또한, EBS는 자동으로 규모를 조정하고 추가적인 스토리지를 제공하지는 않습니다.   
그럼 EFS는 자동으로 규모를 조정을 시켜주면서도 동시에 여러 EC2 인스턴스가 읽기 혹은 쓰기 작업을 진행할 수가 있습니다. 하지만, EFS는 빈 하드 드라이브아닙니다. 또한, EFS는 리전 기반의 리소스입니다.   

### 파일 스토리지
여러 클라이언트(예: 사용자, 애플리케이션, 서버 등)가 공유 파일 폴더에 저장된 데이터에 액세스할 수 있습니다.  이 접근 방식에서는 스토리지 서버가 블록 스토리지를 로컬 파일 시스템과 함께 사용하여 파일을 구성합니다. 클라이언트는 파일 경로를 통해 데이터에 액세스합니다.   
블록 스토리지 및 객체 스토리지와 비교하면, 파일 스토리지는 많은 수의 서비스 및 리소스가 동시에 동일한 데이터에 액세스해야 하는 사용 사례에 이상적입니다.   
EFS는 AWS 클라우드 서비스 및 온프레미스 리소스와 함께 사용되는 확장 가능한 파일 시스템입니다. 파일을 추가 또는 제거하면 Amazon EFS가 자동으로 확장하거나 축소됩니다.    

### EBS와 EFS 비교
- EBS
    - 단일 가용 영역에 데이터를 저장
    - EC2 인스턴스를 EBS 볼륨에 연결하려면 Amazon EC2 인스턴스와 EBS 볼륨 모두 동일한 가용 영역에 상주해야함.

- EFS
    - 리전별 서비스
    - 여러 가용 영역에 걸쳐 데이터를 저장
    - 중복 스토리지를 사용하면 파일 시스템이 위치한 리전의 모든 가용 영역에서 동시에 데이터에 액세스할 수 있음
    - 온프레미스 서버는 AWS Direct Connect를 사용하여 Amazon EFS에 액세스할 수 있음

## Amazon Relational Database Service(Amazon RDS)
다양한 유형의 데이터 간의 관계를 유지해야 할 때, **RDBMS**을 사용해야 합니다.   
AWS에서는 MySQL, PostgreSQL, Oracle, Microsoft SQL Server 등을 지원합니다. 근데 온프레미스에서 실행중인 데이터를 AWS로 옮길 때 **리프트 앤 시프트라는 작업**을 이용하면 데이터베이스를 마이그레이션하여 EC2에서 실행시킬 수 있습니다.    
클라우드에서 데이터베이스를 실행하는 다른 방법은 Amazon Relational Database Service(RDS)가 있습니다. RDS의 이점은 일반적으로 사용자가 관리해야 하는 패치 적용, 백업, 이중화, 장애 조치, 재해 복구를 **자동으로 진행**한다는 점입니다.   
AWS는 클라우드에서 데이터베이스 워크로드를 실행할 수 있게 해주는 Amazon Aurora을 지원합니다. Amazon Aurora는 MySQL과 Oracle과의 호환성이 높은 PostgreSQL 이 두 가지 형태로 제공됩니다. 가격 또한 10분의 1입니다. 또한 데이터가 세 개의 시설에서 두 개씩 복제되기 때문에 항상 복제본 6개가 존재한다는 이점도 있으며 읽기 작업만을 수행하는 읽기 전용 복제본을 15개까지 배포할 수 있어 읽기 및 규모 조정 성능을 오프로드할 수 있습니다. S3로의 지속적 백업도 제공합니다.    
### 관계형 데이터베이스
관계형 데이터베이스는 정형 쿼리 언어(SQL)를 사용하여 데이터를 저장하고 쿼리합니다.   
### Amazon Relational Database Service
AWS RDS는 AWS 클라우드에서 관계형 데이터베이스를 실행할 수 있는 서비스입니다. Amazon RDS는 **하드웨어 프로비저닝, 데이터베이스 설정, 패치 적용 백업과 같은 작업을 자동화하는 관리형 서비스**입니다. Amazon RDS를 다른 서비스와 통합하면 AWS Lambda를 사용하여 서버리스 애플리케이션에서 데이터베이스를 쿼리하는 등 비즈니스 및 운영 요구 사항을 충족할 수 있습니다.   
또한, Amazon RDS는 보안옵션도 제공하며 저장 시 암호화(데이터가 저장되는 동안 데이터를 보호) 및 전송 중 암호화(데이터를 전송 및 수신하는 동안 데이터를 보호)를 제공합니다.   

### Amazon RDS 데이터베이스 엔진
Amazon Aurora, PostgreSQL, MySQL, MariaDB, Oracle Database, Microsoft SQL Server를 지원합니다.   
### Amazon Aurora
Amazon Aurora는 엔터프라이즈급 관계형 데이터베이스입니다. Aurora는 MySQL 및 PostgreSQL 관계형 데이터베이스와 호환됩니다. 또한, MySQL보다 5배 빠르며, PostgreSQL보다도 3배 빠릅니다. 데이터베이스 리소스의 신뢰성 및 가용성을 유지하면서도 불필요한 입/출력(I/O) 작업을 줄여 데이터베이스 비용을 절감합니다.   
워크로드에 고가용성이 필요한 경우 Amazon Aurora를 고려하는 것이 좋습니다. Aurora는 6개의 데이터 복사본을 3개의 가용 영역에 복제하고 지속적으로 Amazon S3에 데이터를 백업합니다.   

## Amazon DynamoDB
Amazon DynamoDB는 **서버리스 데이터베이스**이기 때문에, 인스턴스나 인프라를 관리할 필요가 없이 사용할 수가 있습니다.   
DynamoDB에는 **테이블**이 있는데, 이는 데이터를 저장하고 쿼리하는 곳입니다. DynamoDB는 NoSQL로 **유동적이고 아주 빠른 속도로 액세스**되어야 하는 데이터에서는 적합합니다.   

### 비관계형 데이터베이스
비관계형 데이터베이스에서는 테이블을 생성합니다. 테이블은 데이터를 저장하고 쿼리할 수 있는 장소입니다. 비관계형 데이터베이스의 구조적 접근 방식 중 한 유형은 키-값 페어입니다. 키-값 페어에서는 데이터가 항목(키)으로 구성되고 항목은 속성(값)을 갖습니다. 속성을 데이터의 여러 기능으로 생각할 수 있습니다.   

### Amazon DynamoDB
Amazon DynamoDB는 키-값 데이터베이스 서비스입니다. 모든 규모에서 한 자릿수 밀리초의 성능을 제공합니다.
DynamoDB는 **서버리스**이므로 서버를 프로비저닝, 패치 적용 또는 관리할 필요가 없습니다. 또한 **소프트웨어를 설치, 유지 관리, 운영할 필요도 없습니다.**    
또한, DynamoDB는 데이터베이스 크기가 **축소 또는 확장되면 DynamoDB는 용량 변화에 맞춰 자동으로 크기를 조정**하면서도 일관된 성능을 유지해줍니다.    

### RDS vs DynamoDB
- Amazon RDS
    - 자동 고가용성, 복구 제공
    - 고객이 데이터 소유
    - 고객이 스키마 소유
    - 고객이 네트워크 제어

- DynamoDB
    - 키-값 사용
    - 대규모 처리량 기능
    - 페타바이트 크기 확장 가능
    - 세분화된 API 액세스

## Amazon Redshift
Amazon Redshift는 **빅데이터 분석에 사용할 수 있는 데이터 웨어하우징 서비스**입니다. 이 서비스는 여러 원본에서 데이터를 수집하여 데이터 간의 관계 및 추세를 파악하는 데 도움이 되는 기능을 제공합니다.   

## AWS Database Migration Service
DMS(Database Migration Service)는 기존 데이터베이스를 안전하고 쉽게 **AWS로 마이그레이션**할 수 있는 서비스입니다.   
마이그레이션 중에도 **원본 데이터베이스가 정상적으로 동작**하므로 데이터베이스에 의존하는 애플리케이션의 **다운타임이 최소화**된다는 점입니다.   
우선 마이그레이션에서 동종 마이그레이션은 같은 유형의 데이터베이스로 마이그레이션을 하는 것입니다. **스키마 구조, 데이터 유형, 데이터베이스 코드가 원본과 대상 사이에서 호환**되기 때문에 매우 간단합니다.   
이번에는 **이종 마이그레이션**입니다. 크게 2단계로 진행됩니다. 첫번째는 원본과 대상의 스키마 구조, 데이터 유형, 데이터베이스 코드가 다르므로 먼저 Amazon Schema Conversion Tool, 줄여서 **SCT라고 하는 툴을 사용해서 변환**해야 합니다. 그 후 2단계인 **DMS를 사용하여 원본 데이터베이스의 데이터를 대상 데이터베이스로 마이그레이션**하는 것입니다.    

### AWS Database Migration Service(AWS DMS)
관계형 데이터베이스, 비관계형 데이터베이스 및 기타 유형의 데이터 저장소를 마이그레이션할 수 있는 서비스입니다.   
마이그레이션하는 동안 원본 데이터베이스가 계속 작동하므로 데이터베이스를 사용하는 애플리케이션의 가동 중지 시간을 줄일 수 있습니다.    

① 개발 및 테스트 데이터베이스 마이그레이션   
프로덕션 사용자에게 영향을 주지 않고 개발자가 프로덕션 데이터에서 애플리케이션을 테스트할 수 있도록 지원   


② 데이터베이스 통합   
여러 데이터베이스를 단일 데이터베이스로 결합   


③ 연속 복제   
일회성 마이그레이션을 수행하는 것이 아니라 데이터의 진행 중 복제본을 다른 대상 원본으로 전송   

## 추가 데이터베이스 서비스

<span style='color:blue'>**① Amazon DocumentDB**</span>    
Amazon DocumentDB는 MongoDB 워크로드를 지원하는 문서 데이터베이스 서비스입니다.   


<span style='color:blue'>**② Amazon Neptune**</span>   
그래프 데이터베이스 서비스입니다.    
Amazon Neptune을 사용하여 추천 엔진, 사기 행위 탐지, 지식 그래프와 같이 고도로 연결된 데이터 세트로 작동하는 애플리케이션을 빌드하고 실행할 수 있습니다.   
   

<span style='color:blue'>**③ Amazon QLDB**</span>   
Amazon Quantum Ledger Database(Amazon QLDB)는 원장 데이터베이스 서비스입니다.    
Amazon QLDB를 사용하여 애플리케이션 데이터에 발생한 모든 변경 사항의 전체 기록을 검토할 수 있습니다.   
   

<span style='color:blue'>**④ Amazon Managed Blockchain**</span>   
Amazon Managed Blockchain은 오픈 소스 프레임워크를 사용하여 블록체인 네트워크를 생성하고 관리하는 데 사용할 수 있는 서비스입니다.    
Blockchain은 여러 당사자가 중앙 기관 없이 거래를 실행하고 데이터를 공유할 수 있는 분산형 원장 시스템입니다.
   
<span style='color:blue'>**⑤ Amazon ElastiCache**</span>    
Amazon ElastiCache는 자주 사용되는 요청의 읽기 시간을 향상시키기 위해 데이터베이스 위에 캐싱 계층을 추가하는 서비스입니다.   
이 서비스는 두 가지 데이터 스토어 Redis 및 Memcached를 지원합니다.   
   

<span style='color:blue'>**⑥ Amazon DynamoDB Accelerator**</span>    
Amazon DynamoDB Accelerator(DAX)는 DynamoDB용 인 메모리 캐시입니다.   
응답 시간을 한 자릿수 밀리초에서 마이크로초까지 향상시킬 수 있습니다.   


