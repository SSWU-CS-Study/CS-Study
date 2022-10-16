# [TCP] 3 way handshake & 4 way handshake

<br>

### 📌 Index

- [TCP(Transmission Control Protocol)란](#tcptransmission-control-protocol란)
- [TCP의 연결 설정 및 해제 과정](#tcp의-연결-설정-및-해제-과정)
  - [3 way handshake란?](#3-way-handshake란)
  - [4 way handshake란?](#4-way-handshake란)

- [참고](#참고)
  - [포트(PORT) 상태 정보](#포트port-상태-정보)
  - [TCP 헤더 안의 플래그 정보](#tcp-헤더-안의-플래그-정보)

<br>

<br>

## TCP(Transmission Control Protocol)란

TCP는 네트워크 계층 중 **전송 계층(4계층)에서 사용하는 프로토콜**로서, 장치들 사이에 논리적인 접속을 성립(establish)하기 위하여 연결을 설정하여 **신뢰성을 보장하는 연결형 서비스**이다.

#### TCP의 특징

- 인터넷 상에서 데이터를 메세지의 형태(**세그먼트**라는 블록 단위)로 보내기 위해 IP와 함께 사용하는 프로토콜이다.
- **연결형 서비스**로, 가상 회선 방식을 제공한다. `3-way handshaking` 과정을 통해 연결을 설정하고, `4-way handshaking`을 통해 연결을 해제한다. (데이터를 전송하기 전에 논리적 연결이 설정되는데, 이를 가상회선이라고 한다, UDP는 데이터그램 교환 방식)

- 흐름제어 및 혼잡제어를 제공한다.
  - 흐름 제어 : 데이터를 송신하는 곳과 수신하는 곳의 데이터 처리 속도를 조절하여 수신자의 버퍼 오버플로우를 방지하는 것
  - 혼잡 제어 : 네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지하는 것
- 높은 신뢰성을 보장한다.
- UDP 보다 속도가 느리다.
- 전이중(Full-Duplex), 점대점(Point to Point) 방식이다.
  - 전이중 : 전송이 양방향으로 동시에 일어날 수 있다.
  - 점대점 : 각 연결이 정확히 2개의 종단점을 가지고 있다.
  - 멀티 캐스팅이나 브로드 캐스팅을 지원하지 않는다.
- 연속성보다 신뢰성있는 전송이 중요할 때 사용한다.

<br>

## TCP의 연결 설정 및 해제 과정

TCP는 계속해서 통신을 확인하면서 데이터를 전송한다. 이제 통신 과정을 살펴보자.

이러한 연결을 맺는 과정을 3-Way Handshaking, 연결을 해제하는 과정을 4-Way Handshaking라 하고, 이를 말그대로 만나서 반갑다고 악수하는과정, 헤어지기 전 악수를 하는 과정이라고 생각하면 이해가 빠르다.

#### 3 way handshake란?

TCP 통신을 이용하여 데이터를 전송하기 위해 **네트워크 연결을 설정 (Connection Establish) 하는 과정**으로, TCP를 이용한 데이터 통신을 할 때 프로세스와 프로세스를 연결하기 위해 **가장 먼저 수행되는 과정**이다.

양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽이 준비되었다는 것을 알 수 있도록 한다.  즉, TCP/IP 프로토콜을 이용하여 통신을 하는 응용 프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.

<br>

<img src="https://k.kakaocdn.net/dn/U4p6B/btrmFMGAGRM/tYtqWSbBJpTCLb4lWNMVJk/img.png" width=300/>

1. 클라이언트가 서버에게 요청 패킷을 보낸다 : `연결 요청`
2. 서버가 클라이언트의 요청을 받아들이는 패킷을 보낸다 : `확인 + 연결 요청`
3. 클라이언트는 이를 최종적으로 수락하는 패킷을 보낸다 : `확인`

<br>

🚀 **예시** : A 프로세스(Client)가 B 프로세스(Server)에 연결을 요청

<img src= "https://gmlwjd9405.github.io/images/network/3-way-handshaking.png" width=700/>



1)A->B : `SYN`

- 접속 요청 프로세스 A가 연결 요청 메세지 전송 (SYN)
- 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 포트 상태 : A = `CLOSED`, B = `LISTEN`

2)B->A : `SYN` + `ACK`

- 접속 요청을 받은 프로세스 B가 요청을 수락했으며, 접속 요청을 프로세스인 A도 포트를 열어 달라는 메세지 전송 (SYN + ACK)
- 수신자는 Acknowledgement Number 필드를 `Sequence Number +1`로 지정하고, SYN과 ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 포트 상태 : A = `CLOSED`, B = `SYN_RCV`

3)A->B : ACK

- 포트 상태 : A = `ESTABLISHED`, B = `SYN_RCv`
- 마지막으로 접속 요청 프로세스 A가 수락 확인을 보내 연결을 맺음(ACK)
- 이때, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
- 포트 상태 : A = `ESTABLISHED`, B = `ESTABLISHED`

<br>

#### 4 way handshake란?

TCP의 **연결을 해제 (Connection Termination) 하는 과정**이다.

<br>

🚀 **예시** : A 프로세스(Client)가 B 프로세스(Server)에 연결 해제를 요청

<img src="https://gmlwjd9405.github.io/images/network/4-way-handshaking.png" width=700/>

1)A->B : `FIN`

- 프로세스 A가 연결을 종료하겠다는 FIN 플래그를 전송
- 프로세스 B가 FIN 플래그로 응답하기 전까지 연결을 계속 유지

2)B->A : `ACK`

- 프로세스 B는 일단 확인 메세지를 보내고, 자신의 통신이 끝날 때까지 기다린다. (이 상태가 TIME_WAIT 상태)
- 수신자는 Ackowledgement Number 필드를 `Sequence Number + 1`로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송한다.
- 그리고 자신이 전송할 데이터가 남아있다면 이어서 계속 전송한다.

3)B->A : `FIN`

- 프로세스 B가 통신이 끝났으면 연결 종료 요청에 합의한다는 의미로 프로세스 A에게 FIN 플래그를 전송

4)A->B : `ACK`

