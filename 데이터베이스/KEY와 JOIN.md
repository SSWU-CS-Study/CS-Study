## KEY

**특정 튜플을 식별할 때 사용하는 속성 혹은 속성의 집합**

슈퍼키
>튜플을 식별할 수 있는 하나의 속성 혹은 속성의 집합
포함하지 않아도 되는 속성을 포함할 수 있다.

후보키
>튜플을 식별할 수 있는 속성의 **최소 집합**
>두 개 이상의 속성으로 이루어진 키는 **복합키**

기본키(PK)
>여러 후보키 중 하나를 선정하여 대표로 삼는 키
>고유한 값을 가져야 하고 NULL값은 허용하지 않는다. ⇒ 개체 무결성 제약조건

대체키
>기본키로 선정되지 않은 나머지 후보키

외래키(FK)
>다른 릴레이션의 기본키를 참조하는 속성, 릴레이션 간의 관계를 표현한다.

<img src="./img/key.png",  width="400px"/>

## JOIN

**두 개 이상의 테이블이나 데이터베이스를 연결하는 연산**

INNER JOIN (내부 조인)
>교잡합으로 테이블의 중복된 값을 보여준다.
OUTER JOIN
> 기준 테이블값과 조인 테이블의 중복된 값을 보여준다.
그림만 보면 왼쪽 테이블의 값만 모두 가져오는 것 같아 무슨 의미가 있나 싶겠지만 실제로는 다른 속성을 가져오기 때문에 다른속성이 없을 때는 `default값`이나 `null값`을 보여준다는 의미가 있다.

예시)
```sql
SELECT Customer.name, saleprice
FROM Customer LEFT OUTER JOIN Orders 
	 ON Customer.custid = Orders.custid
```
**결과 **

| name | saleprice |
| --- | --- |
| ㅇㅇㅇ | 200,000 |
| ㅂㅂㅂ | 14,000 |
| ㅁㅁㅁ | null |

FULL OUTER JOIN
> 합집합으로 두 테이블의 모든 데이터가 검색된다.
CROSS JOIN
> 모든 경우의 수를 전부 표현해주는 방식.
SELF JOIN
>자기자신의 테이블을 조인하는 것
하나의 테이블을 여러번 복사하여 조인한다고 생각하면 편하다.


##### 참고
[https://coding-factory.tistory.com/87](https://coding-factory.tistory.com/87)
MySQL로 배우는 데이터베이스 개론과 실습
