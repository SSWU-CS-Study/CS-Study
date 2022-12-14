# OSI 7계층

통신 기술의 도입과 통신 기능의 확장을 쉽게 하기 위해 프로토콜을 몇 개의 계층으로 나누는 것을 **계층화**라고 하며, 통신 기능을 7 계층으로 분류하여 각 계층마다 프로토콜을 규정한 규격을 **OSI 모델**이라 한다. OSI 7 계층은 네트워크에서 통신이 일어나는 과정을 7단계로 나눈 것을 말한다.

<br>

<img src="https://s7280.pcdn.co/wp-content/uploads/2018/06/osi-model-7-layers-1.png" width=500 />



그렇다면 이와 같이 <u>**7 계층으로 나누는 이유**</u>는 무엇일까?

**통신이 일어나는 과정을 단계 별로 알 수 있으며, 특정한 곳에서 이상이 생기면 해당 단계만 수정**할 수 있기 때문이다.

```
PC방에서 오버워치를 하는데 연결이 끊겼다. 어디에 문제가 있는지 확인해 보자.
모든 PC에 문제가 있다면, 라우터의 문제(3계층 네트워크 계층)이거나 
광랜을 제공하는 회사의 회선 문제(1계층 물리 계층)일 가능성이 높다.

한 PC만 문제가 있다면, 오버워치 소프트웨어에 문제(7계층 애플리케이션 계층)일 수 있다.
오버워치 소프트웨어에 문제가 없고, 스위치에 문제(2계층 데이터 링크 계층)일 수 있다.

이렇듯 다른 계층에 있는 장비나 소프트웨어를 건드리지 않고 문제를 해결할 수 있다.
```

<br>

각 계층은 헤더(Header)와 데이터 단위(Data Unit 또는 Protocol Data Unit)으로 정의되는데, 헤더에는 각 계층의 기능과 관련된 정보가 포함된다. 상위 계층이나 하위 계층 사이에 주고 받는 것을 `서비스 데이터 단위(SDU)`라 하고, 같은 계층 사이에서 주고 받는 것을 `프로토콜 데이터 단위(PDU)`라고 한다. 이러한 데이터 단위는 송신 측이나 수신 측의 다음 계층에 데이터 정보를 전송할 때 사용한다. 

<img src="https://images.velog.io/images/nellholic108/post/c9a7aba0-5b6b-46b4-ac57-7e7c6d0de788/%E1%84%8C%E1%85%A6%E1%84%86%E1%85%A9%E1%86%A8%E1%84%8B%E1%85%B3%E1%86%AF%20%E1%84%8B%E1%85%B5%E1%86%B8%E1%84%85%E1%85%A7%E1%86%A8%E1%84%92%E1%85%A2%E1%84%8C%E1%85%AE%E1%84%89%E1%85%A6%E1%84%8B%E1%85%AD.-001.png" alt="네트워크 OSI 7 계층" width=500 />

1계층에서 7계층으로 갈 수록 점점 헤더를 붙여 data가 늘어나고, **캡슐화**(encapsulation)된다. 

<br>

## 1) 물리(Physical) 계층

데이터 전기적인 아날로그 신호로 변환해서 주고 받는 기능을 하는 공간이다. 단지 데이터를 전달만 할 뿐 전송하거나 받으려는 데이터가 무엇인지, 어떤 에러가 있는지 등에는 전혀 신경 쓰지 않는다. 주로 전기적, 기계적, 기능적인 특성을 이용한 통신 케이블로 데이터를 전송하게 된다. 즉, **데이터를 전송하는 역할만 진행**하며, 전송 매체를 통해 비트(bit)를 전송한다. 

- 리피터, 케이블, 허브, LAN 카드 등

<br>

## 2) 데이터 링크(Data Link) 계층

**물리 계층으로 송수신 되는 정보를 관리하여, 안전하게 전달되도록 도와주는 역할**을 한다. **MAC 주소**(물리 주소)를 통해 통신하며, 프레임(전송 단위)에 MAC 주소를 부여하고 에러 검출, 재전송, 흐름 제어를 진행한다. 오류 검사는 패리티 비트 검사, 해밍 부호 검사 등이 있다. 즉, 물리적 링크를 이용하여 신뢰성 있는 전송로 역할을 한다.

- 브릿지, 스위치 

<br>

## 3) 네트워크(Network) 계층

