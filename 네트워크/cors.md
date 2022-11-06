# SOP와 CORS

## 리소스 요청 관련 정책
웹 생태계에는 다른 출처로의 리소스 요청을 제한하는 것과 관련된 두 가지 정책이 존재한다.

1. SOP(Same Origin Policy)
2. CORS(Cross Origin Resource Sharing)

SOP는 2011년, RFC 6454에서 처음 등장한 보안 정책으로 “같은 출처에서만 리소스를 공유할 수 있다”라는 규칙을 가진 정책이다.

하지만, 이를 좀 더 완화시키기 위해서 몇 가지 예외 조항을 두고 이 조항에 해당하는 요청은 허용하기로 했다.

그 중 하나가 “CORS 정책을 지킨 리소스 요청”이다.

## 출처(Origin)이란?
<img width="698" alt="Screen Shot 2022-11-03 at 5 19 46 PM" src="https://user-images.githubusercontent.com/68562176/200162982-7516010b-590f-4405-b1c1-5d1d1a89ae2a.png">
이 때 출처란 Protocol과 Host, Port를 모두 합친 것을 의미한다.

## SOP
💡 http://localhost 와 동일 출처 URL은 무엇일까?

1. https://localhost
2. http://localhost:80
3. http://127.0.0.1
4. http://localhost/api/cors
<details>
<summary>정답</summary>
2,4
>1번: 프로토콜이 다르다.   
2번: http의 기본 포트번호는 80이다. 따라서 localhost에는 생략되어 있는 것이고, 포트번호는 같다.   
3번: localhost의 IP가 127.0.0.1이긴 하지만, 브라우저 입장에서는 문자열을 비교하기 때문에 다른 출처라고 판단한다.    
4번: path를 제외한 Protocol, Host, Port이 모두 같다.    
</details>

### 필요성
나쁜 의도를 가진 개발자가 사용자의 정보를 이용해 악의적인 일을 꾸밀 수 있기 때문에, 그러한 일을 방지하기 위해 필요하다.
<img width="1430" alt="Screen Shot 2022-11-06 at 8 54 28 AM" src="https://user-images.githubusercontent.com/68562176/200163038-e842e33c-42a2-4648-a4fe-4ab431a6a6fc.png">
위의 예시에서 사용자가 이미 구글에 로그인을 했었던 상태라면, 브라우저는 관련된 쿠키를 가지고 있을 것이다.
해당 쿠키를 이용하여, 비밀번호를 바꾸거나 메일을 보내는 등의 행동이 가능해진다.
→ 따라서 구글 출처와 다른 [https://suspicious.com에서](https://suspicious.com에서) 리소스에 접근하는 것을 제한한다.
![Untitled (11)](https://user-images.githubusercontent.com/68562176/200163051-9aeaa392-23a5-4e4a-9563-7838d78ed6f7.png)

→ 이 때, 출처를 비교하는 로직은 브라우저에 구현되어 있는 스펙이다.

## CORS 동작 과정
**기본적인 동작 과정**
1. 다른 출처의 리소스를 요청할 때는 HTTP 요청을 보낼 때 요청 헤더에 `Origin` 이라는 필드에 출처를 담아 보낸다. 
2. 서버가 요청에 대한 응답을 할 때, 응답 헤더에 `Access-Control-Allow-Origin` 이라는 값에 리소스 접근을 허용하는 출처에 대한 정보를 담는다. 
3. 브라우저는 `Origin`과 `Access-Control-Allow-Origin` 을 비교해본 후 이 응답이 유효한지 아닌지를 결정한다.

세부적인 동작과정은 세 가지의 시나리오에 따라 변경된다.

**CORS 접근제어 시나리오**
1. 단순 요청 (Simple Request)
2. 프리플라이트 요청 (Preflight Request)
3. 인증정보 포함 요청 (Credentialed Request)

### 1. 단순 요청 (Simple Request)
![Untitled (12)](https://user-images.githubusercontent.com/68562176/200163083-a447bbf1-0dc1-4f1a-9299-691c99598429.png)

### 2. 프리플라이트 요청 (Preflight Request)
브라우저가 본 요청을 보내기 전에 예비 요청을 보내며, 이 요청을 Preflight라고 부른다.
이 예비 요청에는 HTTP 메서드 중 OPTIONS 메서드가 사용된다.

![Untitled (13)](https://user-images.githubusercontent.com/68562176/200163096-e271afc3-676d-4040-bf25-65a0aab72835.png)

### 3. 인증정보 포함 요청 (Credentialed Request)

요청할 때 인증과 관련된 정보를 헤더에 담고 싶을 때 `credentials` 옵션을 사용한다.

구글 크롬 브라우저의 credentials 기본 값은 같은 `same-origin` 이다.

| 옵션 값 | 설명 |
| --- | --- |
| same-origin (기본값) | 같은 출처 간 요청에만 인증 정보를 담을 수 있다 |
| include | 모든 요청에 인증 정보를 담을 수 있다 |
| omit | 모든 요청에 인증 정보를 담지 않는다 |

## CORS Error 해결 방법
### 1. 남이 만든 프록시 서버 사용하기

### 2. 직접 프록시 서버 구축하기

### 3. http-proxy-middleware 사용하기
**클라이언트 측 해결방법**
![Untitled (14)](https://user-images.githubusercontent.com/68562176/200163116-025a46c0-98e5-4e48-92ba-a2af17057eae.png)
![Untitled (15)](https://user-images.githubusercontent.com/68562176/200163125-f7043b0e-8eb9-4fc0-907c-d0c004f07b01.png)

### 4. Access-Control-Allow-Origin 헤더 세팅하기
서버 측 해결방법
1. CorsFilter 생성하기
2. CorssOrigin 어노테이션 사용하기
3. WebMvcConfigurer에서 설정하기

## 참고
**출처(Origin)이란?**    

[[WEB] 📚 CORS 개념 💯 완벽 정리 & 해결 방법 👏](https://inpa.tistory.com/entry/WEB-%F0%9F%93%9A-CORS-%F0%9F%92%AF-%EC%A0%95%EB%A6%AC-%ED%95%B4%EA%B2%B0-%EB%B0%A9%EB%B2%95-%F0%9F%91%8F)
    
**CORS 동작 과정**

[[Spring Boot] CORS 를 해결하는 3가지 방법 (Filter, @CrossOrigin, WebMvcConfigurer)](https://wonit.tistory.com/572)    
[나를 너무나 힘들게 했던 CORS 에러 해결하기 😂](https://xiubindev.tistory.com/115)    
[[Spring Boot] CORS 설정하기](https://dev-pengun.tistory.com/entry/Spring-Boot-CORS-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)   
