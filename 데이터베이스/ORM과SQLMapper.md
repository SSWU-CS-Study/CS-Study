# ORM과 SQL Mapper
## Persistence (영속성)
- 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성
- 영속성을 갖지 않는 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 모두 잃어버리게 된다.

Mark Richards의 소프트웨어 아키텍처 패턴
![Untitled](https://user-images.githubusercontent.com/68562176/202895276-ba19c68b-a1e4-4bc4-bf4f-e2b3cabd82b3.png)

- 소프트웨어를 개발할 때는 위의 아키텍처와 같이 역할과 관심사에 따라 계층을 구분한다. 계층을 구분하여 해당 계층의 구선요소는 관련 기능만 수행하도록 구현한다.
    
    여기서 **Persistence Layer**란 애플리케이션 영속성을 구현하기 위해 데이터 출처와 그 데이터를 가져오고 다루는 것을 주 관심사로 둔다. 대표적인 구성요소는 Repository, DAO 등이 있다.
    
- persistance layer를 어떻게 구현하느냐에서 따라 JDBC 만을 이용하는 방법과 persistence framework를 이용하여 구현하는 방법 이렇게 크게 2가지로 나눌 수 있다.

## JDBC
- Java Database Connectivity의 약자
- 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API
- 자바 애플리케이션에서 DBMS의 종류에 상관없이 하나의 JDBC API를 이용해 DB 작업을 처리
- 각각의 DBMS는 이를 구현한 JDBC 드라이버를 제공한다.    
    
    <img width="749" alt="스크린샷 2022-11-20 오후 5 33 47" src="https://user-images.githubusercontent.com/68562176/202895319-e1f0804d-cb32-408f-bd20-31b87247e20b.png">

- DB와 연결하여 데이터 연산을 수행하기 위해 다음과 같은 과정을 수행해야함
    
    <img width="1284" alt="스크린샷 2022-11-20 오후 5 37 57" src="https://user-images.githubusercontent.com/68562176/202895332-2ea8fb5e-fa4a-41e4-bb33-8f41dff66bdd.png">
    
    <img width="1510" alt="스크린샷 2022-11-20 오후 5 40 28" src="https://user-images.githubusercontent.com/68562176/202895342-aaa7a4ee-24ac-4df3-b4a6-dd31bdbb9846.png">

    1) 드라이버 로드
    
    2) 데이터베이스와 연결할 통로 역할을 할 connection 객체 생성
    
    3) 쿼리문 생성 및 실행
    
    4) 결과를 위한 객체 생성하여 필요한 데이터를 가져오기
    
    5) 반대의 순서를 실행하여 사용된 자원 해제해주기
    

### 단점

- 간단한 SQL을 실행하는데도 중복된 코드를 반복적으로 사용하게 됨
- 데이터베이스에 따라 일관성 없는 정보를 가진 채로 Checked Exception cjfl
- Connection과 같은 공유 자원을 제대로 반환해주지 않으면 시스템의 자원이 바닥나는 버그 발생

## SQL Mapper
- 객체와 SQL문의 결과를 매핑
    
    <img width="1208" alt="Screen Shot 2022-11-20 at 6 27 37 PM" src="https://user-images.githubusercontent.com/68562176/202895379-3cf822a9-af3f-42f9-a6ea-d1fd66c565b3.png">
    
- SQL문을 이용하여 데이터베이스에 접근해야한다.

### 장점
- 세부적인 SQL을 사용하기 편리하다.

### 단점

- 테이블 필드가 변경될 시 이와 관련된 모든 DAO의 SQL문, 객체의 필드 등을 수정해야 한다.
- 테이블마다 비슷한 CRUD SQL 을 반복적으로 구현해야한다.
- SQL 의존적인 방법

### 자바 프레임워크 종류

Jdbc Template, MyBatis

## ORM
- 객체와 데이터베이스 테이블을 매핑하여 데이터를 객체화하는 기술
    
    <img width="751" alt="Screen Shot 2022-11-20 at 6 32 59 PM" src="https://user-images.githubusercontent.com/68562176/202895406-ebc21ecc-ad57-4ae6-bd36-5caa5c02d3ee.png">

    
- 설정된 객체 간의 관계를 바탕으로 자동으로 SQL문을 생성하여 실행한다.
- ORM의 대표적인 예로 JPA가 있다.
- JPA(Java Persistence API)는 자바 ORM 기술에 대한 API 표준 명세로, 인터페이스를 모아둔 것.
- 따라서 JPA를 사용하려면 JPA 인터페이스를 구현한 ORM 프레임워크를 사용해야함.
- 프레임워크 대표적인 예: Hibernate, EclipseLink, DataNucleus

### 장점

- SQL 쿼리가 아닌 직관적인 코드(메서드)로 데이터 조작
- SQL을 직접 작성하지 않아도 되고, DBMS에 종속적이지 않다.

### 단점

- 프로젝트가 복잡해질수록 난이도가 올라감
- Jpa에서 제공하는 기본 메서드만으로는 복잡한 로직을 수행하기 어렵다. (복잡한 쿼리를 수행하기 위해서 @Query 어노테이션을 활용해 JPQL 문을 직접 작성해주거나, QueryDSL을 활용할 수 있다.)

### 자바 프레임워크 종류

Hibernate, EclipseLink, DataNucleus

## 면접 예상 질문

- 영속성이란 무엇인가요?
    
    데이터를 생성한 프로그램의 실행이 종료되어도 사라지지 않는 데이터의 특성을 말합니다. 데이터는 메모리에서만 존재하기 때문에, 프로그램을 종료하면 모두 잃어버리게 되는데, 데이터베이스를 활용하여 데이터를 저장하면 영속성을 가지도록 할 수 있습니다.
    
- SQL Mapper와 ORM의 차이는?
    
    SQL Mapper는 객체와 SQL문의의 결과를 매핑하지만, ORM은 객체와 데이터베이스의 테이블을 매핑합니다. SQL Mapper를 사용할 때는 SQL 문을 통해 데이터베이스에 접근하게 되지만, ORM을 사용하면 개발 언어를 통해 데이터베이스에 접근이 가능합니다. 따라서 ORM은 개발 언어의 일관성과 가독성을 높여준다는 장점을 가지고 있습니다.
    
- 최근 ORM을 많이 사용하는데 그 이유가 뭐라고 생각하는지?
    
    첫째로 ORM을 사용하면, 개발 언어를 통해 데이터베이스를 접근할 수 있어서 일관성과 가독성을 높일 수 있습니다. 
    
    둘째로 SQL Mapper를 사용하면, 필드가 변경될 경우 이와 관련된 객체와 SQL문을 모두 수행해야하는데, ORM은 객체와 테이블을 매핑하기 때문에, 객체의 필드만 변경하면 됩니다.
    
    셋째로, SQL Mapper의 경우 기본적인 CRUD 연산을 테이블마다 반복적으로 구현해주어야 하지만, ORM의 경우 SQL문을 자동으로 생성해주기 때문에 그러한 과정을 없애고 더 효율적이고 빠르게 개발이 가능하도록 도와줍니다.
    

## 참고
- https://www.youtube.com/watch?v=VTqqZSuSdOk
- https://antstudy.tistory.com/447
- https://dreaming-soohyun.tistory.com/entry/JPA와-MyBatis의-차이-ORM과-SQL-Mapper
