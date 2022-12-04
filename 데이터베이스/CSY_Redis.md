# **Redis**

> Remote Dictionary Server : 메모리 기반의 key-value구조 데이터 관리 시스템
> 

보통 데이터베이스는 하드 디스크나 SSD에 저장한다. 

하지만 Redis는 메모리(RAM)에 저장해서 디스크 스캐닝이 필요없어 매우 빠른 장점이 존재함

캐싱도 가능해 실시간 채팅에 적합하며 세션 공유를 위해 세션 클러스터링에도 활용된다.

key-value형태이므로 별도의 쿼리 없이 Key를 통해 빠르게 결과를 가져올 수 있다.
또한, 디스크에 데이터를 쓰는 구조가 아니라 메모리에서 데이터를 처리하기 때문에 작업 속도가 상당히 빠르다.

### Redis 영속성

> *RAM은 휘발성 아닌가요? 껐다키면 다 날아가는데..*
> 

이를 막기위한 백업 과정이 존재한다.

- RDB(snapshot) : 특정 지점을 설정하고 디스크에 백업
- AOF(Append Only File) : 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태. 서버 재시작 시 write/update를 순차적으로 재실행하고 데이터를 복구

⇒ 가장 좋은 방식은 두 방법을 혼용해서 사용하는 방법으로 주기적으로 snapshot으로 백업을 하고 다음 snapshot까지의 저장을 AOF 방식으로 수행하는 방식

### **value**

**(JAVA 자료구조와 유사한 영속적인 자료구조 제공)**

![https://miro.medium.com/max/700/1*tMiZs3RCrmxLGiFZgWRP6g.png](https://miro.medium.com/max/700/1*tMiZs3RCrmxLGiFZgWRP6g.png)

1. String (text, binary data) - 512MB까지 저장이 가능함
2. set (String 집합)
3. sorted set (set을 정렬해둔 상태)
4. Hash
5. List (양방향 연결리스트도 가능)


## 예상 질문

<details>
  <summary>Redis란?</summary>
  <div markdown="1">
   Redis는 고성능 키-값 저장소로서 String, list, hash, set, sorted set 등의 자료 구조를 지원하는 NoSQL입니다.</div>
</details>
<details>
  <summary>Redis의 특징은 무엇인가요?</summary>
  <div markdown="1">
   - 영속성을 지원하는 인 메모리 데이터 저장소
    - 다양한 자료 구조를 지원함
  </div>
</details>
<details>
  <summary>Redis는 왜 속도가 빠른가?</summary>
  <div markdown="1">
   Key-Value 방식이므로 쿼리를 날리지 않고 결과를 얻을 수 있다.</div>
</details>
<details>
  <summary>Redis는 영속성을 어떻게 보장하는가?</summary>
  <div markdown="1">
    RDB 또는 AOF 방식을 통해 영속성을 보장한다.

    RDB(Snapshotting) 방식: 순간적으로 메모리에 있는 내용 전체를 디스크에 옮겨 담는 방식
    AOF(Append On File) 방식 : Redis의 모든 write/update 연산 자체를 모두 log 파일에 기록하는 형태
  </div>
</details>


### 참고자료

- [https://steady-coding.tistory.com/586](https://steady-coding.tistory.com/586)
- [https://azderica.github.io/01-db-nosql-redis/](https://azderica.github.io/01-db-nosql-redis/)
