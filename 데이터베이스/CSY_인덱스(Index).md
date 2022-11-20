## 인덱스

인덱스(index, 색인)이란 자료를 쉽고 빠르게 찾을 수 있도록 만든 데이터 구조

원하는 데이터를 빨리 찾기 위해 투플의 키 값에 대한 물리적 위치를 기록해둔 자료구조

### 인덱스를 왜 사용하는가?

데이터베이스의 물리적 저장

<img src="https://velog.velcdn.com/images/hoonki/post/f19ea138-4889-4e9b-9909-6fa479bead4b/image.png" width=500px />

테이블을 생성하고 테이블에 데이터를 저장할 떼 DBMS는 고유한 방식으로 운영체제의 파일 시스템에 종속적인 데이터베이스 파일을 저장한다.
여기서 중요한 점은 실제 데이터가 저장되는 곳이 **보조기억장치**(별도의 저장소)라는 것이다.
저장소에서 데이터를 읽어오는 동안 필연적으로 액세스 시간이 발생하므로 데이터를 찾아오는데 시간이 걸린다.

| 학번 | 수강 강좌 | 중간고사 점수 |
| --- | --- | --- |
| 101 | 데이터베이스 | 60 |
| 102 | 선형대수학 | 80 |
| 103 | 졸업작품연구 | 70 |
| 101 | 운영체제 | 50 |

근데 이 때, SELECT문을 사용해 어떤 튜플을 찾는다고 하자. 사람은 눈으로 훑어보며 원하는 행을 한 번에 찾을 수 있지만, 컴퓨터는 테이블의 모든 데이터를 조회해야 되기 때문에 성능이 저하된다.
그러므로 인덱스에서 자주 사용되는 column에 대한 index table을 따로 만들어서 요청이 들어왔을 때 이 index table에 있는 값들로 결과 값을 조회해온다.

<br />

---
<br />

## 인덱스의 구조(알고리즘)

### B-Tree 인덱스

