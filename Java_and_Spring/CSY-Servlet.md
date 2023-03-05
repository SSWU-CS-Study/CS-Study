# [Web] Servlet

![https://gmlwjd9405.github.io/images/web/web-service-architecture.png](https://gmlwjd9405.github.io/images/web/web-service-architecture.png)

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

## Web Service의 기본적인 동작 과정

![https://gmlwjd9405.github.io/images/web/servlet-process.png](https://gmlwjd9405.github.io/images/web/servlet-process.png)

1. 사용자가 웹 페이지 form(HTML Form)을 통해 자신의 정보를 입력한다. (Input)
2. Servlet의 doGet() 또는 doPost() 메서드는 입력한 form data에 맞게 DB 또는 다른 소스에서 관련된 정보를 검색한다.
3. 이 정보를 이용하여 사용자의 요청에 맞는 적절한 동적 컨텐츠(HTML Page)를 만들어서 제공한다. (Output)

## HTML Form

```html
<form name="loginForm" method="post" action="loginServlet">
    Username: <input type="text" name="username"/> <br/>
    Password: <input type="password" name="password"/> <br/>
    <input type="submit" value="Login" />
</form>
```

`action="loginServlet"`

URL of the servlet
해당 URL로 request가 간다.  WAS에서의 어떤 Servlet인지 지정하는 것

---

## Servlet이란

- 정적(static)리소스(=html)를 -> 동적(dynamic) 리소스로 활용하는 JAVA에서 제공하는 클래스
- html만 가지고서는 정해진 모습만 보여줄 수 있다면 html의 껍데기 안에 java의 기술을 합쳐서 클라이언트(=브라우져 사용자)의 요청에 대한 응답을 해줄 수 있는 기술
- 즉, 서블릿은 자바를 활용해서 웹을 만들기 위해 필요한 기술
- 클라이언트의 요청(requset)에 동적(dynamic)으로 대응하는 웹 어플리케이션 컴포넌트
- html(=정적리소스)를 활용하여 클라이언트의 요청에 응답
- HttpServlet 클래스를 상속받아 활용한다.
- 개발자가 작성해야 하는 부분

![servlet-program.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9f2a002a-a1a7-4c83-a137-ead20c4e3ea8/servlet-program.png)

- 과정
    1. Web Server는 HTTP request를 Web Container(Servlet Container)에게 위임한다.
        1. web.xml 설정에서 어떤 URL과 매핑되어 있는지 확인
        2. 클라이언트(browser)의 요청 URL을 보고 적절한 Servlet을 실행
    2. Web Container는 service() 메서드를 호출하기 전에 Servlet 객체를 메모리에 올린다.
        1. Web Container는 적절한 Servlet 파일을 컴파일(.class 파일 생성)한다.
        2. .class 파일을 메모리에 올려 Servlet 객체를 만든다.
        3. 메모리에 로드될 때 Servlet 객체를 초기화하는 init() 메서드가 실행된다.
    3. Web Container는 Request가 올 때마다 thread를 생성하여 처리한다.
        1. 각 thread는 Servlet의 단일 객체에 대한 service() 메서드를 실행한다.
- Servlet Program에서 Thread
    
    Web Container(Servlet Container)는 thread의 생성과 제거를 담당한다.
    
    Servlet Program에서 thread가 수행할 메서드가 지정/할당되면
    thread는 생성 후 즉시 해당 메서드만 열심히 수행한다.
    
    해당 메서드가 return하면 thread는 종료되고 제거된다.
    
    ⇒ 실제로 thread의 역할: Servlet의 doGet() 또는 doPost()를 호출하는 것.
    
    하지만 thread의 생성과 제거의 반복은 큰 오버헤드를 만든다.
    
    이를 위해 Tomcat(WAS)은 “Thread Pool”(미리 thread를 만들어 놓음) 이라는 적절한 메커니즘을 사용하여 오버헤드를 줄인다.
    
    즉, WAS는 Servlet의 life cycle을 담당한다.
    
    웹 브라우저 클라이언트의 요청이 들어왔을 때 Servlet 객체 생성은 WAS가 알아서 처리한다.
    
    WAS 위에서 Servlet이 돌아다니고 개발자는 이 Servlet을 만들어야 한다.
    

---

### Servlet Life Cycle

![servlet-life-cycle.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ea346650-9bf2-4adf-8ecd-f41e8ee071b2/servlet-life-cycle.png)

1. 클라이언트의 요청이 들어오면 WAS는 해당 요청에 맞는 Servlet이 메모리에 있는지 확인
    1. Servlet이 메모리에 없으면 Servlet 객체를 생성하고 `init` 메서드 실행해서 객체 초기화 ⇒ `service` 메서드 실행
    2. 메모리에 있으면 `service` 메서드 실행
2. `service` 메서드에서 request type에 따라 적절한 메서드(doGet, doPost, doPut, doDelete 등)를 호출한다.
    1. 메서드가 return하면 해당 thread는 제거된다.
3.  Web Application이 갱신되거나 WAS가 종료될 때 `destroy` 메서드가 호출되어 Servlet 객체를 메모리에서 제거

```java
@WebServlet("/loginServlet")
public class LoginServlet extends HttpServlet {
    // doPost()를 재정의 
		@Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response 
															throws ServletException, IOException {
        // read form fields
        String username = request.getParameter("username");
        String password = request.getParameter("password");
         
        // get response writer
        PrintWriter writer = response.getWriter();

        /* 여기서 -> DB 접근 등 Business Logic 부분을 처리 */
         
        // build HTML code (view 생성 부분)
        String htmlResponse = "<html>";
        htmlResponse += "<h2>Your username is: " + username + "<br/>";      
        htmlResponse += "Your password is: " + password + "</h2>";    
        htmlResponse += "</html>";	
         
        // return response
        writer.println(htmlResponse);         
    }

		@Override
    public void init(ServletConfig config) throws ServletException {
        System.out.println("init method 호출!");
    }
    
    @Override
    public void destroy() {
        System.out.println("destroy method 호출!");
    }
}
```

## Servlet과 JSP

### Servlet

- Java 코드 안에 HTML코드
- data processing(Controller)에 좋다.
- DB와의 통신, Business Logic 호출, 데이터를 읽고 확인하는 작업 등에 유용

### JSP

- HTML코드 안에 java 코드
- View에 좋다.
- 요청 결과를 나타내는 HTML 작성에 유용하다.

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<HTML>
	<HEAD>
		<TITLE>Reading Three Request Parameters</TITLE>
		<LINK REL=STYLESHEET HREF="JSP-Styles.css" TYPE="text/css">
	</HEAD>
	
	<BODY>
		<H1>Reading Three Request Parameters</H1>
		<UL>
		    <LI><B>param1</B>: <%= request.getParameter("param1") %>
		    <LI><B>param2</B>: <%= request.getParameter("param2") %>
		    <LI><B>param3</B>: <%= request.getParameter("param3") %>
		</UL>
	</BODY>
</HTML>
```

## 예상 질문

- 서블릿에 대해 설명해주세요.
    
    클라이언트의 요청을 처리하고, 그 결과를 반환하는 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술입니다. 사용자의 요청을 받아 처리한 후에 결과를 반환합니다.
    
    간단히 - 자바를 사용해 웹을 만들기 위해 필요한 기술
    
- 서블릿의 동작 방식에 대해 설명해주세요.
    1. 사용자(Client)가 URL을 입력하면 HTTP Request가 Servlet Container로 전송됩니다.
    2. 요청 받은 Servlet Container는 HttpServletRequest, HttpServletResponse 객체를 생성합니다.
    3. web.xml을 기반으로 사용자가 요청한 URL이 어느 서블릿에 대한 요청인지 찾습니다.
    4. 해당 서블릿에서 service메소드를 호출한 후 GET, POST여부에 따라 doGet() 또는 doPost()를 호출합니다.
    5. doGet() or doPost() 메소드는 동적 페이지를 생성한 후 HttpServletResponse객체에 응답을 보냅니다.
    6. 응답이 끝나면 HttpServletRequest, HttpServletResponse 두 객체를 소멸시킵니다.

## 참고 자료

- [서블릿 개념](https://soonggi.tistory.com/61)
- [예제 코드, 설명](https://gmlwjd9405.github.io/2018/10/28/servlet.html)
- [https://coding-factory.tistory.com/742#:~:text=서블릿이란 Dynamic Web Page,에는 규칙이 존재합니다](https://coding-factory.tistory.com/742#:~:text=%EC%84%9C%EB%B8%94%EB%A6%BF%EC%9D%B4%EB%9E%80%20Dynamic%20Web%20Page,%EC%97%90%EB%8A%94%20%EA%B7%9C%EC%B9%99%EC%9D%B4%20%EC%A1%B4%EC%9E%AC%ED%95%A9%EB%8B%88%EB%8B%A4).
- [면접 질문](https://dev-coco.tistory.com/163)
