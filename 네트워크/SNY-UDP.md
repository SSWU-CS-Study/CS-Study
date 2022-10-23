# UDP

User Datagram Protocol의 약자로, 데이터를 데이터그램(Datagram) 단위로 처리하는 프로토콜이다.

TCP와는 다르게 데이터를 패킷으로 나누고 반대편에서 재조립하는 과정을 거치지 않으며, **수신지에서 제대로 받든 받지 않든 상관하지 않고 데이터를 보내기만 한다.** 즉, 비연결형, 신뢰성 없는 전송 프로토콜로, 데이터그램 단위로 쪼개면서 전송을 해야하기 때문에 전송 계층에 속한다.

<br>

또한 목적지에 도달하려고 하지만 에러가 날 수도 있고, 재전송이나 순서 뒤바뀜에 대한 대처는 애플리케이션에서 처리해주어야한다.

그러나 **UDP는 속도가 빠르다.** 별도의 연결도 필요하지 않고, TCP 처럼 ACK 메세지를 통해 확인을 받거나 하는 작업이 없기 때문에 TCP보다 빠르며, 이러한 속도의 장점으로 UDP는 실시간 방송 등에 사용된다. 

<br>

#### TCP vs UDP

|                         | **TCP**          | **UDP**                 |
| ----------------------- | ---------------- | ----------------------- |
| **데이터 전송단위**     | 세그먼트         | 블록 형태의 다이어 그램 |
| **서비스의 형태**       | 연결형           | 비연결형                |
| **수신 순서**           | 송신 순서와 동일 | 수신 수서와 불일치      |
| **오류 제어, 흐름제어** | 있음             | 없음                    |

<br>

<br>

#### TCP와 UDP는 왜 나오게 되었는가?

1. IP의 역할은 Host to Host (장치 to 장치)만을 지원한다. 장치에서 장치로의 이동은 IP로 해결되지만, 하나의 장비 내에서 수많은 프로그램들이 통신을 할 경우에는 IP만으로는 한계가 있다.
2. IP에서 오류가 발생하는 경우 ICMP에서 알려준다. 그러나 ICMP는 알려주기만 할 뿐, 대처를 하지 못하기 때문에 IP보다 위에서 처리를 해줘야한다.

1번의 문제를 해결하기 위하여 포트 번호가 나오게되었고, 2번을 해결하기 위해 상위 프로토콜인 TCP와 UDP가 만들어지게 되었다.

> **ICMP** : 인터넷 제어 메시지 프로토콜로 네트워크 컴퓨터 위에서 돌아가는 운영체제에서 오류 메시지를 전송받는데 주로 쓰인다.

<br>

#### TCP와 UDP에서의 오류는 어떻게 해결하는가?

<img src="https://t1.daumcdn.net/cfile/tistory/232D0545586CAD7410" width=400/>

**TCP**

데이터의 분실, 중복, 순서가 뒤바뀜을 자동으로 보정해줘서 송수신 데이터의 정확한 전달을 할 수 있도록 해준다. -> 신뢰성

**UDP**

IP가 제공하는 정도의 수준만을 제공하는 간단한 IP 상위 계층의 프로토콜이다. TCP와는 다르게 에러가 날 수도 있고, 재전송이나 순서가 뒤바뀔 수도 있어, 이 경우 애플리케이션에서 처리해야한다는 번거로움이 존재한다. -> 비신뢰성

<br>

#### UDP를 사용하는 이유

UDP의 장점인 **데이터의 신속성** 때문이다. 데이터의 처리가 TCP보다 빠르다.

주로 실시간 방송과 온라인 게임에서 사용되며, 네트워크 환경이 안 좋을 때 끊기는 현상을 생각해볼 수 있다.

<br>

<br>

## UDP Header

![img](https://t1.daumcdn.net/cfile/tistory/272A5A385759267B36)

- Source port : 시작 포트
- Destination port : 도착지 포트
- Length : 길이
- Checksum : 오류 검출 
  - 중복 검사의 한 형태로, 오류 정정을 통해 공간이나 시간 속에서 송신된 자료의 무결성을 보호하는 단순한 방법이다.
  - 헤더와 데이터의 에러를 확인하기 위한 필드이다.(전송된 데이터가 변형이 되지 않았는지 확인하는 값) UDP 헤더는 에러복구를 위한 필드가 불필요하기 때문에 TCP 헤더에 비해 간단하다.

<br>

🔎 TCP 헤더

<img src="https://itwiki.kr/images/c/cf/TCP_%EC%84%B8%EA%B7%B8%EB%A8%BC%ED%8A%B8_%ED%97%A4%EB%8D%94.jpg" width =400/>

TCP헤더와 비교했을 때, UDP는 별도의 연결 없이, 데이터를 단순히 전송만 하고 응답을 확인하지 않으므로 상당히 단순하고 직관적인 형태인 것을 확인할 수 있다.

참고)

![img](https://t1.daumcdn.net/cfile/tistory/2332D743539FF90A31)

<br>

<br>

## DNS에서 UDP의 사용

DNS는 데이터를 교환하는 경우이다. 이러한 경우 TCP를 사용하게 되면, 데이터를 송신할 때 세션 확립을 위한 처리를 해야하며, 송신한 데이터가 수신되었는 점검하는 과정도 필요하기 때문에 UDP에 비해 프로토콜 오버헤드가 크다.

DNS는 Application layer protocol(7계층)이다. 

<br>

#### DNS에서 UDP를 사용하는 이유

- TCP가 3 way handshake를 사용하는 반면, UDP는 connection을 유지할 필요가 없다. (No Handshaking)
- DNS request는 UDP 세그먼트에 꼭 들어갈 정도로 작다. 
  - DNS query는 single UDP request와 서버로부터의 single UDP reply로 구성되어 있다.
  - 단, 크기가 512(UDP 제한)이 넘을 때(Zone transfer), 혹은 응답을 받지 못한 경우는 TCP를 사용해야한다.
    - Zone Transfer : DNS 서버 간의 요청을 주고 받을 때 사용하는 Transfer
- Request에 대한 손실은 Application Layer에서 제어가 가능하다. (UDP는 not reliable이나, reliability는 application layer에 추가될 수 있다)

<br>

<br>

## 기술 질문/예상 질문

## 

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
<summary>www.google.com에 접속하면 일어나는 일</summary>




<br>

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221023181551231.png" width=500/>

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221023181627189.png" width=500/>



</details>

<br>

<br>

<br>

Reference

- https://gyoogle.dev/blog/computer-science/network/UDP.html

- https://itstudyblog.tistory.com/295

- https://codens.info/767