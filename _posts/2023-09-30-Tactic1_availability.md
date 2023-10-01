---
layout: single
title:  "[소프트웨어아키텍처] 전술(1) - 가용성"
categories: Achitecture
toc: true
---

# 가용성
가용성이란 시스템, 네트워크, 애플리케이션 또는 서비스가 요청된 기능을 수행할 준비가 되어 있을 확률 또는 시간의 비율로 정의되는 품질 속성이다. 즉, 사용자가 시스템을 사용하려 할 때 시스템이 정상적으로 작동하고 응답하는 비율(= 정상 작동 시간/정상 작동 시간+ 다운 타임)로 생각할 수 있다. 
{: .notice--info}


## 1. 결함 탐지

### ping/echo
노드 사이에 교환되는 비동기적 요청를 보내고 응답 메시지를 수신한다.
핑/에코를 이용해서 네트워크 경로가 연결 됐음을 확인할 수 있다.
즉, 서비스 사용 가능성 및 round-trip delay(네트워크 지연 시간)을 결정하는 데 사용된다.

### monitor
다양한 시스템 일부(프로세서, 프로세스, 입출력, 메모리 등) 상태를 모니터링한다. 시스템 모니터는 보안 공격(ex. denial of service, 서비스 거부 공격)으로 발생된 네트워크의 실패를 탐지한다.

### heartbeat
시스템과 모니터링 대상 프로세스 간 주기적 메시지 교환으로 결함 없음을 탐지한다.

### time stamp
time stamp는 어떤 이벤트가 발생한 시간을 기록한 것입니다. 컴퓨터과학에서는, 데이터나 이벤트가 언제 생성되었는지를 알려주기 위해 타임 스탬프를 사용합니다.

분산 시스템에서는 여러 다른 시스템이나 컴퓨터가 서로 메시지를 주고받아서 일을 하게 된다. 이 때 네트워크 환경이나 외부 요소에 따라서 먼저 발생한 이벤트의 메시지를 늦게 받을 수도 있기 때문에 이벤트의 순서가 부정확할 수 있다. 이런 문제를 해결하기 위해서, 각 이벤트나 메시지에 발생 시간을 기록한 타임 스탬프를 붙여서 보냅니다. 그러면 메시지를 받은 시스템은 이 타임 스탬프를 보고 이벤트의 순서를 판단할 수 있다. 

### sanity checking
sanity checking(정상 검사)란 내부 설계 지식, 시스템 상태 정보를 기반으로 컴포넌트의 동작, 출력의 유효성을 검사하는 것을 말한다.

### condition monitoring
프로세스, 디바이스의 조건을 검사하거나 설계 시에 이루어진 가정을 검사한다.

### voting
여러 컴포넌트가 같은 결과를 산출하는 지 검사한다. 
<div class= "notice">
<h4> 방법 </h4>
<ui>
    <li> replication: </li> 
    컴포넌트를 복제하여 확인한다.
    <li> functional redundancy: </li> 
    같은 기능에 대해 서로 다른 알고리즘을 이용하여 결과 산출 & 이를 비교한다.
    <li> analytic redundancy: </li> 
    해당 기능에 대해 모델링(시뮬레이션)하여 결과를 비교한다.
</ui>
</div>

### exception detection
system exception, parameter fence(파라미터 범위값 검사), parameter typing(파라미터 타입 검사), timeout과 같은 정상 동작을 변경하는 조건을 탐지한다.

### self-test
컴포넌트가 정확한 동작을 수행하기 위해 스스로 테스트하는 구문을 실행한다.

## 2.1 결함 복구(준비와 보수)
### active redundancy(hot spare)
protection group 내 모든 node(active node and redundancy spare)는 병렬로 동일한 입력을 받아서 처리하고, redundancy spare는 active node와 동기적 상태를 유지하게 한다.

### passive redundancy(warm spare)
protection group 내 단 하나의 active만 입력을 받아서 처리하고, active는 redundancy spare로 하여금 주기적으로 상태 갱신을 수행한다.

