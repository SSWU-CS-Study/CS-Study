운영체제는 주기억장치보다 더 큰 용량의 프로그램을 실행하기 위해 프로그램의 일부만 주기억장치에 적재하여 사용한다. 이를 가상메모리 기법이라 한다.

페이징 기법으로 메모리를 관리하는 운영체제에서 필요한 페이지가 주기억장치에 적재되지 않았을 시(페이지 부재) 어떤 페이지 프레임을 선택하여 교체할 것인지 결정하는 방법을 페이지 교체 알고리즘이라고 한다.

*프레임 : 물리 메모리를 일정한 크기로 나눈 블록

*페이지 : 가상 메모리를 일정한 크기로 나눈 블록

## 요구 페이징

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd6jRIR%2FbtrmYbF1bv9%2FLjPKWCgIPQef2Dz7ZwN9G0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fd6jRIR%2FbtrmYbF1bv9%2FLjPKWCgIPQef2Dz7ZwN9G0%2Fimg.png)

- 프로세스의 주소 공간을 메모리로 적재하는 기법
- 프로그램 실행 시 프로세스를 구성하는 모든 페이지를 한꺼번에 메모리에 올리는 것이 아니라, 당장 사용될 페이지만 올리는 방식
    - 특정 페이지에 대해 CPU의 요청이 들어온 후에 해당 페이지를 메모리에 적재
- 특정 프로세스를 구성하는 페이지 중, 어떤 페이지가 메모리에 존재하고 어떤 페이지가 메모리에 존재하지 않는 지를 구별해야 함
    - 요구 페이징에서는 valid-invalid(유효-무효 비트)를 사용하여 각 페이지가 메모리에 존재하는지 표시
- 프로세스가 실행되기 전에는 모든 페이지의 유효-무효 비트가 무효 값으로 초기화되어 있지만, 특정 페이지가 참조되어 메모리에 적재되는 경우 해당 페이지의 유효-무효 비트는 유효 값으로 바뀜
    - 그리고 해당 페이지가 디스크의 스왑 영역으로 쫓겨날 때는 다시 무효 값으로 바뀜

### 페이지 부재(Page Fault)

- CPU가 접근하려는 페이지가 메모리에 없는 상황
- 즉, 페이지 테이블의 유효-무효 비트가 0인 상태
- 페이지 부재 발생 시 페이지를 디스크에서 읽어봐야 하는데 이 과정에서 막대한 오버헤드가 발생
    - 따라서 요구 페이징 기법은 페이지 부재 발생률이 성능에 큰 영향
- 동작 과정
    - 찾으려는 페이지가 TLB(페이지 테이블의 캐시)에 없는 경우, 해당 페이지가 메모리에 있는지 페이지테이블에서 유효-무효 비트를 확인
    - 유효-무효 비트가 0이라면 MMU가 페이지 부재 트랩을 발생 (이때 CPU의 제어권이 커널 모드로 전환되고, 운영체제의 페이지 부재 처리 루틴이 호출되어 페이지 부재를 처리)
    - 운영체제는 해당 페이지에 대한 접근이 문제가 없는지 확인 (사용되지 않은 주소 영역에 속한 페이지를 접근하려 했거나, 해당 페이지가 접근 위반일 경우에는 해당 프로세스를 종료) (접근 위반의 예시 : 읽기 전용 페이지에 대해 쓰기 접근을 시도하려는 경우)
    - 해당 페이지에 대한 접근이 허용 가능하다면, 물리적 메모리에서 비어 있는 프레임을 할당 받아 그 공간에 페이지를 읽어옴 (만약 비어있는 프레임이 없다면 기존 메모리에 올라와 있는 페이지 중 하나를 디스크의 스왑 영역으로 쫓아냄)
        - 이처럼 이미 메모리에 있는 페이지 중 하나를 다시  backing store (swqp device라는 하드웨어의 일부분인데 디스크라고 생각해도 무방하며, 페이지를 임시로 보관하는 장소)에 보내는 것을 page-out, 새로운 페이지를 메모리에 올리는 것을 page-in이라고 한다.
        - 만약 비어 있는 프레임이 없다면 페이지 교체 알고리즘을 통해 물리적 메모리에 있는 프레임 하나를 스왑 영역으로 쫓아내 비어 있는 프레임을 만든 후 적재. 이때, 디스크 스왑 영역에 있던 페이지를 물리 메모리에 적재하기 위해서는 시간이 많이 걸리므로, 해당 프로세스는 CPU 제어권을 빼앗기고 현재까지 수행되면 CPU레지스터 상태 및 프로그램 카운터 값을 PCB에 저장
    - 페이지 테이블에서 해당 페이지의 유효-무효 비트를 유효 비트로 설정하고, 프로세스를 Ready-Queue로 이동
    - 다시 CPU를 할당 받았을 때, PCB에 있던 값을 복원하여 중단되었던 명령을 수행

