# 엘라스틱서치(Elastic Search)

Elasticsearch는 Apache Lucene(아파치 루씬) 기반의 Java 오픈 소스 분산 검색 엔진이다. Elasticsearch를 통해 루씬 라이브러리(Java에서 개발한 정보 검색용 라이브러리)를 단독으로 사용할 수 있으며, JSON 형태의 방대한 데이터를 신속하게(거의 실시간) 저장, 검색, 분석을 수행할 수 있다.

Elastic Search는 검색을 위해 단독으로 쓰이기도 하며 ELK(Elastic Search/ Logstash/ Kibana) 스택으로 사용되기도 한다.
ELK는 흩어져 있는 로그를 한 곳으로 모으고, 원하는 데이터를 빠르게 검색한 뒤 시각화하여 모니터링하기 위해 사용한다.

- **ELK 스택이란**
    
    ![https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2017/11/2-2.png](https://d1jnx9ba8s6j9r.cloudfront.net/blog/wp-content/uploads/2017/11/2-2.png)
    
    - **Logstash**
        - 다양한 소스( DB, csv파일 등 )의 로그 또는 트랜잭션 데이터를 수집, 집계, 파싱하여 Elasticsearch로 전달
    - **Elasticsearch**
        - Logstash로부터 받은 데이터를 검색 및 집계를 하여 필요한 관심 있는 정보를 획득
    - **Kibana**
        - Elasticsearch의 빠른 검색을 통해 데이터를 시각화 및 모니터링

## 기본 개념

### **NRT(Near Realtime)**

Elasticsearch는 NRT 검색 플랫폼입니다. 즉 문서를 색인화하는 시점부터 문서가 검색 가능해지는 시점까지 약간의 대기 시간(대개 1초)이 있다.

### 클러스터

하나 이상의 노드(서버)가 모인 것이며 이를 통해 전체 데이터를 저장하고 모든 노드를 포괄하는 통합 색인화(인덱싱) 및 검색 기능을 제공한다.

### 노드

노드는 클러스터에 포함된 단일 서버로서 데이터를 저장하고 클러스터의 색인화 및 검색 기능에 참여한다.

### **도큐먼트**

문서는 색인화할 수 있는 기본 정보 단위이다. 예를 들어 어떤 단일 고객, 단일 제품, 단일 주문에 대한 문서가 각각 존재할 수 있다. 이 문서는 [JSON](http://json.org/)(JavaScript Object Notation) 형식이다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5f712c3a-0403-4f06-abfe-9e082fa05a03/Untitled.png)

| Elastic Search | RDBMS |
| --- | --- |
| Index | Database |
| Type | Table |
| Document | Row |
| Field | Column |

### REST API

HTTP 프로토콜로 접근이 가능한 REST API를 통해 데이터 조작을 지원한다.

| Elastic Search HTTP Method | RDBMS SQL |
| --- | --- |
| GET | SELECT |
| PUT | INSERT |
| POST | UPDATE, SELECT |
| DELETE | DELETE |

## 관계형 데이터베이스 VS 엘라스틱 서치

### 관계형 데이터베이스에서의 검색 방식

SQL문을 사용하여 원하는 검색을 할 경우

```sql
SELECT * FROM table WHERE word LIKE '%...%';
```

⚠️ **문제점!**

- 관계형 데이터베이스는 단순 텍스트매칭에 대한 검색만을 제공
- 최근 MySQL 최신 버전에서 n-gram 기반의 검색을 지원하지만 한글 검색의 경우 한계점 존재
- 텍스트를 여러 단어로 변형하거나 텍스트의 특징을 이용한 동의어나 유의어를 활용한 검색이 불가능

### 엘라스틱 서치

- 관계형 데이터베이스에서 불가능한 **비정형 데이터**의 색인과 검색이 가능
    - 빅데이터 처리에서 매우 중요한 사항
- 형태소 분석을 통한 자연어 처리가 가능
- **역색인 분석**으로 매우 빠른 검색이 가능

## **장점**

- **오픈소스 검색엔진**이다. 활발한 오픈소스 커뮤니티가 ES를 끊임없이 개선하고 발전시키고 있다.
- **전문검색**
내용 전체를 색인해서 특정 단어가 포함된 문서를 검색할 수 있다. 기능별, 언어별 플러그인을 적용할 수 있다.
- **통계 분석**
비정형 로그 데이터를 수집하여 통계 분석에 활용할 수 있다. Kibana를 연결하면 실시간으로 로그를 분석하고 시각화할 수 있다.
- **Schemaless**
정형화되지 않은 문서도 자동으로 색인하고 검색할 수 있다.
- **RESTful API**
HTTP기반의 RESTful를 활용하고 요청/응답에 JSON을 사용해 개발 언어, 운영체제, 시스템에 관계없이 다양한 플랫폼에서 활용이 가능하다.
- **Multi-tenancy**
서로 상이한 인덱스일지라도 검색할 필드명만 같으면 여러 인덱스를 한번에 조회할 수 있다.
- **Document-Oriented**
여러 계층 구조의 문서로 저장이 가능하며, 계층 구조로된 문서도 한번의 쿼리로 쉽게 조회할 수 있다.
- **역색인(Inverted Index)**
- **확장성**
분산 구성이 가능하다. 분산 환경에서 데이터는 shard라는 단위로 나뉜다.

## **단점**

- **완전 실시간은 아니다.**
색인된 데이터는 1초 뒤에나 검색이 가능하다. 내부적으로 commit과 flush같은 복잡한 과정을 거치기 때문.
- **Transaction Rollback을 지원하지 않는다.**
전체적인 클러스터의 성능 향상을 위해 시스템적으로 비용 소모가 큰 롤백과 트랜잭션을 지원하지 않는다.
- **데이터의 업데이트를 제공하지 않는다.**
업데이트 명령이 올 경우 기존 문서를 삭제하고 새로운 문서를 생성한다. 
업데이트에 비해서 많은 비용이 들지만 이를 통해 불변성(Immutable)이라는 이점을 취한다.

## **색인과 역색인**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8c25e41e-cb39-4fdd-8c72-201053f36d24/Untitled.png)

### **색인 (Index)**

- 문서에서 키워드를 찾아 보기 쉽도록 정렬 및 나열한 목록이다.
- 책에서 맨 앞에 나와 있는 목차를 떠올리면 된다. (좌측 사진)
- 데이터베이스 관점으로 보자면 '추천사', '들어가며' 같은 목차의 제목은 PK와 같은 레코드를 대표하는 값을 의미하고, 몇번 페이지를 나타내는 것은 실제 데이터 레코드가 존재하는 물리적 주소로 이해하면 된다. 레코드에서 대표하는 값을 뽑기 때문에 문서에서 키워드를 뽑는 색인이라고 명명하는 것이다.

### **역색인 (Inverted Index)**

- 키워드를 통해 문서를 찾아내는 방식이다.
- 책에서 맨 뒤에 나와 있는 찾아 보기를 떠올리면 된다.
- 검색 성능이 매우 빠르다.

## **역색인은 왜 검색 성능이 빠를까?**

- 검색은 대량의 문서 집합에서 "특정" 키워드가 포함된 문서를 찾아 내는 행위이다.
- 우선 색인을 사용하여 검색을 시도해 보자.

### **색인을 사용한 검색**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/402fd2af-174c-4a48-8883-aec471c86d8d/Untitled.png)

- 우리가 ‘fox’라는 키워드를 통해 검색한다고 가정해 보자.
- 일반적으로 위의 테이블 데이터에서 한 줄씩 like ‘%fox’ 검색을 통해 데이터를 검색한다.
- 단순히 컬럼을 읽는 느낌이 아니라, 텍스트 하나 하나를 읽어야 한다는 사실을 명심해야 한다. 만약 Text가 1700쪽에 달하는 양으로 저장되어 있다면, 검색을 위해 엄청난 양의 텍스트를 읽어야 한다.
- 전통적인 RDBMS에서는 like 검색을 사용하기 때문에 데이터가 늘어날수록 검색해야 할 대상이 늘어나 시간도 오래 걸리고, row 안의 내용을 모두 읽어야 하기 때문에 기본적으로 속도가 느리다.

### **역색인을 사용한 검색**

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e630af50-074c-461c-8ce2-50efc3300ea9/Untitled.png)

- 역색인은 앞서 말한 책의 맨 뒤에 있는 찾아 보기와 유사하다. (흔히 단어 사전이라고도 함)
- 역색인을 사용하는 검색 엔진에서는 추출된 각 키워드를 term이라고 부른다.
- 이렇게 역색인이 있으면, fox를 포함하고 있는 문서의 id를 바로 얻어 올 수 있다.
    - 문서의 id는 PK, 물리적 주소 등에 해당한다.
- 역색인을 사용하면 데이터가 늘어나도 찾아가야 할 행이 늘어나는 것이 아니라 역색인이 가리키는 id의 배열 값이 추가되는 것이므로 큰 속도의 저하 없이 여전히 빠른 속도로 검색이 가능하다.
- 참고로 역색인을 데이터가 저장되는 과정에서 만들기 때문에 검색 엔진은 데이터를 입력할 때 저장이 아닌 색인을 한다고 표현한다.

### **term은 어떻게 만들어질까?**

- 위에서 검색 엔진에서 추출된 키워드를 term라고 하였다. 즉, 검색 엔진은 특정 문서에서 여러 키워드로 쪼갠다고 유추할 수 있다. 그렇다면, 문서를 여러 term으로 어떻게 나누는지 살펴 보자.
    - 문서를 term으로 나누는 것을 텍스트 분석 혹은 처리한다고 한다.
- 두 가지 예문을 문서로 사용하겠다.
    - The quick brown fox jumps over the lazy dog.
    - Fast jumping rabbits.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8d695580-5f9c-48d5-943e-0c1076cf4643/Untitled.png)

