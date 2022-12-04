# 3. Spring security , Jwt 토큰

분Spring
생성 일시: 2022년 12월 1일 오후 8:18

# 면접 질문

<aside>

📌  **Spring security , Jwt 토큰**

- 보안에서의 인증과 인가의 차이는 무엇인지?
    
    → 인증(Authentication)은 사용자가 누구인지 확인하는 단계를 의미하며 인가 (Authorization)는 인증을 통해 검증된 사용자가 애플리케이션 내부의 리소스에 접근할 때 사용자가 해당 리소스에 접근할 권리가 있는지를 확인하는 과정
    
- JWT란 ?
    
    → JSON Web Token : 당사자간의 정보를 JSON형태로 안전하게 전송하기 위한 토큰
    
- JWT의 구성은?
    
    → 토큰의 타입과 해시 암호화 알고리즘으로 구성되어 있는 헤더, 토큰에 담을 클레임(claim) 정보를 포함하는 페이로드, secret key를 포함한 서명으로 이루어져 있다. 
    
- JWT의 장단점은?
    - 장점
        - 인증된 정보이기 때문에 세션 저장소와 같은 별도의 인증 저장소가 필요하지 않다
        - 세션과는 다르게 클라이언트의 상태를 서버가 저장해두지 않아도 된다
        - 데이터의  보완성이 늘어남
    - 단점
        - 사용자 정보를 조작하더라도 토큰에 직접 적용할 수 없다.
        - 토큰은 거의 모든 요청에 대해 전송되므로 데이터 트래픽 크기에 영향을 미칠 수 있다
    
- JWT의 보완 방안
    
    → Slifing Session, Refresh Token
    
</aside>

# 1. 서비스의 인증과 권한 부여

### 1.1 인증 (Authentication)

> 사용자가 누구인지 확인하는 단계를 의미
> 

 인증의 대표적인 예시로는 Login 이 있으며. 로그인은 DB에 등록된 아이디와 패스워드를 사용자가 입력한 아이디와 비밀번호와 비교해서 일치 여부를 확인하는 과정
로그인에 성공하면 애플리케이션 서버는 응답으로 사용자에게 토큰 (token) 을 전달한다. 

### 1.2 인가 (Authorization)

> 인증(Authentication)을 통해 검증된 사용자가 애플리케이션 내부의 리소스에 접근할 때 사용자가 해당 리소스에 접근할 권리가 있는지를 확인하는 과정
> 

로그인한 사용자가 특정 게시판에 접근해서 글을 보려고 하는 경우 게시판 등급을 확인해 접근을 허가 하거나 거부하는 것이 대표적인 인가의 사례
일반적으로 사용자가 인증 단계에서 발급받은 토큰은 인가 내용을 포함하고 있으며 사용자가 리ㅗ스에 접근하면서 토큰을 함께 전달하면, 애플리케이션 서버는 토큰을 통해 권한 유무 등을 확인해 인가를 수행 합니다. 

### 1.3 접근 주체 (Principal)

> 애플리케이션의 기능을 사용하는 주체
> 

접근 주체는 사용자가 될 수도 있고, 디바이스, 시스템 등이 될 수도 있다. 애플리케이션은 앞서 소개한 인증 과정을 통해 접근 주체가 신뢰할 수 있는지 확인하고, 인가 과정을 통해 접근 주체에게 부여된 권한을 확인하는 과정 드을 거침

# 2. Spring security

> 애플리케이션의 인증, 인가 등의 보안 기능을 제공하는 하위 프로젝트 중 하나
> 

### 2.1 Spring security의 동작 구조

> Client (request) → **`Filter`** → DispatcherServlet → **`Interceptor`** →  Controller
> 

스프링 시큐리티는 서블릿 필터 (Servlet Filter) 를 기반으로 동작하며 DispatherServlet 앞에 필터가 배치되어 있다. Spring Security는 '인증'과 '권한'에 대한 부분을 필터(Filter) 흐름에 따라 처리한다. 요청이 들어오면, 인증과 권한을 위한 필터들을 통하게 된다. 유저가 인증을 요청할때 필터는 인증 메커니즘과 모델을 기반으로 한 필터들을 통과한다.

