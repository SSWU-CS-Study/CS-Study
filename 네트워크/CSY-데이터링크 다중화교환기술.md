# 데이터링크계층의 다중화/교환기술

데이터링크계층에서 원활하게 데이터 전송을 하기 위해 다중화와 교환 기술을 사용한다.

## 다중화

: 데이터링크의 공유에 대한 총괄적인 표현

- MUX: Multiplexing, 다중화
- DEMUX: Demultiplexing, 역다중화

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/6npJa/btq0tfqkVbu/WL7hscOXVFzMVzFZRwzSkK/img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/6npJa/btq0tfqkVbu/WL7hscOXVFzMVzFZRwzSkK/img.png)

데이터링크의 효율성을 극대화하기 위해 다수의 디바이스가 단일 데이터링크를 공유해 전송하는 전송 기술

⇒ 여러 대의 디바이스로부터 나오는 데이터 신호를 하나의 전송링크로 전송되도록 하는 다중화 과정
       + 링크를 통해 수신된 신호를 출력 디바이스에 일치시키는 역다중화 과정

### 주파수 분할 다중화

주파수 대역폭을 몇 개의 작은 주파수 대역으로 나누어 각각 부채널로 나누어 구성한 후 부채널을 여러 디바이스에 할당

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/9949DA33599C156C30](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/9949DA33599C156C30)

- 간단한 구조로 구현되므로 가격이 저렴
- 별도의 변조기나 복조기가 필요하지 않다.
- 부채널 간의 상호 간섭을 방지하기 위해 보호 대역을 사용→ 대역폭 낭비

### 시분할 다중화

채널에 할당된 데이터 전송 허용시간을 일정한 시간 슬롯으로 나누고 채널도 다시 부채널로 나누어 각 시간 슬롯을 부채널에 순차적으로 할당하여 사용하는 방식

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/99AAD533599C160A1F](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/99AAD533599C160A1F)

- 구조 간단/비용저렴
- 데이터 전송률의 조절이 가능하다.
- 부채널에 전송할 데이터가 없어도 시간 슬롯이 할당 → 시간 낭비

### 통계적 시분할 다중화

부채널의 대역폭을 동적으로 할당하여 시분할 다중화의 단점을 보완

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/99A90B33599C16202E](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/99A90B33599C16202E)

데이터 전송을 하고자 하는 부채널에만 시간 슬롯을 할당→ 대역폭의 낭비 최소화

- 회로가 복잡해지고 비용이 증가한다.

## 교환 기술

데이터 전송을 하기 위해선 연결이 필요하다. 
이때 최적의 연결성을 제공하기 위해 교환기술을 사용한다.

- 교환 데이터 통신 네트워크
: 상호 연결된 노드들의 집합이 데이터링크를 적절히 선택하여 전송을 수행한다.
    - 회선교환 네트워크
    - 메시지교환 네트워크
    - 패킷교환 네트워크
- 방송 데이터통신 네트워크
: 중간 교환 노드 없이 전송매체를 통해 직접 접속한다.
    - 패킷 라디오 네트워크
    - 위성통신 네트워크
    - 저역 네트워크

### 회선교환 네트워크

데이터 통신이 이루어지기 전에 먼저 전용 전송로를 설정

- 안정적이고 실시간 데이터 서비스가 가능
- 연결이 설정된 회선은 데이터가 전송되지 않을 때도 다른 사용자가 사용할 수 없으므로 효율성이 떨어진다.
- 전화, 센서, 원격측정 등 비교적 데이터 흐름이 연속적인 경우에 사용

### 메시지교환 네트워크

데이터의 논리적인 단위를 교환하는 방식

전용 전송로를 설정할 필요가 없고 메시지에 수신지 주소를 첨부해 전송된다.

메시지는 노드에서 노드로 네트워크를 통해 전송되는데, 각 노드에서는 메시지를 수신하면 잠시 저장했다가 다음 노드로 보낸다 ⇒ 축적 후 전달(Store and Forward)

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/2674DA4653D37DD713](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/2674DA4653D37DD713)