- 먼저 텍스트를 다 뜯어서 검색어 사전을 만든다. 띄어 쓰기 단위로 다 쪼개면 된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/dbcff9c4-78d0-412f-ae5d-33088040cadf/Untitled.png)

- 이때 대소문자를 소문자로 변환한다.
- 토큰을 ascii 순서로 재정렬한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/5b36374b-e17f-437e-9166-81f28b18d6be/Untitled.png)

- 불용어를 제거한다.
    - 검색어로서 가치가 없는 단어
    - a, an, are, the, ...

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/137b527e-4b7a-4595-8e92-8b55029faf19/Untitled.png)

- 형태소 분석 과정을 거친다.
    - 보통 ~s, ~ing을 제거한다.
    - 한글은 의미 분석을 해야 해서 좀 더 복잡하다.
    - Elasticsearch에서는 한글 형태소 분석기로 nori를 주로 사용한다. (물론 Elasticsearch는 global하므로 한글 형태소 분석기를 수동으로 설치해야 함)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/436411aa-650b-4b6f-808b-6ace8aa80a2d/Untitled.png)

- 중복된 jump 토큰을 하나로 병합하고, 동의어를 처리한다.
- 이로써 역색인 구성이 완료된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/1e79d454-04a0-4ea3-a105-c0828b98266f/Untitled.png)

