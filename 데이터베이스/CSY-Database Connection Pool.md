# Database Connection Pool

## DB Connection

- DB를 사용하기 위해 DB와 어플리케이션 간 통신을 할 수 있는 수단
- DB Connection은 Database Driver와 Database 연결정보를 담은 URL이 필요하다.
- Java의 DB Connection은 JDBC를 주로 이용하는데, URL타입을 사용한다.

![https://user-images.githubusercontent.com/48278519/147056416-700778f1-5533-4d5f-b58d-bd4ea585fac2.jpg](https://user-images.githubusercontent.com/48278519/147056416-700778f1-5533-4d5f-b58d-bd4ea585fac2.jpg)

## JDBC

- Java Database Connectivity의 약자
- 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API
- 자바 애플리케이션에서 DBMS의 종류에 상관없이 하나의 JDBC API를 이용해 DB 작업을 처리
- 각각의 DBMS는 이를 구현한 JDBC 드라이버를 제공한다.

![https://user-images.githubusercontent.com/68562176/202895319-e1f0804d-cb32-408f-bd20-31b87247e20b.png](https://user-images.githubusercontent.com/68562176/202895319-e1f0804d-cb32-408f-bd20-31b87247e20b.png)

- DB와 연결하여 데이터 연산을 수행하기 위해 다음과 같은 과정을 수행해야함

![https://user-images.githubusercontent.com/68562176/202895332-2ea8fb5e-fa4a-41e4-bb33-8f41dff66bdd.png](https://user-images.githubusercontent.com/68562176/202895332-2ea8fb5e-fa4a-41e4-bb33-8f41dff66bdd.png)

![https://user-images.githubusercontent.com/48278519/147056919-d8006b55-f193-4f59-88fe-02074ae190e2.png](https://user-images.githubusercontent.com/48278519/147056919-d8006b55-f193-4f59-88fe-02074ae190e2.png)

## 커넥션 비용?

WAS(Web Application Server)와 데이터베이스 사이의 연결에는 많은 비용이 든다. MySQL 8.0을 기준으로 INSERT 문을 수행할 때 필요한 비용의 비율은 다음과 같다. 

1. Connecting (3)

2. Sending query to server (2)

3. Parsing query (2)

4. Inserting row (1)

5. Inserting index(1)

6. Closing (1)

```java
Connection conn = null;
PreparedStatement  pstmt = null;
ResultSet rs = null;

try {
    sql = "SELECT * FROM T_BOARD"

    // 1. 드라이버 연결 DB 커넥션 객체를 얻음
    connection = DriverManager.getConnection(DBURL, DBUSER, DBPASSWORD);

    // 2. 쿼리 수행을 위한 PreparedStatement 객체 생성
    pstmt = conn.createStatement();

    // 3. executeQuery: 쿼리 실행 후
    // ResultSet: DB 레코드 ResultSet에 객체에 담김
    rs = pstmt.executeQuery(sql);
    } catch (Exception e) {
    } finally {
        conn.close();
        pstmt.close();
        rs.close();
    }
}
```

즉, 서버가 DB에 연결하기 위한 Connecting 비용이 가장 큰 비율을 차지한다. 이처럼 Connection을 생성하는 작업은 비용이 많이 드는 작업이다. 

또, 한 명의 접속자가 웹 사이트에 접속했다고 가정하자.

접속자는 게시판을 확인하고 자신이 쓴 게시물을 수정하고, 또 새로운 게시물을 등록한다. 그럼 이 한명의 접속자로 인해 DB접속은

- 데이터 취득
- 검색 후 데이터 취득
- 데이터 갱신
- 데이터 새등록

단시간에 4번의 DB 접속이 일어난다. [위의 과정](https://www.notion.so/16-02-12-a752fb6bfbf9489cb75f78b7ca8052f0)을 4번 반복하는 것이다.
만약 접속자가 수천, 수만명으로 늘어난다면 과부하가 올 수 있는 것이다.

이를 보완할 수 있는 방법이 바로 **Connection Pool**이다.

## 커넥션 풀(Connection Pool)이란?

![https://blog.kakaocdn.net/dn/k0H3I/btrAbvRB3hT/0DUKGMlajXYSMeNmDlMCH1/img.gif](https://blog.kakaocdn.net/dn/k0H3I/btrAbvRB3hT/0DUKGMlajXYSMeNmDlMCH1/img.gif)

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2321E63A589F1D5532](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F2321E63A589F1D5532)

커넥션 풀은 데이터베이스와 연결된 일정 개수의 커넥션을 미리 만들어 놓고 이를 pool로 관리하는 것이다. 즉, 필요할 때마다 커넥션 풀의 커넥션을 이용하고 반환하는 기법이다. 
이처럼 미리 만들어 놓은 커넥션을 이용하면 Connection에 필요한 비용을 줄여 DB에 빠르게 접속할 수 있다.

~~1. Connecting (3)~~

2. Sending query to server (2)

3. Parsing query (2)

4. Inserting row (1)

5. Inserting index(1)

6. Closing (1)

또한 커넥션 풀을 사용하면 커넥션 수를 제한할 수 있어서 과도한 접속으로 인한 서버 자원 고갈을 방지할 수 있으며 DB 접속 모듈을 공통화해 DB 서버의 환경이 바뀔 경우 유지보수를 쉽게 할 수 있다.

### 정리

- 여러 개의 DB Connection을 하나의 Pool에 모아놓고 관리
- DB 커넥션 객체를 여러 개 생성한 뒤 Pool에 담아놓고 필요할 때 불러와서 사용
- Pool에 미리 connection이 생성되어 있기 때문에 connection을 생성하는 데 드는 요청마다 연결 시간이 소비되지 않는다.
- 만약, 빌려줄 수 있는 Connection이 없다면 Connection 객체가 반환할 때 까지 클라이언트는 대기 상태로 전환
- 사용이 끝난 커넥션 객체는 다른 작업에서 다시 사용할 수 있도록 Pool에 반환
⇒ 클라이언트가 사용을 마친 후에는 Pool에 다시 Connection 객체를 반환하기 때문에 데이터베이스 접근 요청이 존재하더라도 새로운 Connection 객체를 생성할 필요가 없다.

### DBCP의 종류

커넥션 풀에는 Apache의 Commons DBCP와 Tomcat-JDBC, BoneCP, HikariCP 등이 있다.

스프링 같은 경우 스프링부트2.0부터 기본적으로 `HikariCP` 를 사용한다.

## 커넥션 풀의 크기와 성능

커넥션 풀이 크면 클수록 좋을까?

Connection을 사용하는 주체인 Thread의 개수보다 커넥션 풀의 크기가 크다면 사용하지 않고 남는 커넥션이 생겨 메모리의 낭비가 발생하게 된다.

MySQL의 [공식레퍼런스](https://dev.mysql.com/doc/connector-j/8.0/en/connector-j-usagenotes-j2ee-concepts-connection-pooling.html)에서는 600여 명의 유저를 대응하는데 15~20개의 커넥션 풀만으로도 충분하다고 언급하고 있다. MySQL은 최대 연결 수를 무제한으로 설정한 뒤 부하 테스트를 진행하면서 최적화된 값을 찾는 것을 추천한다.

[커넥션 풀과 성능에 대하여](https://wiki.postgresql.org/wiki/Number_Of_Database_Connections)

![https://velog.velcdn.com/images%2Fmooh2jj%2Fpost%2F437ad95c-58bb-4a4a-b898-a00eb2fbe09c%2Fimage.png](https://velog.velcdn.com/images%2Fmooh2jj%2Fpost%2F437ad95c-58bb-4a4a-b898-a00eb2fbe09c%2Fimage.png)

![https://velog.velcdn.com/images%2Fmooh2jj%2Fpost%2Fd969a678-2176-4ccd-a32f-60494e4991be%2Fimage.png](https://velog.velcdn.com/images%2Fmooh2jj%2Fpost%2Fd969a678-2176-4ccd-a32f-60494e4991be%2Fimage.png)

## 예상 질문

- Connection Pool이란?
    
    DB와의 커넥션 객체를 미리 할당을 해서 Pool에 넣어둡니다. 
    이후 DB연결 요청이 들어왔을때 Pool의 있는 Connection을 사용하고, 
    작업이 끝나면 반납하는방식입니다. 
    Connection을 잇는 작업을 중복으로 처리되지 않도록해서 성능을 올리는 방법이라고 볼 수 있습니다.
    
- Connection Pool은 왜 쓰는가?
    
    Connection Pool을 사용할경우, 연결이 되어있는 Connection이 이미 Pool에 있기 때문에, Connection을 매번 지어줄 필요가 없습니다.
    따라서, 기존의 작업을 할때마다 conection을 맺고 끊는 과정을 줄여서 성능을 향상시킬 수 있습니다.
    
- 실시간 통신과 Pool 사용 시의 차이가 무엇인가요?
    
    실시간 통신은 DB에 작업을 할떄마다
    
    1. DB connection을 만든다.
    2. DB의 작업을 처리한다
    3. DB와 Connection을 해제한다.
    의 과정을 거치지만, Pool을 사용하면 1번과 3번의 과정이 생략이되기에, 많은 request처리에서 성능이 더 좋습니다..
    
    하지만, Connection도 객체기 때문에, 너무 많이 만들어 두게되면 메모리를 더 많이 소모해서 오히려 성능이 떨어지기에 주의해야합니다.
    

## 참고 자료

- [DB 커넥션 풀이란?](https://code-lab1.tistory.com/209)
- [사진, 예시 참고](https://brownbears.tistory.com/289)
- [JDBC 유정님 자료](https://github.com/SSWU-CS-Study/CS-Study/blob/main/%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4/ORM%EA%B3%BCSQLMapper.md)
- [예상질문](https://burning-camp.tistory.com/91)