<aside>
✅ 용어 설명

- `Filter`
    
     Dispatcher Servlet으로 가기 전에 적용되므로 가장 먼저 URL 요청을 받아 웹 컨테이너에서 관리
    
- `Interceptor`
    
    Dispatcher와 Controller 사이에 위치 위치 하며 스프링 컨테이너에서 관리
    
- **GrantAuthority**
    
    ROLE_*ADMIN, ROLE_USER등 Principal이 가지고 있는 권한을 나타내며 .prefix로 'ROLE*' 이 붙는다.
    
    인증 이후에 인가를 할 때 사용하며  권한은 여러개 일수 있기 때문에 Collection<(GrantedAuthority)>형태로 제공
    
- **SecurityContext**
SecurityContext 는 접근 주체와 인증에 대한 정보를 담고 있는 Context 이다. 즉, Authentication 을 담고 있음
- **SecurityContextHolder**
    
    SecurityContext를 제공하는 static 메소드(getContext)를 지원
    

</aside>

### 2.2 Spring security **동작 순서**

<aside>
✅  **How to Login on Spring security**

![**Authentication Architecture**](https://velog.velcdn.com/images/u-nij/post/c37adff6-d4fa-4e0f-a848-b2795a0d0671/image.png)

**Authentication Architecture**

1. 사용자가 로그인 정보와 함께 인증 요청을 한다.

2.**`AuthenticationFilter`** 가 요청을 가로채고, 가로챈 정보를 통해 **`UsernamePasswordAuthenticationToken`** 의 인증용 객체를 생성한다.

3. **` AuthenticationManager `** 의 구현체인 **`ProviderManager`** 에게 생성한 **`UsernamePasswordToken`** 객체를 전달한다.

4. **`AuthenticationManager`** 는 등록된 **`AuthenticationProvider`** 를 조회하여 인증을 요구한다.

5. 실제 DB에서 사용자 인증정보를 가져오는 **`UserDetailsService`** 에 사용자 정보를 넘겨준다.

6. 넘겨받은 사용자 정보를 통해 DB에서 찾은 사용자 정보인 **`UserDetails`** 객체를 만든다.

7. **`AuthenticationProvider`** 는 **`UserDetails`** 를 넘겨받고 사용자 정보를 비교한다.

8. 인증이 완료되면 권한 등의 사용자 정보를 담은 **`Authentication 객체`**

를 반환한다.

9. 다시 최초의 **`AuthenticationFilter`** 에 **`Authentication 객체`** 가 반환된다.

10. **`Authentication 객체`** 를 **`SecurityContext`** 에 저장한다.

</aside>

### 2.3 세션 - 쿠키 방식

<aside>
✅ session- cookie

1. 유저가 로그인을 시도 (http request)
2. AuthenticationFilter 에서부터 user DB까지 타고 들어감
3. DB에 있는 유저라면 UserDetails로 꺼내서 유저의 session 생성
4. spring security의 인메모리 세션저장소인 SecurityContextHolder 에 저장
5. 유저에게 session ID와 함께 응답을 내려줌
6. 이후 요청에서는 요청쿠키에서 JSESSIONID를 까봐서 검증 후 유효하면 Authentication를 쥐어준다
</aside>

# 3. JWT

> JSON Web Token : 당사자간의 정보를 JSON형태로 안전하게 전송하기 위한 토큰
> 

URL로 이용할 수 있는 문자열로만 구성되어 있으며, 디지털 서명이 적용돼 있어 신뢰할 수 있다. JWT는 주로 서버와의 통신에서 권한 인가를 위해 사용된다. URL에서 사용할 수 있는 문자열로만 구성되어 있기 때문에 HTTP 구성요소 중 어디든 위치할 수 있다

### 3.1 JWT의 구조

> JWT는 점(.)으로 구분되어 헤더, 내용, 서명으로 나뉜다
> 

JWT는 세 파트로 나누어지며, 각 파트는 점로 구분하여 xxxxx.yyyyy.zzzzz 이런식으로 표현된다. 

Base64 인코딩의 경우 “+”, “/”, “=”이 포함되지만 JWT는 URI에서 파라미터로 사용할 수 있도록 URL-Safe 한  Base64url 인코딩을 사용한다

![스크린샷 2022-12-04 14.50.17.png](3%20Spring%20security%20,%20Jwt%20%E1%84%90%E1%85%A9%E1%84%8F%E1%85%B3%E1%86%AB%20b389db5e2827468ab5cb9552c69ecb17/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-04_14.50.17.png)

![스크린샷 2022-12-04 14.53.09.png](3%20Spring%20security%20,%20Jwt%20%E1%84%90%E1%85%A9%E1%84%8F%E1%85%B3%E1%86%AB%20b389db5e2827468ab5cb9552c69ecb17/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2022-12-04_14.53.09.png)

- **헤더 (Header)**
    
    토큰의 타입과 해시 암호화 알고리즘으로 구성되어 있다
    
    첫째는 토큰의 유형 (JWT)을 나타내고, 두 번째는 HMAC, SHA256 또는 RSA와 같은 해시 알고리즘을 나타내는 부분
    
- **페이로드 (Payload)**
    
    토큰에 담을 클레임(claim) 정보를 포함하고 있다. Payload 에 담는 정보의 한 ‘조각’ 을 클레임이라고 부르고, 이는 name / value 의 한 쌍으로 이뤄져있으며  토큰에는 여러개의 클레임 들을 넣을 수 있습니다. 클레임의 정보는 등록된 (registered) 클레임, 공개 (public) 클레임, 비공개 (private) 클레임으로 세 종류가 있다.
    
    **중요한 것은 paylod에 민감한 정보를 담지 않는 것이다.** 
    
- **서명 (Sinature)**
    
    secret key를 포함하여 암호화되어 있다.
    서버에 있는 개인키로만 암호화를 풀 수 있으므로 다른 클라이언트는 임의로 Signatyre를 복호화하ㅗ 수 없다. 
    

### 3.2 JWT의 장단점과 목적

- JWT를 권장하는 이유
    - URL 파라미터와 헤더로 사용
    - 수평 스케일이 용이
    - 디버깅 및 관리가 용이
    - 트래픽 대한 부담이 낮음
    - REST 서비스로 제공 가능
    - 내장된 만료
    - 독립적인 JWT

- **`JWT의 장점`**
    
    JWT 의 주요한 이점은 사용자 인증에 필요한 모든 정보는 토큰 자체에 포함하기 때문에 별도의 인증 저장소가 필요없다.
    분산 마이크로 서비스 환경에서 중앙 집중식 인증 서버와 데이터베이스에 의존하지 않는 쉬운 인증 및 인가 방법을 제공한다.
    
    개별 마이크로 서비스에는 토큰 검증과 검증에 필요한 비밀 키를 처리하기위한 미들웨어가 필요합니다. 검증은 서명 및 클레임과 같은 몇 가지 매개 변수를 검사하는 것과 토큰이 만료되는 경우로 구성됩니다. 토큰이 올바르게 서명되었는지 확인하는 것은 CPU 사이클을 필요로하며 IO 또는 네트워크 액세스가 필요하지 않으며 최신 웹 서버 하드웨어에서 확장하기가 쉽습니다.
    
- **`JWT의 단점`**
    - 토큰은 클라이언트에 저장되어 데이터베이스에서 사용자 정보를 조작하더라도 토큰에 직접 적용할 수 없습니다.
    - 더 많은 필드가 추가되면 토큰이 커질 수 있습니다.
    - 비상태 애플리케이션에서 토큰은 거의 모든 요청에 대해 전송되므로 데이터 트래픽 크기에 영향을 미칠 수 있습니다.
    

### 3.2 JWT의 보완

> 짧은 JWT유효 시간을 보완하는 방법
> 
1. **Slifing Session**
    
    특정한 서비스를 계속 사용하고 있는 특정 유저에 대해 만료 시간을 연장시켜 주는 것
    
2. **Refresh Roken**
    
    가장 많이 사용하는 방법으로 JWT를 처음 발급할 때 Acess Token 과 함께 Refresh Token 을 처름 발급할 때 엑세스 토큰과 함께 Refresh Token을 발급하여 짧은 만료 시간 해결