- 중요한 점은 검색 엔진에서 사용할 검색어도 위와 동일한 방식으로 텍스트 처리를 한다.
- “The lazy spiders”를 검색어로 사용하면, lazi와 rabbit으로 분리가 된다. 그리고 기존의 구성한 역색인을 활용하여 방금 분석한 term에 해당하는 문서의 ID를 알 수 있으므로 빠르게 ID에 맞는 실제 문서를 얻어올 수 있다.

### **기타 궁금증**

**Q. 검색어에 대해 텍스트 처리를 하고, 쪼개진 term의 개수가 많다면 매번 term을 찾기 위해 역색인을 O(N)만큼 순회하면 색인보다 성능이 떨어지는 것 아닌가?**

루씬의 역색인 구조를 이용하는 Elasticsearch는 B-Tree와 유사하게 동작하는 Skip-Lists와 FST를 활용하여 O(log N)을 보장한다. 그래서 term의 개수가 많더라도 길이가 짧은 문서가 매우 적게 저장되어 있는 환경이 아닌 이상 역색인이 검색 성능이 좋다.

## 예상 질문

- **Elasticsearch란?**
    
    Elasticsearch는 Apache Lucene(아파치 루씬) 기반의 Java 오픈 소스 분산 검색 엔진이다. Elasticsearch를 통해 루씬 라이브러리(Java에서 개발한 정보 검색용 라이브러리)를 단독으로 사용할 수 있으며, 방대한 양의 데이터를 신속하게(거의 실시간) 저장, 검색, 분석을 수행할 수 있다.
    
- **Elasticsearch는 언제 사용하는가?**
    
    Elasticsearch는 검색을 위해 단독으로 사용되기도 하며, ELK(Elasticsearch / Logstash / Kibana) 스택으로 사용되기도 한다.
    
