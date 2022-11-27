# TLS/SSL HandShake

<br>

## HTTPS를 사용한 안전한 웹

인터넷은 안전한 통신을 위해 암호화를 진행하게 되는데, 암호화란 일반적인 평문(text)를 알아볼 수 없도록 암호문으로 만드는 과정이다. 암호문을 상대에게 전달하고, 상대는 이를 다시 복호화하여 평문으로 확인할 수 있다. 이와 같은 과정을 웹 브라우저와 웹 서버에게 사용하는 기술이 바로 HTTPS(Hypertext Transfer Protocol Secure)이다. 

HTTPS는 SSL(Secure Socket Layer) / TLS(Transport Layer Security) 전송기술을 사용한다. TCP, UDP와 같은 일반적인 인터넷 통신에 안전한 계층(layer)를 추가하는 방식을 말하며, 이러한 기술을 구현하기 위해 웹 서버에 설치하는 것이 SSL/TLS 인증서이다. 

TLS란 SSL의 개선 버전으로, 최신 인증서는 TLS를 사용하지만, 편의적으로 SSL 인증서라고 쭉 불리고 있다.

<br>

TLS/SSL Handshake란 HTTPS에서 클라이언트와 서버간 통신 전 **SSL 인증서로 신뢰성 여부를 판단하기 위해 연결하는 방식**이다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221127002308423.png" width=400/>

handshake는 악수를 의미하는데, 통신을 하는 브라우저와 웹 서버가 서로 암호화 통신을 시작할 수 있도록 신분을 확인하고, 필요한 정보를 클라이언트와 서버가 주거니 받거니 하는 과정이 악수와 비슷하여 붙여진 이름이다.

TLS/SSL Handshake는 다음 과정으로 이루어져있다.

<img src="https://blog.kakaocdn.net/dn/ce8Q1f/btrlS1Lj6iN/xiyxlVDLKKcF7HtfK4JJG1/img.png" width=600/>

파란색 네모에 해당되는 패킷 `SYN` `SYN+ACK` `ACK`는 TCP 레이어의 3-way handshake로, HTTPS가 TCP 기반의 프로토콜이므로 SSL Handshake에 앞서 연결을 생성하기 위해 실시하는 과정이다. 노란색 네모에 해당하는 패킷들이 SSL Handshake라고 보면 된다.

<br>

#### SSL 인증서

SSL 인증서에는 서비스의 정보(인증서를 발급한 CA, 서비스의 도메인 등)와 서버 측 공개키가 들어있다.

서버에서는 키 페어(공개키+개인키)를 생성하고, 클라이언트가 사용할 서버의 공개키는 인증서에 담기게 담기게 된다.

인증서는 CA의 개인 키로 암호화되는데, 클라이언트는 이렇게 CA의 개인키로 암호화 된 인증서를 어떻게 복호화하는 것일까?

우리가 사용하는 일반적인 **브라우저에는 신뢰할 수 있는 CA 기관의 리스트와 해당 기관의 공개키를 이미 가지고** 있기 때문에, 클라이언트는 **내장된 CA의 공개키를 활용하여 인증서를 복호화**함으로써 인증서를 검증한 뒤, 서버의 공개키를 가져올 수 있다.

<br>

<br>

## Handshake Protocol

#### ClientHello

**클라이언트가 서버에 연결을 시도하며 전송하는 패킷**으로, 자신이 사용 가능한 Cipher Suite 목록, 세션 ID, SSL 프로토콜 버전, Random Byte 등을 전달한다. Cipher Suite는 SSL 프로토콜 버전, 인증서 검정, 데이터암호화 프로토콜, Hash 방식 등의 정보를 담고 있는 존재로, 선택된 **Cipher Suite 알고리즘에 따라 데이터를 암호화**하게 된다.

다음 사진 처럼 클라이언트가 사용 가능한 Cipher Suite를 서버에 제공하는 것을 확인할 수 있다. 

<img src="https://blog.kakaocdn.net/dn/tHP9u/btrlSwkw1rc/YskKZt678LDHKb041zm5DK/img.png" width=500/>

참고로 Cipher Suite의 구성은 다음과 같다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221126234602915.png" width=700/>

<br>

#### ServerHello

서버는 클라이언트가 보내온 Client Hello 패킷을 받아 **Cipher Suite 중 하나를 선택**한 다음 클라이언트에게 이를 알린다. 또한, 자신의 SSL 프로토콜 버전 등도 함께 보낸다. 다음의 사진을 보면, Client Hello에서 17개였던 Cipher Suite와 달리, Server가 선택한 한 줄만 존재하는 것을 확인할 수 있다.

<img src="https://blog.kakaocdn.net/dn/besPmi/btrlXwcfJF3/bG6MMtzXjO6rWooFNujPfK/img.png" width=500/>

<br>

#### Certificate

**Server가 자신의 SSL 인증서를 클라이언트에게 전달**한다. 인증서 내부에는 Server가 발행한 공개키가 들어있다.

