# W****eb Server와 WAS(Web Application Server)****

## 정적 페이지와 동적 페이지

- 정적 페이지 - 즉시 응답가능한, 항상 동일한 컨텐츠
    - 단순 HTML 문서
    - CSS
    - javascript
    - 이미지, 파일 등
- 동적 페이지 - 인자의 내용에 맞게 변경되는 컨텐츠
    - 웹 서버에 의해 실행되는 프로그램을 통해 만들어진 결과물
    *Servlet: WAS 위에서 돌아가는 Java Program

![https://gmlwjd9405.github.io/images/web/static-vs-dynamic.png](https://gmlwjd9405.github.io/images/web/static-vs-dynamic.png)

## Web Server

> 웹 브라우저 클라이언트로부터 HTTP 요청을 받아들이고 HTML 문서와 같은 웹 페이지를 반환하는 컴퓨터 프로그램
> 

사용자가 웹 브라우저에서 어떠한 페이지 요청을 하면 웹 서버에서 그 요청을 받아 **정적 컨텐츠**를 제공하는 서버

- 개념에 있어서는 소프트웨어와 하드웨어로 구분된다.
    - 하드웨어 : Web 서버가 설치되어 있는 컴퓨터
    - 소프트웨어 : 웹 브라우저 클라이언트로부터 HTTP 요청을 받아 정적인 컨텐츠(.html .jpeg .css 등)를 제공하는 컴퓨터 프로그램
- 기능
    - HTTP 프로토콜을 기반으로 클라이언트의 요청을 서비스
        - 정적인 컨텐츠를 WAS를 거치지 않고 제공
        - 동적인 컨텐츠 제공을 위한 요청 전달
            - 동적 컨텐츠를 요청 받으면(Request) WAS에 해당 요청을 넘겨주고, WAS에서 처리한 결과를 클라이언트에게 전달(Response)해주는 역할

## WAS (Web Application Server)

> 인터넷 상에서 HTTP 프로토콜을 통해 사용자 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어로서, 주로 동적 서버 컨텐츠를 수행하는 것으로 웹 서버와 구별이 되며, 주로 데이터베이스 서버와 같이 수행
> 

웹 서버와 웹 컨테이너가 합쳐진 형태로서, 웹 서버 단독으로는 처리할 수 없는 데이터베이스의 조회나 다양한 로직 처리가 필요한 동적 컨텐츠를 제공한다.

- HTTP를 통해 컴퓨터나 장치에 애플리케이션을 수행해주는 미들웨어이다.
- JSP, Servlet 구동환경을 제공해주기 때문에 웹 컨테이너 혹은 서블릿 컨테이너라고도 불린다.
- 웹 컨테이너 : 웹 서버가 보낸 JSP, PHP 등의 파일을 수행한 결과를 다시 웹 서버로 보내주는 역할
    
    **웹 컨테이너**(web container, 또는 **서블릿 컨테이너**)는 [웹 서버](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%84%9C%EB%B2%84)의 컴포넌트 중 하나로 자바 [서블릿](https://ko.wikipedia.org/wiki/%EC%84%9C%EB%B8%94%EB%A6%BF)과 상호작용한다. 웹 컨테이너는 서블릿의 생명주기를 관리하고, [URL](https://ko.wikipedia.org/wiki/URL)과 특정 서블릿을 맵핑하며 URL 요청이 올바른 접근 권한을 갖도록 보장한다.[[1]](https://ko.wikipedia.org/wiki/%EC%9B%B9_%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88#cite_note-1)
    
    웹 컨테이너는 [서블릿](https://ko.wikipedia.org/wiki/%EC%84%9C%EB%B8%94%EB%A6%BF), [자바서버 페이지](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EC%84%9C%EB%B2%84_%ED%8E%98%EC%9D%B4%EC%A7%80)(JSP) 파일, 그리고 서버-사이드 코드가 포함된 다른 타입의 파일들에 대한 요청을 다룬다. 웹 컨테이너는 서블릿 객체를 생성하고, 서블릿을 로드와 언로드하며, 요청과 응답 객체를 생성하고 관리하고, 다른 서블릿 관리 작업을 수행한다.
    
    웹 컨테이너는 웹 컴포넌트 [자바 EE](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_EE) 아키텍처 제약을 구현하고, [보안](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EB%B3%B4%EC%95%88), [병행성](https://ko.wikipedia.org/wiki/%EB%B3%91%ED%96%89%EC%84%B1), [생명주기 관리](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94_%EC%84%9C%EB%B8%94%EB%A6%BF), 트랜잭션, 배포 등 다른 서비스를 포함하는 웹 컴포넌트의 [실행 환경](https://ko.wikipedia.org/w/index.php?title=Runtime_environment&action=edit&redlink=1)을 명세한다.
    
- WAS의 역할
    - WAS = Web Server + Web Container
    - Web Server 기능들을 구조적으로 분리하여 처리하고자하는 목적으로 제시되었다.
        - 분산 트랜잭션, 보안, 메시징, 쓰레드 처리 등의 기능을 처리하는 분산 환경에서 사용된다. 주로 DB 서버와 함께 수행된다.
        

![https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png](https://gmlwjd9405.github.io/images/web/webserver-vs-was1.png)

## 웹 서버와 WAS를 구분하는 이유

### Web Server가 필요한 이유?

클라이언트(웹 브라우저)에 이미지 파일(정적 컨텐츠)을 보내는 과정을 생각해보자.

- 이미지 파일과 같은 정적인 파일들은 웹 문서(HTML 문서)가 클라이언트로 보내질 때 함께 가는 것이 아니다.
- 클라이언트는 HTML 문서를 먼저 받고 그에 맞게 필요한 이미지 파일들을 다시 서버로 요청하면 그때서야 이미지 파일을 받아온다.
- Web Server를 통해 정적인 파일들을 Application Server까지 가지 않고 앞단에서 빠르게 보내줄 수 있다.
- 따라서 Web Server에서는 정적 컨텐츠만 처리하도록 기능을 분배하여 서버의 부담을 줄일 수 있다.

### WAS가 필요한 이유?

웹 페이지는 정적 컨텐츠와 동적 컨텐츠가 모두 존재한다.
사용자의 요청에 맞게 적절한 동적 컨텐츠를 만들어서 제공해야 한다.

- 이때, Web Server만을 이용한다면 사용자가 원하는 요청에 대한 결과값을 모두 미리 만들어 놓고 서비스를 해야 한다.
- 따라서 WAS를 통해 요청에 맞는 데이터를 DB에서 가져와서 비즈니스 로직에 맞게 그때 그때 결과를 만들어 제공함으로써 자원을 효율적으로 사용할 수 있다.

### 그렇다면 WAS가 Web Server의 기능도 모두 수행하면 되지 않을까?

- 기능을 분리하여 서버 부하 방지
    - WAS는 DB 조회나 다양한 로직을 처리하느라 바쁘기 때문에 단순한 정적 컨텐츠는 Web Server에서 빠르게 클라이언트에 제공하는 것이 좋다.
        - WAS는 기본적으로 동적 컨텐츠를 제공하기 위해 존재하는 서버이다.
        만약 정적 컨텐츠 요청까지 WAS가 처리한다면 정적 데이터 처리로 인해 부하가 커지게 되고, 동적 컨텐츠의 처리가 지연됨에 따라 수행 속도가 느려진다.
- 물리적으로 분리하여 보안 강화
    - SSL에 대한 암복호화 처리에 Web Server를 사용
- 여러 대의 WAS를 연결 가능
    - Load Balancing을 위해서 Web Server를 사용
    - fail over(장애 극복), fail back 처리에 유리
    - 특히 대용량 웹 어플리케이션의 경우(여러 개의 서버 사용) Web Server와 WAS를 분리하여 무중단 운영을 위한 장애 극복에 쉽게 대응할 수 있다.
- 여러 웹 어플리케이션 서비스 가능
하나의 서버에서 PHP Application과 Java Application을 함께 사용하는 경우
- 기타
접근 허용 IP 관리, 2대 이상의 서버에서의 세션 관리 등도 Web Server에서 처리하면 효율적이다.
- **즉, 자원 이용의 효율성 및 장애 극복, 배포 및 유지보수의 편의성 을 위해 Web Server와 WAS를 분리한다.**
- Web Server를 WAS 앞에 두고 필요한 WAS들을 Web Server에 플러그인 형태로 설정하면 더욱 효율적인 분산 처리가 가능하다.

## Web Service Architecture

![https://gmlwjd9405.github.io/images/web/web-service-architecture.png](https://gmlwjd9405.github.io/images/web/web-service-architecture.png)

[클라이언트(사용자) → Web Server → WAS → DB]

1. Web Server는 웹 브라우저 클라이언트로부터 HTTP 요청을 받는다.
2. Web Server는 클라이언트의 요청을 WAS에 보낸다.
3. WAS는 관련된 Servlet(클라이언트가 어떠한 요청을 하면 그에 대한 결과를 다시 전송해는 역할을 하는 자바 프로그램)을 메모리에 올린다.
4. WAS는 web.xml을 참조하여 해당 Servlet에 대한 Thread를 생성한다. (스레드 풀 이용)
    
    web.xml : web application의 설정을 위한 deployment descriptor
    
    이때 HttpServletRequest와 HttpServletResponse 객체를 생성해 Servlet에게 전달
    
    1. 스레드는 Servlet의 service() 메소드를 호출
    2. service() 메소드는 요청에 맞게 doGet()이나 doPost() 메소드를 호출
5. doGet()이나 doPost() 메소드는 인자에 맞게 생성된 적절한 동적 페이지를 Response 객체에 담아 WAS에 전달
6. WAS는 Response 객체를 HttpResponse 형태로 바꿔 웹 서버로 전달
7. 생성된 스레드 종료하고, HttpServletRequest와 HttpServletResponse 객체 제거

## 예상 질문

- 정적 페이지와 동적 페이지의 차이를 서술해주세요.
    
    정적 페이지는 항상 동일한 컨텐츠를 가지고 있어 즉시 응답 가능한 페이지이고
    
    동적 페이지는 사용자의 요청이나 인자에 따라 그 내용에 맞게 변화되는 페이지입니다.
    
- Web Server와 WAS의 차이를 설명해주세요.
    
    웹 서버는 정적인 컨텐츠를 처리하고 WAS는 동적인 컨텐츠를 주로 처리합니다.
    하지만 웹 서버 또한 동적인 컨텐츠를 위한 요청 전달 및 응답 전송이 가능합니다.
    
- Web 서비스 구조를 설명해주세요.
    
    [클라이언트(사용자) → Web Server → WAS → DB]
    

## 참고자료

- [이미지 및 개념자료](https://gmlwjd9405.github.io/2018/10/27/webserver-vs-was.html)
- [개념 및 순서](https://codechasseur.tistory.com/25)
