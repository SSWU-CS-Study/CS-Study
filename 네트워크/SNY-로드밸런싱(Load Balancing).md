# 로드밸런싱(Load Balancing)

<br>

## 로드 밸런싱(Load Balancing)

로드밸런싱이란, 네트워크 또는 서버에 가해지는 로드를 분산해주는 기술이다. 둘 이상의 CPU 혹은 저장 장치와 같은 컴퓨터 자원들에게 작업을 나눠주는 것을 의미한다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20230107194257864.png" width=500/>

### 로드 밸런싱의 필요성

로드 밸런싱은 여러 대의 서버를 두고 서비스를 제공하는 분산 처리 시스템에서 필요한 기술이다.

서버가 단 하나만 존재할 때, 수천만 명의 사람들이 동시에 서버에 접속하면 어떻게 될까?

하나의 서버는 부하를 감당하지 못할 수도 있을 것이고, 결국 정상적인 서비스가 불가능 해질 것이다.

이를 해결하는 방식에는 2가지가 있다.

<img src="https://velog.velcdn.com/images%2Fyanghl98%2Fpost%2F92aabceb-210d-49cd-a680-eabbec653ced%2Fimage.png" width=500/>

1. **Scale-up** : 서버 자체의 성능을 높이는(확장하는) 것이다. 비유하자면 CPU가 i3인 컴퓨터를 i7으로 업그레이드하는 것과 같다. 
2. **Scale-out** : 기존 서버와 동일하거나 낮은 성능의 서버를 여러 개 두어 운영하는 것이다. CPU가 i3인 컴퓨터를 여러 대 추가 구입해 운영하는 것에 비유할 수 있다. Scale-out의 경우, 여러 대의 서버로 트래픽을 균등하게 분산해주는 **로드밸런싱이 반드시 필요**하다.

<br>

### 로드 밸런싱의 목표

네트워크 뿐만 아니라 OS, 쿠버네티스와 같은 컨테이너 오케스트레이터 등에서 다음과 같은 공통적인 목표를 가지고 활용되고 있다.

1. 자원 사용 최적화
2. throughput(처리 용량) 증가
3. 응답 속도 감소
4. 특정 서버의 과부하 방지
5. 안정성, 가용성 제고

<br>

<br>

## 로드 밸런서(Load Balancer)

다시 한번 언급하면, 로드 밸런싱은 네트워크 또는 서버에 가해지는 부하를 분산해주는 기술이다.

그리고 **로드 밸런싱 기술을 제공하는 서비스 또는 장치**를 로드 밸런서(LB, Load Balancer)라고 부른다. LB는 클라이언트와 네트워크 트래픽이 집중되는 서버들 또는 네트워크 허브 사이에 위치한다. 또한, 특정 서버에 부하가 집중되지 않도록 트래픽을 다양한 방법으로 분산하여 서버들의 성능을 최적인 상태로 유지할 수 있도록 한다. 

<br>

로드 밸런서를 활용하면 실제로는 여러 대의 백엔드 서버가 동작하더라도, 사용자에게는 하나의 서비스처럼 보이게 할 수 있다.

방법은 로드밸런서가 서비스에서 사용되는 대표 IP 주소를 갖고 클라이언트의 요청을 받되, 자신과 연결된 백엔드 서버의 실제 IP로 요청을 보내는 것이다.

로드 밸런서는 이와 같이 동작하기 위해 다음과 같은 중요한 업무를 수행한다.

1. **Health Checking**
   - 로드 밸런서는 서버들에 대한 주기적인 상태 확인(Health Check)을 통해 서버들의 장애 여부를 판단할 수 있다.
   - healthy = 클라이언트의 요청을 처리할 수 있는 상태를 의미
2. **Service Discovery**
   - 현재 available한 서버의 목록을 유지하고, 그들의 IP 주소를 관리한다.
3. **Load Balancing**
   - 각 사용자의 요청을 특정 알고리즘을 활용하여 healthy한 백엔드 서버들에게 네트워크 부하를 분산한다.

