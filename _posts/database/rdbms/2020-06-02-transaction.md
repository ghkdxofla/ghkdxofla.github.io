---
layout: post
title:  "트랜잭션이란"
categories: [Database, RDBMS]
shortinfo: "Back-end 개발을 하면서 필수적으로 다루게 되는 부분이 있다. 바로 트랜잭션인데..."
tags: [개발, Database, RDBMS, Transaction, ACID]
comments: true
---

### 트랜잭션?

업무 처리를 위한 논리적인 작업 단위(논리적 단위가 단일 연산이 아닐 수 있음)

이는 곧 *"A 업무 후 B 업무를 수행"* 자체가 트랜잭션 단위가 될 수 있음을 의미한다

트랜잭션의 특징으로는 보통 *"ACID"*라 불리우는 4가지 특성이 있다

- 원자성(Atomicity)   
  트랜잭션은 더 이상 분해가 불가능한 업무의 최소단위이므로, 전부 처리되거나 아예 하나도 처리되지 않아야 한다(All or Nothing)

- 일관성(Consistency)   
  일관된 상태의 데이터베이스에서 하나의 트랜잭션을 성공적으로 완료하고 나면 그 데이터베이스는 여전히 일관된 상태여야 한다   
  즉, 트랜잭션 실행의 결과로 데이터베이스 상태가 모순되지 않아야 한다

- 격리성(Isolation)   
  실행 중인 트랜잭션의 중간결과를 다른 트랜잭션이 접근할 수 없다

- 영속성(Durability)   
  트랜잭션이 일단 그 실행을 성공적으로 완료하면 그 결과는 데이터베이스에 영속적으로 저장된다

### 트랜잭션의 격리성

트랜잭션의 격리성은, 일관성과 마찬가지로 Lock을 강하게 오래 유지할수록 강화되고, Lock을 최소화할수록 약화된다

Lock에 대해서는 다른 포스팅 글을 게시할 예정이다

- 낮은 단계의 격리성 수준에서 발생할 수 있는 현상들   
  1. Dirty Read   
  다른 트랜잭션에 의해 수정됐지만 아직 커밋되지 않은 데이터를 읽는 것을 말한다   
  변경 후 아직 커밋되지 않은 값을 읽었는데 변경을 가한 트랜잭션이 최종적으로 롤백된다면   
  그 값을 읽은 트랜잭션은 비일관된 상태에 놓이게 된다

  2. Non-Repeatable Read   
  한 트랜잭션 내에서 같은 쿼리를 두 번 수행했는데, 그 사이에 다른 트랜잭션이 값을 수정 또는 삭제하는 바람에   
  두 쿼리 결과가 다르게 나타나는 현상을 말한다

  ![Non-Repeatable Read](/assets/media/SQL_277.jpg)

  위의 그림에서 t1 시점에 123번 계좌번호의 잔고는 55,000원이었다고 가정하자   
  ①번 쿼리를 통해 자신의 계좌에 55,000원이 남아 있음을 확인하고 t4 시점에 10,000원을 인출하려는데,   
  중간에 TX2 트랜잭션에 의해 이 계좌의 잔고가 5,000원으로 변경되었다   
  그러면 TX1 사용자는 잔고가 충분한 것을 확인하고 인출을 시도했음에도 불구하고 잔고가 부족하다는 메시지를 받게 된다

  3. Phantom Read
  한 트랜잭션 내에서 같은 쿼리를 두 번 수행했는데, 첫 번째 쿼리에서 없던 유령(Phantom) 레코드가 두 번째 쿼리에서 나타나는 현상을 말한다

  ![Phantom Read](/assets/media/SQL_278.jpg)

  위의 그림에서 TX1 트랜잭션이 지역별고객과 연령대별고객을 연속해서 집계하는 도중에 새로운 고객이 TX2 트랜잭션에 의해 등록되었다   
  그 결과, 지역별고객과 연령대별고객 두 집계 테이블을 통해 총고객수를 조회하면 서로 결과 값이 다른 상태에 놓이게 된다

# 정리 필요

나. 트랜잭션 격리성 수준
ANSI/ISO SQL 표준(SQL92)에서 정의한 4가지 트랜잭션 격리성 수준(Transaction Isolation Level)은 다음과 같다.