**데이터를 목적지까지 가장 안전하고 빠르게 전달**하는 기능을 담당한다. **라우터를 통해 이동할 경로를 선택**하여 **IP 주소**(논리적인 주소)를 지정하고, 해당 경로에 따라 **패킷을 전달**해준다. 

라우팅, 흐름 제어, 오류 제어, 세그먼테이션 등을 수행한다.

- 라우터, IP 

<br>

🔎 **패킷(packet)이란?**

네트워크에서 출발지와 목적지 간에 라우팅되는 데이터 단위이다. 패킷은 사용자 데이터(페이로드)와 제어 정보로 이루어지며, 제어 정보는 페이로드를 전달하기 위한 정보이다. 전송되는 패킷에는 소스 및 대상, 프로토콜 또는 ID와 같은 정보가 저장된다.

<br>

## 4) 전송(Transport) 계층

주로 TCP, UDP 프로토콜을 사용하며, 헤더에 송신지와 수신지의 **포트 번호**를 포함하여 전달하는 계층이다. 

전송 계층에서는 데이터를 전송하고, 전송 속도를 조절하며, 오류가 발생된 부분은 다시 맞춰 주는 계층이다. 데이터를 전송 받은 경우, 전송 계층에서 데이터를 합산하여 세션 계층으로 보내주게 된다.

참고로, TCP의 데이터 전송 단위는 Segment, UDP의 데이터 전송 단위는 Datagram이라고 부른다.

- TCP : 신뢰성, 연결지향적
- UDP : 비신뢰성, 비연결성, 실시간

<br>

**🔎 포트 번호란?**

하나의 컴퓨터에서 동시에 실행되고 있는 프로세스들이 서로 겹치지 않게 가져야 하는 정수 값이다.

🔎 **네트워크 계층 vs 전송 계층**

네트워크 계층은 호스트 간의 논리적 통신을 돕지만, 전송 계층은 응용 프로세스 간의 논리적을 통신을 돕는다.

<br>

## 5) 세션(Session) 계층

세션 계층은 네트워크상 양쪽 연결을 관리하고 연결을 지속 시켜 주는 계층이다.

**데이터가 통신하기 위한 논리적 연결을 담당**하고, 주로, TCP/IP 세션을 만들고 유지하며, 세션이 종료되거나 전송이 중단될 시 복구하는 기능이 있다. 데이터의 단위(메세지)를 전송 계층으로 전송할 순서를 결정하고, 데이터를 점검 및 복구하는 동기 위치(Synchronization Point)를 제공한다. 또한 응용 프로그램 계층 사이의 접속을 설정, 유지, 종료시켜주는 역할을 한다.

- API, Socket

<br>

**🔎세션이란?**

세션이란 일정 시간 동안 같은 사용자로부터 들어오는 일련의 요구를 하나의 상태로 보고 그 상태를 일정하게 유지하는 기술을 말한다. 여기서 일정 시간이란, 방문자가 웹 브라우저를 통해 웹 서버에 접속한 시점으로부터 웹 브라우저는 종료함으로써 연결을 끝내기 까지의 기간을 말한다. 즉, 방문자가 웹 서버에 접속해 있는 상태를 하나의 단위로 보고 세션이라고 부른다.

<br>

## 6) 표현(Presentation) 계층

**데이터 표현에 대한 독립성을 제공하고 암호화**하는 역할을 담당한다. 파일을 인코딩하고, 명령어를 포장, 압축, 암호화한다.

**데이터 표현 차이를 해결하기 위해 서로 다른 형식으로 변환하거나 공통 형식을 제공**하는 계층으로, 송신 측에서는 수신 측에 맞는 형태로 변환하고, 수신 측에서는 응용 계층에 맞는 형태로 변환한다. 

- JPEG, MPEG 등

<br>

예를 들어, EBCDIC로 인코딩된 문서 파일을 ASCII로 인코딩된 파일로 바꿔 주기도하고, 특정 데이터가 text인지, gif인지, jpg인지 등의 구분을 해 준다.

<br>

## 7) 응용(Application) 계층

최종 목적지로, 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행한다. **실질적인 프로그램**을 나타내며, 사용자 인터페이스, 전자우편, 데이터베이스 관리 등의 서비스를 제공한다.  최종 목적지로서 HTTP, FTP, SMTP, POP3, IMAP, Telnet 등과 같은 프로토콜이 있다.

- HTTP, FTP, DNS 등

<br>

<br>

