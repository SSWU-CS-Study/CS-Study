## RESTful API란?

> REST API(REpresentational State Transfer)는 웹상에서 사용되는 여러 리소스를 **HTTP URI로 표현**하고, 해당 리소스에 대한 행위를 **HTTP Method로 정의**하여 **설계된 API**를 말한다.
> 

**리소스(HTTP URI로 정의됨)를 어떻게 하겠다(HTTP Method + Payload)고 구조적으로 깔끔하게 표현하는 방법.**

**REST**(**RE**presentational **S**tate **T**ransfer)는 웹을 위한 설계원칙.
REST의 기본 원칙을 성실히 지킨 서비스 디자인은 “RESTful 하다.” 고 표현한다.

RESTful API란 REST API의 설계 가이드를 준수한다는 것이다.
<br />
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2FcWqHLz%2FbtrHOzmDVjH%2FRE31DMePptNgcIQU1CsqK1%2Fimg.png" width=500 />
<br />
---

## REST의 요소

- Resource (자원)
    - 'http://myweb/users'와 같은 URI
        - **자원을 표현하는 URI란 ?**
            
            URI(Uniform Resource Identifier)
            
            인터넷 자원을 나타내는 고유 식별자 이다.  URI 에 "I" 가 Identifier인 것은 인터넷에 있는 자료의 ID를 뜻하는 것이다.
            요청하는 주소가 파일의 위치를 말하는 것(Location)이기 보다는 구분자(Identifier)로 보는 것이다.
            
            - **인터넷 상에서 특정 자원(파일)을 나타내는 유일한 주소**
            - **자원(Resource): 문서, 그림, DB, 이미지, 동영상, 해당 소프트웨어 자체 등**
                ![image](https://user-images.githubusercontent.com/84116086/200160059-b5056d1e-ceb5-493c-96e9-17dbf3625e36.png)
    - 모든 것을 Resource (명사)로 표현하고, 세부 Resource에는 id를 붙임
- Method (행위)
: 자원에 대한 행위는 **HTTP Method(GET, POST, PUT, DELETE 등)**으로 표현한다.
    
    
    | Method | 의미 |
    | --- | --- |
    | POST | Create |
    | GET | Select |
    | PUT | Update |
    | DELETE | Delete |
- Representation of Resource (표현)
: Client가 자원의 상태(정보)에 대한 조작을 요청하면 Server는 이에 적절한 응답표현(Representation) 을 보낸다
REST 에서 하나의 자원은 JSON, XML, TEXT, RSS 등 여러 형태의 Representation 으로 나타내어 질 수 있다
옛날에는 XML 이였지만 현대에는 JSON 으로 표현하고 있다
    
    ```jsx
    HTTP POST, http://myweb/users/ {
    	"users" : {
    		"name" : "terry"
    	}
    }
    ```
    

REST는 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화해주고, HTTP 프로토콜의 표준을 최대한 활용하여 여러 추가적인 장점을 함께 가져갈 수 있게 해줍니다.

---

## REST 특징

**1) Uniform (유니폼 인터페이스)**

HTTP 표준만 맞는다면, 어떤 기술도 가능한 Interface 스타일
Uniform Interface는 URI로 지정한 리소스에 대한 조작을 통일되고 한정적인 인터페이스로 수행하는 아키텍처 스타일

**2) Stateless (무상태성)**

REST는 무상태성 성격을 갖는다. 다시 말해 작업을 위한 *상태정보를 따로 저장하거나 관리하지 않는다.* 
세션 정보나 쿠키정보를 별도로 저장하고 관리하지 않기 때문에 API 서버는 들어오는 요청만을 단순히 처리하면 된다. 
때문에 서비스의 자유도가 높아지고 서버에서 불필요한 정보를 관리하지 않음으로써 구현이 단순해진다.

**3) Cacheable (캐시 가능)**

REST의 가장 큰 특징 중 하나는 HTTP라는 기존 웹표준을 그대로 사용하기 때문에, 웹에서 사용하는 기존 인프라를 그대로 활용이 가능하다. 
따라서 HTTP가 가진 **캐싱 기능이 적용 가능**합니다. 

**4) Self-descriptiveness (자체 표현 구조)**

REST의 또 다른 큰 특징 중 하나는 REST API 메시지만 보고도 이를 쉽게 이해 할 수 있는 **자체 표현 구조**로 되어 있다는 것이다.
메시지만 보고도 무슨 행위를 하는지 직관적으로 이해할 수 있다.