### spare(cold spare)
protection group 내 redundancy spare는 fail-over가 발생하기 전 까지는 작동하지 않는다. 실패 복구 시점에 서비스를 대체하기 전에 redundancy spare에서 재시동 로직이 시작된다.

### exception handling
예외의 원인을 수정하고 재시도함으로써 결함을 복구한다.

### software update
"Non-service affection 방식으로 실행 코드 이미지에 in-service upgrade를 수행한다"
즉, 소프트웨어 업데이트가 실행되는 동안에도 현재 서비스에 영향을 주지 않으면서(=서비스가 중단되지 않고) 계속 운영되는 것을 의미한다. 이런 방식은 사용자에게 끊임없는 서비스를 제공함으로써 높은 사용자 만족도를 유지하고, 업데이트로 인한 장애나 중단 시간을 최소화하는 데 큰 도움을 준다.

<div class= "notice">
<h4> 예시 </h4>
<ui>
    <li> 클라우드 서비스 (예: Amazon Web Services, Google Cloud Platform): </li> 
    클라우드 서비스에서는 수많은 사용자가 동시에 다양한 서비스를 이용하고 있다. 이러한 환경에서는 시스템 업데이트가 필요할 때도 사용자의 작업에 방해가 되면 안된다. 따라서 클라우드 서비스 제공자들은 보통 롤링 업데이트 방식을 사용하여 차례대로 서버나 서비스를 업데이트하면서도 전체 서비스는 계속해서 유지될 수 있도록 한다.
    <li> 웹 서비스 (예: Facebook, Twitter): </li> 
    대규모 웹 서비스에서도 사용자가 서비스를 계속해서 이용할 수 있게 하면서 필요한 소프트웨어 업데이트를 수행해야 한다. 이때도 롤링 업데이트나 블루-그린 배포와 같은 기법을 활용하여 사용자에게는 중단이 느껴지지 않게 하면서도 서비스를 안정적으로 업데이트할 수 있다.
</ui>
</div>

롤링 업데이트나 블루-그린 배포에 대해 더 알고 싶다면 
[Click](../in-service_upgrade_tech/){: .btn .btn--primary .btn--large}

### retry

### ignore fautty behavior
메시지가 비정상(가짜)인 경우에 해당 메시지를 무시한다.

### degradation
컴포넌트가 실패할 때 가장 중요한 시스템 기능을 유지하고 덜 중요한 기능은 성능을 떨어뜨린다.

### reconfiguration
실패된 컴퓨터의 일을 다른 컴퓨터가 나눠가지도록 한다. 즉 작업을 재할당(=reconfiguration, 재설정)하여 회복 하도록 한다.

## 2-2 결함 복구(재가동)

### shadow
일반적으로 그림자(shadow) 컴포넌트(또는 프로세스, 서버)를 만들어 메인 컴포넌트의 작업을 병렬로 실행한다. 메인 컴포넌트에서 문제가 발생하면 그림자 컴포넌트가 해당 작업을 즉시 잡아서 서비스의 중단 없이 계속될 수 있도록 한다.

<div class= "notice">
<h4> 예시 </h4>
<ui>
    <li> 데이터베이스 미러링 (Database Mirroring): </li> 
    데이터베이스 미러링에서는 두 개의 동일한 데이터베이스 서버가 있으며, 하나는 주(primary) 서버로, 다른 하나는 미러(secondary) 서버로 작동합니다. 주 서버에서 트랜잭션이 발생할 때마다 해당 트랜잭션이 미러 서버로 복사됩니다. 만약 주 서버에 문제가 발생하면 미러 서버가 그 역할을 바로 잡아서 서비스의 중단 없이 데이터베이스 작업을 계속할 수 있다.
    <li> 핫 스탠바이 (Hot Standby): </li> 
    큰 서비스에서는 주 서버와 함께 핫 스탠바이라고 하는 백업 서버를 항상 실행 상태로 둘 수 있다. 이 백업 서버는 주 서버와 동일한 요청을 받아 처리하지만, 그 결과는 일반적으로 외부에는 공개되지 않는다. 주 서버에 문제가 발생하면 트래픽은 즉시 백업 서버로 전환되어 서비스 중단 없이 계속될 수 있다.
</ui>
</div>

