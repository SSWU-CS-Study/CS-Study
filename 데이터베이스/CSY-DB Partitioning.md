# DB Partitioning

큰 테이블이나 인덱스를 작은 파티션(Partition) 단위로 나누어 관리하는 기법

파티셔닝은 데이터베이스에서 중요한 튜닝 기법으로, 
데이터가 너무 커져서 조회하는 시간이 길어질 때 
또는 관리 용이성, 성능, 가용성 등의 향상을 이유로 행해지게 된다.

> 즉 큰 table이나 index를, 관리하기 쉬운 partition이라는 작은 단위로 물리적으로 분할하는 것을 의미한다.
물리적인 데이터 분할이 있더라도, DB에 접근하는 application의 입장에서는 이를 인식하지 못한다. ⇒ 논리적으로는 하나의 테이블이기 때문에 개발되어 있는 쿼리문을 변경할 필요가 없음
> 

### 장점

1. 성능(Performance)
    1. 특정 DML과 Query의 성능을 향상시킨다.
    2. 주로 대용량 Data WRITE 환경에서 효율적이다.
    : 많은 INSERT가 있는 OLTP(online transaction processing) 시스템에서 INSERT 작업을 작은 단위인 partition들로 분산시켜 충돌을 줄인다.
    3. Full Scan에서 데이터 Access 범위를 줄여 성능 향상을 가져온다.
2. 가용성(Availability) - 시스템을 정상적으로 사용 가능한 정도 | 가동률
    1. 물리적인 파티셔닝으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.
    2. 각 partition별로 독립적으로 백업하고 복구할 수 있다.
3. 관리용이성(Manageability)
    1. 큰 테이블을 제거하여 관리를 쉽게 해준다.

### 단점

- 파티션을 나누어 저장하기 때문에 다른 파티션에 저장된 테이블 간 JOIN에 대한 비용이 증가한다.
- 테이블과 인덱스를 별도로 파티셔닝 할 수 없어 같이 파티셔닝해야 한다.

## DB Partitioning의 종류

### 수평 파티셔닝

**하나의 테이블의 각 행을 다른 테이블에 분산**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a88ab6b7-3778-40b3-a18e-af4a9528ef92/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230226%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230226T081107Z&X-Amz-Expires=86400&X-Amz-Signature=42da70a9db70f89e0cfb6ad4b01767f14cdaaaef49e67b961eb03e62888da5e6&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**장점**

- 데이터의 개수를 기준으로 나누어 파티셔닝한다.
- 데이터의 개수와 인덱스의 개수가 줄어들어 성능이 향상된다.

**참고 : 샤딩(Sharding)이란?**

샤딩은 같은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 기법을 뜻한다. 종종 샤딩과 수평 파티셔닝이 같은 의미로 사용되곤 하지만, 사실 둘은 조금 다르다.

![https://blog.kakaocdn.net/dn/X7NCc/btrzdN7s7DS/uleNAyoP3KekFj8M4H4kLk/img.webp](https://blog.kakaocdn.net/dn/X7NCc/btrzdN7s7DS/uleNAyoP3KekFj8M4H4kLk/img.webp)

수평 파티셔닝은 **같은 데이터베이스 내에서** 하나의 큰 테이블을 쪼개 분산 저장하는 기법이다.

반면, 샤딩은 하나의 큰 테이블을 쪼개 **각각 다른 데이터베이스에** 분산 저장하는 기법이다.

이러한 샤딩은 수평 파티셔닝의 장점을 모두 갖는다.

샤딩은 데이터베이스 서버 간의 연결 과정이 많아져 비용이 증가할 수 있다. 또한 하나의 서버가 고장 나면 데이터의 무결성이 깨질 수 있다는 단점도 있다.

### 수직 파티셔닝

**테이블의 일부 열을 빼내는 형태로 분할**

![Untitled](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/0119c8d2-62ec-42f6-9805-2218e1805f85/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20230226%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20230226T081206Z&X-Amz-Expires=86400&X-Amz-Signature=df6391a1b5d9439e1803c3b00eceb5cb5254764d9697bea9d6609931e59a37e7&X-Amz-SignedHeaders=host&response-content-disposition=filename%3D%22Untitled.png%22&x-id=GetObject)

**장점**

- 자주 사용하는 칼럼을 분리하여 성능을 향상할 수 있다.
- 같은 타입의 데이터가 저장되어 데이터 압축률을 높일 수 있다.
- 조회 시 필요 없는 칼럼을 조회하지 않아도 되므로 성능상의 이점이 있다.

## DB Partitioning의 분할 기준

![https://gmlwjd9405.github.io/images/database/partitioning.png](https://gmlwjd9405.github.io/images/database/partitioning.png)

### 1. 범위 분할 (range partitioning)

- 연속적인 숫자나 날짜를 기준으로 파티셔닝 한다.
- 분할 키 값이 범위 내에 있는지 여부로 구분한다.
- 우편 번호, 날짜 등의 데이터에 적합하다.

### 2. 목록 분할 (list partitioning)

- 할당 분할 키 값을 값 목록에 비추어 파티션을 선택한다.
- 특정 파티션에 저장될 데이터에 대한 명시적 제어가 가능하다.
- 분포도가 비슷하며, 많은 SQL에서 해당 칼럼의 조건이 많이 들어오는 경우 유용하다.

### 3. 해시 분할 (hash partitioning)

- 파티션 키(Key)의 해시 값에 의한 파티셔닝이 이루어진다.
- 균등한 데이터 분할이 가능하다.
- 해시 함수의 값에 따라 파티션에 포함할지 여부를 결정한다.
- 특정 데이터가 어느 Hash 파티션에 있는지 판단하기 어렵다.
- 파티션을 위한 범위가 없는 데이터에 적합하다.

### 4. 합성 분할 (composite partitioning)

- 상기 기술을 결합하는 것을 의미한다. 즉, 파티션의 sub-partitioning을 뜻한다.
- 큰 파티션에 대한 I/O 요청을 여러 파티션으로 분산할 수 있다.
- 예를 들면 먼저 범위 분할하고, 다음에 해시 분할 같은 것을 생각할 수 있다.

## 예상 질문

- DB 파티셔닝에 대해 설명해주세요.
    
    큰 테이블을 관리하기 쉬운 partition 이라는 작은 단위로 물리적으로 분할하는 것을 의미합니다. 이를 통해 데이터를 분산처리하여 성능이 저하되는 것을 방지하고 관리를 보다 수월하게 할 수 있습니다.
    
- DB 파티셔닝의 수평분할과 샤딩의 차이를 설명해주세요.
    
    수평분할은 같은 데이터베이스 내에서 각 행을 다른 테이블에 분산하는 것이고, 샤딩은 하나의 테이블을 다른 데이터베이스의 테이블에 분산하는 것으로 같은 데이터베이스에서 분할하는지 여부에 차이가 있습니다.
    
- DB 파티셔닝의 장단점을 설명해주세요.
    
    장점으로는, insert문과 select문 등 특정 쿼리의 성능 향상과 가용성의 상승, 관리의 용이성이 있고 단점으로는 다른 파티션에 저장된 테이블 간의 join문의 경우 속도가 느려질 수 있으며 테이블과 인덱스를 같이 파티셔닝해야한다는 점입니다.
    

## 참고 자료

- [https://code-lab1.tistory.com/202](https://code-lab1.tistory.com/202)
- [https://gmlwjd9405.github.io/2018/09/24/db-partitioning.html](https://gmlwjd9405.github.io/2018/09/24/db-partitioning.html)
