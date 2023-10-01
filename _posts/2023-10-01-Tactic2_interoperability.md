---
layout: single
title:  "[소프트웨어아키텍처] 전술(2) - 상호운용성"
categories: Achitecture
toc: true
---

# 상호운용성
하나의 시스템이 동일 또는 이기종의 다른 시스템과 아무런 제약이 없이 서로 호환되어 사용할 수 있는 속성을 의미한다.
{: .notice--info}

## 1. 위치

### discover service
공개된 디렉터리 서비스를 검색하여 원하는 서비스의 위치를 찾는다. 이렇게 위치를 검색하는 방식을 통해 시스템이나 서비스는 동적으로 다른 서비스를 찾아 상호 작용 할 수 있다.

## 2. 인터페이스 관리

### orchestrate
통제 메커니즘을 사용하여 서비스의 호출을 조정하고 관리하며 순서를 정한다. 시스템이 복잡한 작업을 수행하기 위해 복잡한 방식으로 상호작용해야만 할 때 orchestration이 사용된다.

예를 들어, 온라인 쇼핑몰에서 주문을 처리하는 과정에서 고객이 주문을 하면, 재고 확인, 결제 처리, 배송 준비 등 다양한 서비스가 연속적으로 호출되어야 한다. 이때 "Orchestrate" 전략을 사용하면, 이러한 서비스들 사이의 상호작용과 순서를 정확하게 관리하고 조정할 수 있다.

<div class= "notice">
<h4> 예시(넷플릭스 오픈소스 Conductor의 Workflow Orchestration Engine 기능) </h4>
<ui>
    <li> Conductor 요약 </li> 
Conductor는 비즈니스 프로세스를 실행하기 위해서 사전에 전체 작업 순서가 정의된 workflow와 개별 작업이 정의된 task를 conductor 서버에 REST API 방식으로 등록해서 사용한다. task는 하나의 마이크로 서비스가 처리하는 작업 단위로, 처리할 작업을 사용자가 마이크로 서비스(Micro Service)에 구현해야 한다. (각 마이크로 서비스는 단일 기능에 대한 책임을 갖는다)
    <li> 기능 </li> 
프로세스 일시 중지/재개/재시작 기능, workflow 추적 및 관리, 프로세스 흐름을 시각화하기 위한 사용자 인터페이스, HTTP, gRPC와 같은 여러 프로토콜 전송 기능을 포함한다.
    <li> 내용 출처 </li> 
https://luniverse.io/workflow-engine/?lang=ko
</ui>
</div>

### tailor interface
translation, buffering, data-smoothing과 같이 인터페이스에 기능을 추가 혹은 삭제한다. 이를 통해 서로 다른 시스템 간 데이터 교환을 원활하게 한다.

```markdown
- translation :
데이터 포맷 변환(ex XML <->JSON)
- buffering :
데이터 전송 속도 보완
- data-smoothing :
데이터 불규칙성을 줄이기 위해 시계열 데이터 이상치/노이즈 제거 
```