- 프로세스 A는 확인했다는 메세지를 전송

<br>

## 참고

#### 포트(PORT) 상태 정보

- `CLOSED` : 포트가 닫힌 상태
- `LISTEN` : 포트가 열린 상태로 연결 요청 대기 중
- `SYN_RCV` : SYNC 요청을 받고 상대방의 응답을 기다리는 중
- `ESTABLISHED` : 포트 연결 상태

<br>

#### TCP 헤더 안의 플래그 정보

TCP Header에는 CONTROL BIT(플래그 비트, 6 bit)가 존재하며, 각각의 bit는 `URG-ACK-PSH-RST-SYN-FIN`의 의미를 가진다.

해당 위치의 bit가 1이면 해당 패킷이 어떠한 내용을 담고 있는 패킷인지를 나타낸다.

![img](https://mblogthumb-phinf.pstatic.net/MjAxOTA1MTVfMTkz/MDAxNTU3ODk4MjkyNzMw.ne0R09ZKfbH0hn5w3wvEcf-m469wWfBu8kSAnHdFI2sg.qT5Gl2BL0cBW_Soo2si9xWMaetDowy-Q5yT8OvWLNzMg.JPEG.ak0402/1.jpg?type=w800)

- `SYN` (Synchronize Sequence Number)
  - 연결 설정 / `000010`
  - Sequence Number를 랜덤으로 설정하여 세션을 연결하는 데 사용하며, 초기에 Sequence Number를 전송한다.
- `ACK` (Acknowledgement)
  - 응답 확인 / `010000`
  - 패킷을 받았다는 것을 의미한다.
  - Acknowledgement Number 필드가 유효한 지를 나타낸다.
  - 양단 프로세스가 쉬지 않고, 데이터를 전송한다고 가정하면  최초 연결 설정 과정에서 전송되는 첫 번째 세그먼트를 제외한 모든 세그먼트의 ACK 비트는 1로 지정된다고 생각할 수 있다.
- `FIN` (Finish)
  - 연결 해제 / `000001`
  - 세션 연결을 종료시킬 때 사용되며, 더 이상 전송할 데이터가 없음을 의미한다.

<br>

## 기술 질문/예상 질문

<details>
<summary>TCP와 UDP의 차이에 대해서 설명해 주세요.</summary>




<br>

**TCP**

- 신뢰성 있는 데이터 전송을 지원하는 연결 지향형 프로토콜

- 흐름제어, 혼잡제어, 오류제어 지원

- 연결 설정시 3 way handshake를, 연결 해제시 4 way handshake 진행

- UDP보다 속도가 느리다

- **EX)** 웹 http 통신, 이메일, 파일 전송

**UDP**

- 데이터를 데이터그램 단위로 처리하는 프로토콜

- 신뢰성 낮음

- 속도가 빠르고 부하가 적다

- **EX)** Real Time Protocol(RTP), Multicast, DNS

</details>

<br>

<details>
<summary>3-way hand shake, 4-way hand shake 흐름에 대해서 설명해주세요.</summary>




<br>

**3 way handshake**

- TCP/IP 프로토콜을 사용해 통신을 진행할 때, 두 종단간 정확한 데이터 전송 보장하기 위해 연결을 설정

- SYN(Synchronize Sequence Number)

- ACK(Acknowledgement)

1. **클라이언트** → **서버** : 서버 접속 요청 **SYN 패킷** 보냄

2. **서버** → **클라이언트** : 요청 수락 응답 **ACK 패킷**과 포트 열어달라는 **SYN 패킷** 보냄

3. **클라이언트** → **서버** : 확인 응답으로 **ACK 패킷** 보냄

**4 way handshake**

- 연결 설정 해제함

1. **클라이언트**→ **서버** : 연결 해제하겠다는 **FIN 패킷** 보냄

2. **서버** → **클라이언트** : 응답으로 **ACK 패킷** 보냄

3. **서버** → **클라이언트**: 처리해야할 모든 통신 끝내고 연결 종료하겠다는 **FIN 패킷** 보냄

4. **클라이언트**→ **서버** : 확인 응답으로 **ACK 패킷** 보냄

</details>

<br>

<br>

<br>

Reference

- https://gmlwjd9405.github.io/2018/09/19/tcp-connection.html

- https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=ak0402&logNo=221539958656

- https://daengsik.tistory.com/m/30
