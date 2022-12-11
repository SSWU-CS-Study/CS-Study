## 트랜잭션

> 데이터베이스의 상태를 변화시키기 위해 수행하는 작업 단위
하나의 논리적 작업 단위를 구성하는 일련의 연산들의 집합
> 

## 트랜잭션의 특징(ACID)

- 원자성(Atomicity)
    
    트랜잭션이 DB에 모두 반영되거나, 혹은 전혀 반영되지 않아야 된다.
    
- 일관성(Consistency)
    
    트랜잭션의 작업 처리 결과는 항상 일관성 있어야 한다.
    트랜잭션이 일어난 이후의 데이터베이스는 데이터베이스의 제약이나 규칙을 만족해야 한다.
    
- 독립성(Isolation)
    
    둘 이상의 트랜잭션이 동시에 병행 실행되고 있을 때, 어떤 트랜잭션도 다른 트랜잭션 연산에 끼어들 수 없다. → 동시에 수행되는 트랜잭션이 같은 데이터를 가지고 **충돌하지 않도록 제어**하는 작업 필요 **⇒ 동시성 제어**
    
- 지속성(Durability)
    
    트랜잭션이 성공적으로 완료되었으면, 결과는 영구적으로 반영되어야 한다.
    

## 트랜잭션의 동시성 제어

동시에 수행되는 트랜잭션이 같은 데이터를 가지고 **충돌하지 않도록 제어**하는 작업

트랜잭션의 읽기/쓰기 시나리오

| 상황 | 트랜잭션1 | 트랜잭션2 | 발생 문제 | 동시 접근 |
| --- | --- | --- | --- | --- |
| 상황1 | 읽기 | 읽기 | 없음 | 허용 |
| 상황2 | 읽기 | 쓰기 | dirty read, non-repeatable read, phantom read | 허용 혹은 불가 선택 |
| 상황3 | 쓰기 | 쓰기 | 갱신손실(허용 불가) | 허용 불가(LOCK 이용) |

### 갱신손실 문제

: 두 개의 트랜잭션이 한 개의 데이터를 동시에 갱신할 때 발생. 절대 발생하면 안 되는 현상

시나리오: T1이 계좌 X에서 100을 빼고 T2가 계좌 X에 100을 넣는 작업.