- **ELK를 왜 사용하는가?**
    
    ELK는 흩어져 있는 로그를 한 곳으로 모으고, 원하는 데이터를 빠르게 검색한 뒤 시각화하여 모니터링하기 위해 사용한다.
    
- **Elasticsearch는 왜 검색 속도가 빠른가?**
    
    역색인 자료 구조로 인해 빠르다.
    
- **Elasticsearch의 장점은?**
    - Scale out
        - 샤드를 통해 규모가 수평적으로 늘어날 수 있음
    - 고가용성
        - Replica를 통해 데이터의 안전성을 보장
    - Schema Free
        - Json 문서를 통해 데이터 검색을 수행하므로 스키마 개념이 없음
    - Restful
        - 데이터 CRUD 작업은 HTTP Restful API를 통해 수행한다.
            - SELECT = GET
            - INSERT = PUT
            - UPDATE = POST
            - DELETE = DELETE
        - Restful API를 사용한다는 것은 다양한 플랫폼에서 응용이 가능하다는 장점이 있다.
    - 역색인
- **Elasticsearch의 단점은?**
    - 실시간 처리가 불가능하다.
        - elasticsearch는 인메모리 버퍼를 사용하므로 쓰기와 동시에 읽기 작업을 할 경우, 세그먼트가 생성되기 전까지는 해당 데이터를 검색할 수 없다.
    - 트랜잭션을 지원하지 않는다.
        - 분산 시스템 구성의 특징 때문에 시스템적으로 비용 소모가 큰 트랜잭션 및 롤백을 지원하지 않는다.
    - 진정한 의미의 업데이트를 지원하지 않는다.
- **Elasticsearch와 RDBMS의 차이는?**
    
    Elasticsearch는 Json 문서를 통해 검색을 수행하므로 스키마가 없지만, RDBMS는 엄격한 스키마가 존재한다.
    
    Elasticsearch는 역색인 자료 구조로 검색을 수행하지만, RDBMS는 인덱스 자료 구조로 검색을 수행한다.
    
    Elasticsearch는 저장소보다는 검색 엔진에 가깝다.
    
- **Elasticsearch와 NoSQL의 차이는?**
    
    Elasticsearch는 실시간 처리가 불가능하지만, NoSQL은 실시간 처리가 가능하다.
    
    Elasticsearch는 역색인 자료 구조가 있지만, NoSQL은 없다.
    
    Elasticsearch는 저장소보다는 검색 엔진에 가깝다.
    
- **역색인이란?**
    
    키워드를 통해 문서를 찾아내는 방식이다.
    
    책에서 맨 뒤에 나와 있는 찾아 보기를 떠올리면 된다.
    
- **색인을 사용한 검색은 왜 느린가?**
    
    전통적인 RDBMS에서는 like 검색을 사용하기 때문에 데이터가 늘어날수록 검색해야 할 대상이 늘어나 시간도 오래 걸리고, row 안의 내용을 모두 읽어야 하기 때문에 기본적으로 속도가 느리다. 특히 row 안의 내용이 길다면 하나의 row를 읽더라도 시간이 오래 걸린다.
    
- **역색인을 사용한 검색은 왜 빠른가?**
    
    역색인을 사용하면 데이터가 늘어나도 찾아가야 할 행이 늘어나는 것이 아니라 역색인이 가리키는 id의 배열 값이 추가되는 것이므로 큰 속도의 저하 없이 여전히 빠른 속도로 검색이 가능하다.
    

## 참고 자료

- [공식 설명서(한)](https://www.elastic.co/guide/kr/elasticsearch/reference/current/gs-basic-concepts.html)
- [역색인이란?](https://steady-coding.tistory.com/581)
- [엘라스틱서치 예제](https://dealicious-inc.github.io/2021/11/22/dealibird-elastic-search.html)
- [https://needjarvis.tistory.com/345](https://needjarvis.tistory.com/345)
- [https://the-dev.tistory.com/30](https://the-dev.tistory.com/30)
- [https://woongsin94.tistory.com/345https://woongsin94.tistory.com/345](https://woongsin94.tistory.com/345)
- [https://stackoverflow.com/questions/2602253/how-does-lucene-index-documents](https://stackoverflow.com/questions/2602253/how-does-lucene-index-documents)
