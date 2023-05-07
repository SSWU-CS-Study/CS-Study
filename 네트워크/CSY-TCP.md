# 전송 계층

![https://lamarr.dev/images/basicnetworking/network-049.jpg](https://lamarr.dev/images/basicnetworking/network-049.jpg)

TCP, UDP, SCTP

- 프로세스 대 프로세스 프로토콜

## TCP

- 데이터 전송의 신뢰성 제공
    - 종단간(End-To-End, E2E) 의 흐름제어 및 오류제어 기능
        - 전송지 호스트 컴퓨터에서 최종 수신지 호스트까지의 신뢰성
        - 하위 계층(네트워크 계층)은 종단간이 아닌 이웃 노드 간의 신뢰성
            
            ![https://blog.kakaocdn.net/dn/DLMNL/btqCioJ46w0/ibswSMmR6tJISDEHV8dKSK/img.png](https://blog.kakaocdn.net/dn/DLMNL/btqCioJ46w0/ibswSMmR6tJISDEHV8dKSK/img.png)
            

- 다중 수신지 분류, 패킷 손실이나 중복과 같은 오류를 복구하는 기법 포함
- 스트림 지향성
    - 데이터를 전송할 때 스트림 형태로 처리
    - 전송지에서의 데이터 순서가 최종 수신지에서도 일치되도록 한다.
- 연결지향적
    - 두 개체 간에 하나 이상의 메시지가 연결 상태를 유지하며 데이터교환이 가능하도록 하는 것
    - 데이터의 지속적이고 연속적인 흐름에 적합
    - 연결지향 서비스가 진행되는 과정
        1. 전송지와 수신지의 응용프로그램은 각각의 운영체제 하에서 동작하고, 운영체제 내의 소프트웨어 모듈이 네트워크를 통해 메시지를 보내 데이터 전송 준비를 한다.
        2. TCP 프로토콜 모듈이 연결설정을 실행한다(**논리적 연결**, 가상회선 연결)
        3. 연결이 되면 데이터를 전송하고 상호 간의 전송 상태를 확인한다.
        4. 데이터가 모두 전송되면 연결을 끊는다.
- 전이중 양방향 전송 지원
    - 쌍방이 동시에 송신할 수 있는 것
    - 하나의 데이터 스트림에 대해 반대 방향으로 제어 정보를 보낼 수 있어 효율적인 전송이 가능해진다.
        
        ![https://mblogthumb-phinf.pstatic.net/MjAxOTA1MDJfMTk3/MDAxNTU2NzYwOTQzOTAy.ihyxp3EhdQenFoN9ZtLjx7KpgSgLcISOExWycfX9jdgg.6m63VDrqyVMdwLebyZJJ74d55nctFrVIHMp330AZDdog.PNG.cni1577/캡처.PNG?type=w800](https://mblogthumb-phinf.pstatic.net/MjAxOTA1MDJfMTk3/MDAxNTU2NzYwOTQzOTAy.ihyxp3EhdQenFoN9ZtLjx7KpgSgLcISOExWycfX9jdgg.6m63VDrqyVMdwLebyZJJ74d55nctFrVIHMp330AZDdog.PNG.cni1577/캡처.PNG?type=w800)
        

## TCP 세그먼트의 형식과 동작

### TCP 세그먼트의 형식

세그먼트: 전송 계층의 TCP 프로토콜에서 두 호스트 간에 사용하는 전송 단위

| 헤더 | 데이터 |
| --- | --- |

![헤더의 구성](https://blog.kakaocdn.net/dn/mM3q3/btrfWb0AT8N/NkJOhIeVk8Kbz3lfp0n2kK/img.png)

헤더의 구성

- 포트 번호 : 응용프로그램을 식별하도록 TCP 포트 주소 표기
- 시퀀스 번호 : 데이터 스트림의 흐름을 구별하는 데 사용
- ACK번호 : 확인응답번호, 수신자가 예상하는 다음 바이트의 번호
- 플래그 : 회선과 데이터의 관리, 제어 기능을 수행하는 영역
    - ACK(유효), SYN(동기화), FIN(종료) 등등..
- 체크섬 : 헤더와 데이터의 에러 확인용

### TCP의 동작

**연결 설정**

3-way handshake 사용

![https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_1.png](https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_1.png)

0. 전송지와 수신지 모두 시퀀스 번호를 난수로 초기화*하고 데이터 전송 대기상태이다.

1. SYN 플래그를 설정하고 시퀀스 번호를 담은 세그먼트를 수신지에 전송한다.
시퀀스 번호: `x`
SYN FLAG: `1` 
2. 수신지는 세그먼트를 수신, SYN+ACK을 포함한 세그먼트를 전송지로 전송
시퀀스 번호: `y`
ACK 번호: `x + 1`
ACK FLAG: `1` - 수신한 세그먼트에 대한 응답임
SYN FLAG: `1` - 교신 과정이 진행되고 있음
3. 전송지는 수신지로부터 세그먼트를 수신받고 그에 대한 ACK(응답)정보를 전송한다
시퀀스 번호: `x + 1`
ACK 번호: `y + 1`

- 시퀀스 번호에 난수를 사용하는 이유?
    
    연결할 때 사용하는 포트는 유한 범위이기 때문에 재사용이 된다.
    
    따라서 두 통신 호스트가 같은 포트 번호 쌍을 사용할 때 패킷의 SYN 번호를 보고 패킷이 어떤 연결에 대한 것인지 구분하게 된다.
    
    현재 번호에 대해 순차적인 번호일 경우: 이전의 연결
    
    순차적인 번호가 아닐 경우: 새로운 연결
    

**데이터 송수신**

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/2254D2375908281F0C](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://t1.daumcdn.net/cfile/tistory/2254D2375908281F0C)

**연결 종료**

4-way handshake 사용

![https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_2.png](https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_2.png)

0. 전송지와 수신지의 시퀀스 번호는 이전까지 사용하던 번호이다.

1. 전송지에서 FIN 플래그를1로 설정해 세그먼트 전송
시퀀스 번호: `t`
FIN_FLAG: `1`
2. 수신지에서 세그먼트 수신, 요청에 대한 응답 전송
시퀀스 번호: `u`
ACK 번호: `t + 1`
전송할 데이터(optional): `v`
3. 수신지에서 작업이 끝나면 FIN 플래그를1로 설정해 세그먼트 전송
시퀀스 번호: `u(+ v)`
ACK 번호: `t + 1`
FIN_FLAG: `1`
4. 전송지에서도 ACK 세그먼트를 보내 응답한다.
시퀀스 번호: `t + 1`
ACK 번호: `u(+ v) + 1`
수신지는 ACK 세그먼트를 받으면 더 이상 데이터를 활성화하지 않고 연결을 종료한다.
전송지에서는 일정 시간의 TIME_WAIT 이후에 연결 정보를 삭제한다.
- TIME_WAIT 을 설정하는 이유
    
    데이터 유실로 인한 재전송이나 라우팅 지연 등으로 인해 데이터가 늦게 도착하거나 응답을 받지 못하는 경우를 대비하기 위해서
    

## 예상 질문

- TCP의 동작을 설명해주세요.
    
    신뢰성 있는 데이터 연결을 지원하는 연결지향형 프로토콜로 3-way handshake를 이용한 연결 설정, 데이터 송수신, 4-way handshake를 이용한 연결 종료로 동작합니다.
    
- TCP연결 설정할 때 시퀀스 번호를 난수로 사용하는 이유가 무엇인가요?
    
    TCP계층에서 호스트끼리 연결할 때 사용하는 포트는 유한 범위이기 때문에 시간이 지나면 재사용을 하게 됩니다.
    
    따라서 두 통신 호스트가 같은 포트 번호 쌍을 사용할 때 패킷의 SYN 번호를 보고 패킷이 어떤 연결에 대한 것인지 구분하게 된다.
    

## 참고자료

- [구글에 접속하기까지](https://lamarr.dev/ja/networkingbeginner/2020/03/23/13.html)
- [3/4-way handshaking](https://m.blog.naver.com/sgs03091/222080657380)
- [데이터 송수신](https://copycode.tistory.com/100)
