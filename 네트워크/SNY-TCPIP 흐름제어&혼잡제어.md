# TCP/IP 흐름제어 & 혼잡제어

<br>

### 📌Index

- [TCP 통신이란?](#tcp-통신이란)
- [흐름 제어](#흐름-제어)
  - [Stop and Wait](#stop-and-wait)
  - [Sliding Window](#sliding-window)
- [혼잡 제어](#혼잡-제어)
  - [AIMD (Additive Increase/Multiplicative Decrease)](#aimd-additive-increase/multiplicative-decrease)
  - [Slow Start(느린 시작)](#slow-start느린-시작)
  - [Fast Retransmit(빠른 재전송)](#fast-retransmit빠른-재전송)
  - [Fast Recovery(빠른 회복)](#fast-recovery빠른-회복)

<br>

<br>

## TCP 통신이란?

흐름제어와 혼잡제어에 대해 알아보기 앞서, TCP 통신를 보고 넘어가자.

TCP 통신이란 네트워크 통신에서 **신뢰적인 연결 방식**을 의미한다. TCP는 기본적으로 **reliable network**를 보장할 수 있도록 하는 프로토콜로, network congestion avoidance algorithm을 사용한다.

🔎 Unreliable vs Reliable

- Unreliable : 전송한 데이터그램이 유실될 수 있고, 순서가 바뀌어 도착할 수 있다.
- Reliable : 세그먼트가 유실될 경우 재전송을 통해 복구해주며, 순서가 바뀌어 도착하더라도 순서 번호를 이용하여 제대로 맞추어 전달한다.

<br>

#### Reliable Network의 4가지 문제점(조건)

1. 손실 : packet이 손실될 수 있는 문제
2. 순서 바뀜 : packet의 순서가 바뀌는 문제
3. Congestion : 네트워크가 혼잡한 문제
4. Overload : receiver가 overload 되는 문제

<br>

보통 TCP의 전송 제어 방법은 전송되는 데이터의 양을 조절하는 `흐름 제어`, 통신 도중에 데이터가 유실되거나 잘못된 데이터가 수신되었을 경우 대처하는 방법인 `오류 제어`, 네트워크 혼잡에 대처하는 `혼잡 제어`로 나누어진다.

<br>

<br>

## 흐름 제어

수신 측이 송신 측보다 데이터 처리 속도가 빠르면 문제가 없다. 주는 족족 빠르게 처리해주니 딱히 문제될 것이 없는 것이다. 그러나 수신 측의 처리 속도보다 송신 측이 더 빠른 경우 문제가 생긴다. 송신 측과 수신 측은 모두 데이터를 저장할 수 있는 버퍼를 가지고 있다. 이때 수신 측이 자신의 버퍼 안에 있는 데이터를 처리하는 속도보다 **송신 측이 데이터를 전송하는 속도가 더 빠르다면, 당연히 수신 측의 버퍼는 언젠가 꽉 차버릴 것이기 때문**이다. 수신 측에서 제한된 저장 용량을 초과한 이후에 도착하는 패킷은 손실될 수 있으며, 만약 손실된다면 불필요한 추가 패킷 전송이 발생하게 된다.

<img src="https://evan-moon.github.io/static/ffb20ecdb68523bcd2bca1b47ccabf1c/c08c5/overflow.jpg" alt="overflow" style="zoom:33%;" />

따라서 송신 측은 수신 측의 데이터 처리 속도를 파악하고 자신이 얼마나 빠르게, 많은 데이터를 전송할 지 결정해야한다. 이것이 바로 TCP의 흐름 제어인 것이다. 정리하자면, 흐름 제어는 위와 같이 **송신 측과 수신 측의 TCP 버퍼 크기 차이로 인해 생기는 데이터 처리 속도 차이를 해결하기 위한 기법**이다. 

🔎 TCP 버퍼 : 송신 측은 버퍼에 TCP 세그먼트를 보관한 후 순차적으로 전송하고, 수신 측은 도착한 TCP 세그먼트를 애플리케이션이 읽을 때까지 버퍼에 보관한다. 

<br>

#### Stop and Wait

이름 그대로 **상대방에게 데이터를 보낸 후 잘 받았다는 응답이 올 때까지 기다리는 모든 방식**을 통칭하는 말이다. 매번 전송한 패킷에 대해 확인 응답(ACK)을 받은 후에 다음 패킷을 전송한다. 

Stop and Wait로 흐름 제어를 할 경우의 대원칙은 단순히 `상대방이 응답을 하면 데이터를 보낸다`이기 때문에 구현 자체도 간단하고 개발자가 어플리케이션의 작동 원리를 파악하기도 쉬운 편이다. 

<br>

<img src="https://evan-moon.github.io/static/65ff8e861f0894835574fb210cb11888/6af66/stop-and-wait.png" width=300/>



그러나 패킷을 하나씩 보내기 때문에 비효율적인 방법이다. 따라서 Stop and Wait 방식을 사용하여 흐름 제어를 할 경우에는, 이러한 비효율성을 커버하기 위해 이런 단순한 구현이 아닌 여러가지 오류 제어 방식을 함께 도입해서 사용한다.

<br>

#### Sliding Window

앞서 살펴봤듯이 Stop and Wait를 사용하여 흐름 제어를 하게되면 비효율적인 부분이 있기 때문에, 오늘날의 TCP는 특별한 경우가 아닌 이상 대부분 슬라이딩 윈도우(Sliding Window) 방식을 사용한다.

슬라이딩 윈도우는 **수신 측이 한 번에 처리할 수 있는 데이터를 정해놓고 그때 그때 수신 측의 데이터 처리 상황을 송신 측에 알려줘서 데이터의 흐름을 제어하는 방식**이다. 수신 측에서 설정한 윈도우 크기 만큼 송신 측에서 확인 응답(ACK) 없이 패킷을 전송할 수 있게하여 데이터 흐름을 동적으로 조절한다.

Stop and Wait과 가장 큰 차이점은 송신 측이 **수신 측이 처리할 수 있는 데이터의 양을 알고 있다**는 점이다. 이 정보를 알고 있기 때문에 굳이 수신 측이 처리 가능(ACK)이라는 대답을 해주지 않아도 데이터를 보내기 전에 이게 처리될 지 어떨지 어느 정도 예측이 가능하다는 말이다. 

<br>

**윈도우(Window)**

송신 측과 수신 측은 각각 데이터를 담을 수 있는 버퍼를 가지고 있고, 별도로 **윈도우**라는 일종의 마스킹 도구를 가지고 있다. 이때 송신 측은 이 윈도우에 들어있는 데이터를 수신 측의 응답이 없어도 연속적으로 보낼 수 있다.

![window](https://evan-moon.github.io/static/1b3989f922769714854cd9b45b5bd9ca/6af66/window.png)

최초의 **윈도우 크기는 호스트들의 3-way handshake를 통해 수신 측 윈도우 크기로 설정**되며, 이후 수신 측의 버퍼에 남아있는 공간에 따라 변한다. 윈도우 크기는 수신 측에서 송신 측으로 확인 응답(ACK)을 보낼 때 TCP 헤더(window size)에 담아서 보낸다. 이때 송신 측과 수신 측은 자신의 현재 버퍼 크기를 서로에게 알려주게 되고, 송신 측은 수신 측이 보내준 버퍼 크기를 사용하여 자신의 윈도우 크기를 정하게 된다. 즉, 윈도우는 메모리 버퍼의 일정 영역이라고 생각하면 된다.

좀 더 자세하게 살펴보면,

<img src="https://blog.kakaocdn.net/dn/bLnwnr/btrORYlYivv/z1B0ZTnwXkJq65nhKgJCk0/img.png" width=300/>

처음의 `SYN`과 `SYN+ACK` 패킷에는 각자 자신의 버퍼를 알려준 후 마지막 `ACK` 패킷 때 송신 측이 자신이 정한 윈도우 사이즈를 상대방에게 통보한다.

```shell
localhost.initiator > localhost.receiver: Flags [S], seq 1487079775, win 63323
localhost.receiver > localhost.initiator: Flags [S.], seq 3886578796, ack 1487079776, win 63323
localhost.initiator > localhost.receiver: Flags [.], ack 1, win 6156
```

이때 송신 측과 수신 측 모두 자신의 버퍼 크기를 `63323`이라고 얘기했지만, 최종적으로 송신 측이 정한 자신의 윈도우 크기는 `6379`이다. 왜 송신 측은 수신 측 버퍼 크기의 10분의 1로 자신의 윈도우 크기를 정한 것일까?

송신 측의 윈도우 크기는 수신 측의 버퍼 크기로만 결정되는 것이 아니라 다른 여러가지 요인들을 함께 고려하여 결정된다.

<br>

**동작 방식**

윈도우에 포함된 패킷을 계속 전송하고, 수신 측으로부터 확인 응답(ACK)이 오면, 윈도우를 옆으로 옮겨 다음 패킷들을 전송한다.

즉, **송신 TCP는 ACK을 받기 전까지 윈도우 크기 만큼의 세그먼트를 연속해서 전송**할 수 있다.

![[네트워크] TCP/IP 흐름 제어 & 혼잡 제어 - 흐름 제어 - Sliding Window - 동작 방식](https://blog.kakaocdn.net/dn/cTbios/btrloyXkTF6/NCXRvhUJvAzXkz7xOiOLQ0/img.png)

- 최초로 수신자는 윈도우 사이즈를 7로 정한다.
- 송신자는 수신자의 확인 응답(ACK)을 받기 전까지 데이터를 보낸다.
- 수신자는 확인 응답(ACK)을 송신자에게 보내면, 슬라이딩 윈도우 사이즈을 충족할 수 있게끔 윈도우를 옆으로 옮긴다
- 이후 데이터를 다 받을 때까지 위 과정을 반복한다.

<br>

송신 측이 `0~6`번의 시퀀스 번호를 가진 데이터를 상대방에게 전송하고 싶다고 해보자.

이때 송신 측의 버퍼에는 전송해야할 데이터들이 다음과 같이 담겨져있을 것이다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221030044912157.png" width=400/>

송신 측은 수신 측에게 받은 윈도우 크기와 현재 네트워크 상황을 고려하여 윈도우 크기를 3으로 결정하였고, 윈도우 안에 있는 데이터들을 우선 주르륵 전송한다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221030035347515.png" width=400/>



이때 윈도우 안에 들어있는 데이터는 어떤 상태일까? 일단 데이터를 전송하기는 했으나, 아직 수신 측으로부터 잘 받았다는 응답은 받지 못한 상태일 것이다. 윈도우에 들어있는 데이터들은 항상 **전송은 했지만, 상대방이 처리했는 지는 모르는 상태**이다. 

이후 수신 측은 자신의 처리 속도에 맞춰 **데이터를 처리한 후 응답으로 현재 자신의 버퍼에 남아있는 공간의 크기를 알려준다.** 만약 수신 측이 응답으로 `Window Size : 1` 을 보냈다면, "내 버퍼 공간이 1 byte 만큼 남았으니까 그 만큼만 더 보내봐" 라는 의미가 된다.

이제 송신 측은 자신이 데이터 한 개를 더 보낼 수 있다는 사실을 알았고, 자신의 윈도우를 한 칸 옆으로 밀고 새롭게 윈도우에 들어온 3번 데이터를 수신 측에게 전송한다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221030044640236.png" width=400/>

윈도우를 옆으로 이동시키며 새로 들어온 데이터를 전송하기 때문에 **슬라이딩 윈도우**라고 하는 것이며, 만약 수신 측이 윈도우 크기를 1이  아니라 더 큰 수를 보냈다면 송신 측은 그 만큼 윈도우를 옆으로 밀고 더 많은 데이터를 연속적으로 전송할 수 있을 것이다.

이렇게 데이터를 전송하는 송신 측의 버퍼는 대략 3가지 상태로 나눠질 수 있다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221030044708988.png" width=500/>

정리하자면, 슬라이딩 윈도우 방식은 **보내고 -> 응답 받고 -> 윈도우 밀고**를 반복하면서, 현재 자신이 보낼 수 있는 데이터를 최대한 연속적으로 보내는 방법이라고 할 수 있다. 

**tcp sliding window animation**

출처 : https://www.youtube.com/watch?v=xOSzPSJ6qVc

![tcp sliding window animation gif](https://raw.githubusercontent.com/na3150/typora-img/main/img/tcp%20sliding%20window%20animation%20gif.gif)

<br>

**재전송**

송신 측은 일정 시간 동안 수신 측으로부터 확인 응답(ACK)을 받지 못하면(time out), 패킷을 재전송한다. 만약 송신 측에서 재전송을 했는데 패킷이 소실된 경우가 아니라 수신 측의 버퍼에 남는 공간이 없는 경우이면 문제가 생긴다. 이를 해결하기 위해 송신 측은 해결 응답(ACK)을 보내면서 남은 버퍼의 크기(윈도우 크기)를 함께 보내는 것이다.

<img src="https://raw.githubusercontent.com/na3150/typora-img/main/img/image-20221030051021693.png" width=400/>

<br>

<br>

## 혼잡 제어

데이터의 양이 라우터가 처리할 수 있는 양을 초과하면, 초과된 데이터는 라우터가 처리하지 못한다. 이때 송신 측에서는 라우터가 처리하지 못한 데이터를 손실 데이터로 간주하고 계속 재전송하여 네트워크를 혼잡하게 한다. 이와 같은 **네트워크의 혼잡을 피하기 위하여 송신 측에서 보내는 데이터의 전송 속도를 강제로 줄이게 되는데**, 이러한 작업을 혼잡 제어(Congestion Control)이라고 한다.

정리하자면, 흐름 제어는 송 수신 측 사이의 패킷 수를 제어하는 기능이라 할 수 있으며, 혼잡 제어는 네트워크 내의 패킷 수를 조절하여 네트워크의 오버플로우를 방지하는 기능이다.

- 네트워크 내에 패킷의 수가 과도하게 증가하는 현상을 혼잡이라 하며, 혼잡 현상을 방지하거나 제거하는 기능을 혼잡 제어라고한다.
- 흐름제어가 송신 측과 수신 측 사이의 전송 속도를 다루는 데 반해, 혼잡 제어는 호스트와 라우터를 포함한 보다 넓은 관점에서 전송 문제를 다루게 된다.

<br>

![img](https://blog.kakaocdn.net/dn/kGivV/btrakSMcXlW/VyWirqwWBvkEw1nDkUyoyK/img.png)

#### AIMD (Additive Increase/Multiplicative Decrease)

한글로 직역하면 합 증가/곱 감소 방식이다. **AIMD 방식은 처음에 패킷을 하나씩 보내고, 문제 없이 도착하면 윈도우의 크기를 1씩 증가시켜가며 전송한다. 만약 전송에 실패하거나 일정 시간을 넘기면 윈도우의 크기를 반으로 줄인다.**

공평한 방식으로, 여러 호스트가 한 네트워크를 공유하고 있으면 나중에 진입하는 쪽이 처음에는 불리하지만, 시간이 흐르면 평형상태로 수렴하게 되는 특징이 있다. 다만, 윈도우의 크기를 굉장히 조금씩 늘리기 때문에 네트워크의 모든 대역을 활용하여 제대로 된 속도로 통신하기까지 시간이 오래 걸린다는 단점이 있다.

<img src="https://blog.kakaocdn.net/dn/brHwax/btrlqLVUHpr/7v1nCsodFMee5av1Yuzmxk/img.png" width=450/>



<br>

#### Slow Start(느린 시작)

앞서 얘기했듯이 AIMD 방식은 윈도우 크기를 선형적으로 증가시키기 때문에 제대로 된 속도가 나오기까지 시간이 오래 걸린다. 

반면 Slow Start는 윈도우의 크기를 1, 2, 4, 8, ...과 같이 지수적으로 증가시키다가 혼잡이 감지되면 윈도우의 크기를 1로 줄이는 방식이다.

이 방식은 보낸 데이터의 ACK가 도착할 때마다 윈도우의 크기를 증가시키기 때문에 처음에는 윈도우 크기가 조금 느리게 증가할지라도, 시간이 가면 갈수록 윈도우의 크기가 점점 빠르게 증가한다는 장점이 있다.

![[네트워크] TCP/IP 흐름 제어 & 혼잡 제어 - 혼잡 제어 - 혼잡 제어 기법 - Slow Start (느린 시작)](https://blog.kakaocdn.net/dn/HOqHV/btrlozPpRSA/DkpDCvOo0Sv3egRkeVW3W1/img.png)



<br>

#### Fast Retransmit(빠른 재전송)

패킷을 받는 수신자 입장에서는 세그먼트로 분할된 내용들이 **순서대로 도착하지 않는 경우**가 생길 수 있다. 이러한 상황이 발생했을 때 **수신 측에서는 순서대로 잘 도착한 마지막 패킷의 다음 순번을 ACK 패킷에 실어서 보낸다.** 그리고 이런 **중복 ACK를 3개 받으면 재전송이 이루어진다.** 송신 측은 자신이 설정한 타임 아웃 시간이 지나지 않았어도 바로 해당 패킷을 재전송할 수 있기 때문에 보다 빠른 재전송률을 유지할 수 있다.

![img](https://www.gatevidyalay.com/wp-content/uploads/2018/09/Retransmission-after-receiving-3-duplicate-acknowledgements-TCP-Retransmission.png)

🔎 예시 

•세그먼트 1을 수신하면 수신자는 다음에 세그먼트 2를 요청하는 확인을 보낸다.

(원래 ACK)

• 세그먼트 3을 수신하면 수신자는 다음에 세그먼트 2를 요청하는 확인을 보낸다.

(첫 번째 중복 ACK)

• 세그먼트 4를 수신하면 수신자는 다음에 세그먼트 2를 요청하는 확인을 보낸다.

(두 번째 중복 ACK)

• 세그먼트 5를 수신하면 수신자는 다음에 세그먼트 2를 요청하는 확인을 보낸다.

(세 번째 중복 ACK)

![fast retransmit animation gif](https://raw.githubusercontent.com/na3150/typora-img/main/img/fast%20retransmit%20animation%20gif.gif)

<br>

#### Fast Recovery(빠른 회복)

빠른 회복은 혼잡한 상태가 되면 윈도우 크기를 1로 줄이지 않고, 반으로 줄이고 선형 증가시키는 방법이다. 이 방법을 적용하면 혼잡 상황을 한 번 겪고나서부터는 AIMD 방식으로 동작한다.

<br>

<br>

## 기술 질문/예상 질문

<details>
<summary>TCP/IP 통신에서 흐름 제어 기법이 왜 사용되는가?</summary>



<br>

송 수신자 간의 TCP 버퍼의 크기 차이로 인해 생기는 데이터 처리 속도 차이를 해결하기 위해 사용된다.

</details>

<br>

<details>
<summary>TCP/IP 흐름 제어 기법은 무엇이 있는가?</summary>



<br>

Stop and Wait과 Sliding Window 기법이 있다. Stop and Wait은 전송한 패킷에 대해 확인 응답(ACK)을 받으면 다음 패킷을 전송하는 제어 기법이고, Sliding Window는 수신 측에서 설정한 윈도우 크기만큼 송신 측에서 확인 응답(ACK) 없이 패킷을 전송할 수 있게 하여 데이터 흐름을 동적으로 조절하는 제어 기법이다.

</details>

<br>

<details>
<summary>TCP/IP 혼잡 제어 기법은 왜 사용되는가?</summary>



<br>

송신 측에서 보내는 데이터의 양이 라우터가 처리할 수 있는 양을 초과하면 초과된 데이터는 라우터가 처리하지 못한다. 송신 측은 초과된 데이터를 손실 데이터로 간주하고 계속 재전송하여 네트워크를 혼잡하게 한다. 이런 상황을 예방하기 위해 송신 측의 전송 속도를 적절히 조절하는 혼잡 제어 기법이 사용된다. 대표적으로 AIMD, Slow Start, 빠른 재전송, 빠른 회복 등이 있다.

</details>

<br>

<br>

<br>

<br>

<br>

<br>

<br>









Reference

- https://gyoogle.dev/blog/computer-science/network/%ED%9D%90%EB%A6%84%EC%A0%9C%EC%96%B4%20&%20%ED%98%BC%EC%9E%A1%EC%A0%9C%EC%96%B4.html
- https://steady-coding.tistory.com/507

- https://evan-moon.github.io/2019/11/22/tcp-flow-control-error-control/

- https://korshika.tistory.com/185