<br>

### 로드 밸런서의 기본 기능

1. **상태 확인(Health Check)**
   - 로드 밸런서는 서버들에 대한 주기적인 상태 확인을 통해 서버들의 장애 여부를 판단할 수 있다.
   - health checking을 통해 서버에 이상이 생기면 정상적으로 동작하는 서버로 트래픽을 보내주는 Fail-over가 증하며, TCP/UDP 분석이 가능하기 때문에 방화벽(Firewall)의 역할도 수행할 수 있다.
2. **Tunneling**
   - 인터넷 상에서 눈에 보이지 않는 통로를 만들어 통신할 수 있게 하는 개념이다.
   - 데이터를 캡슐화하여, 연결된 상호 간에만 캡슐화된 패킷을 구별하여 캡슐화를 해제할 수 있다.
3. **NAT (Network Address Translation)**
   - IP 주소를 변환해주는 기능이다.
   - 로드 밸런싱 관점에서, 여러 개의 호스트가 하나의 공인 IP 주소(VIP, Virtual IP)를 통해 접속하는 것이 주 목적이다.
   - SNAT(Source Network Address Translation) : 내부에서 외부로 트래픽이 나가는 경우, 내부 Private IP 주소를 외부 Public IP 주소로 변환하는 방식이다. (집에서 사용하는 공유기가 대표적인 방식)
   - DNAT(Destination Network Address Translation) : 외부에서 내부로 트래픽이 들어오는 경우, 외부 Public IP 주소를 내부의 Private IP 주소로 변환하는 방식이다.
4. **DSR(Direct Server Routing)**
   - 서버에서 클라이언트로 되돌아가는 경우, 목적지를 클라이언트로 설정한 다음, 네트워크 장비나 로드 밸런서를 거치지 않고 클라이언트로 찾아가는 방식이다.
   - 이 경우, 로드 밸런서의 부하를 줄여줄 수 있다는 장점이 있다.

<br>

### 로드 밸런서 동작 방식

1) **L4 로드 밸런싱**
   - **전송 계층**에서 로드를 분산한다.
   - **IP 주소나 포트 번호, MAC 주소** 등에 따라 트래픽을 나누고 분산 처리가 가능하다.
   - CLB(Connection Load Balancer) 혹은 SLB(Session Load Balancer)라고 부르기도 한다.

<img src="https://user-images.githubusercontent.com/63101648/125741001-501ec659-dbde-4cd5-8bec-bbe131fb5030.png" width=500/>

2. **L7 로드 밸런싱**
   - 애플리케이션 계층에서 로드를 분산한다.
   - OSI 7계층의 프로토콜(HTTP, SMTP, FTP 등)을 바탕으로도 분산 처리가 가능하다.

<img src="https://user-images.githubusercontent.com/63101648/125741034-5ca914f9-27cc-42ed-ac14-7b38a3c6c3c8.png" width=500/>

<br>

<br>

## 로드 밸런싱 알고리즘(Load Balancing Algorithm)

### L4 로드 밸런싱 

L4 로드 밸런서에는 다음과 같은 로드 밸런싱 방법들이 있다.

1. **라운드로빈 방식(Round Robin Method)**
   - 서버에 들어온 요청을 **순서대로** 돌아가며 배정하는 방식
   - 클라이언트의 요청을 순서대로 분배하기 때문에, <u>여러 대의 서버가 동일한 스펙</u>을 갖고 있고, 서버와의 연결(세션)이 오래 지속되지 않는 경우에 활용하기 적합함
2. **가중 라운드로빈 방식(Weighted Round Robin Method)**
   - 각각의 서버마다 가중치를 매기고, **가중치가 높은 서버에 클라이언트 요청을 우선적으로 배분**하는 방식
   - 주로 서버의 <u>트래픽 처리 능력이 상이한 경우</u> 사용되는 부하 분산 방식

