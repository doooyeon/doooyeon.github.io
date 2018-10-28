---
layout: post
title: 트랜잭션 격리 수준(Isolation Level)
tags: [트랜잭션, 트랜잭션 격리 수준, transaction, Isolation Level]
---
> 트랜잭션 격리 수준과 그에 따른 현상을 이해한다.
<!--more-->

### 트랜잭션 격리 수준(Isolation Level)이란
* 트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준

### 트랜잭션 격리 수준(Isolation Level)의 필요성
* 데이터베이스는 ACID 같이 트랜잭션이 원자적이면서도 독립적인 수행을 하도록 한다.
* 그래서 Locking 이라는 개념이 등장한다.
    * 트랜잭션이 DB를 다루는 동안 다른 트랜잭션이 관여하지 못하게 막는 것
* 하지만 무조건적인 Locking으로 동시에 수행되는 많은 트랜잭션들을 순서대로 처리하는 방식으로 구현되면 DB의 성능은 떨어지게 된다.
* 반대로 응답성을 높이기 위해 Locking 범위를 줄인다면 잘못된 값이 처리 될 여지가 있다.
* 그래서 최대한 효율적인 Locking 방법이 필요하다.

### 트랜잭션 격리 수준(Isolation Level)의 종류
* Isolation level 조정은 동시성이 증가되는데 반해 데이터 무결성에 문제가 발생할 수 있고, 데이터의 무결성을 유지하는 데 반해 동시성이 떨어질 수 있다.
* 레벨이 높아질수록 비용이 높아진다.

#### 1. Read Uncommitted (레벨 0)
* SELECT 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸리지 않는 Level
* 트랜잭션에 처리중인 혹은 아직 커밋되지 않은 데이터를 다른 트랜잭션이 읽는 것을 허용한다.
* 따라서, 어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 아직 완료되지 않은(Uncommitted 혹은 Dirty) 트랜잭션이지만 변경된 데이터인 B를 읽을 수 있다.
* 데이터베이스의 일관성을 유지할 수 없다.

#### 2. Read Committed (레벨 1)
* SELECT 문장이 수행되는 동안 해당 데이터에 Shared Lock이 걸리는 Level
* 트랜잭션이 수행되는 동안 다른 트랜잭션이 접근할 수 없어 대기하게 된다.
* Commit이 이루어진 트랜잭션만 조회할 수 있다.
* 따라서, 어떤 사용자가 A라는 데이터를 B라는 데이터로 변경하는 동안 다른 사용자는 해당 데이터에 접근할 수 없다.
* SQL Server가 Default로 사용하는 Level

#### 3. Repeatable Read (레벨 2)
* 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리는 Level
* 트랜잭션이 범위 내에서 조회한 데이터의 내용이 항상 동일함을 보장한다.
* 따라서, 다른 사용자는 그 영역에 해당되는 데이터에 대한 수정이 불가능하다.

#### 4. Serializable (레벨 3)
* 트랜잭션이 완료될 때까지 SELECT 문장이 사용하는 모든 데이터에 Shared Lock이 걸리는 Level
* 완벽한 읽기 일관성 모드를 제공한다.
* 따라서, 다른 사용자는 그 영역에 해당되는 데이터에 대한 수정 및 입력이 불가능하다.

### 낮은 단계의 트랜잭션 격리 수준(Isolation Level) 이용시 발생하는 현상
<img src="{{ site.baseurl }}/assets/img/post/isolation-level.png" width="90%" height="90%">

#### 1. Dirty Read
* 커밋되지 않은 수정 중인 데이터를 다른 트랜잭션에서 읽을 수 있도록 허용할 때 발생하는 현상
* 어떤 트랜잭션에서 아직 실행이 끝난지 않은 다른 트랜잭션에 의한 변경 사항을 보게 되는 되는 경우

#### 2. Non-Repeatable Read
* 한 트랜잭션에서 같은 쿼리를 두 번 수행할 때, 두 쿼리의 결과가 상이하게 나타나는 비 일관성 현상
* 한 트랜잭션이 수행중일 때 다른 트랜잭션이 값을 수정 또는 삭제함으로써 나타난다.

#### 3. Phantom Read
* 한 트랜잭션에서 같은 쿼리를 두 번 수행할 때, 첫 번째 쿼리에서 없던 레코드가 두 번째 쿼리에서 나타나는 현상
* 한 트랜잭션이 수행중일 때 다른 트랜잭션이 새로운 레코드가 삽입함으로써 나타난다.

<br>

---
### Reference
- [http://hundredin.net/2012/07/26/isolation-level/](http://hundredin.net/2012/07/26/isolation-level/)
- [http://egloos.zum.com/ljlave/v/1530887](http://egloos.zum.com/ljlave/v/1530887)