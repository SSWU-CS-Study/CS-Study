# TCP/IP 4계층 모델

<br>

네트워크 전송 시 데이터 표준을 정리한 것이 OSI 7계층이었다면, 이 이론을 실제로 사용하는 인터넷 표준으로, 각종 기능을 계층화하고 복수의 프로토콜을 조합하여 실현시킨 것이 TCP/IP 4계층이다.

TCP/IP 모형은 현재의 인터넷에서 컴퓨터들이 서로 정보를 주고받는데 쓰이는 통신 규약(프로토콜)의 모음으로, 각 계층은 담당하는 위치마다 처리 역할을 구분해 진행함으로 서로 간의 간섭을 최소화하여 사용의 편리성을 높인다.

<img src="https://blog.kakaocdn.net/dn/bTFbPP/btqObZlPFkB/rtTQk14mYJGkjx3T9cGfd1/img.png" width=500/>

상위 계층인 **TCP**는 메세지나 **파일들을 좀 더 작은 패킷으로 나누어 인터넷을 통해 전송**하는 일과, **수신된 패킷들을 원래의 메세지로 재조립**하는 일을 담당한다. 반면, 하위 계층의 **IP**는 **각 패킷의 주소 부분을 처리**하며 패킷들이 **목적지에 정확하게 도달할 수 있게 한다.**

| TCP/IP 4계층                       | 역할                                             | 데이터 단위  | 전송 주소 | 예시                                                    | 장비           |
| ---------------------------------- | ------------------------------------------------ | ------------ | --------- | ------------------------------------------------------- | -------------- |
| 응용 계층(Application)             | 응용프로그램 간의 데이터 송수신                  | Data/Message | -         | 파일 전송, 이메일, FTP, HTTP, SSH, Telnet, DNS, SMTP 등 | -              |
| 전송 계층(Transport)               | 호스트 간의 자료 송수신                          | Segment      | Port      | TCP, UDP, RTP, RTCP 등                                  | 게이트웨이     |
| 인터넷 계층(Internet)              | 데이터 전송을 위한 논리적 주소 지정 및 경로 지정 | Packet       | IP        | IP, ARP, ICMP, RARP, OSPF                               | 라우터         |
| 네트워크 연결 계층(Network Access) | 실제 데이터인 프레임을 송수신                    | Frame        | MAC       | Ethernet, PPP, Token Ring 등                            | 브리지, 스위치 |

<br>

![Data-Flow-of-TCP/IP-protocol](https://www.elprocus.com/wp-content/uploads/Data-Flow-of-TCP-IP-protocol.jpg)

<br>

## 1) 네트워크 액세스 계층 (Network Access, Network Interface)

데이터 단위 : Frame

- OSI 계층의 1,2 계층에 해당된다.
- TCP/IP 패킷을 네트워크 매체로 전달하는 것과 네트워크 매체에서 TCP/IP 패킷을 받아들이는 과정을 담당한다.
- 물리적인 주소로 **MAC**을 사용
- 에러 검출 기능과 패킷의 프레임화 기능을 수행한다.
- 네트워크 접근 방법, 프레임 포맷, 매체에 대해 독립적으로 동작하도록 설계되었다.
- 흐름 제어(Flow Control)는 Header(MAC)에서, 에러 제어(Error Control)는 Tailer(CRC)에서 수행한다.

예시 : MAC, LAN, 패킷망 등에 사용되는 것(Ethernet, PPP, Token Ring 등)

<br>

## 2) 인터넷 계층 (Internet)

데이터 단위 : Packet

- OSI 계층에서 3 계층에 해당된다.
- 어드레싱(addressing), 패키징(packaging), 라우팅(routing) 기능을 제공한다.
- 통신 노드 간의 IP 패킷을 전송하는 기능과 라우팅 기능을 담당
- 논리적 주소인 IP를 이용한 노드간 전송과 라우팅 기능을 처리하게 된다.
- 네트워크상 최종 목적지까지 정확하게 연결되도록 연결성을 제공한다.
- 네트워크 상에서 데이터의 전송(패킷의 이동)을 담당하는 계층으로 서로 다른 네트워크 간의 통신을 가능하게 하는 역할을 수행한다.

예시 : IP, ARP, ICMP, RARP, OSPF

<br>

## 3) 전송 계층 (Transport)

데이터 단위 : Segment

- OSI 계층에서 4 계층에 해당된다.
- 자료의 송수신을 담당한다.
- 애플리케이션 계층의 세션과 데이터그램 통신 서비스를 제공한다.
- TCP/UDP가 핵심 프로토콜이다. TCP/UDP에 대한 구분을 하고 데이터에 대한 제어 정보가 여기에 포함된다.
- 데이터의 전송 및 흐름에 있어 **신뢰성 보장** 을 담당한다.

예시 : TCP, UDP, RTP, RTCP 등

<br>

## 4) 응용 프로그램 계층 (Applicatioin)

데이터 단위 : Data/Message

- 다른 계층의 서비스에 접근할 수 있게 하는 애플리케이션을 제공한다.
- 애플리케이션들이 데이터를 교환하기 위해 사용하는 프로토콜을 정의한다.
- TCP/IP 네트워크를 사용하거나 관리하는 것을 도와주는 프로토콜이다.

예시 : 파일 전송, 이메일, FTP, HTTP, SSH, Telnet, DNS, SMTP 등

<br>

<br>

<br>

## 기술 질문/예상 질문

<details>
<summary>TCP/IP 4계층에 대해서 설명해주세요</summary>



<br>

1계층 네트워크 액세스 : 물리+데이터링크, MAC주소 사용

2계층 인터넷 : 네트워크, 통신 노드간의 IP패킷을 전송하는 기능과 라우팅 기능 담당

3계층 전송 : 전송, 통신 노드간의 연결 제어 및 신뢰성 있는 데이터 전송 담당

4계층 응용 : 세션+표현+응용, 응용 프로그램 구현

</details>

<br>

<br>
<br>

<br>







Reference

- https://devowen.com/344
- https://velog.io/@jehjong/%EA%B0%9C%EB%B0%9C%EC%9E%90-%EC%9D%B8%ED%84%B0%EB%B7%B0-TCPIP-4%EA%B3%84%EC%B8%B5

- https://junu0516.github.io/posts/tcp_ip_4%EA%B3%84%EC%B8%B5/