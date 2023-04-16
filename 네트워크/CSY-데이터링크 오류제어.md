# 데이터링크-오류제어

오류 검출 과정과 재전송 과정을 포함

데이터 전송 시 오류가 발생하면 NAK를 반환하고 오류가 발생한 프레임은 전송지에서 재전송되도록 한다. ⇒ **ARQ**(Automatic Repeat Request)

데이터링크 계층에서 수행되는 ARQ는 흐름제어를 위한 방식과 관련된다.

- 정지-대기 방식의 흐름제어
    
    ⇒ 정지-대기 ARQ 방식으로 구현
    
- 슬라이딩 윈도우 방식의 흐름제어
    
    ⇒ GBn ARQ 방식 (Go-Back-n)
    
    ⇒ SR ARQ 방식 (Selective-Repeat | Selective-Reject)
    

## 정지-대기 ARQ

![https://blog.kakaocdn.net/dn/rSK5R/btq6Qug3mn3/aKQ3kMYn6wlE2oPaGy8wDk/img.png](https://blog.kakaocdn.net/dn/rSK5R/btq6Qug3mn3/aKQ3kMYn6wlE2oPaGy8wDk/img.png)

1. 전송지는 전송한 프레임에 대한 ACK 프레임을 받을 때까지 프레임의 복사본을 유지한다.
2. 식별을 위해 데이터 프레임과 ACK 프레임은 각각 0, 1 의 값으로 번호를 부여한다.
    
    *보내고 확인받고 보내고 확인 받고 하기 때문에 메세지가 길 필요가 없다. 바로 앞 프레임과 구분되면 된다. 그래서 0,1로 만 구분한다.
    
3. 프레임에서 오류가 발견되면 NAK 프레임이 반환되고, 전송지는 복사해두었던 프레임을 전송한다.
    1. 전송장치에는 타이머가 있어서 주어진 시간 내에 ACK이 오지 않으면 재전송을 수행한다.

![분실된 데이터 프레임의 처리 절차](https://blog.kakaocdn.net/dn/bZkOf8/btq6PDMgi2s/Ku3C6tm1WMBbR28FPXIRQk/img.png)

분실된 데이터 프레임의 처리 절차

![분실된 ACK 프레임의 처리 절차](https://blog.kakaocdn.net/dn/baoKYP/btq6PaJWetz/2uzGTJLYT0TzkyVc4iNlcK/img.png)

분실된 ACK 프레임의 처리 절차

## GBn ARQ 방식

슬라이딩 윈도우 방식에 기초하고 있으며 재전송을 위한 절차는 정지-대기 ARQ 방식의 절차에서 수정된 내용을 담고 있다.

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/t8eMg/btq6Ub8pTau/4ttUA4uAZBfJdPa1yU4fl1/img.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/t8eMg/btq6Ub8pTau/4ttUA4uAZBfJdPa1yU4fl1/img.png)

1. 전송지는 전송된 모든 프레임의 복사본을 가지고 있다.
2. 슬라이딩 윈도우는 연속적인 프레임 전송 방식이므로 한번에 여러 프레임을 보낸다.
3. 이때 ACK, NAK 프레임이 모두 각각 구별되어야 한다. 
    1. ACK은 다음 프레임을 보내라는 의미이다.
    2. NAK은 손상된 프레임 그 자체에 대한 번호를 가지고 반환된다.
    3. 전송지는 분실된 ACK 프레임을 다루기 위한 타이머를 갖추고 있다.

![GBn-ARQ 분실된 데이터 프레임의 처리 절차](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/YG6rD/btq6QPejooi/2TpjYU1uQNaOaI0MkZzu10/img.png)

GBn-ARQ 분실된 데이터 프레임의 처리 절차

![GBn-ARQ 분실된 ACK 프레임의 처리절차](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https://blog.kakaocdn.net/dn/yag3P/btq6Swyd0bj/DIkcTBxu74p5jJ9D2oNUg1/img.png)

GBn-ARQ 분실된 ACK 프레임의 처리절차

## SR ARQ 방식

손상되거나 잃어버린 프레임만 다시 보내는 방식.

수신지는 어긋난 순서로 도착한 프레임을 다시 정렬하고, 재전송해야하는 프레임을 선택해 전송지는 해당 프레임을 재전송한다.

수신지는 재전송 프레임이 도착해서 정렬될 때까지 프레임들을 저장할 버퍼가 필요하다.

## 예상 질문
<details>
<summary>데이터링크 계층의 오류제어방식의 특성을 비교하여 설명해주세요.</summary>
  <div markdown="1">
    정지-대기ARQ방식은 한 번에 하나의 데이터프레임에 대해서 오류를 처리하므로 구조가 간단하여 구현하기 용이하지만 비효율적이어서 활용도가 낮습니다.
    
    GBn ARQ방식은 슬라이딩 윈도우 흐름 제어에서 사용하여 구조가 간단하고 효율성이 향상되어 가장 널리 사용됩니다.
    
    SR ARQ 방식은 가장 효율적이지만 버퍼를 사용하는 등 구조가 복잡하여 유지 관리 비용이 높아진다는 단점이 있습니다.
  </div>
</details>


<details>
<summary> 다음 예를 가정하여 GBn ARQ 방식의 구체적인 동작을 설명해주세요.<br />
  | 전송지에서 수신지로 7개의 프레임을 연속해서 전송하고 있다고 가정한다.<br />
  | 수신지에서 프레임 4의 오류가 발견되어 수신지는 전송지로 ACK 4를 반환했다.<br />
  </summary>
  <div markdown="1">
   수신지에서 프레임 4의 오류가 발견되었기 때문에 ACK 4이후에 곧바로 NAK 4를 전송하고 뒤의 5, 6, 7 프레임은 폐기됩니다.
   이때 전송지에서는 NAK 4를 수신함으로써 프레임 4가 잘못되었음을 알고 프레임 4, 5, 6, 7을 재전송합니다.
  </div>
</details>


## 참고자료

- [https://wonin.tistory.com/88?category=966930](https://wonin.tistory.com/88?category=966930)