3. **IP 해시 방식(IP Hash Method)**
   - 클라이언트의 **IP주소를 특정 서버로 매핑**하여 요청을 처리하는 방식
   - 사용자의 IP주소를 해싱하여 로드를 분배하기 때문에, <u>사용자가 항상 동일한 서버로 연결</u>되는 것을 보장
4. **최소 연결 방식(Least Connection Method)**
   - 요청이 들어온 시점에 **가장 적은 연결 상태를 보이는 서버에 우선적**으로 트래픽을 배분하는 방식
   - 자주 세션이 길어지거나, 서버에 분배된 트래픽들이 일정하지 않은 경우에 적합
5. **최소 리스폰타임(Least Response Time Method, Fastest Response Time Method)**
   - 서버의 현재 연결 상태와 응답 시간(Response Time, 서버에 요청을 보내고 최초 응답을 받을 때까지 소요되는 시간)을 모두 고려하여 트래픽을 배분하는 방식
   - 가장 적은 연결 상태와 **가장 짧은 응답 시간**을 보이는 서버에 우선적으로 로드를 분

<br>

### L7 로드 밸런싱

L7 로드밸런서는 L4 로드 밸런서의 기능을 포함하는 것 뿐만 아니라, 애플리케이션 계층(HTTP, SMTP, FTP)의 정보를 바탕으로도 분산 처리가 가능하다. 즉, HTTP 헤더, 쿠키 등과 같은 사용자의 요청을 기준으로 특정 서버에 트래픽을 분산하는 것이 가능하다.

URL에 따라 부하를 분산시키거나, HTTP 헤더의 쿠키 값에 따라 부하를 분산하는 등 클라이언트의 요청을 보다 세분화하여 서버에 전달할 수 있다. 또한, L7 로드 밸런서의 경우 특정한 패턴을 지닌 바이러스를 감지해 네트워크를 보호할 수 있으며, DoS/DDoS와 같은 비정상적인 트래픽을 필터링할 수 있어 네트워크 보안 분야에서도 활용되고 있다.

L7 로드 밸런서에는 다음과 같은 로드 밸런싱 방법들이 있다.

1. **URL 스위칭 방식(URL Switching Method)**
   - 특정 하위 URL들은 특정 서버로 처리하는 방식
   - 예) `.../images` 또는 `.../video`와 같은 URL은 서버가 아닌 별도의 스토리지에 있는 객체 데이터로 바로 연결되도록 구성

2. **컨텍스트 스위칭 방식(Context Swiching Method)**
   - 클라이언트가 요청한 특정 리소스에 따라 특정 서버로 연결하는 방식
   - 예) 이미지 파일에 대해서는 확장자를 참조하여, 별도로 구성된 이미지 파일이 있는 서버 또는 스토리지로 직접 연결
3. **쿠키 지속성(Persistence with Cookies)**
   - 쿠키 정보를 바탕으로 클라이언트가 연결했었던 서버에 계속 할당 해주는 방식

<br>

<br>

## 로드 밸런서 주요 성능 지표

L4/L7 로드밸런서의 성능을 결정하는 주요한 지표들은 다음과 같다.

- 초당 연결수(Connections per second) : 최대 처리 가능한 초당 TCP 세션 개수를 의미
- 동시 연결수(Concurrent connections) : 동시에 유지할 수 있는 최대 세션 개수를 의미
- 처리 용량(Throughput) : UDP 프로토콜에 대한 로드밸런싱 성능 지표로, FWLB(Firewall Load Balancing)에서 중요하며, 단위는 bps(bit per second) 또는 pps(packet per second)를 사용

<br>

<br>

## 로드 밸런서 장애 대비

갑작스러운 장애에 대비하여, 로드밸런서 서버는 다음과 같이 이중화를 기본으로 구성한다.

<img src="https://user-images.githubusercontent.com/55661631/145209135-0adaedce-cfd1-4471-b3f7-efa60a7212c0.png" width=500/>

로드 밸런서에 장애가 발생했을 시의 시나리오는 다음과 같다.