![https://velog.velcdn.com/images%2Fkjh03160%2Fpost%2F7bd74498-4adc-46eb-82c6-aeb4764573bd%2Fimage.png](https://velog.velcdn.com/images%2Fkjh03160%2Fpost%2F7bd74498-4adc-46eb-82c6-aeb4764573bd%2Fimage.png)

칼럼의 값을 변형하지 않고 원래의 값을 이용해 인덱싱 하는 알고리즘이다.

그림과 같이 각각의 노드는 **key값, data, 포인터**를 가진다. 키 값은 오름차순으로 저장되어 있으며 
새로 추가되거나 삭제될 경우 동적으로 노드를 분할하거나 통합하여 항상 균형 상태를 유지한다.
⇒ 데이터의 변경이나 추가가 잦을 경우 균형을 유지하기 위해 **노드의 분할 및 이동이 자주 발생하는 문제**

### B+Tree 인덱스

B-Tree에서 발전한 알고리즘으로 일반적인 RDBMS의 인덱스는 대부분 B+Tree 구조로 이루어져 있다.

![https://velog.velcdn.com/images%2Fkjh03160%2Fpost%2F1bb7d383-0d70-42ad-b8d7-1a6bb4877000%2Fimage.png](https://velog.velcdn.com/images%2Fkjh03160%2Fpost%2F1bb7d383-0d70-42ad-b8d7-1a6bb4877000%2Fimage.png)

**B-Tree와는 다른 점은 리프 노드에만 key와 data를 저장하고, 리프 노드끼리 Linked list로 연결되어 있다**는 것이다.

### Hash 인덱스

키 값의 해시 값을 구한 후, 버킷의 내용과 비교하여 레코드 위치를 찾을 수 있는 인덱스 기법이다.

![https://velog.velcdn.com/images%2Fkjh03160%2Fpost%2F41003846-ef3f-431e-867e-eafb34a1ae38%2Fimage.png](https://velog.velcdn.com/images%2Fkjh03160%2Fpost%2F41003846-ef3f-431e-867e-eafb34a1ae38%2Fimage.png)

<br />

---
<br />
## 인덱스의 종류

### 클러스터 인덱스(Clustered Index)

![[https://stackoverflow.com/questions/55836641/disk-io-in-mysql-clustered-index](https://stackoverflow.com/questions/55836641/disk-io-in-mysql-clustered-index)](https://sqlhints.com/wp-content/uploads/2018/05/Structure-of-Clustered-Index.jpg)

[https://stackoverflow.com/questions/55836641/disk-io-in-mysql-clustered-index](https://stackoverflow.com/questions/55836641/disk-io-in-mysql-clustered-index)

연속된 키 값의 레코드를 묶어서 같은 블록에 저장하는 방법
테이블 당 하나만 생성할 수 있다.
리프노드의 data에 페이지의 주소 값 대신 테이블의 열 자체가 저장되는 형태

보통 Primary Key(PK) 로 설정된 column이 설정된다.

### 비클러스터 인덱스(**Non-Clustered Index, 보조인덱스)**

![https://slideplayer.com/slide/9415637/28/images/8/Example%3A+Simple+Index+on+GPA.jpg](https://slideplayer.com/slide/9415637/28/images/8/Example%3A+Simple+Index+on+GPA.jpg)

리프 노드에 실제 데이터 값이 아닌 테이블상의 데이터 위치를 지정하는 값을 저장

실제 테이블 데이터 엔트리 순서와 레코드 순서가 같지 않다. → **논리적으로만 행이 정렬되어 있다.**

**테이블 당 여러 개 column에 대해 설정 가능**

Mysql innoDB에서는 unique Contraint, FK를 걸면 자동으로 Non-clustered index가 생성됨

<br />

---
<br />

## 인덱스 생성 시 장/단점

### 장점

- 검색 효율이 좋다.

### 단점

- 검색 이외의 처리 속도가 느리다.
: 데이터 변경 작업(추가, 삭제 등)이 자주 일어날 경우 인덱스를 재작성해야 할 필요가 있기 때문에 성능에 영향을 끼칠 수 있다.
- 인덱스를 저장하는 파일의 용량이 늘어난다.

### 사용하면 좋은 경우

- where 절이나 외래키, join에서 자주 사용되는 column

### 사용을 피해야 하는 경우

- 데이터의 중복도가 높은 column
- 삽입, 삭제, 수정 연산이 자주 일어나는 column

---

## 예상 질문
<details>
<summary>데이터베이스의 인덱스란 무엇인가요?</summary>
  <div markdown="1">
    원하는 데이터를 빨리 찾기 위해 투플의 키 값에 대한 물리적 위치를 기록해둔 자료구조입니다.
  </div>
</details>
<details>
<summary>인덱스를 사용하면 좋은 경우와 피해야 하는 경우를 설명해주세요.</summary>
  <div markdown="1">
    <b>인덱스를 사용하면 좋은 경우</b> <br />
    • where절이나 join문에서 자주 사용되는 column 을 인덱스로 사용하면 검색 효율이 좋아질 수 있습니다. <br />
    <b>인덱스를 피해야 하는 경우</b> <br />
    • 데이터의 삽입, 삭제, 수정 연산이 자주 일어나는 column은 인덱스를 구축하더라도 자주 다시 정렬되어야 되기 때문에 인덱스 사용을 피해야 합니다.
  </div>
</details>
<details>
<summary>인덱스의 알고리즘에는 어떤 것들이 있나요?</summary>
  <div markdown="1">
    1. B-Tree 인덱스 알고리즘 : 칼럼의 값을 변형하지 않고 원래의 값을 이용해 인덱싱하는 알고리즘입니다. <br />
    2. Hash 인덱스 알고리즘 : 해시값을 이용해 인덱싱하는 알고리즘이다. <br />
  </div>
</details>
<details>
<summary>클러스터드 인덱스와 비클러스터드 인덱스란 무엇인지 설명해주세요.</summary>
  <div markdown="1">
    클러스터드 인덱스 : 테이블 당 하나만 생성 가능하며 연속된 키 값의 레코드를 묶어서 같은 블록에 저장하는 방법입니다. <br />
    비클러스터드 인덱스 : 테이블당 여러개 생성 가능하며 리프 노드에 실제 데이터 값이 아닌 테이블상의 데이터 위치를 지정하는 값을 저장하는 방법입니다. <br />
    삭제 이상 : 데이터를 삭제하는 데 의도하지 않은 것이 함께 삭제됨, 정보 손실이 일어남 <br />
  </div>
</details>


## 참고 자료

[https://brunch.co.kr/@skeks463/25](https://brunch.co.kr/@skeks463/25)

[https://velog.io/@kjh03160/DB-Index](https://velog.io/@kjh03160/DB-Index)
