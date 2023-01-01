# 저장 프로시저(Stored Procedure)

> 일련의 쿼리를 마치 하나의 함수처럼 실행하기 위한 쿼리의 집합
> 

SQL Server에서 제공되는 프로그래밍 기능이다.

프로시저를 만들어두면, 애플리케이션에서 여러 상황에 따라 해당 쿼리문이 필요할 때 인자 값만 전달하여 쉽게 원하는 결과물을 받아낼 수 있다.

**저장 프로시저의 정의 단계**

![https://velog.velcdn.com/images/sweet_sumin/post/217c6c81-8206-4bce-aff8-4ec30052264e/image.png](https://velog.velcdn.com/images/sweet_sumin/post/217c6c81-8206-4bce-aff8-4ec30052264e/image.png)

**저장 프로시저의 실행 단계**

![https://velog.velcdn.com/images/sweet_sumin/post/f3e37ce8-3253-438f-864d-7402214ccc86/image.png](https://velog.velcdn.com/images/sweet_sumin/post/f3e37ce8-3253-438f-864d-7402214ccc86/image.png)

**매개변수**를 받을 수 있는 [PL/SQL BLOCK](https://www.notion.so/11-01-01-2379634dda4e4689bde7524a74c68ad9)이다.

![https://www.oreilly.com/api/v2/epubs/9780596805401/files/httpatomoreillycomsourceoreillyimages322998.png](https://www.oreilly.com/api/v2/epubs/9780596805401/files/httpatomoreillycomsourceoreillyimages322998.png)

`Header` : 프로시저의 이름, 파라미터 선언

`Declaration Section` : 실행부에서 사용할 변수 선언

`Execution Section` : 실행 내용, BEGIN부터 END 까지를 말한다.

`Exception Section` : 오류에 대한 예외 처리 선언 (선택적 선언)

프로시저의 호출 : `EXECUTE 프로시저_명(파라미터_명1, 파라미터_명2, ...);`

대입연산자 : `:=`

### 프로시저 예시

```sql
CREATE OR REPLACE PROCEDURE TEST_PROCEDURE -- TEST_PROCEDURE 프로시저 생성
(
	V_DATE IN CHAR(8) -- 파라미터
)
IS
V_CNT NUMBER := 0; -- 내부에서 사용할 변수

BEGIN -- 프로시저 시작

-- 제어부
IF V_DATE < "20210215" THEN
SET
  V_DATE = "20210201";
END IF;

-- SQL
SELECT SUM(C_CNT)
INTO V_CNT 
FROM TEST_TABLE
WHERE C_DATE = V_DATE;
-- SELECT 문에서 나온 결과는 INTO 문에 선언된 변수에 값이 전달
-- (SELECT에서 조회된 결과가 없을 경우에는 NO_DATA_FOUND 라는 결과가 발생합니다.)

-- 예외부
EXCEPTION
	WHEN NO_DATA_FOUND THEN
		SET V_CNT = 0;

INSERT INTO TEST_TABLE(
	C_DATE,
	C_CNT)
VALUES(
	V_DATE,
	V_CNT);

COMMIT; -- 트랜잭션 완료
END; -- 프로시저 종료
```

```sql
EXECUTE TEST_PROCEDURE('20210205');
```
<br />

---

<br />


## 프로시저 vs 함수

둘 다 미리 작성된 스크립트 구문을 일괄처리함.

| 프로시저 | 함수 |
| --- | --- |
| 넓은 의미로는 어떤 업무를 수행하기 위한 절차 | 프로시저의 각 프로세스를 수행하기 위해 필요한 기능들 |
| 리턴값이 있을수도 없을수도 있다. (IN 또는 OUT) | 리턴값이 필수이다. |
| 서버에서 실행되기 때문에 속도가 빠르다. | 클라이언트에서 실행되기 때문에 프로시저보다는 느리다. |

```sql
//날짜를 입력하면 YYYY-MM-DD 형태로 바꿔주는 함수
CREATE OR REPLACE FUNCTION testDate ( date Date )
RETURN VARCHAR2
IS
       changeDate VARCHAR2 ( 20 ) ;    --사용할 변수
BEGIN 
       changeDate := NULL ;
       changeDate := TO_CHAR ( date, 'YYYY-MM-DD' ) ;
       RETURN changeDate ;
END ;
```

### 트리거

- 트리거는 특정 테이블에 대한 이벤트에 반응해 INSERT, DELETE, UPDATE 같은 DML 문이 수행되었을 때, 데이터베이스에서 자동으로 동작하도록 작성된 프로그램
- 사용자가 직접 호출하는 것이 아닌, 데이터베이스에서 자동적으로 호출한다는 것이 가장 큰 특징

---

## 저장 프로시저의 장/단점

**장점**

- 여러 개의 쿼리를 한 번에 실행할 수 있고, 캐시에 있는 것을 가져와 사용하므로 속도가 빨라진다.
- 유지보수 및 재활용 측면에서 좋다.
: C#, Java등으로 만들어진 응용프로그램에서 직접 SQL문을 호출하지 않고 저장 프로시저의 이름을 [호출하도록 설정](https://daspace.tistory.com/104)하여 사용하는 경우가 많은데, 이때 개발자는 수정요건이 발생할때 코드 내 SQL문을 건드리는게 아니라 **저장 프로시저 파일만 수정하면 되기** 때문에 유지보수 측면에서 유리해진다.
- 클라이언트에서 서버로 쿼리의 모든 텍스트가 전송될 경우 부하가 발생하지만, 저장 프로시저의 경우엔 프로시저의 이름, 매개변수 등만 전달하면 되므로 부하를 크게 줄일 수 있다.

**단점**

- DB의 확장이 어렵다.
- 문제가 생겼을 경우 추적이 힘들다.

---

## PL/SQL

: Oracle’s Procedural Language extension to SQL (절차형 SQL)

응용 프로그램에서의 데이터베이스 처리 성능을 향상시키기 위해 SQL문장에서 변수 정의, 조건 처리(IF), 반복 처리(LOOP, WHILE, FOR)등을 지원하며, 오라클 자체에 내장되어 있는 Procedure Language이다.

다양한 저장 모듈을 개발할 수 있으며 종류에는 **프로시저, 사용자 정의 함수, 트리거**가 있다.

블록 구조로 되어있어 다수의 SQL문을 한 번에 DB로 보내 처리하므로 수행 속도를 향상시킬 수 있다. 

<br />

---

## 면접 예상 질문

<details>
  <summary>저장 프로시저란 무엇인가요?</summary>
  <div markdown="1">
    특정 작업을 수행하기 위해 단일 단위로 실행되는 SQL 쿼리 그룹을 저장 프로시저라고합니다.
  </div>
</details>
<details>
  <summary>저장 모듈의 종류와 특징을 설명해주세요.</summary>
  <div markdown="1">
    
    저장 프로시저는 특정 작업을 수행하기 위해 단일 단위로 실행되는 SQL 쿼리 그룹을 저장 프로시저라고합니다. 캐시되어 있는 것을 사용하여 속도가 빠르다는 특징이 있습니다.
    <br />
    사용자 정의 함수는 프로시저와 비슷해보이지만 리턴값이 필수로 요구되고 프로시저보다는 느립니다.
    <br />
    트리거는 특정 이벤트가 발생했을 때마다 자동으로 실행되는 작업을 말합니다.
  </div>
</details>
    
    
<br />

### 참고

- [DB 프로시저](https://benggri.tistory.com/76)
- [저장프로시저정의](https://velog.io/@sweet_sumin/%EC%A0%80%EC%9E%A5-%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80-Stored-Procedure)
- [오라클 PL/SQL과 블록 구조 및 특징](https://hoon93.tistory.com/38)
- [프로시저와 함수의 차이](https://mjn5027.tistory.com/47)
