# 쿠키와 세션, 그리고 웹 스토리지 : cookie vs localStorage

HTTP 프로토콜의 약점이나 특징을 보완하기 위해 고안된 것이 쿠키와 세션이다.

쿠키와 세션 위주의 자세한 내용은 [여기](https://github.com/na3150/Today_I_Learn/blob/main/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/%EC%BF%A0%ED%82%A4(cookie)%EC%99%80%20%EC%84%B8%EC%85%98(session).md)에서 살펴볼 수 있다. 

쿠키(cookie)는 앱 또는 웹사이트를 방문하는 사용자들에 대한 정보를 저장하는 주된 방법이었다. 그러다 HTML5가 등장하게 되었고, **localStorage를 데이터 저장 옵션으로 도입**하게 되었다. 본 글에서는 cookies와 localStorage를 중심적으로 비교하고 대조해보려 한다. 

<br>

## 쿠키(cookie)

사용자의 로컬에 저장되는 작은 정보 기록 파일이다. 로컬에 파일 형태로 저장되기 때문에, 요청 속도는 서버를 통하는 다른 저장소보다 비교적 빠르지만, 사용자의 로컬에서 관리되기 때문에 보안적인 부분에서 취약할 수 있다는 단점이 있다.

<br>

🔎 쿠키 확인하기

1. [F12] - [Application] - [Cookies]

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221230224138233.png" width=800/>

2. `document.cookie;`

<br>

Cookie에는 두 가지 유형이 있다. 바로 session cookies와 persistent cookies이다. 

### Session Cookies 

 세션 쿠키는 **만료일을 포함하지 않는다**. 대신, **브라우저나 탭이 열려있는 동안(세션 단위)에만 저장**된다.

브라우저가 닫히면 세션 쿠키는 영구적으로 삭제된다. 이러한 세션 쿠키는 작업을 하고 있다가 탭을 닫으면 모든 정보를 잊어버리기 때문에, 은행 유저들의 자격 증명을 저장하는데 사용될 수 있다.

### Persistent cookies

퍼시스턴트 쿠키는 만료일을 가진다. 따라서 만료일까지 사용자의 로컬에 저장되고, 만료일이 지나면 삭제된다. 사용자들이 방문할 때마다 사용자의 경험을 커스텀하기 위해 특정 웹사이트에서 행동을 기록하는 등 여러 활동들에 사용될 수 있다.

<br>

쿠키는 4KB 용량 제한이 있고, **매 서버 요청마다 서버로 쿠키가 같이 전송**된다. 왜 서버에 쿠키가 전송될까?

HTTP 요청은 상태를 가지고 있지 않기 때문에 서버는 요청 자체만으로는 그 요청이 누구에게서 오는 지 알 수 없다. 그래서 응답을 보낼 수가 없고, **쿠키에 나에 대한 정보를 담아서 서버로 보내면 서버는 쿠키를 읽어서 내가 누군지를 파악한다.** 쿠키는 처음부터 서버와 클라이언트 간의 지속적인 데이터 교환을 위해 만들어졌기 때문에 서버로 계속 전송되는 것이다.

<br>

그러나 이것이 문제가 되기도 한다. 만약 4KB 용량 제한을 거의 다 채운 쿠키가 있다면, 요청을 할 때마다 기본 4KB의 데이터를 사용하게 된다. 4KB 중에는 서버에 필요하지 않은 데이터들도 있을 것이다. 즉, **데이터 낭비**가 발생하는 것이다. HTML5 이전에 사용자의 정보는 이처럼 쿠키에 저장된 채로 매번 서버에 전송되었다. 

**이러한 데이터들을** 이제 HTML5부터 **로컬 스토리지와 세션 스토리지에 저장할 수 있고, 이 두 저장소의 데이터는 서버로 자동 전송되지 않는다.** 웹 스토리지(로컬/세션 스토리지)를 살펴보기에 앞서, 세션에 대해서 간단하게만 짚고 넘어가보자.

<br>

<br>

## 세션(session) 

사용자가 접속한 상태를 하나의 단위로 보고 세션이라 한다. 세션은 쿠키를 기반으로 하지만, 로컬에 저장하는 쿠키와 달리 **세션은 서버에서 관리**된다. 쿠키를 이용하여 session id만 저장하고, 이를 통해 구분하여 서버에서 처리하기 때문에 **보안적인 부분에서는 쿠키보다 우수**하다고 할 수 있다. 다만 요청 속도는 서버의 처리가 필요한 세션에 비해 쿠키가 우수하며, 세션은 (만료 기간을 정할 수는 있으나) **브라우저가 종료되면 그에 상관없이 삭제**된다.

 <br>

<br>

## 웹 스토리지(Web Storage)

웹 스토리지란, **클라이언트에 데이터를 저장**(브라우저 상에 저장)할 수 있도록 **HTML5부터 추가된 저장소**이다. (브라우저의 저장 공간이라고 이해하면 좋)

간단한 Key-Value 스토리지 형태로, 쿠키와 달리 서버에 전송되지 않고 **명시적으로만 전송 가능**하기 때문에 서버에 부담이 가지 않는다. 

필요한 경우에만 꺼내 쓰는 것임으로 **자동 전송의 위험성이 없으며**, 도메인 별 용량 제한이 있어, 프로토콜, 호스트, 포트가 같으면 스토리지를 공유한다. 또한 다른 오리진(도메인, 프로토콜, 포트)이 요청할 때에는 꺼내 쓰고 싶어도 **오리진 단위로 접근이 제한되는 특성** 덕분에 값을 꺼내 쓸 수 없다(CSRF로 부터 안전).

서버가 HTTP헤더를 통해 스토리지 객체를 조작할 수 없고(웹 스토리지 객체 조작은 JavaScript 내에서만 수행), 지속성에 따라 로컬 스토리지(local Storage)와 세션 스토리지(sessionStorage)로 구분할 수 있다. 

<br>

### 로컬 스토리지(localStorage)

HTML5가 등장한 이후 쿠키의 많은 사용 방법들은, 더 많은 장점을 가진 localStorage의 사용으로 대체되었다. 

HTTP 요청에서 데이터를 주고 받지 않는 localStorage를 사용하면, **클라이언트와 서버 간의 전체 트래픽과 낭비되는 대역폭의 양을 줄일 수 있다.** 또한 로컬 스토리지는 직접 지우지 않는다면 **별도의 만료 기간이 없고**(영구성), 도메인만 같으면 **전역적으로 데이터가 공유**되는 특성을 가지고 있다. 

그리고 **최대 5MB의 정보를 저장**할 수 있다. (이는 쿠키가 보유할 수 있는 4KB보다 훨씬 크다)

따라서 로컬 스토리지는 쿠키보다 더욱 영구적인 솔션이라 할 수 있다.

<br>

### 세션 스토리지(sessionStorage)

세션 스토리지는 데이터가 오리진(도메인, 프로토콜, 포트) 뿐만 아니라 **<u>브라우저 탭에서 종속</u>되기 때문에, 윈도우나 브라우저 탭을 닫을 경우 제거**된다. 따라서 일시적으로 필요한 데이터를 저장(일회성 로그인 정보, 입력폼 저장 등)할 때 사용할 수 있다. 

즉, 세션 스토리지는 세션(프로세스, 탭, 브라우저)이 종료될 때까지 지속되는 스토리지이다.

|             | 로컬 스토리지                           | 세션 스토리지                    |
| ----------- | --------------------------------------- | -------------------------------- |
| 데이터 영구 | O (사용자가 지우지 않는 한)             | X (윈도우, 탭 닫을 시 내용 제거) |
| 사용 방법   | 자동 로그인                             | 일회성 로그인                    |
| 주의사항    | 비밀번호와 같은 중요 정보는 절대 저장 X |                                  |

🔎 크롬에서 로컬 스토리지와 세션 스토리지 확인하기

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/F12%20-%20local%2C%20session.png" width=800/>

<br>

#### 🔎 **로컬 스토리지 vs 쿠키 - 로컬 스토리지의 우위성**

쿠키와 달리 로컬(웹) 스토리지는 javascript 객체 자체를 넣는 것 역시 가능하며, 문자열에 크기 제한이 있는 cookie 비해서 비교 우위를 가진다. 또한 세션을 이용하는 경우, request와 response 시에 모든 쿠키를 다 넘겨야 했는데, 이는 원래 비용이 큰 HTTP 통신에 더 큰 부하를 주게 된다. 반면에 웹 스토리지는 그냥 개발자가 선별해서 넘기면 되므로, HTTP 통신의 부하를 줄일 수 있다.

<br>

브라우저가 종료되면 자동으로 사라지는 쿠키를 재현하기 위해서 sessionStorage를 도입하였다. 세션 스토리지의 존재로, 세션이 유지되는 동안만 필요한 데이터를 저장할 수 있게 되었다.

<br>
또한 쿠키는 domain 별로 데이터가 분리되지만, 같은 브라우저라면 값을 공유한다. 반면에 세션 스토리지는 다른 탭이라면 데이터가 공유되지 않고, 만약 공유할 데이터라 로컬 스토리지에 넣으면 되기 때문에 선택의 폭이 커졌다.

정리하자면 다음과 같다.

<br>

**1. 제한**
cookie - 용량 제한, 시간 제한, 개수 제한 존재
webstorage - 용량 제한만 존재​

**2. 시간 제한 설정**
cookie - 시간 제한 설정 가능
webstorage - 시간 제한 설정 불가능​

**3. 데이터형**
cookie - 문자열만 저장 가능
webstorage - javascript 객체 저장 가능​

**4. 데이터 전송**
cookie - 모든 쿠키를 전송해야함, cookie를 가공함으로써 발생하는 side effect 존재
webstorage - 개발자가 선택해서 전송할 수 있고 가공할 수도 있음​

**5. 세션의 정의**
cookie - 같은 브라우저면 다른 탭이나 다른 창(프로세스)일지라도 같은 세션이라고 정의
webstorage - 같은 브라우저일지라도 sessionStorage의 경우 다른 탭이면 다른 세션이라고 정의​

**6. 이벤트**
cookie - 이벤트 없음
webstroage - 이벤트 존재

| -                     | cookies                                      | local storage                                | session storage                                 |
| --------------------- | -------------------------------------------- | -------------------------------------------- | ----------------------------------------------- |
| 용량                  | 4KB                                          | 5MB                                          | 5MB                                             |
| 브라우저              | HTML4 / HTML5                                | HTML5                                        | HTML5                                           |
| 접근 가능 경로        | 동일 브라우저의 모든 window로 부터 접근 가능 | 동일 브라우저의 모든 window로 부터 접근 가능 | 같은 tab(브라우저 context)으로 부터만 접근 가능 |
| 만료                  | 수동으로 만료 기간 설정                      | 만료 x (명시적으로 지워야 함)                | 탭을 닫을 시 자동으로 삭제 됨                   |
| 저장 위치             | 브라우저, 서버                               | only 브라우저                                | only 브라우저                                   |
| 요청과 함께 전송 여부 | yes (request header에 담겨 전송됨)           | no                                           | No                                              |

<br>

<br>

마지막으로, 각 예를 어느 경우에 사용하는 것이 좋을까?

- 자동 로그인 -> 로컬 스토리지
- 입력 폼 정보 -> 세션 스토리지
- 비로그인 장바구니 -> 세션 스토리지
- 7일 동안 다시 보지 않음 팝업 창 -> 쿠키(로컬 스토리지는 만료 기한이 없기 때문)

<br>

<br>

## 기술면접/예상질문 

<details>
<summary>쿠키와 세션의 차이에 대해서 설명해주세요.</summary>




<br>

쿠키와 세션의 가장 큰 차이점은 정보가 저장되는 위치입니다. 쿠키는 클라이언트 측에서, 세션은 서버 측에서 저장되고 관리됩니다. 사용자의 편의를 위하되, 지워지거나 가로채거나 했을 때 큰 문제가 생기지 않을 만한 정보들은 쿠키에, 사용자나 다른 누군가에게 노출되어서는 안되고 서비스 제공자가 직접 관리해야하는 정보는 세션에 저장되게 됩니다. 

다만, 모두 세션에 저장하게 된다면 접속자가 많을 때 서버에 부담이 커지므로, 개발자의 판단하에 적절하게 분배해서 저장해두어야 합니다.

</details>

<br>

<details>
    <summary>웹 스토리지에 대해서 설명해 주세요.</summary>



<br>

웹 스토리지는 HTML5부터 고안된 key-value 저장소로, 로컬 스토리지와 세션 스토리지가 있습니다.

모든 요청마다 서버로 전송되는 쿠키와 달리, 웹 스토리지는 필요한 경우에만 사용하는 것이기 때문에 자동 전송되지 않고, 쿠키 대신 웹 스토리지를 사용한다면, 트래픽과 낭비되는 대역폭의 양을 줄일 수 있습니다. 

로컬 스토리지는 별도의 만료기간이 없는 반면에, 세션 스토리지는 만료기간을 가진다는 가장 큰 차이점이 있고, 로컬 스토리지는 도메인에 종속되는 반면, 세션 스토리지는 브라우저 탭에 종속된다는 특징이 있습니다. 

</details>

<br>

<br>

<br>

<br>

<br>

Reference

- [링크](https://erwinousy.medium.com/%EC%BF%A0%ED%82%A4-vs-%EB%A1%9C%EC%BB%AC%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-28b8db2ca7b2)

- [링크](https://www.zerocho.com/category/HTML&DOM/post/5918515b1ed39f00182d3048)

- [링크](https://inpa.tistory.com/entry/JS-%F0%9F%93%9A-localStorage-sessionStorage)