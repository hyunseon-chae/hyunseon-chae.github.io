---
layout: single
title:  "AWS EC2 인스턴스 생성"
categories: GAN
---


# AWS EC2 인스턴스 생성





> **목적 : 비용이 덜 드는 방향으로 개발환경 준비**
>
>  GPU , 운영체제, CPU, RAM을 전부 다 구매하려면 비용은 만만치 않다. ㅠ_ㅠ
>
> 따라서 이번 시간에는 아마존 웹서비스를 활용하여 개발 환경을 셋팅해볼 것이다.





----

### 1. 아마존 웹서비스 (Amazon Web Services; AWS)






- **아마존 웹 서비스**는 클라우드 컴퓨팅 사업부다.  
- 다른 웹 사이트나 클라이언트측 응용 프로그램에 대해 온라인 서비스를 제공하고 있다. 



---



### 2.  Amagon EC2 (Amazon Elastic Compute Cloud)



- 클라우드 컴퓨팅 플랫폼

- 사용자가 가상 컴퓨터를 임대 받아서 그 위에 자신만의 컴퓨터 어플리케이션을 실행할 수 있게 한다.

- 아마존 머신 이미지(AMI)로 부팅하여 인스턴스라 부르는 가상머신을 구성할 수 있게하는 웹서비스를 제공한다.

  

---



### 3. EC2 생성 실습



> **개발 환경** 
>
> - GPU: 10 시리즈 CUDA 지원 엔비디아 GPU
> - 운영체제: ubuntu linux 16.04
> - CPU: 펜티엄 i5 or i7
> - RAM: 8G



#### 3.1 회원 가입

#####   < 주의사항 >

1. 비밀번호는 반드시 보안 높은 것으로 설정
2. 회원가입 후에는 Multi-Factor Authentication (MFA) 를 사용하여 2중 로그인



#### 3.2  vCPU 제한 사항 확인

무작정 EC2를 생성하고 사용하려 하니까 아래의 에러가 나온다. 

> *You have requested more vCPU capacity than your current vCPU limit of 0*



내가 원하는 사양 (p2.xlarge)를 사용하기 위해서는 vCPU가 적어도 4개 이상 되어야 한다는 것을 깨달았다.

문의 방법은 아래와 같다.

1. Amargon Support Center 클릭
2. 내용 작성 (p2.xlarge 쓰고 싶으니 vCPU제한을 4개로 올려달라 어쩌구~ 작성)
3. 아마존측 메일 확인(며칠 걸림)
4. 적용됨을 확인



#### 3.3 EC2 인스턴스 생성

1. AWS - EC2 클릭

2. 인스턴스 시작 클릭

   - 단계 1: Amazon Machine Image(AMI) 선택
     - ubuntu 16.04 선택(Deep Learning AMI (Ubuntu 18.04) Version 43.0)
   - 단계 2: 인스턴스 유형 선택
     - p2.xlarge 선택

   - 단계 3,4,5 pass
   - 단계 6: 보안그룹 구성
     - 새 보안 그룹 생성
     - 유형: ssh, 프로토콜: tcp, 포트 범위: 22, 소스: 내IP, 설명: ~
   - 검토 및 시작 클릭

3. 생성 완료 확인!!!!!



---



다음 포스팅에서는 생성한 우분투 인스턴스에 GUI 방식으로 접속하는 방법을 작성할 것입니다.

그럼 안녀어엉~ 
