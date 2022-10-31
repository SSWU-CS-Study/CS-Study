# 검색창에 google.com을 치면 일어나는 일
![Untitled](https://user-images.githubusercontent.com/68562176/198972682-233b7db5-1bf1-4c94-9932-6ad43c0a74ea.png)

IP 주소 알아내기 → 요청 → 응답 → 브라우저 렌더링

## 1. IP 주소 알아내기
도메인 이름을 IP주소로 변환하는 과정은 DNS 영역을 사용하여 계층적으로 수행된다.
![Untitled (6)](https://user-images.githubusercontent.com/68562176/198972803-72c49446-99f3-41a5-b68e-1213006ea408.png)

1. Local DNS cache에서 google.com(이하 호스트네임)에 대한 IP 주소 검색
2. Local DNS에 IP 주소가 없다면, 다른 DNS Name Server(Root DNS) 정보를 응답
3. Root DNS Server에게 호스트네임에 대한 IP 주소 요청
4. Root DNS Server는 .com 도메인을 관리하는 TLD(Top Level Domain) Name Server 정보 응답
5. TLD에게 호스트네임에 대한 IP 주소 요청
6. TLD는 호스트네임을 관리하는 DNS Server 정보 응답
7. [google.com](http://google.com) 도메인을 관리하는 DNS Server에게 호스트네임에 대한 IP 주소 요청
8. DNS Server는 호스트네임에 대한 IP 주소 응답
9. 캐시에 기록 저장
10. Local DNS Server는 응답으로 받은 IP주소를 캐싱하고 IP 주소 정보로 HTTP 요청
<details>
  <summary>DNS 이해하기</summary>
  
## DNS Servers
권한 있는 이름 서버는 도메인 이름에 대한 DNS 레코드(A, CNAME, MX, TXT 등)를 저장하는 서버입니다. 이 서버는 로컬로 저장된 DNS 영역 파일에 대한 쿼리에만 응답합니다. 우리 네트워크의 서버가 example.com에 대한 A 레코드를 저장했다고 가정하자. 해당 서버는 example.com 도메인 이름의 권한 있는 서버입니다.

## Recursive Nameserver
재귀 네임 서버는 정보 제공을 목적으로 쿼리를 수신하는 DNS 서버입니다. 이러한 유형의 서버는 DNS 레코드를 저장하지 않습니다. 쿼리가 수신되면 캐시 메모리에서 IP 주소에 연결된 주소를 검색합니다. 재귀 네임 서버에 정보가 있으면 쿼리 발송인에게 응답을 반환합니다. 레코드가 없으면 쿼리는 다른 재귀 네임 서버로 전송됩니다. 이 작업은 IP 주소를 제공할 수 있는 권한 있는 DNS 서버에 도달할 때까지 계속됩니다.

## DNS Zones
DNS 영역은 도메인 이름 시스템 내의 관리 공간입니다. 영역은 관리자 또는 특정 엔티티에게 위임된 DNS 네임스페이스의 일부를 구성합니다. 각 영역에는 모든 도메인 이름에 대한 리소스 레코드가 포함됩니다.

## DNS Zone File
DNS 영역 파일은 서버에 저장된 텍스트 파일입니다. 여기에는 해당 영역 내의 모든 도메인에 대한 모든 레코드가 포함됩니다. 영역 파일의 경우 다른 정보보다 먼저 TTL(Time to Live)을 나열해야 합니다. TTL은 DNS 레코드가 서버의 캐시 메모리에 있는 기간을 지정합니다. 영역 파일은 한 줄당 하나의 레코드만 나열할 수 있습니다. 그러면 먼저 SOA(Start of Authority) 레코드가 표시됩니다. SOA 레코드에는 DNS 영역에 대한 주요 권한 있는 이름 서버를 포함한 필수 도메인 이름 정보가 포함되어 있습니다.
</details>

### Root Server (= DNS Root Server)

- 인터넷 뿐만 아니라 DNS의 기능을 담당하는 네임 서버이다.
- 루트 서버는 계층의 꼭대기에 있는 루트 영역(root zone)을 제공하고 루트 영역 파일(root zone file)을 발행한다.
- 루트 영역은 Top level domain의 목록이고, 최상위 도메인(.com, .net, .org), 국가 코드 최상위 도메인(.no, .se, .uk) 및 국가별 로컬 문자로 작성된 ccTLD인 국제화된 최상위 도메인을 포함한다.
![Untitled (1)](https://user-images.githubusercontent.com/68562176/198973401-dbbe264a-a3b1-47eb-ad45-64a81e7f5803.png)

## 2. **요청 전송 (Web Browser(=Client))**
![Untitled (2)](https://user-images.githubusercontent.com/68562176/198973518-1034e9a1-c533-465c-820c-37c18e66a3e8.png)

www(world wide web)서비스이기 때문에 TCP 연결 후 HTTP 요청 전송
<details>
  <summary>TCP 연결은 뭐고 어떻게 하는걸까?</summary>
</details>
<details>
  <summary>추가설명</summary>
  
**Http (HyperText Transfer Protocol)**
- HTTP는 웹 브라우저와 웹 서버간의 통신 규칙을 뜻한다. 웹에서 데이터를 주고받는 웹브라우저와 웹서버 애플리케이션 간에 지켜야할 약속이다.
- HTTP는 TCP를 사용하는 대표적인 응용 프로토콜이다. TCP 기반으로 HTTP가 만들어진다. 그 외에도 FTP, telnet, SMTP 등이 TCP 프로토콜을 사용한다.

**WWW**

- World Wide Web(=W3, =Web)은 인터넷에 연결된 컴퓨터를 통해 사람들이 정보를 공유할 수 있는 전 세계적인 정보 공간을 말한다.
- 팀 버너스리는 하이퍼링크(Hyperlink) 기능을 담은 HTML(HyperText Markup Language)라는 언어를 개발하고, 이 언어로 웹페이지라는 문서를 만들었다. 웹페이지는 .html 이라는 확장자를 가진 문서이다.
- Web Site는 웹페이지의 집합을 말한다.

**Web vs Internet**

- Web은 인터넷에서 동작하는 하나의 서비스일 뿐이다.

**TCP vs UDP**

- 주로 IP 전화나 스트리밍 서비스와 같이 실시간으로 빠르게 전송이 요구되는 곳에는 비연결형 프로토콜인 UDP가 사용된다.
</details>

## 3-1. 요청 처리-1 (Web Server)
- 필요한 요청을 처리하거나, 처리할 수 없는 요청은 WAS로 전달한다.
>정적 콘텐츠의 경우 웹 서버가 처리할 수 있지만, 동적 콘텐츠의 경우 웹 서버가 처리할 수 없다.     
>정적 콘텐츠는 실시간으로 변경할 필요가 없는 데이터이고, 어떤 접속자든 동일한 모습을 반환한다. 웹 서버의 디스크에 저장을 해두고 요청이 있으면 바로 반환해준다.

## 3-2. 요청 처리-2 (WAS, Web Application Server)
![Untitled (3)](https://user-images.githubusercontent.com/68562176/198974216-068f04b2-6d7c-4ef4-a3e4-bef701f1ff0a.png)
![Untitled (4)](https://user-images.githubusercontent.com/68562176/198974257-859d8661-b8d7-43bc-8e61-15b1845e1630.png)

- Web Server로부터 받은 요청과 관련된 Servlet(서블릿)을 메모리에 로딩
- web.xml을 참조하여 해당 Servlet에 대한 Thread생성 (Tread Pool 활용)
- HttpServletRequest, HttpServletResponse 객체를 생성하여 생성된 Servlet에 전달
- doGet() 이나 doPost() 는 인자에 맞게 생성된 적절한 동적 콘텐츠를 Response 객체에 담아 WAS에 전달
- WAS는 Response 객체를 HttpResponse 형태로 바꾸어 Web Server에 전달
- 생성된 Thread를 종료하고 HttpServletRequest, HttpServletResponse 객체 제거
- Tomcat 추가설명
    - Application Server는 서버 사이드 코드를 이용하여 동적인 컨텐츠를 만드는 서버를 의미한다. 톰캣은 정확히 말하면 Servlet Container에 가깝다. Application Server의 필수적이라고 할 수 있는 EJB 기술이 적용되어 있지 않기 때문이다.
    - Servlet Container 오직 서블릿 API만 지원하지만, Application Server는 Java EE(EJB, JMS, CDI, JTA, Servlet API) 전체를 지원한다.

### Spring Container 내에서의 요청 처리 흐름
![Untitled (5)](https://user-images.githubusercontent.com/68562176/198974379-c8396288-5d5f-4056-b9c6-079922b72428.png)

DispatcherServlet이 요청을 받으면, Spring Container의 HandlerMapping을 이용하여 적절한 핸들러를 찾는다. 조회한 핸들러를 실행할 수 있는 Handler Adapter를 조회한다. 그 후 HandlerAdapter를 이용하여 Controller의 메서드를 실행한다.

- DispatcherServlet: Servlet Request를 controller로 전달(요청 위임)
- HandlerMapping: Request URL과 매칭되는 Controller를 선택
- HandlerAdapter: Controller의 메서드 실행
- ViewResolver: Controller에서 view를 return시 해당 view를 찾아 반환

## 4. 브라우저 렌더링
서버로부터 받아온 HTML 파일을 브라우저가 렌더링과정을 거쳐 보여주게 됨.

## 참고

- IP 주소 알아내기
    
    [[HTTP Spring 통신 흐름 과정] HTTP Request 부터 HTTP Response 까지의 여정](https://data-make.tistory.com/714)
    
- IP 주소 알아내기 > Root Server (= DNS Root Server)
    - 내용: https://securitytrails.com/blog/dns-root-servers
    
    - 이미지: [DNS와 작동원리](https://velog.io/@goban/DNS%EC%99%80-%EC%9E%91%EB%8F%99%EC%9B%90%EB%A6%AC)
    
- 2. **요청 전송 (Web Browser(=Client))** > 추가 설명
    
    [쉽게 이해하는 네트워크 3. 인터넷과 월드 와이드 웹(ft. 팀 버너스 리의 위대한 결단)](https://better-together.tistory.com/50)
    
    [코딩교육 티씨피스쿨](http://www.tcpschool.com/webbasic/www)
    
- 3. 요청 처리 (Web Server) > 처리할 수 있는 요청의 기준은 뭘까?
    
    [[Web] 정적 컨텐츠와 동적 컨텐츠란?](https://cceeun.tistory.com/68)
    
- 3-2. 요청 처리-2 (WAS, Web Application Server)

    [Tomcat, Spring MVC의 동작 과정](https://taes-k.github.io/2020/02/16/servlet-container-spring-container/)

    [Dispatcher Servlet 약식 정리](https://www.google.com/url?sa=i&url=https%3A%2F%2Fvelog.io%2F%40paulhana6006%2FDispatcher-Servlet%25EC%2597%2590-%25EB%258C%2580%25ED%2595%259C-%25EC%259D%25B4%25EC%2595%25BC%25EA%25B8%25B0&psig=AOvVaw0EzCNvPu6Fnji6gixCm0A2&ust=1667070112027000&source=images&cd=vfe&ved=2ahUKEwiB5IGgzoP7AhXWtFYBHTfCDc0Qjhx6BAgAEA0)

    [[Servlet] 웹서버 vs 애플리케이션 서버 vs 서블릿 컨테이너 - Tomcat 은 서블릿 컨테이너일까?](https://pjh3749.tistory.com/267)
