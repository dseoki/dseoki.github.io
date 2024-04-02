---
title: Infrastructure as Code
summary: Infrastructure as Code에 대해서 알아보자
categories: [Dev]
comments: true
---

## 코드형 인프라(Infrastructure as Code, IaC)
코드형 인프라(Infrastructure as Code)는 코드를 통해 인프라 설정을 자동화 하는 것을 말한다.\
IaC는 매번 동일한 환경을 프로비저닝하도록 보장하고, 구성 사양을 코드화하고 문서화 함으로써 구성 관리를 지원한다.

서버, 운영체제, 스토리지, 기타 인프라 구성 요소를 코드화 하여 템플릿을 만들고 프로비저닝할때 이 템플릿을 사용하면 된다.

### Infrastructure as Code 장점
* 인프라를 만드는 과정이 자동화되므로, 오류가 훨씬 덜 발생하고 안전하다.
* 쉽게 공유할 수 있고, 버전 관리에도 용이하다.
* 코드와 현재 상태를 비교하여, 추후 인프라 상태의 변경에 따르는 위험을 분석하고 검증할 수 있다.
* 배포 과정을 소수의 시스템 관리자만 진행하는 것이 아닌, 개발자 스스로가 배포하고 인프라를 통제할 수 있는 환경을 만들 수 있다.

### 프로비저닝
클라우드 서비스를 시작하고 구성하는 것을 말한다. 시스템, 데이터 및 소프트웨어로 서버를 준비하고 네트워크 작동을 준비한다.

### 배포
프로비저닝된 서버를 실행하기 위해 애플리케이션 버전을 제공하는 작업이다. 배포는 AWS CodePipeline, Jenkins, Github Actions를 통해 수행할 수 있다.

### 오케스트레이션
여러 시스템 또는 서비스를 조정하는 작업이다.\
마이크로서비스, 컨테이너 및 Kubernetes로 작업할 때 일반적인 용어이다.\
Kubernetes, Salt, Fabric가 있다.

## IaC 종류
IaC에 대한 접근 방식에는 절차형 방식과 선언형 방식 두 가지가 있다.

### 절차형 IaC
- 프로그래밍 언어를 이용해 직접 순차적으로 인프라를 생성하도록 코드 작성
- 선언형에 비해 더 강력한 일들을 할 수 있으나, 실제 적용된 결과를 가늠하기 어려움
- 코드를 읽기에 직관적이지 않음
- 종류: AWS CDK, Pulumi

### 선언형 IaC
- JSON, WAML 등 을 사용
- 실제 인프라가 적용된 결과와 적용할 내용이 직관적으로 매핑됨
- 종류:\
    AWS CloudFormation (AWS에서만 사용가능)\
    Azure Blueprint (Azure에서만 사용가능)\
    Cloud Deployment Manager (GCP에서만 사용가능)\
    **Terraform (어떤 클라우드 서비스에도 적용되는 범용 IaC)**


