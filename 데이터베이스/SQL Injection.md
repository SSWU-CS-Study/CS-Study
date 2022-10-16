## SQL injection

> 응용 프로그램 보안 상의 허점을 의도적으로 이용해서 악의적인 SQL문을 실행되게 함으로써 데이터베이스를 비정상적으로 조작하는 코드 공격 방법.
> 
<br />
일반적인 쿼리문 : 

```sql
txtUserId = getRequestString("UserId");
txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
```
<br />


### 공격 방법

1. 인증 우회 <br />
: 아이디와 비밀번호를 입력하는 페이지가 있다고 하자. SQL injection으로 공격할 때 input창에 비밀번호와 다른 쿼리문을 함께 입력하여 데이터베이스에 영향을 준다. 이외에도 기본 쿼리문의 WHERE절에 OR문을 추가하여 `'1' = '1'` 과 같은 true문을 작성하여 무조건 적용되도록 수정한 뒤 DB를 마음대로 조작할 수 있다.
    
    ```sql
    //사용자 정보를 가져올 수 있음
    SELECT UserId, Name, Password FROM Users WHERE UserId = 105 or 1=1;
    
    txtUserId = getRequestString("UserId");
    txtSQL = "SELECT * FROM Users WHERE UserId = " + txtUserId;
    
    //이때 **txtUserId가 105; DROP TABLE Suppliers**가 되면
    SELECT * FROM Users WHERE UserId = 105; DROP TABLE Suppliers;
    //이런 식으로 DB 조작이 가능해짐.
    ```
    <br />
2. 데이터 노출(Blind SQL Injection) <br />
: 시스템에서 발생하는 **에러 메세지를 이용**해 공격하는 방법. 쿼리의 참과 거짓에 대한 반응을 구분을 통한 **TABLE 정보 획득 공격 기법**
GET 방식으로 동작하는 URL쿼리스트링을 추가하여 에러를 발생시킨다. 이때 오류가 발생하면, 데이터베이스의 **에러 메세지**를 반환한다. 이 에러 메세지를 통해 해당 웹앱의 데이터베이스 구조를 유추할 수 있고 해킹에 활용된다.
    
    ```sql
    ecommerce.com/books.php?author=john AND 1=2
    SELECT * FROM bookinfo WHERE author='john' AND 1=2; < 에러 발생!
    ```
    
 <br /> <br />
### 방어 방법

1. **Prepared Statement를 사용한다.** <br />
   ⇒ 개발자가 SQL Statement를 미리 준비해두고, Parameter를 입력받아 Statement에서 사용하는 방법입니다. 이 방법은 코드와 데이터를 분리할 수 있도록 하며, 공격자가 SQL Statement의 목적을 바꿀 수 없도록 합니다.
     (구문을 다시 붙여 공격자가 공격자가 SQL명령을 삽입한 경우에도 쿼리의 의도를 변경하지 못하도록 할 수 있다. 공격자가 `'Tom' or '1=1'`의 사용자 ID를 입력하는 경우, 매개 변수화된 쿼리는 쿼리문으로써 작동하지 않는다.) <br />
     ```
     SELECT * FROM bookinfo WHERE author=?, ["john"]
     ```
2. Stored Procedure <br />
   ⇒ (완벽한 방어 기법은 아니지만, 위 방법과 같은 효율을 낼 수 있다.) 자동으로 매개 변수가 매개 변수화 된 SQL문만 작성하도록 개발자가 요구하는 방식.
3. White List <br />
   ⇒ 특정 값의 Parameter만 허용하는 방법으로, 부차적인 보호 기법입니다.
4. Escaping Input <br />
   ⇒ User로부터 받는 모든 값들에 대해서 EScape를 진행한다. 
      문제가 있는 문자열도 모두 plain text로 적용되도록 코드를 작성한다.
 <br /> <br />
---
 <br />

## 예상 질문
  <details>
    <summary>SQL injection에 대해 설명하시오.</summary>
    <div markdown="1">       
      ⇒  SQL injection은 악의적으로 SQL문을 실행해서 비정상적으로 데이터베이스를 조작하는 공격 방법입니다.
    </div>
  </details>
  
  <details>
    <summary>SQL injection을 방어하기 위한 방법을 설명하시오.</summary>
    <div markdown="1">       
      ⇒ Prepared Statement를 사용하여 서버에서 input 값의 필터링 과정을 거쳐 공격을 방어합니다.
    </div>
  </details>
  
  <details>
  <summary>Statement와 Prepared Statement의 차이를 설명하시오.</summary>
    <div markdown="1">       
      - Statement는 한번만 실행될 쿼리에 사용되며 예를 들면 DDL문(CREATE, ALTER, DROP)에 적합합니다. Parameter를 SQL문에 직접 넣습니다.
    - Prepared Statement를 전달인자 값을 `?` 로 받습니다. 서버측에서는 이런 필터링 과정을 통해서 공격을 방어합니다.
    </div>
  </details>

<br /><br />



---
### 🔗참고 링크
[SQL Injection](https://gyoogle.dev/blog/computer-science/data-base/SQL%20Injection.html)
[Blind Injection](https://www.thesecuritybuddy.com/vulnerabilities/what-is-blind-sql-injection-attack/)