Read Uncommitted
트랜잭션에서 처리 중인 아직 커밋되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용한다
Read Committed
트랜잭션이 커밋되어 확정된 데이터만 다른 트랜잭션이 읽도록 허용함으로써 Dirty Read를 방지해준다. 커밋된 데이터만 읽더라도 Non-Repeatable Read와 Phantom Read 현상을 막지는 못한다. 읽는 시점에 따라 결과가 다를 수 있다는 것이다. 한 트랜잭션 내에서 쿼리를 두 번 수행했는데 두 쿼리 사이에 다른 트랜잭션이 값을 변경/삭제하거나 새로운 레코드를 삽입하는 경우로서, [그림 Ⅲ-2-1]과 [그림 Ⅲ-2-2]에서 TX1 트랜잭션을 참조하기 바란다.
Repeatable Read
트랜잭션 내에서 쿼리를 두 번 이상 수행할 때, 첫 번째 쿼리에 있던 레코드가 사라지거나 값이 바뀌는 현상을 방지해 준다. 이 트랜잭션 격리성 수준이 Phantom Read 현상을 막지는 못한다. 첫 번째 쿼리에서 없던 새로운 레코드가 나타날 수 있다는 것이다. 한 트랜잭션 내에서 쿼리를 두 번 수행했는데 두 쿼리 사이에 다른 트랜잭션이 새로운 레코드를 삽입하는 경우로서, [그림 Ⅲ-2-2]에서 TX1 트랜잭션을 참조하기 바란다.
Serializable Read
트랜잭션 내에서 쿼리를 두 번 이상 수행할 때, 첫 번째 쿼리에 있던 레코드가 사라지거나 값이 바뀌지 않음은 물론 새로운 레코드가 나타나지도 않는다.
sql가이드
트랜잭션 격리성 수준은 ISO에서 정한 분류 기준일 뿐이며, 모든 DBMS가 4가지 레벨을 다 지원하지는 않는다. 예를 들어, SQL Server와 DB2는 4가지 레벨을 다 지원하지만 Oracle은 Read Committed와 Serializable Read만 지원한다. (Oracle에서 Repeatable Read를 구현하려면 for update 구문을 이용하면 된다.) 대부분 DBMS가 Read Committed를 기본 트랜잭션 격리성 수준으로 채택하고 있으므로 Dirty Read가 발생할까 걱정하지 않아도 되지만, Non-Repeatable Read, Phantom Read 현상에 대해선 세심한 주의가 필요하다. 그런 현상이 발생하지 않도록 DBMS 제공 기능을 이용할 수 있지만, 많은 경우 개발자가 직접 구현해 주어야 하기 때문이다. 다중 트랜잭션 환경에서 DBMS가 제공하는 기능을 이용해 동시성을 제어하려면 트랜잭션 시작 전에 명시적으로 Set Transaction 명령어를 수행하기만 하면 된다. 아래는 트랜잭션 격리성 수준을 Serializable Read로 상향 조정하는 예시다.

set transaction isolation level read serializable;
트랜잭션 격리성 수준을 Repeatable Read나 Serializable Read로 올리면 ISO에서 정한 기준을 만족해야 하며, 대부분 DBMS가 이를 구현하기 위해 Locking 메커니즘에 의존한다. 좀 더??까지 유지하는 방식을 사용한다. 앞서 보았던 [그림 Ⅲ-2-1]를 예로 들어, TX1 트랜잭션을 Repeatable Read 모드에서 실행했다고 하자. 그러면 t1 시점에 ①번 쿼리에서 설정한 공유 Lock을 t6 시점까지 유지하므로 TX2의 ②번 update는 t6 시점까지 대기해야 한다. 문제는 동시성이다. [그림 Ⅲ-2-1]처럼 한 건씩 읽어 처리할 때는 잘 느끼지 못하는 수준이겠지만, 대량의 데이터를 읽어 처리할 때는 동시성이 심각하게 나빠진다. 완벽한 데이터 일관성 유지를 위해 심지어 테이블 레벨 Lock을 걸어야 할 때도 있다. 이에 대한 대안으로 다중버전 동시성 제어(Multiversion Concurrency Control)을 채택하는 DBMS가 조금씩 늘고 있다. ‘스냅샷 격리성 수준(Snapshot Isolation Level)’이라고도 불리는 이 방식을 한마디로 요약하면, 현재 진행 중인 트랜잭션에 의해 변경된 데이터를 읽고자 할 때는 변경 이전 상태로 되돌린 버전을 읽는 것이다. 변경이 아직 확정되지 않은 값을 읽으려는 것이 아니므로 공유 Lock을 설정하지 않아도 된다. 따라서 읽는 세션과 변경하는 세션이 서로 간섭현상을 일으키지 않는다. [그림 Ⅲ-2-2]를 예로 들면, TX2 트랜잭션에 의해 새로운 고객이 등록되더라도 TX1은 트랜잭션은 그 값을 무시한다. 트랜잭션 내내 자신이 시작된 t1 시점을 기준으로 읽기 때문에 데이터 일관성은 물론 높은 동시성을 유지할 수 있다.
  

### 참조 링크
[Lock과 트랜잭션 동시성 제어](http://www.dbguide.net/db.db?cmd=view&boardUid=148216&boardConfigUid=9&categoryUid=216&boardIdx=138&boardStep=1)