### 페이지 교체

- 페이지 부재가 발생하면 요청된 페이지를 디스크에서 메모리로 읽어와야하는데, 물리적 메모리에 빈 프레임이 존재하지 않을 수 있음
- 이러한 경우, 메모리에 올라와 있는 페이지 중 하나를 디스크로 쫓아내 메모리에 빈 공간을 확보하여 새로운 페이지를 메모리에 올려야 함
- 이러한 과정을 페이지 교체라고 함

### 희생양 페이지(Victim Page)

- 페이지 교체 과정에서 page-out이 된 페이지
- 희생양 페이지는 보통 메모리에 올라가 있는 페이지 중 CPU에 수정되지 않는 페이지를 고르는 것이 효율적이다. 수정되지 않은 페이지는 page-out이 될 때 backing store에 쓰기 연산을 할 필요가 없기 때문
- 해당 페이지가 수정되었는지 판단하기 위해, 페이지 테이블에 modified bit(=ditry bit)를 추가하여 이를 검사
    - 해당 페이지가 수정되었다면 이 비트를 1로 두고, 수정되지 않으면 0
    - 이를 이용해서 희생양 페이지는 최대한 수정되지 않은 페이지를 선택

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbbel7I%2Fbtrm0pQO5Zg%2FjHDh1ODYyGohxYmExtlda0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbbel7I%2Fbtrm0pQO5Zg%2FjHDh1ODYyGohxYmExtlda0%2Fimg.png)

- 위 예시에서 수정되지 않은 페이지는 0, 2, 3번으로 총 3개의 페이지가 존쟇는데 이 중에서 하나의 페이지를 선택해서 교체

## 페이지 교체 알고리즘

페이지 교체를 할 때 어떠한 프레임에 있는 페이지를 쫓아낼 것인지 결정하는 알고리즘을 페이지 교체 알고리즘이라고 하며, 페이지 부재율을 최소화 하는 것이 페이지 교체 알고리즘의 목표

### FIFO

가장 먼저 들어온 페이지를 교체하는 알고리즘

### OPT

앞으로 가장 오랫동안 사용되지 않을 페이지 교체

### LRU

가장 오랫동안 사용하지 않은 페이지를 교체

### LFU

참조횟수가 가장 낮은 페이지를 교체

### MFU

참조 횟수가 가장 많은 페이지 교체

### FIFO

가장 오래된 페이지를 교체하는 알고리즘

- 특징
    - 가장 간단한 알고리즘
        - 이해가 쉽고 구현이 간단
    - 각 페이지가 올라온 시간을 기록하거나, 페이지가 올라온 순서를 큐에 저장하는 방식등을 사용
        - 큐의 head에 있는 페이지를 교체
        - 페이지가 메모리에 적재되면 큐의 꼬리에 삽입
    - 성능이 좋지 않음