![https://velog.velcdn.com/images/songyw0517/post/ca748366-bf02-4c7d-94a2-23c62fea6243/image.png](https://velog.velcdn.com/images/songyw0517/post/ca748366-bf02-4c7d-94a2-23c62fea6243/image.png)

문제! T2가 T1이 갱신을 준비 중인 데이터를 읽어와 작업을 진행한다. T1의 갱신 작업을 무효화하고 덧쓰기를 수행한 것. ⇒ 갱신손샐 문제가 발생

### 락

: 트랜젝션이 데이터를 읽거나 수정할 때 데이터에 표시하는 잠금 장치
락을 사용해 데이터를 자믁면 다른 트랜잭션은 잠금이 풀릴 때까지 기다려야 한다.

![https://velog.velcdn.com/images/songyw0517/post/43b29c46-e528-4936-947e-a849a835629a/image.png](https://velog.velcdn.com/images/songyw0517/post/43b29c46-e528-4936-947e-a849a835629a/image.png)

**락의 유형**

- 공유락 : 트랜잭션이 읽기를 할 때 사용하는 락
- 베타락 : 읽기/쓰기를 할 때 사용하는 락

**2단계 락킹**

  : 데이터에 락을 걸었다 풀고, 다시 거는 중간 과정에 락의 해지상태가 생기면서 다른 트랙잭션에게 중간 결과를 보일 수 있다.

**데드락**

  : 두 개 이상의 트랜잭션이 각각 자신의 데이터에 대하여 락을 획득하고 상대방 데이터에 대해 락을 요청하면 생기는 무한 대기 상태 
⇒ 강제 중지로 해결

# ****트랜잭션 격리 수준(Transaction Isolation Level)****

## 트랜잭션 동시 실행 문제

| 상황 | 트랜잭션1 | 트랜잭션2 | 발생 문제 | 동시 접근 |
| --- | --- | --- | --- | --- |
| 상황1 | 읽기 | 읽기 | 없음 | 허용 |
| 상황2 | 읽기 | 쓰기 | dirty read, non-repeatable read, phantom read | 허용 혹은 불가 선택 |
| 상황3 | 쓰기 | 쓰기 | 갱신손실(허용 불가) | 허용 불가(LOCK 이용) |

### Dirty Read(오손 읽기)

: 읽기 작업을 하는 트랜잭션 1이 쓰기 작업을 하는 트랜잭션2의 작업 중간 데이터를 읽어올 때 발생 
트랜잭션2가 롤백(Rollback)을 할 경우 무효가 된 데이터를 읽어오게 됨.

![https://velog.velcdn.com/images/songyw0517/post/f6d52ad4-387a-49e9-a716-31ffaee52505/image.png](https://velog.velcdn.com/images/songyw0517/post/f6d52ad4-387a-49e9-a716-31ffaee52505/image.png)

<br />

### Non-repeatable read(반복불가능 읽기)

![https://velog.velcdn.com/images/songyw0517/post/89ac1ae7-459f-40a4-8fd9-daa02ca51ebd/image.png](https://velog.velcdn.com/images/songyw0517/post/89ac1ae7-459f-40a4-8fd9-daa02ca51ebd/image.png)

문제! T1이 읽기 작업을 반복할 때 이전의 결과가 반복되지 않는 현상
⇒ 틀린 데이터는 아니지만 같은 SQL문이 다른 결과를 도출했다는 문제

<br />

### Phantom read(유령데이터 읽기)

![https://velog.velcdn.com/images/songyw0517/post/3ad0e8b7-31d9-49b1-b946-ae5a7a470ce1/image.png](https://velog.velcdn.com/images/songyw0517/post/3ad0e8b7-31d9-49b1-b946-ae5a7a470ce1/image.png)

문제! T1이 읽기 작업을 반복했을 때 이전에 없던 데이터(유령 데이터)가 나타나는 현상 ⇒ 틀린 데이터는 아니지만 같은 SQL문이 다른 결과를 도출했다는 문제 | non-repeatable과 비슷하지만 없던 데이터가 삽입되기 때문에 다르게 구분

<br /><br />

---

 

## 트랜잭션 고립 수준 명령어(transaction isolation level instruction)

: 앞의 세 가지 문제를 해결하기 위해 사용하는 방법.
트랜잭션을 동시에 실행시키면서 락보다 좀 더 완화된 방법으로 문제를 해결하는 명령어

| 고립 수준\문제 | dirty read | non-repeatable | phantom read |
| --- | --- | --- | --- |
| READ UNCOMMITTED | O | O | O |
| READ COMMITTED | X | O | O |
| REPEATABLE READ | X | X | O |
| SERIALIZABLE | X | X | X |

### 1. READ UNCOMMITTED (Level=0)

- 고립 수준이 가장 낮은 명령어
- 자신의 데이터에 아무런 공유락을 걸지 않는다. (배타락은 갱신손실 문제로 건다.)
- 다른 트랜잭션에 **공유락과 배타락이 걸린 데이터를 바로 읽는다**. -> non-repeatable read, phantom read 가능
- 심지어, 다른 트랜잭션이 COMMIT하지 않은 데이터도 읽을 수 있다. -> dirty read 가능

### 2. READ COMMITTED (Level=1)

- 커밋이 완료된 데이터를 조회할 수 있다.
- 다른 트랜잭션이 설정한 **공유락은 읽지만, 배타락은 읽지 못한다.**
- non-repeatable read, phantom read 가능
- 온라인 서비스에서 가장 많이 선택되는 격리 수준

### 3. REPEATABLE READ (Level=2)

- 트랜잭션이 사용하는 데이터에 설정된 공유락과 배타락을 트랜잭션이 종료할 때까지 유지
- 다른 트랜잭션이 자신의 데이터를 갱신(UPDATE)할 수 없도록 한다.
- 다른 고립화 수준에 비해 데이터의 동시성이 낮아(응답시간에 영향을 준다.) 특별한 상황이 아니면 사용하지 않는 것이 좋다.
- phantom read 가능
    - MySQL에서는 SNAPSHOT을 사용하여 phantom read도 방지한다.

### 4. SERIALIZABLE (Level=3)

- 고립 수준이 가장 높은 명령어
- 실행 중인 트랜잭션은 다른 트랜잭션으로부터 완벽하게 분리된다.
- 제한이 가장 심하고, 데이터의 동시성도 낮다.

***선택 시 고려사항***

Isolation Level에 대한 조정은 동시성과 데이터 무결성에 연관되어 있음

동시성을 증가시키면 데이터 무결성에 문제가 발생하고, 데이터 무결성을 유지하면 동시성이 떨어지게 됨

레벨을 높게 조정할 수록 발생하는 비용이 증가함

<br />

---

## 예상 질문

<details>
  <summary>트랜잭션의 동시성 문제를 해결하기 위해 어떤 방법을 사용하나요?</summary>
  <div markdown="1">
    트랜젝션이 데이터를 읽거나 수정할 때 데이터에 표시하는 잠금 장치인 락을 사용합니다.
    락에는 읽기 작업을 수행할 때 사용하는 공유락과 쓰기 작업에 사용하는 베타락이 있습니다.
  </div>
</details>

<details>
  <summary>트랜잭션 고립 수준이란?</summary>
  <div markdown="1">
    트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준입니다.
  </div>
</details>

<details>
  <summary>트랜잭션 고립 수준의 필요성을 설명해주세요.</summary>
  <div markdown="1">
    데이터베이스는 ACID 같이 트랜잭션이 원자적이면서도 독립적인 수행을 하도록 합니다.
    
    트랜잭션이 DB를 다루는 동안 다른 트랜잭션이 관여하지 못하게 막는 Locking 이라는 개념이 등장한다.
    
    하지만 무조건적인 Locking으로 동시에 수행되는 많은 트랜잭션들을 순서대로 처리하는 방식으로 구현되면 DB의 성능은 떨어지게 됩니다.
    반대로 응답성을 높이기 위해 Locking 범위를 줄인다면 잘못된 값이 처리 될 여지가 있어 최대한 효율적인 Locking 방법이 트랜잭션 고립 수준 명령어를 사용하는 것입니다.
  </div>
</details>


<details>
  <summary>고립 수준 종류를 설명해주세요.</summary>
  <div markdown="1">
     READ UNCOMMITTED : 각 트랜잭션의 변경 내용이 COMMIT이나 ROLLBACK 여부와 상관 없이 다른 트랜잭션에서 보여지게 됩니다.
    
    READ COMMITTED : 커밋이 완료된 데이터만 다른 트랜잭션에서 조회할 수 있습니다.
    
    REPEATABLE READ : 트랜잭션이 사용하는 데이터에 설정된 공유락과 배타락을 트랜잭션이 종료할 때까지 유지
    
    SERIALIZABLE : 트랜잭션에서 읽고 쓰는 레코드를 다른 트랜잭션에서는 절대 접근할 수 없는 수준입니다.
  </div>
</details>
    
    

<br /><br />

---
    

## 참고

[https://velog.io/@songyw0517/트랜잭션](https://velog.io/@songyw0517/%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98)

[https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html](https://gmlwjd9405.github.io/2017/10/01/basic-concepts-of-development-db.html)

[https://steady-coding.tistory.com/562](https://steady-coding.tistory.com/562)