### escalating restart
escalating restart(단계적 재시작)은 재시작하는 컴포넌트의 범위에 점진적으로 변화를 주어, 결함 복구에 대해 불편함을 최소화하면서도 효과적으로 복구하는 데 도움을 줍니다.

<div class= "notice">
<h4> 예시 </h4>
<ui>
    <li> [웹 서버 클러스터] 하나의 웹 서버 인스턴스에서 문제가 감지됐을 때: </li> 
        해당 인스턴스만을 재시작 > (문제 지속 시) 해당 웹 서버가 속한 서버 그룹 전체 재시작 > (문제 지속 시) 백엔드 서비스나 데이터베이스를 재시작 or 전체 웹 서버 클러스터 재시작
</ui>
</div>

### non-stop forwarding(NSF)
네트워크에서 활용되는 결함복구 전략이며, 주로 라우터에서 사용한다. 기능을 supervisory와 data plane 두 부분으로 분리한다. supervisory는 라우터의 '뇌'역할이고, data plane은 라우터의 '데이터 전송' 역할이다.  supervisory가 실패하면 프로토콜 정보가 복구/검증되기 전 까지 라우터는 알고있는 경로에 따라 패킷을 포워딩한다.

## 3. 결함 방지

### removal from service
잠재적인 시스템 실패를 완화시킬 목적으로 컴포넌트를 일시적으로 서비스 불능 상태로 만든다.

### transaction
상태 갱신을 묶어 분산 컴포넌트 사이에 교환되는 비동기적인 메시지가 ACID 속성을 갖도록 한다. 

<div class= "notice">
<h4> ACID 예시(은행 시스템에서 두 계좌 간의 이체 작업)</h4>
<ui>
    <li> 트랜잭션</li> 
    송금자의 계좌 출금, 수신자의 계좌 입금 이 두 단계는 하나의 트랜잭션으로 묶여야 합니다.
    <li> 원자성(Atomicity)</li> 
    출금과 입금 단계가 모두 성공하거나 모두 실패하도록 보장합니다. 중간 단계에서 실패할 경우 롤백(원상태로 복구)됩니다.
    <li> 일관성(Consistency)</li> 
    트랜잭션이 성공적으로 완료되면, 시스템은 항상 일관된 상태를 유지합니다. 예를 들어, 돈의 총량은 변하지 않습니다.
    <li> 고립성(Isolation)</li> 
    동시에 여러 트랜잭션이 진행되더라도, 각 트랜잭션은 서로 독립적으로 수행됩니다. 다른 트랜잭션의 중간 상태를 볼 수 없습니다.
    <li> 지속성(Durability)</li> 
    트랜잭션이 완료된 후, 그 결과는 영구적으로 저장됩니다. 시스템 장애가 발생하더라도, 완료된 트랜잭션의 결과는 보존됩니다.
</ui>
</div>

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

# 데이터베이스 연결 설정
engine = create_engine('sqlite:///example.db')
Session = sessionmaker(bind=engine)

# 세션 생성
session = Session()

try:
    # 연산1: 출금
    sender_account = session.query(Account).filter_by(id='12345').one()
    sender_account.balance -= 100
    
    # 연산2: 입금
    receiver_account = session.query(Account).filter_by(id='67890').one()
    receiver_account.balance += 100
    
    # 트랜잭션 커밋
    session.commit()

except Exception as e:
    # 예외 발생 시 롤백
    session.rollback()
    print(f"Transaction failed: {e}")

finally:
    # 세션 닫기
    session.close()
```

### predictive model
시스템의 상태를 모니터링하여 파라미터 값이 정상 범주인지 확인하고 향후 결함의 가능성이 예측되는 조건이 탐지될 때 교정하는 행위를 수행한다.

### exception prevention
결함을 감추거나 smart pointer, wapper를 통해 방지함으로써 시스템 예외가 발생하는 것을 방지한다.

### increase competence set
increase competence set(역량 집합 증가)는 시스템이 다양한 상황, 문제를 처리 하도록 능력을 확장하는 방식이다. 즉, 애초에 다양한 상황을 고려해서 처리하도록 설계하자는 전술이다.