- 방법
    - 페이지 7의 경우에는 프로세스 초기에 쓰인 후 한동안 쓰이지 않기 때문에 FIFO교체 방식이 큰 문제를 일으키지 않음
    - 페이지2(9번쨰)의 경우, 직전 페이지 부재로 인해 페이지 4와 페이지 2가 교체되고 난 후, 또 다시 페이지 2를 사용하기 위해 교체했던 페이지 2를 다시 불러들임
    
    ![https://miro.medium.com/max/828/1*PisBTTZmXb2ZLHix7RdBCQ.webp](https://miro.medium.com/max/828/1*PisBTTZmXb2ZLHix7RdBCQ.webp)
    
- 문제점
    - Belady’s Anomaly
        - 보통 프레임의 수가 많아질수록 페이지 결함의 횟수는 감소할 것이라 생각할 수 있지만, 물리적 공간이 늘어났음에도 성능이 더 나빠지는 경우도 발생하는 이상 현상
        
        [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbeNEXx%2FbtrmYEVuQWr%2FkLKo0DKBdZEpeKyRwPqki0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbeNEXx%2FbtrmYEVuQWr%2FkLKo0DKBdZEpeKyRwPqki0%2Fimg.png)
        

### OPT

앞으로 가장 오랫동안 사용되지 않을 페이지를 교체하는 알고리즘

- 특징
    - 빌레디의 최적 알고리즘
    - 모든 페이지 교체 알고리즘을 통틀어 가장 페이지 교체 수가 적다
    - 프로세스가 앞으로 사용할 페이지를 미리 알아야 한다는 선제조건이 필요
        - 미래에 어떤 페이지가 선택될 지 알 수 없음
    - 실제 구현 목적보다 다른 알고리즘과 비교 연구 목적을 위해 주로 사용
- 방법
    - 페이지 7의 경우, 18번째에 가서나 다시 쓰일 것을 미리 알고 있기 때문에 페이지 2와 페이지 7을 교체
    - 페이지 1의 경우, 현재 올라와있는 {2, 0, 1} 중 페이지 1이 14번째 참조로 가장 뒤에 쓰일 것을 알기 때문에 페이지 1과 페이지 3을 교체
    
    ![https://miro.medium.com/max/828/1*MHoq4CVbRsKyXwycQanhnA.webp](https://miro.medium.com/max/828/1*MHoq4CVbRsKyXwycQanhnA.webp)
    
- 문제점
    - 구현 불가

### LRU

가장 오랫동안 사용하지 않은 페이지를 교체

- 특징
    - OPT알고리즘은 실제 구현이 불가능하므로, 최적 알고리즘의 방식과 비슷한 효과를 낼 수 있는 방법을 사용한 알고리즘
        - 과거의 데이터를 바탕으로 페이지가 사용될 시간을 예측하여 가장 오랜 기간 사용되지 않은 페이지를 교체하는 방식
    - FIFO 알고리즘보다 좋은 성능을 보여줌
        - Belady’s Anomaly 없음
    - 많은 운영체제가 채택하는 알고리즘이며, 좋은 알고리즘이라고 평가 받음
    - ~~성능을 늘리기 위해서 각 페이지가 언제 사용되는지에 관한 정보를 위한 HW 지원이 필요~~
- 방법
    - 페이지 2(9번째)의 경우, 직전의 페이지 부재에서 페이지 2가 바로 다음에 사용될 것을 모르기 때문에 {2, 0, 3} 중 가장 오랫동안 사용되지 않았던 페이지 2를 교체
    - 페이지 0의 경우(11번째), 가장 오랫동안 사용되지 않았던 페이지4를 페이지 0과 교체, 실제로 페이지 4는 이후에 해당 프로세스에서 참조되지 않음
    
    ![https://miro.medium.com/max/828/1*2KmdY3wX68yaZnF6MwwTqg.webp](https://miro.medium.com/max/828/1*2KmdY3wX68yaZnF6MwwTqg.webp)
    

### LFU

참조횟수가 가장 낮은 페이지를 교체하는 알고리즘

- 특징
    - 만약 교체 대상인 페이지가 여러 개인 경우, LRU 알고리즘을 따라 가장 오래 사용되지 않은 페이지로 교체
    
    ![https://miro.medium.com/max/828/1*nIY4ek2yu3jsND7na4AF-Q.webp](https://miro.medium.com/max/828/1*nIY4ek2yu3jsND7na4AF-Q.webp)
    
- 방법
- 문제점
    - 초기에 한 페이지를 집중적으로 참조하다가 이후 다시 참조하지 않는 경우 문제 발생
    - 앞으로 사용하지 않아도 초기에 사용된 참조횟수가 높아 메모리에 계속 남아있기 때문
    
    ![https://miro.medium.com/max/828/1*mBZHbLoadaZEfTMkbhcJQQ.webp](https://miro.medium.com/max/828/1*mBZHbLoadaZEfTMkbhcJQQ.webp)
    
    - 구현에 상당한 비용
    - 최적 페이지 교체 정책을 LRU만큼 제대로 유사하게 구현하지 못함

### MFU

참조 횟수가 가장 많은 페이지를 교체하는 알고리즘

- 특징
    - 가장 많이 사용된 페이지가 앞으로는 사용되지 않을 것이다라는 가정 전제
- 방법
    
    ![https://miro.medium.com/max/828/1*e5lca52SoQeiDmy4CvykIA.webp](https://miro.medium.com/max/828/1*e5lca52SoQeiDmy4CvykIA.webp)
    
- 문제점
    - 구현에 상당한 비용
    - 최적 페이지 교체 정책을 LRU만큼 제대로 유사하게 구현하지 못함

### NUR=NRU (Not used recently, not recently used)

오랫동안 참조하지 않은 페이지를 교체하는 알고리즘

- 특징
    - LRU 근사 알고리즘
    - 2차 기회 알고리즘
        - 시계바늘이 한 바퀴 도는 동안 걸리는 시간만큼 페이지를 메모리에 유지시켜 페이지 부재율을 줄이도록 설계됨
    - 오랫동안 참조하지 않은 페이지 중 하나를 선택하지만 가장 오래된 페이지라는 보장 없음
- 방법
    - 각 페이지마다 참조 비트와 변형 비트
        - 참조 비트 : 페이지가 참조되지 않았을 때 0, 호출되었을 때 1(모든 참조 비트는 주기적으로 0으로 변경)
        - 변형 비트 : 페이지 내용이 변경되지 않았을 때는 0, 변경되었을 때 1
    - 어떤 고정된 시간 간격이 있어서 그 시간 간격이 지나면 주기적으로 모든 페이지의 참조 비트를 초기화
    - 페이지를 교체하려 할 때에는 다음과 같이 4가지의 클래스로 나누고, 가장 낮은 클래스의 랜덤한 페이지를 선택하여 교체
        - Class0 : 참조되지 않음. 수정되지 않음
        - Class1 : 참조되지 않음. 수정됨
        - Class2 : 참조됨. 수정되지 않음
        - Class3 : 참조됨. 수정됨
        
        [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0vmFA%2FbtrmYdjwJ26%2FFELwkyLAX64kzH54Ns6FTK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0vmFA%2FbtrmYdjwJ26%2FFELwkyLAX64kzH54Ns6FTK%2Fimg.png)
        
        [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FC9OaF%2FbtrmY5SOBut%2FmSCrnGLVbPnKdxG1eeUuW1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FC9OaF%2FbtrmY5SOBut%2FmSCrnGLVbPnKdxG1eeUuW1%2Fimg.png)
        

### SCR

- 가장 오랫동안 주기억장치에 있던 페이지 중 자주 사용되는 페이지의 교체를 방지하기 위한 것으로, FIFO 기법의 단점을 보완하는 기법이다.
- 큐의 상단에서 꺼낸 대상의 참조 비트를 검사하여 0이면 교체 대상으로 선택하고, 1이면 0으로 바꿔 큐의 뒤에 삽입한다.
- 만약 모든 페이지의 참조 비트가 세팅되어 있다면, 큐의 첫 번째 요소였던 페이지가 두 번 검사될 것이고, 해당 페이지를 내쫓는다.

### 클럭 알고리즘

페이지가 참조되어 1이 되고, 한 바퀴 도는 동안 사용되지 않으면 0이 되고 다시 한 바퀴를 도는 동안 사용되지 않는 페이지는 참조되지 않았으므로 교체 대상 페이지로 선정하는 알고리즘

- SCR을 원형 큐를 이용하여 구현한 것이다.
- LRU 알고리즘, LFU 알고리즘과는 달리 하드웨어적인 자원을 통해 동작한다. (해당 알고리즘은 참조 시각 및 참조 횟수를 소프트웨어적으로 유지 및 비교해야 한다.)
- 페이지 프레임의 참조 비트를 조사해서 참조 비트가 1인 페이지는 0으로 바꾼 후 지나가고, 0인 페이지를 찾으면 그 페이지와 교체한다.

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFLfwl%2FbtrmGg7P1GN%2FRhZ8a263lfv7OxMiWytbw1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbFLfwl%2FbtrmGg7P1GN%2FRhZ8a263lfv7OxMiWytbw1%2Fimg.png)

클럭 알고리즘은 그림처럼 참조 비트(reference bit)를 순차적으로 조사하며 동작

1. 프레임 내의 페이지가 참조될 때 하드웨어에 의해 1로 자동 세팅
2. 클럭 알고리즘은 한 바퀴 돌며 참조되지 않은 페이지의 참조 비트 값을 0으로 바꾼 후 지나감
3. 참조 비트가 0인 페이지를 방문하면 해당 페이지를 교체

## QUIZ

- 페이지 교체란?
    
    페이지 폴트 발생 시, 새로 진입할 페이지를 위한 공간을 만들기 위해 이미 존재하고 있는 페이지 중 하나를 내보내는 것.
    
- 페이지 교체 알고리즘 종류에는 어떤 것들이 있나요?
    
    OPT : 최적 교체. 앞으로 가장 오랫동안 사용하지 않을 페이지 교체 (실현 가능성 희박)
    
    FIFO : 메모리가 할당된 순서대로 페이지를 교체
    
    LRU : 최근에 가장 오랫동안 사용하지 않은 페이지를 교체
    
    LFU : 사용 빈도가 가장 적은 페이지를 교체
    
    NUR : 최근에 사용하지 않은 페이지를 교체
    
- 기본적인 페이지 교체 알고리즘의 과정은?
    1. 먼저 보조저장장치에서 페이지의 위치를 찾는다.
    2. 빈 페이지 프레임을 찾는다.
        - 비어있다면 그걸 사용
        - 비어있지 않다면 희생 프레임을 선정하기 위해 페이지 교체 알고리즘을 수행
        - 필요한 경우 내보낼 페이지를 보조저장장치에 기록, 관련 테이블을 수정
    3. 새로 비워진 프레임에 새 페이지를 읽어오고 테이블을 수정
    4. 페이지 폴트가 발생한 시점에서 프로세스를 재개
- LRU 페이지 교체 알고리즘에서 직접 구현하려면 어떤 자료구조를 써야할까?
    
    **직접 구현**하려면 LinkedHashMap을 쓰면 된다.
    
    LRU 구현 자체는 간단하게 하려면 PriorityQueue를 사용하면 되는데, 정렬하는데 소요되는 log n 조차 O(1)로 하기 위해서 DoubleLinkedList와 HashMap을 사용한다.
    

## RE

[[운영체제] 페이징 교체 알고리즘](https://steady-coding.tistory.com/526)

[[면접 예상 질문] 운영체제(2/3)](https://ddb8036631.github.io/question/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-2/#16-%ED%8E%98%EC%9D%B4%EC%A7%80-%EA%B5%90%EC%B2%B4%EB%9E%80)