**5) Server-Client 구조**

REST 서버는 API 제공, 클라이언트는 사용자 인증이나 컨텍스트(세션, 로그인 정보)등을 직접 관리하는 구조로 각각의 역할이 확실히 구분되기 때문에 
클라이언트와 서버에서 개발해야 할 내용이 명확해지고 서로 간 의존성이 줄어들게 된다.

**6) 계층형 구조**

REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드 밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트웨이 같은 네트워크 기반의 중간매체를 사용할 수 있게 한다.

---

## REST API 디자인 가이드

REST API 설계 시 가장 중요한 항목은 다음의 2가지로 요약할 수 있다.

**첫 번째,** URI는 정보의 자원을 표현해야 한다.
**두 번째,** 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE)로 표현한다.

### REST API 중심 규칙

---

**1) URI는 정보의 자원을 표현해야 한다. (리소스명은 동사보다는 명사를 사용)**

```jsx
GET /members/show/1
```

위와 같은 방식은 REST를 제대로 적용하지 않은 URI이다. **URI는 자원을 표현하는데 중점을 두어야 한다.** show와 같은 행위에 대한 표현이 들어가서는 안된다.

**2) 자원에 대한 행위는 HTTP Method(GET, POST, PUT, DELETE 등)로 표현**

위의 잘못 된 URI를 HTTP Method를 통해 수정해 보면

```jsx
GET /members/1
```

으로 수정할 수 있다. 회원정보를 가져올 때는 GET, 회원 추가 시의 행위를 표현하고자 할 때는 POST METHOD를 사용하여 표현합니다.

**회원정보를 가져오는 URI**

```jsx
GET /members/show/1     (x)
GET /members/1          (o)
```

**회원을 추가할 때**

```jsx
GET  /members/insert/2 (x)  - GET 메서드는 리소스 생성에 맞지 않는다.
POST /members/2       (o)
```
<br />
---

### RESTful 하면 뭐가 좋지?

'RESTful'하다는것은 REST API 설계 가이드를 준수한다는 것인데, 'RESTful'하면 뭐가 좋을까?

- **self-descriptiveness**: RESTful API는 그 자체만으로도 API의 목적이 무엇인지 쉽게 알 수 있다.

> API를 RESTful 하게 만들어서 **API의 목적이 무엇인지 명확하게** 하기 위해 RESTful 함을 지향한다.
> 
<br />
---

## 예상 질문

- **REST란 무엇이고, RESTful하게 API를 디자인한다는 것은 무엇인지 설명하시오.**
    
    **리소스** 와 **행위** 를 명시적이고 직관적으로 분리하여 API를 작성한다.
    
    - 리소스는 `URI`로 표현되는데 리소스가 가리키는 것은 `명사`로 표현되어야 한다.
    - 행위는 `HTTP Method`로 표현하고, `GET(조회)`, `POST(생성)`, `PUT(기존 entity 전체 수정)`, `PATCH(기존 entity 일부 수정)`, `DELETE(삭제)`을 분명한 목적으로 사용한다.
- **REST API를 사용하면 어떠한 장점이 존재하는가?**
    1. 원하는 타입으로 데이터를 주고 받을 수 있다.
    2. 기존 웹 인프라(HTTP)를 그대로 사용할 수 있다.
    HTML로 렌더링 하는 웹서버가 아닌, JSON 혹은 XML 과 같은 형식을 통해서 데이터를 다루는 별도의 API 서버가 필요하다.
    이때, RESTful 아키텍쳐를 HTTP Method와 함께 사용하면 웹, 데스크탑 앱, 스마트폰 어플들까지 하나의 API 서버(RESTful API)를 생성해 사용할 수 있다.
    3. REST API 메시지가 의도하는 바를 명확하게 나타내므로 의도하는 바를 쉽게 파악할 수 있다.
    4. 서버와 클라이언트의 역할을 명확하게 분리한다.
- **REST API를 사용하면 어떠한 단점이 존재하는가?**
    1. 사용할 수 있는 HTTP Method 형태가 제한적이다.
    2. HTTP 통신 모델에 대해서만 지원한다.

---

## 참고자료

- [https://spoqa.github.io/2012/02/27/rest-introduction.html](https://spoqa.github.io/2012/02/27/rest-introduction.html)
- [https://grape-blog.tistory.com/10](https://grape-blog.tistory.com/10)
- [https://velog.io/@taeha7b/api-restapi-restfulapi](https://velog.io/@taeha7b/api-restapi-restfulapi)