## 📌기술 질문/예상 질문

**OSI 7계층만 단독으로 질문하기 보다는 각 계층의 프로토콜이나 사용되는 기술들과 연관 지어서 나올 확률이 높다.**

<br>

<details>
<summary>OSI 7 계층이란?</summary>

<br>

OSI 7 계층은 네트워크에서 통신이 일어나는 과정을 7단계로 나눈 것을 말한다.

</details>

<br>

<details>
<summary>OSI 7 계층으로 나눈 이유가 무엇인가?</summary>

<br>

통신이 일어나는 과정을 단계 별로 파악할 수 있기 때문이다. 흐름을 한 눈에 알아보기 쉽고, 사람들이 이해하기 쉽고, 7단계 중 특정한 곳에 이상이 생기면 다른 단계의 장비 및 소프트웨어를 건드리지 않고도 이상이 생긴 단계만 고칠 수 있다.

</details>

<br>

<details>
<summary>물리 계층이 하는 역할은 무엇인가?</summary>

<br>

데이터를 전기적인 아날로그 신호로 변환하여 주고 받는다.

</details>

<br>

<details>
<summary>데이터 링크 계층이 하는 역할은 무엇인가?</summary>

<br>

물리 계층을 통해 송수신되는 데이터의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 돕는다.

</details>

<br>

<details>
<summary>Mac 주소란 무엇인가?</summary>

<br>

Mac 주소란 컴퓨터 간 데이터를 전송하기 위해 있는 컴퓨터의 물리적 주소 또는 하드웨어 주소라고 하며, 하드웨어 식별 번호(기계의 고유 주소 번호)라고 할 수 있다.

</details>

<br>

<details>
<summary>호스트란 무엇인가?</summary>

<br>

네트워크에 연결 되어 있는 컴퓨터들을 호스트라고 부른다.

</details>

<br>

<details>
<summary>DNS가 무엇인가요?</summary>

<br>

네이버와 구글과 같은 도메인 이름을 IP 주소로 변환하는 기법을 말한다

</details>

<br>

<details>
<summary>IP 주소란 무엇인가요?</summary>

<br>

IP 주소는 컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신하기 위해 사용하는 논리적 주소다.

</details>

<br>

<details>
<summary>Mac 주소와 IP 주소의 차이는 무엇인가?</summary>

<br>

Mac 주소는 컴퓨터 간에 데이터를 전송하기 위한 물리적 주소이고, IP 주소는 컴퓨터 네트워크에서 장치들이 서로를 인식하고 통신하기 위해 사용하는 논리적 주소이다. IP는 내가 미국에 있는 A주소로 메일을 보낼게! 즉, 시작점과 끝점에 해당하는 주소라면, MAC주소는 바로 옆에 나와 물리적으로 연결되어있는 노드들과 통신할 때 사용되는 주소를 의미한다.

IP 주소와 Mac주소는 동작하는 Layer가 다른데, IP주소는 3계층인 네트워크 계층에서 동작하며, Mac 주소는 2계층인 데이터 링크 계층에서 동작한다. 

데이터 링크 계층에서 문제없이 동작해야, 즉 이웃 노드들 간에 통신이 잘 이루어져야, 결국 멀리 있는 노드들과도 통신이 가능하다는 것이다. 정리하자면 거점 사이사이 전달이 가능해야 결국 내가 원하는 먼 주소로 전달이 가능한 것이고, 그래서 IP 주소간의 통신은 사실 MAC 주소와 MAC 주소 간의 통신의 연속적인 과정이라고 할 수 있다.

</details>

<br>

<details>
<summary>OSI 7계층에 대해서 설명해주세요</summary>

<br>

나눈 이유 :  통신이 일어나는 과정을 단계별로 알수있고, 문제가 생기면 그 단계만 수정하면 된다.

- 물리 : 데이터 전송 ex) 리피터, 케이블, 허브
- 데이터링크 : 물리 계층으로 송수신되는 정보 관리, Mac 주소로 통신 ex) 브릿지, 스위치
- 네트워크 : 데이터를 목적지까지 전달, 라우터로 경로를 선택해 IP 지정, 경로에 따라 패킷 전달 ex) 라우터, IP
- 전송 : 통신을 활성화 ex) TCP, UDP
- 세션 : 데이터가 통신하기 위한 논리적 연결 담당 ex) API, Socket
- 표현 : 데이터 표현에 대한 독립성을 제공하고 암호화 ex) JPEG, MPEG
- 응용 : 최종 목적지, 응용프로그램과 연관하여 서비스 수행 ex) HTTP, FTP, DNS