클라이언트는 서버가 보낸 CA의 개인키로 암호화된 이 **인증서를 이미 모두에게 공개된 CA의 공개키를 사용하여 복호화**한다.

복호화에 성공하면 이 인증서는 CA가 서명한 것임을 증명한 것이 된다. 

다음을 보면 SSL 인증서가 어떤 알고리즘으로 암호화되었는 지, 어떤 Hash 알고리즘으로 서명되었는 지 확인할 수 있다.

<img src="https://blog.kakaocdn.net/dn/dm9yAj/btrlPomxiN1/ku5cyokJIhwxJzCHAzyqo0/img.png" width=700/>

<br>

#### Server Key Exchange / ServerHello Done

**서버의 공개키가 SSL 인증서 내부에 없는 경우, 서버가 직접 전달**한다는 뜻이다. 

공개키가 SSL 인증서 내부에 있을 경우 Server Key Exchange는 생략된다. 인증서 내부에 서버의 공개키가 있다면, 클라이언트가 CA의 공개 키를 통해 인증서를 복호화한 후 서버의 공개키를 확보할 수 있다. 그리고 서버가 행동을 마쳤음을 전달한다.

<br>

#### Client Key Exchange

클라이언트는 데이터 암호화에 사용할 **대칭 키를 생성한 후** SSL 인증서 내부에서 추출한 **서버의 공개키를 이용하여 암호화**한 후 **서버에게 전달**한다. 여기서 **전달된 대칭 키가 SSL Handshake의 목적**이자 가장 중요한 수단인 데이터를 실제로 암호화할 대칭 키이다. 이제 본 키를 통해 클라이언트와 서버가 실제로 교환하고자 하는 데이터를 암호화한다.

<br>

#### ChangeCipherSpec/Finished

 ChageCipherSpec 패킷은 클라이언트와 서버 모두가 서로에게 보내는 패킷으로, 교환할 정보를 모두 교환한 뒤 통신할 준비가 다 되었음을 알리는 패킷이다. 그리고 Finished 패킷을 보내어 SSL Handshake를 종료하게 된다. 

<br>

<br>

## 진행 순서

<img src="https://www.thesslstore.com/blog/wp-content/uploads/2017/01/SSL_Handshake_10-Steps-1.png" width=400/>

1) 클라이언트가 Client Hello 메시지를 담아 Cipher Suite 목록, 세션 ID, 프로토콜 버전, 암호 알고리즘 등을 담아 서버로 전송한다.

2) 서버는 Cipher Suite 중 하나를 선택하여 Server Hello 메시지를 함께 담아 응답한다.

3) 서버는 자신의 SSL 인증서를 클라이언트에게 전달한다. 클라이언트는 CA의 공개키로 복호화하여 인증을 검증한다.

4) 서버의 공개키가 SSL 인증서 내부에 없는 경우 서버가 직접 전달

5) 서버가 행동을 마쳤음을 전달

6) 대칭키를 생성 후 서버의 공개키로 암호화하여 서버에게 전달

7) 클라이언트가 통신할 준비가 되었음을 알린다

8) SSL Handshake 종료

9) 서버가 통신할 준비가 되었음을 알린다

10) SSL Handshake 종료

<br>

<br>

## 기술질문/예상질문

<details>
<summary>SSL Handshake란?</summary>



<br>

Handshake란 악수를 의미하는데, 통신을 하는 브라우주어와 웹 서버가 서로 암호화 통신을 시작할 수 있도록 신분을 확인하고 필요한 정보를 클라이언트와 서버가 주거니 받거니 하는 과정이 악수와 비슷하여 붙여진 이름이다.

</details>

<br>

<details>
<summary>SSL Handshake 과정을 손으로 그려보시오</summary>



<br>

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221127165144268.png" width=500/>

대칭 키와 비대칭키를 혼합하여 사용한다는 것을 기본적으로 확실하게 이해하여 설명하는 것을 기반으로 둔다.

</details>

<br>

<details>
<summary>SSL Handshake의 과정을 설명해보세요</summary>



<br>

서버는 CA에 사이트 정보와 공개 키를 전달하여 인증서를 받음 → 클라이언트는 브라우저에 CA 공개 키가 내장되어 있다고 가정 → ClientHello(암호화 알고리즘 나열 및 전달) → Serverhello(암호화 알고리즘 선택) → Server Certificate(인증서 전달) → Client Key Exchange(데이터를 암호화 할 대칭 키 전달) → Client / ServerHello done (정보 전달 완료) → Finished(SSL Handshake 종료)

</details>

<br>

<br>

<br>

<br>

<br>

Reference

- [링크](https://steady-coding.tistory.com/512)
- [링크](https://gyoogle.dev/blog/computer-science/network/TLS%20HandShake.html)
- [링크](https://brunch.co.kr/@sangjinkang/38)