- 이중화된 로드밸런서들은 서로의 상태를 확인(Health Check)한다.
- Master 서버에 Fail 되면 Standby 서버가 자동으로 Master 서버의 역할을 한다.
- Standby 서버는 평상 시에는 대기 상태로만 있다가 Master 서버가 Fail 되었을 경우에만 작동 한다.
- 이 구성을 Fail Over라 한다.

<img src="https://user-images.githubusercontent.com/55661631/145209146-0468a80d-c4dc-49a2-9919-be9e88cbb6aa.png" width=600/>

<br>

<br>

## 기술 면접 / 예상 질문

<details>
<summary>대규모 트래픽에 대처할 수 있는 방법에 대해 설명해주세요</summary>





<br>

서버의 성능을 높이는 Scale-up과 분산 처리를 위해 여러 대의 서버를 두는 Scale-out 방법이 있습니다. Scale-up 방식의 경우 한계가 있으므로, 주로 Scale-out 방식을 사용합니다. Scale-out 방식에서 분산 처리를 하기 위해 로드 밸런싱 기술을 사용합니다.

</details>

<br>

<details>
<summary>로드 밸런서의 기본 기능에는 무엇이 있는지 설명해주세요</summary>





<br>

서버들의 이상 유무를 파악하는 상태 확인(Health Check), 패킷을 캡슐화하여 연결된 상호 간에만 패킷을 구별할 수 있게 해주는 터널링(Tunneling), IP 주소를 변환 해주는 NAT 기능 등이 있습니다. 

</details>

<br>

<details>
<summary>로드 밸런싱의 종류에 대해서 설명해주세요</summary>





<br>

로드 밸런서는 OSI 7 계층을 기준으로 어떻게 부하를 분산하는 지에 따라 종류가 나뉩니다. 그 중 L4 로드밸런서와 L7 로드밸런서가 가장 많이 활용됩니다.

</details>

<br>

<details>
<summary>L4 로드 밸런싱에 대해서 설명해주세요</summary>





<br>

L4 로드 밸런서는 L4 계층(전송 계층)에서 동작하며, 네트워크 계층이나 전송 계층의 정보를 바탕으로 트래픽을 분산합니다. IP 주소, 포트 번호, MAC 주소, 전송 프로토콜에 따라 트래픽을 분산 처리하는 것이 가능합니다.

</details>

<br>

<details>
<summary>L7 로드 밸런싱에 대해서 설명해주세요</summary>





<br>

L7 로드 밸런서는 L4 로드 밸런서의 기능을 포함하며, 애플리케이션 계층의 정보를 바탕으로도 분산 처리가 가능합니다. HTTP 헤더, 쿠키 등과 같은 사용자의 요청을 기반으로 특정 서버에 트래픽을 분산할 수 있습니다. 

</details>

<br>

<details>
<summary>로드 밸런싱에 사용되는 알고리즘에 대해 설명해주세요</summary>





<br>

L4 로드 밸런서에는 라운드 로빈, 가중치 할당 방식, 최소 연결, 응답 시간, 해시 기반 등으로 특정 서버에 요청을 분배할 수 있습니다. L7 로드밸런서의 경우 L4 로드밸런서에 사용되는 방식 뿐만 아니라 URL 스위칭 방식, 컨텍스트 스위칭 방식, 쿠키 지속성 방식이 있습니다.

</details>

<br>

<details>
<summary>로드 밸런서가 장애를 대비하는 방법에 대해서 설명해주세요</summary>





<br>

로드 밸런서는 갑작스러운 장애에 대비하여 이중화를 기본으로 구성합니다. 이중화된 로드밸런서들은 서로의 상태를 확인하며 장애가 발생하면 정상적으로 작동하는 로드밸런서로 교체됩니다. 

</details>

<br>

<br>

Reference

- [링크](https://gyoogle.dev/blog/computer-science/network/Load%20Balancing.html)

- [링크](https://velog.io/@yanghl98/OS%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%8B%B1-Load-Balancing-%EC%A0%95%EC%9D%98-%EC%A2%85%EB%A5%98-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)

- [링크](https://etloveguitar.tistory.com/136)