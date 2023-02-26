# 소켓(Socket)과 소켓 통신

<br>

## 소켓(Socket)이란?

소켓은 네트워크를 공유하는 **프로세스 간 통신의 종착점**이다. 프로세스를 네트워크 세계로 데이터를 내보내거나 혹은 그 세계로부터 데이터를 받기 위한 실제적인 창구 역할을 한다고 할 수 있다. 따라서 **프로세스가 데이터를 보내거나 받기 위해서는 <u>반드시 소켓을 열어, 소켓에 데이터를 써보내거나 소켓으로부터 데이터를 읽어들여야한다.**</u>

<br>

<img src="https://www.tutorialspoint.com/data_communication_computer_network/images/sockets.jpg" width=350/><img src="https://blog.kakaocdn.net/dn/cSH8fU/btqvxsTPQ2E/pnl61uUJOAPdf73whDlTW0/img.png" width=400/>

OSI 7계층 중 **응용 계층**에 속하는 프로세스들은 데이터 송수신을 위해 반드시 소켓을 거쳐 **전송 계층**으로 데이터를 전달해야한다. 즉, 소켓은 **전송 계층과 응용 프로그램 사이의 인터페이스 역할을 하며, 떨어져 있는 두 호스트를 연결**해준다.

<br>

소켓은 다음의 3요소로 정의된다. 

- 프로토콜 : 어떤 시스템이 다른 시스템과 통신을 원활하게 수용하도록 해주는 통신 규약, 약속
- IP 주소 : 각 컴퓨터에 부여된 고유의 식별 주소
- 포트 : 네트워크 상에서 통신하기 위해서 호스트 내부적으로 프로세스가 할당받아야하는 고유한 숫자

<br>

<br>

## 일반적인 소켓 통신의 흐름

<img src="https://velog.velcdn.com/images/newdana01/post/817b8ba8-4428-4e66-8128-7c01b554f8ff/image.png" width=450/>

### 서버(Server)

클라이언트 소켓의 연결 요청을 대기하고, 연결 요청이 오면 클라이언트 소켓을 생성하여 통신이 가능하게 한다.

1) `socket()` 함수를 이용하여 소켓을 생성
2) `bind()` 함수로 ip와 port 번호를 설정
3) `listen()` 함수로 클라이언트의 접근 요청에 수신 대기열을 만들어, 몇 개의 클라이언트를 대기 시킬 지 결정
4) `accept()` 함수를 사용하여 클라이언트와의 연결
5) 클라이언트와 서버가 서로 `read()` `write()` 를 하며 통신(데이터 송수신, 이 과정이 반복)
6) `close()` 함수로 소켓 닫기

### 클라이언트(Client)

실제로 데이터 송수신이 일어아는 것은 클라이언트 소켓이다.

1. `socket()` 함수로 가장 먼저 소켓을 생성
2. `connect()` 함수를 이용하여 통신을 할 서버에 설정된 ip와 port 번호에 통신을 시도
3. 통신을 시도 시, 서버가 `accept()` 함수를 이용하여 클라이언트의 socket descriptor를 반환
4. 이를 통해 클라이언트와 서버가 서로 `read()` `write()` 를 하며 통신(데이터 송수신, 이 과정이 반복)
5. `close()` 함수로 소켓 닫기

<br>

<br>

## 소켓의 종류

### 스트림 소켓

- **TCP를 사용하는 연결 지향 방식의 소켓**
- 송수신자의 연결을 보장하여 신뢰성있는 데이터 송수신이 가능
- 데이터의 순서 보장
- 소량의 데이터보다는 대량 데이터 전송에 적합
- 점대점 연결

<img src="https://velog.velcdn.com/images/newdana01/post/2999a10d-fe3a-4cd4-bf5f-90c2f777a2e9/image.png" width=400/>

### 데이터그램 소켓

- **UDP를 사용하는 비연결형 소켓**
- 데이터의 순서와 신뢰성을 보장할 수 없음
- 점대점 뿐만 아니라 일대 다 연결도 가능

- **accept 과정 없이, 소켓 생성 후 바로 데이터 송수신**

![img](https://velog.velcdn.com/images/newdana01/post/6d60e795-cd8d-4823-a6a0-bca9deb50cbf/image.png)

<br>

<br>

## HTTP 통신과 SOCKET 통신의 비교

<img src="https://user-images.githubusercontent.com/57826388/95358518-1ca49780-0904-11eb-9386-e4ecb76381d6.png" width=500/>

### HTTP 통신

- 클라이언트의 요청이 있을 때만 서버가 응답하는 **단방향 통신**
- JSON, HTML, IMAGE 등 다양한 데이터를 주고 받을 수 있음
- 서버가 응답한 후 연결을 바로 종료하는 단방향 통신이지만 Keep Alive 옵션을 주어 일정 시간 동안 커넥션을 유지할 수 있다.
- 실시간 연결이 아닌 데이터 전달이 필요한 경우에만 요청을 보내는 상황에 유리

### SOCKET 통신

- 클라이언트와 서버가 특정 포트를 통해 **양방향 통신**을 하는 방식
- 데이터 전달 후 연결이 끊어지는 것이 아니라 계속해서 연결을 유지 -> HTTP에 비해 더 많은 리소스 소모
- 클라이언트와 서버가 실시간으로 계속하여 데이터를 주고 받아야하는 경우에 유리
- 실시간 동영상 스트리밍이나 온라인 게임 등에 사용

<br>

<br>

## 기술 면접 / 예상 질문

<details>
<summary>소켓이란 무엇인지에 대해 설명해주세요</summary>




<br>

- 네트워크를 공유하는 프로세스 간 통신의 종착점

- 프로세스가 데이터를 보내거나 받기 위해서는 반드시 소켓을 열어, 소켓에 데이터를 써보내거나 소켓으로부터 데이터를 읽어들여야한다.

</details>

<br>

<details>
<summary>https 통신과 socket 통신의 차이점에 대해서 설명해주세요</summary>




<br>

HTTP 통신

- 클라이언트의 요청이 있을 때만 서버가 응답하는 **단방향 통신**
- JSON, HTML, IMAGE 등 다양한 데이터를 주고 받을 수 있음
- 실시간 연결이 아닌 데이터 전달이 필요한 경우에만 요청을 보내는 상황에 유리

SOCKET 통신

- 클라이언트와 서버가 특정 포트를 통해 **양방향 통신**을 하는 방식
- 데이터 전달 후 연결이 끊어지는 것이 아니라 계속해서 연결을 유지 
- 클라이언트와 서버가 실시간으로 계속하여 데이터를 주고 받아야하는 경우에 유리
- 실시간 동영상 스트리밍이나 온라인 게임 등에 사용

</details>

<br>







<br>

<br>

<br>

<br>

Reference

- https://velog.io/@newdana01/%EC%86%8C%EC%BC%93%EC%9D%B4%EB%9E%80-%EC%A2%85%EB%A5%98-%ED%86%B5%EC%8B%A0-%ED%9D%90%EB%A6%84-HTTP%ED%86%B5%EC%8B%A0%EA%B3%BC%EC%9D%98-%EC%B0%A8%EC%9D%B4

- https://helloworld-88.tistory.com/215
- https://www.tutorialspoint.com/data_communication_computer_network/client_server_model.htm