</details>

<br>

<details>
<summary>www.naver.com에 접속할때 일어나는 일에 대해 설명해주세요.</summary>

<br>

<img src="https://velog.velcdn.com/images%2Fjaeyunn_15%2Fpost%2F782b17f6-6009-4259-bbeb-4c05e9be663d%2Fimage.png" width=500 />

①② 사용자가 웹 브라우저를 통해 찾고 싶은 웹 페이지의 URL 주소를 입력함.

③ 사용자가 입력한 URL 주소 중에서 도메인 네임(domain name) 부분을 DNS 서버에서 검색함.

④ DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 전달함.

⑤⑥ 웹 페이지 URL 정보와 전달받은 IP 주소는 HTTP 프로토콜을 사용하여 HTTP 요청 메시지를 생성함.

이렇게 생성된 HTTP 요청 메시지는 TCP 프로토콜을 사용하여 인터넷을 거쳐 해당 IP 주소의 컴퓨터로 전송됨.

⑦ 이렇게 도착한 HTTP 요청 메시지는 HTTP 프로토콜을 사용하여 웹 페이지 URL 정보로 변환됨.

⑧ 웹 서버는 도착한 웹 페이지 URL 정보에 해당하는 데이터를 검색함.

⑨⑩ 검색된 웹 페이지 데이터는 또다시 HTTP 프로토콜을 사용하여 HTTP 응답 메시지를 생성함.

이렇게 생성된 HTTP 응답 메시지는 TCP 프로토콜을 사용하여 인터넷을 거쳐 원래 컴퓨터로 전송됨.

⑪ 도착한 HTTP 응답 메시지는 HTTP 프로토콜을 사용하여 웹 페이지 데이터로 변환됨.

⑫ 변환된 웹 페이지 데이터는 웹 브라우저에 의해 출력되어 사용자가 볼 수 있게 됨

</details>

<br>

<details>
<summary>도메인 이름으로 실제 IP를 어떻게 찾을 수 있는지 흐름을 설명해 주세요.</summary>

<br>

**Recursive Query를 통해 접근 : Local DNS 서버 -> Root DNS 서버 -> com DNS 서버 -> naver.com DNS 서버**

1. 로컬 DNS서버에 해당 url이 등록되어있는지 확인

2. 루트 DNS서버에 문의 후 최상위 도메인 .com이 등록된 네임 서버의 IP주소 전달

3. 로컬 DNS서버는 com DNS 서버에 해당 url을 문의함. 로컬 DNS서버에 naver.com DNS 서버의 IP 주소 알려줌

4. naver..com에 해당 url 문의함. 로컬 DNS는 IP 주소를 받을수있음

</details>

<br>

<details>
<summary>유니캐스트, 멀티캐스트, 브로드캐스트란?</summary>

<br>

유니캐스트 : 특정 대상과 1:1 통신

멀티캐스트 : 특정 다수와 1:N 통신

브로드캐스트 : 네트워크에 있는 모든 대상과 통신

</details>

<br>

<br>

<br>

<br>

<br>

<br>

Reference

- https://gyoogle.dev/blog/computer-science/network/OSI%207%EA%B3%84%EC%B8%B5.html

- https://kongit.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-%ED%8C%A8%ED%82%B7Network-Packet%EC%9D%B4%EB%9E%80-%EC%A0%95%EC%9D%98-%ED%8C%A8%ED%82%B7-%EC%86%90%EC%8B%A4

- https://github.com/alstjgg/cs-study/blob/main/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/osi%207%EA%B3%84%EC%B8%B5.md

- https://jhnyang.tistory.com/404

- https://velog.io/@hidaehyunlee/%EB%8D%B0%EC%9D%B4%ED%84%B0%EA%B0%80-%EC%A0%84%EB%8B%AC%EB%90%98%EB%8A%94-%EC%9B%90%EB%A6%AC-OSI-7%EA%B3%84%EC%B8%B5-%EB%AA%A8%EB%8D%B8%EA%B3%BC-TCPIP-%EB%AA%A8%EB%8D%B8#5-tcpip-%EB%AA%A8%EB%8D%B8

- https://velog.io/@nellholic108/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-OSI-7-%EA%B3%84%EC%B8%B5