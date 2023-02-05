# DNS

<br>

<br>

## DNS란 무엇인가?

**도메인 이름 시스템(DNS)은 사람이 읽을 수 있는 도메인 이름**(예:nayoungs.tistory.com)을 **머신이 읽을 수 있는 IP 주소**(예:211.249.222.33)**로 변환**한다. 이러한 DNS는 어떻게 등장하게 되었을까?

<br>

네트워크 안에서 호스트를 식별하기 위한 목적으로 IP 주소를 사용한다. 사람의 경우, 숫자보다 문자를 사용하는 것이 더 편하므로 도메인 이름을 이용하여 호스트를 식별한다. 도메인 이름을 사용하는 경우에도 최종적으로 IP 주소를 알고 있어야 상대방과 연결이 가능하므로, 네트워크에서 도메인이나 호스트 이름을 숫자로 된 IP 주소로 해석해 주는 TCP/IP 네트워크 서비스인 DNS가 등장하게 되었다. 

<br>

다시 한번 설명하자면, DNS의 정의는 **TCP/IP 상에서 사람이 기억하기 쉽게 문자로 만들어진 도메인을 숫자로 된 IP 주소로 바꾸는 서버**이다. 

<br>

<br>

## DNS 기본 사항

### IP 주소

스마트폰이나 노트북부터 대규모 소매 웹 사이트의 콘텐츠를 서비스하는 서버에 이르기까지, 인터넷 상의 모든 컴퓨터는 숫자를 사용하여 서로를 찾고 통신한다. 그리고 이러한 숫자를 IP주소라고 한다. IP 주소에 대한 더 자세한 설명은 [여기](https://nayoungs.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-IPInternet-Protocol)에서 확인할 수 있다.

### 포트 번호

UDP와 TCP는 모두 53번 포트를 사용한다. **DNS 서버에 질의를 보낼 때는 UDP를 사용**하고, Zone Transfer(영역 전송)과 512 Byte를 초과하는 패킷을 전송해야 하는 경우에는 TCP를 사용한다. 

```
🔎 Zone Transfer(영역 전송)
DNS 서버는 Zone 내의 호스트에 대한 정보를 담고 있는 데이터베이스를 가지고 있으며, 복수의 보조 서버들은 기본 서버 데이터의 복사본을 저장하고 있다. 기존 서버의 정보를 이용하여 보조 서버 데이터를 최신 상태로 업데이트하는 작업을 영역 전송이라고 한다. 
```

<br>

<br>

## DNS가 UDP를 사용하는 이유

DNS는 데이터를 교환하는 경우이다. 이러한 경우 TCP를 사용하게 되면, 데이터를 송신할 때 세션 확립을 위한 처리를 해야하며, 송신한 데이터가 수신되었는 지 점검하는 과정도 필요하기 때문에 UDP에 비해 프로토콜 오버헤드가 크다. UDP에 대한 더 자세한 설명은 [여기](https://nayoungs.tistory.com/entry/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-UDPUser-Datagram-Protocol)에서 확인할 수 있다.

- TCP가 전송 시작 전에 3-way handshaking 과정이 있는 반면 UDP는 연결 설정에 드는 비용이 없다. DNS는 신뢰성보다 속도가 더 중요한 서비스이기 때문에 TCP보다 UDP가 더 적합하다.

- TCP가 3 way handshake를 통해 연결 상태를 유지하는 반면, UDP는 connection을 유지할 필요가 없다. (no handshaking)

<br>

<br>

## DNS 서비스 유형

존재하는 도메인 수가 매우 많기 때문에 DNS 서버 종류를 계층화해서 단계적으로 처리한다. DNS 서버는 크게 권한 없는 네임 서버와 권한 있는 네임 서버로 나뉜다.  

<img src="https://www.cloudflare.com/img/learning/dns/dns-server-types/recursive-resolver.png" width=500/>

### 재귀적 DNS(권한 없는 네임 서버, non-Authoritative NS, Resolver)

대개 클라이언트는 신뢰할 수 있는 DNS 서비스에 직접 쿼리를 수행하지 않는다. 대신 해석기(Resolver) 또는 재귀적 DNS 서비스라고 알려진 다른 유형의 DNS 서비스에 연결하는 경우가 일반적이다. 재귀적 DNS 서비스는 호텔 컨시어지와 같은 역할을 한다. DNS 레코드를 소유하고 있지만 사용자를 대신해서 **DNS 정보를 가져올 수 있는 중간자의 역할**을 한다. 예시로는 ISP(통신사), Public DNS 등이 있다.

<br>

재귀적 DNS가 일정 기간 캐시된 또는 저장된 DNS 참조를 가지고 있는 경우, 소스 또는 IP 정보를 제공하여 DNS 쿼리에 답을 한다. 그렇지 않다면, 해당 정보를 찾기 위해 쿼리를 하나 이상의 신뢰할 수 있는 DNS 서버에 전달한다. 종종 수많은 DNS 조회를 수행해야한다. 원하는 도메인에 대한 권한 있는 서버에 도달할 때까지 계층적 DNS 트리를 위에서 아래로 반복하기 때문에 재귀 서버라고 한다. 

<br>

### 신뢰할 수 있는 DNS(권한 있는 네임 서버, Authoritative NS)

신뢰할 수 DNS 서비스는 개발자가 퍼블릭 DNS 이름을 관리하는 데 사용하는 업데이트 메커니즘을 제공한다. 이 메커니즘을 통해 DNS 시스템은 DNS 쿼리에 응답하고 도메인 이름을 IP 주소로 변환합니다. 그러면 컴퓨터가 서로 통신할 수 있게 된다. 신뢰할 수 있는 DNS는 **도메인에 대해 최종 권한이 있으며 재귀적 DNS 서버에 IP 주소가 담긴 답을 제공할 책임**이 있다.  신뢰할 수 있는 DNS 서버는 계층 구조(Root, TLD, Authoritative)로 구성되어 있으며, 모든 것은 Root Name Server에서 시작된다. 

- `Root DNS Server` : TLD DNS 서버 IP 주소들을 저장해두고 안내하는 역할을 한다. 루트 서버는 도메인 이름을 포함한 재귀 DNS의 쿼리를 수용하며, 해당 도메인의 확장자(com,. net, .org, etc.)에 따라 재귀 DNS를 TLD 네임 서버에 보내 응답한다. 
- `TLD DNS Server` : Authoritative DNS 서버 주소를 저장해 두고 안내하는 역할을 한다. TLD 네임서버는 .com, .net 또는 URL의 마지막 점 뒤에 오는 것 같은 일반적인 도메인 확장자를 공유하는 모든 도메인 이름의 정보를 유지한다. 예를 들어 TLD 네임서버는 ‘.com’으로 끝나는 모든 웹사이트의 정보를 갖고 있다. 사용자가 google.com을 검색하는 경우 재귀 DNS는 루트 네임서버로부터 응답을 받은 후 쿼리를 .com TLD 네임서버에 보내고, 해당 네임서버는 해당 도메인의 권한 있는 네임서버를 가리켜 응답한다.
- `Authoritative DNS Server` : 실제 개인 도메인 IP 주소의 관계가 기록/저장/변경되는 서버이다. 재귀 DNS가 TLD 네임서버로 부터 응답을 받으면, 해당 응답을 권한 있는 네임서버로 보낸다. 일반적으로 권한 있는 네임서버는 IP 주소를 확인하는 마지막 단계이다. 

<br>

### Authoritative vs Recursive

재귀 DNS(Resolver)가 DNS 쿼리의 첫 단계이다. 재귀 DNS는 클라이언트와 DNS 네임서버 사이의 중개자 역할을 한다. 재귀 DNS는 웹 클라이언트로부터 DNS 쿼리를 받은 후 캐시된 데이터로 응답하거나 요청을 루트 네임서버로 보내고 또 다른 요청을 TLD 네임서버로 보낸 후 마지막 요청을 권한 있는 네임서버로 보낸다. 재귀 DNS는 요청된 IP 주소가 있는 권한 있는 네임 서버로부터 응답을 받은 후 응답을 클라이언트에 보낸다.

간단하게 정리해보면 다음과 같다.



<img src="https://miro.medium.com/max/1174/0*UZGZLGsG26UNLmA2.png" width=500/>

웹 사이트를 방문 하는 사람은 재귀 DNS 서버에 조회를 요청하고, 재귀 DNS 서버는 필요한 Authoritative Name Server에 응답을 요청한다. 그런 다음 재귀적 DNS 서버는 정보가 필요한 사용자에게 답변을 전달한다. 이 과정 중 재귀 DNS는 권한 있는 네임 서버에서 받은 정보를 캐시한다. 클라이언트가 최근에 요청된 도메인 이름의 IP 주소를 요청하면, 재귀 DNS는 네임서버와의 통신 프로세스를 우회하고, 캐시에서 요청한 레코드를 클라이언트에 전달할 수 있다.

<br>

DNS Resolver(Recursive Server)가 여러 DNS 서버를 차례대로 (예시 : Root DNS -> com DNS -> naver.com DNS) 물어보기 때문에 그 답을 Recursive Query라고 부른다.

<br>

<br>

## Domain Name 구조(피라미드 구조)

<img src="https://blog.kakaocdn.net/dn/Xi5vo/btrmRas0ODd/a6DcLSLjXPlXq2YT5enWj0/img.png" width=500/>

통상적으로 도메인의 가장 끝에 있는 점(.)은 생략되어 있다. 이것을 루트 도메인이라고 부른다. 루트 도메인의 직속 하위 도메인은 Top-level 도메인이라고 부르며 com, net, co.kr 등이 있다. Top-level 도메인 직속 하위 도메인은 Second-level 도메인이라고 부르며 그 직속 하위 도메인을 Sub 도메인이라고 부른다. 

<br>

다음은 참고로 알아두면 되는 DNS에 통용되는 규칙이다.

- 직속 하위 레벨의 DNS 서버 리스트를 알고 있어야한다.
- 모든 컴퓨터는 적어도 Root 도메인의 DNS 서버를 알고 있어야한다.

<br>

<br>

## DNS 동작 과정

`www.naver.com`을 검색해보았을 때로 가정하여 DNS 동작 과정을 살펴보자.

<img src="https://velog.velcdn.com/images%2Fsms8377%2Fpost%2Fcf81568d-f1a7-43b0-8a00-925cadf7dcf3%2Fimage.png" width=600/>

- PC 브라우저에서 `www.naver.com`을 입력한다.
- PC는 미리 설정되어있는 DNS(local DNS, ISP)에게 `www.naver.com` 이라는 hostname에 대한 IP 주소를 물어본다.
- ISP DNS에 `www.naver.com`에 대한 주소가 캐싱되어있다면 해당 IP 주소를 반환하고, 그렇지 않다면 다음 단계로 넘어간다.
- Local DNS는 `www.naver.com`에 대한 주소를 찾아내기 위해 다른 DNS 서버들과 통신을 시작한다. 먼저 Root DNS 서버에게 `www.naver.com` 에 대한 IP 주소를 아는지에 대해 질의한다.  
- Root DNS 서버는 `.com` 도메인을 관리하는 TLD DNS 서버의 정보를 포함하여 응답한다.
- Local DNS 서버는 com Domain을 관리하는 DNS서버에게 `www.naver.com` 에 대한 IP 주소를 아는지에 대해 질의한다.
- com DNS 서버는  `naver.com` 도메인을 관리하는 DNS 서버의 정보를 포함하여 응답한다.
- Local DNS 서버는 `naver.com` 도메인을 관리하는 DNS 서버에게 `www.naver.com` 에 대한 IP 주소를 아는 지에 대해 질의한다. 
- IP 주소를 수신한 Local DNS는 `www.naver.com` 에 대한 IP 주소를 캐싱하고, 단말(PC)에 전달해준다. 

<br>

마지막으로 모든 과정을 담고 있는 재밌는 만화가 있어서 가져와보았다.

<img src="https://velog.velcdn.com/images/cieroyou/post/a683f906-d1b1-4e8a-b68d-46b0c5b3822f/dns-image.png" width=700/>

출처 : https://velog.io/@cieroyou/DNS-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0

<br>

<br>

## 기술 질문/예상 질문

<details>
<summary>DNS란?</summary>




<br>

TCP/IP 네트워크 상에서 사람이 기억하기 쉽게 문자로 만들어진 도메인을 숫자로 된 IP 주소로 바꾸는 서버를 DNS라고 한다.

</details>

<br>

<details>
<summary>DNS 서버에게 IP 주소를 요청할 때 UDP를 사용하는 이유는?</summary>




<br>

DNS는 신뢰성보다 속도가 더 중요하고, 많은 클라이언트를 수용하는 것을 필요로 한다. 따라서 속도가 빠르고, 연결 상태를 유지하지 않고 정보 기록을 최소화하여 많은 클라이언트 수용이 가능한 UDP를 사용한다.

</details>

<br>

<details>
<summary>DNS 서버의 종류에 대해 설명하라</summary>




<br>

Root DNS Server, TLD DNS Server, Authoritative DNS Server, Recursive DNS Server로 나누어진다.

</details>

<br>

<details>
<summary>DNS 서버를 여러 종류로 나누는 이유는?</summary>




<br>

존재하는 도메인 수가 많기 때문에 하나의 DNS 서버만 사용한다면 성능, 유지 관리 등 많은 문제가 생긴다. 따라서 DNS 서버 종류를 계층화해 단계적으로 처리하여 트래픽을 분산하고 유지 및 관리를 안정적으로 하기 위해 나누었다.

</details>

<br>

<details>
<summary>DNS의 동작 과정을 설명하라.</summary>




<br>

<img src="https://velog.velcdn.com/images%2Fsms8377%2Fpost%2Fcf81568d-f1a7-43b0-8a00-925cadf7dcf3%2Fimage.png" width=600/>

- PC 브라우저에서 `www.naver.com`을 입력한다.
- PC는 미리 설정되어있는 DNS(local DNS)에게 `www.naver.com` 이라는 hostname에 대한 IP 주소를 물어본다.
- ISP DNS에 `www.naver.com`에 대한 주소가 캐싱되어있다면 해당 IP 주소를 반환하고, 그렇지 않다면 다음 단계로 넘어간다.
- Local DNS는 `www.naver.com`에 대한 주소를 찾아내기 위해 다른 DNS 서버들과 통신을 시작한다. 먼저 Root DNS 서버에게 `www.naver.com` 에 대한 IP 주소를 아는지에 대해 질의한다.  
- Root DNS 서버는 `.com` 도메인을 관리하는 TLD DNS 서버의 정보를 포함하여 응답한다.
- Local DNS 서버는 com Domain을 관리하는 DNS서버에게 `www.naver.com` 에 대한 IP 주소를 아는지에 대해 질의한다.
- com DNS 서버는  `naver.com` 도메인을 관리하는 DNS 서버의 정보를 포함하여 응답한다.
- Local DNS 서버는 `naver.com` 도메인을 관리하는 DNS 서버에게 `www.naver.com` 에 대한 IP 주소를 아는 지에 대해 질의한다. 
- IP 주소를 수신한 Local DNS는 `www.naver.com` 에 대한 IP 주소를 캐싱하고, 단말(PC)에 전달해준다. 

</details>

<br>

<br>

<br>

<br>

<br>

<br>

Reference

- https://aws.amazon.com/ko/route53/what-is-dns/
- https://www.cloudflare.com/ko-kr/learning/dns/dns-server-types/
- https://dnsmadeeasyblog.medium.com/authoritative-vs-recursive-dns-servers-whats-the-difference-d0e5821c7617
- https://steady-coding.tistory.com/523