- 메시지를 축적한 후 전송하여 전송로률 효율적으로 이용
- 상대 스테이션이 메시지를 수신할 수 없을 때도 메시지를 저장했다가 수신 가능한 상태일 때 전송할 수 있다.
- 데이터 전송량이 폭주할 때도 축적 기능을 통해 혼란 상태를 피할 수 있다.
- 우선순위 전송이 가능하다.

### 패킷교환

정해진 크기와 형식에 맞도록 구성한 데이터 블록인 **패킷** 형태로 만들어진 데이터를 전송하는 방식

패킷에는 번호, 수신지 주소, 오류검출 비트가 포함됨.

패킷 교환기가 수신지 주소에 따라 적절한 경로를 선택해 전송.

- 노드가 교환기 역할을 수행 ⇒ 메시지 교환 방식과 비슷
- 노드가 메시지를 축적하지는 않는다. ⇒ 빠른 응답시간이 요구된 응용에도 사용할 수 있다.

**데이터그램 패킷교환**

![https://woovictory.github.io/img/datagram_packet.png](https://woovictory.github.io/img/datagram_packet.png)

데이터를 전송하기 전에 논리적 연결이 설정되지 않으며 패킷이 독립적으로 전송

최적의 경로를 선택하여 패킷을 전송하는데 하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있다. (**비연결 지향형**)

**송신 측에서 전송한 순서와 수신 측에 도착한 순서가 다를 수 있다.**

---

**가상회선 패킷교환**

![https://woovictory.github.io/img/virtual_circut_packet.png](https://woovictory.github.io/img/virtual_circut_packet.png)

데이터를 전송하기 전에 논리적 연결이 설정되는데, 이를 가상회선이라고 한다.(**연결 지향형**) 

각 패킷에는 가상회선 식별 번호(VCI)가 포함되고, 모든 패킷을 전송하면 가상회선이 해제되고 패킷들은 전송된 순서대로 도착한다.

데이터 그램은 패킷마다 라우터가 경로를 선택하지만, 가상회선 방식은 경로를 설정할 때 한 번만 수행한다.

정해진 시간 안이나 다량의 데이터를 연속으로 보낼 때는 **가상 회선 방식**이 적합하다.

짧은 메시지의 일시적인 전송에는 **데이터그램 방식**이 적합하다.

## 예상 질문

- 데이터통신 시스템에서 효율적인 데이터 전송을 위해 어떤 방법을 사용하는지 설명하시오.
    
    데이터 전송의 효율성을 높이기 위해 다중화와 교환기술이 있습니다.
    
    다중화는 다수의 디바이스가 하나의 링크를 공유하는 방법으로 주파수 분할 다중화, 시분할 다중화 등이 있습니다.
    
    교환기술은 디바이스 상호 간 데이터 전송 시 최적의 연결성을 제공하는 기술로, 데이터 전송로 구성에 따라 회선교환 방식, 메시지교환 방식, 패킷교환 방식 등이 있습니다. 
    
- 패킷교환 방식에서 데이터그램 패킷 교환과 가상회선 패킷 교환을 비교하시오.
    
    데이터그램 패킷 방식은 패킷이 독립적으로 전송되어 비연결지향형이고 하나의 메시지에서 분할된 여러 패킷은 서로 다른 경로로 전송될 수 있습니다.또, 송신 측에서 전송한 순서와 수신 측에 도착한 순서가 다를 수 있습니다.
    
    가상회선 패킷 교환 방식은 데이터를 전송하기 전에 논리적 연결인 가상회선이 설정되어 연결 지향형이고 패킷이 전송된 순서대로 도착한다는 특징이 있습니다.
    

## 참고자료

- [가상회선과 데이터그램](https://woovictory.github.io/2018/12/28/Network-Packet-Switching-Method/)
