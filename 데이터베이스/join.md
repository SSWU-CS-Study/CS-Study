# [MySQL] JOIN

![Untitled (1)](https://user-images.githubusercontent.com/68562176/204131959-3c6d282a-2166-4dd2-989e-71f9731715b9.png)

- JOIN은 크게 `INNER JOIN`과 `OUTER JOIN`으로 분류된다.
- INNER JOIN은 조건에 해당하는 데이터만 뽑아낸다.
- OUTER JOIN 에는 다음과 같이 분류되지만, MySQL에서는 FULL OUTER JOIN을 지원하지 않는다.
    - `LEFT OUTER JOIN`
    - `RIGHT OUTER JOIN`
    - `FULL OUTER JOIN` (MySQL에서는 지원 X)
- 예시에서 사용할 student 테이블
<img width="552" alt="Screen Shot 2022-11-27 at 5 17 35 PM" src="https://user-images.githubusercontent.com/68562176/204131964-4fbb1f39-8c79-4aa8-aa65-aae3c1cd4048.png">


- 예시에서 사용할 grade 테이블
<img width="255" alt="Screen Shot 2022-11-27 at 5 17 48 PM" src="https://user-images.githubusercontent.com/68562176/204131970-a345d007-e0e7-4cdf-bc13-357ff4a05df3.png">


## INNER JOIN (= JOIN, CROSS JOIN)
- Inner Join이란 조인되는 키 값을 기준으로 교집합인 데이터를 조회
- **MySQL에서 JOIN, CROSS JOIN, INNER JOIN은 구문적으로 동등하다. 결과가 같고, 서로 대체 가능하다.**
    
    (표준 SQL에서는 동일하지 않다. 보통 INNER JOIN은 ON절과 함께 쓰이고, CROSS JOIN은 다르게 사용된다. CROSS JOIN이란 한 쪽 테이블의 행 하나에 다른 쪽 테이블의 행을 각각 조인시키는 걸 말한다. 위의 사진 참고.)
    

### ON 조건 없이 사용

MySQL에서는 JOIN 또는 INNER JOIN을 할 때, ON 으로 조건을 지정해주지 않으면 `CROSS JOIN`의 결과가 나온다.

`JOIN`

```sql
SELECT *
FROM student
JOIN grade;
```

`INNER JOIN`

```sql
SELECT *
FROM student
INNER JOIN grade;
```

`CROSS JOIN`

```sql
SELECT *
FROM student
CROSS JOIN grade;
```

- 결과: 위의 3개 쿼리 모두 동일한 결과가 나오며, ON 조건이 없기 때문에 CROSS JOIN의 결과가 반환된다.
    
    <img width="527" alt="Screen Shot 2022-10-19 at 11 22 20 PM" src="https://user-images.githubusercontent.com/68562176/204132006-a5b12928-d104-4e00-8dd5-8775e1148326.png">


### ON 조건과 함께 사용
![Untitled (2)](https://user-images.githubusercontent.com/68562176/204132049-119e4c1a-25ef-4b73-8f0b-9a930c131362.png)
```sql
SELECT *
FROM student s
JOIN grade g
ON s.grade = g.grade;
```

* 결과
<img width="329" alt="Screen Shot 2022-10-19 at 11 21 28 PM" src="https://user-images.githubusercontent.com/68562176/204132067-acd01e4c-f6b8-4858-b128-09220b69af29.png">

## OUTER JOIN

### LEFT JOIN(= LEFT OUTER JOIN)
![Untitled (3)](https://user-images.githubusercontent.com/68562176/204132092-2824f1da-0016-4de8-acb2-a3f805b518ba.png)
- Outer Join은 기준이 되는 테이블에 조인되는 키를 기준으로 다른 테이블을 합친 결과를 조회
- 왼쪽 테이블을 기준으로 합치며, ON 절에 있는 조건 컬럼의 값이 같은 데이터를 합친다. 오른쪽 테이블에 값이 같은 데이터가 없다면, NULL이 삽입된다.

`JOIN` (조건 컬럼의 값이 같은 데이터가 있을 때만)

```sql
SELECT *

FROM grade g
JOIN student s
ON g.grade = s.grade;
```
<img width="336" alt="Screen Shot 2022-10-20 at 3 07 17 AM (1)" src="https://user-images.githubusercontent.com/68562176/204132180-84e083fa-971c-4c92-bf2e-79b7396bb204.png">

`LEFT JOIN`

```sql
SELECT *
FROM grade g
LEFT JOIN student s
ON g.grade = s.grade;
```
<img width="340" alt="Screen Shot 2022-10-20 at 3 07 32 AM (1)" src="https://user-images.githubusercontent.com/68562176/204132190-750941b4-b38c-46e1-ba04-0f724cc83450.png">


### RIGHT JOIN(= RIGHT OUTER JOIN)

오른쪽 테이블을 기준으로 합치며, 나머지 설명은 LEFT JOIN과 같다.

`JOIN` (조건 컬럼의 값이 같은 데이터가 있을 때만)

```sql
SELECT *
FROM student s
JOIN grade g
ON s.grade = g.grade;
```
<img width="336" alt="Screen Shot 2022-10-20 at 3 07 17 AM" src="https://user-images.githubusercontent.com/68562176/204132121-1fc8b4f7-be9a-4740-b00e-47faa15a9b2d.png">

`LEFT JOIN`

```sql
SELECT *
FROM student s
RIGHT JOIN grade g

ON s.grade = g.grade;
```
<img width="330" alt="Screen Shot 2022-10-20 at 3 10 11 AM" src="https://user-images.githubusercontent.com/68562176/204132147-ac99a66e-27fa-4cb2-8176-ff3992500ee3.png">


## 면접 예상 질문

- JOIN이란?
    
    복수의 테이블을 결합하여 하나의 테이블로 결과를 출력하는 것이고, Inner Join, Outer Join, Self Join, Cross Join으로 나눌 수 있습니다.
    
- Inner Join과 Outer Join의 차이는?
    
    Inner Join이란 조인되는 키 값을 기준으로 교집합인 데이터를 가져오는 것입니다. Outer Join은 기준이 되는 테이블에 조인되는 키를 기준으로 다른 테이블을 합치는 것입니다. 이 때 동일한 값이 있다면, 해당 데이터를 합치게 되지만, 없다면 Null 데이터를 삽입하게 됩니다.
    

## 참고
- [https://dev.mysql.com/doc/refman/8.0/en/join.html](https://dev.mysql.com/doc/refman/8.0/en/join.html)
- [https://www.educba.com/joins-in-mysql/](https://www.educba.com/joins-in-mysql/)
