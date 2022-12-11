# 2. 시스템 구조와 프로그램 실행 

<aside>

📌 **`면접 예상 질문`**

- **넓은 범위의 인터럽트란 ?**
    -  하드웨어가 발생시킨 인터럽트와 소프트웨어 인터럽트(Trap)를 포함

- **System Call 이란 ?**
    - 시스템콜은 커널과 사용자 사이의 인터페이스 역할을 하는 것으로 쉘(Shell)에서 명령어나 서브루틴 형식으로 운영체제의 기능을 호출할 수 있게 하는 것. 
    사용자가 직접 커널에 접근할 수 없기 때문에 시스템콜을 활용해야한다.
- ****DMA Controller 란 ?****
    - 메모리에 직접적으로 접근할 수 있는 컨트롤러.  즉 I/O 장치의 빈번한 인터럽트를 줄여주기 위한 장치
    - I/O 장치의 저장된 데이터를 메모리에 직접 적재를 해주고, 모와서 CPU에 알려주는 역할
- **Timer의 역할은 무엇인가?**
    - CPU가 특정프로그램이 독점하는 것으로부터 보호 하며 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트 발생
- ****Mode bit이란 ?****
    - 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치
    - `0` 은 모니터 모드(커널모드, 시스템 모드) 이며  `1` 은 사용자 모드 이다.
- Mode bit 의 작동 방식
    - interrupt나 Exception 발생시 하드웨어가 mode bit을 0으로 바꿈
    - 사용자 프로그램에게 CPU를 넘기기전 mode bit을 1로 세팅
</aside>

# 컴퓨터 시스템 구조

<img src = "https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F48fc65c6-fda1-4c18-9138-68105d467d27%2FUntitled.png&blockId=344ce9fb-7d3d-4923-a6b6-83c0c275f29a](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F48fc65c6-fda1-4c18-9138-68105d467d27%2FUntitled.png&blockId=344ce9fb-7d3d-4923-a6b6-83c0c275f29a">

- `CPU`: 매 순간마다( clock) 메모리에서 instruction(명령어) 기계어를 하나씩 읽어서 명령하게 된다.
- `I/O` device : 입출력 디바이스
- `Memory`: CPU의 작업공간
- `divice controller`: I/O device에 붙어있는 그 디바이스를 전담하는 작은 CPU
    - disk에서 헤드를 움직이고 어떤 역할을 부여할지 결정
    - local buffer : divice controller의 작업 공간
- `registers`: CPU 안 메모리보다 더 빠르면서 정보를 저장하는 작은 공간
- `mode bit`: 지금 CPU에서 실행되는 것이 운영체제인지 아니면 사용자 프로그램인지 구분
- `interrupt line` :  cpu의 실행 중인 명령을 멈추는 입력신호가 있는지 확인
    - i/o에서 오는 신호가 왔을때
    - timer에서 지정된 시간을 썼다고 왔을때
- `timer` : 특정 프로그램이 CPU를 독점하는 것을 막기위한 하드웨어
    - 다음 프로그램으로 업무를 옮길때 timer에 시간을 셋팅하고 CPU를 넘김

## ****Mode bit****

<img src = "https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F67375ef3-a5ff-4aed-82a5-6a30fad45d83%2FUntitled.png&blockId=b44b3778-b2ca-4414-8e28-311627a9b90c](https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fs3-us-west-2.amazonaws.com%2Fsecure.notion-static.com%2F67375ef3-a5ff-4aed-82a5-6a30fad45d83%2FUntitled.png&blockId=b44b3778-b2ca-4414-8e28-311627a9b90c">

- 사용자 프로그램의 잘못된 수행으로 다른 프로그램 및 운영체제에 피해가 가지 않도록 하기 위한 보호 장치
- CPU를 운영체제가 가지고 있느냐, 사용자 프로그램이 가지고 있느냐를 표시해주는 것
    - `0` : 모니터 모드(커널모드, 시스템 모드) → 운영체제가 cpu에서 실행되는 중.(OS 코드 수행)
    - `1` : 사용자 모드 → 사용자 프로그램 수행
- interrupt나 Exception 발생시 하드웨어가 mode bit을 0으로 바꿈
- 사용자 프로그램에게 CPU를 넘기기전 mode bit을 1로 세팅
    - mode bit이 1일때는 제한된 instruction만 실행할 수 있다
- 보안을 해칠 수 있는 중요한 명령어는 모니터 모드에서만 수행가능 '특권명령'으로 규정

## Timer

- CPU가 특정프로그램이 독점하는 것으로부터 보호
    - 운영체제가 사용자 프로그램한테 CPU를 넘겨줄 때는 타이머에다가 정해진 시간을 셋팅한 뒤에 넘겨줌
- 정해진 시간이 흐른 뒤 운영체제에게 제어권이 넘어가도록 인터럽트 발생
- 타이머는 매 클럭 1틱마다 1씩 감소
- 타이머 값이 `0`이되면 타이머 인터럽트 발생
- 타이머는 timer sharing을 구현하기 위해 이용이 되고, 현재시간을 계산하기위해서도 사용됨

## ****Device Controller****

- Device controller는 I/O가 끝났을 경우 interrupt로 CPU에 그 사실을 알림
- device driver(장치구동기): OS 코드 중 각 장치별 처리 루틴 → software
- device controller(장치제어기): 각 장치를 통제하는 일종의 작은 CPU → hardware
- I/O device controller
    - 해당 I/O 장치유형을 관리하는 일종의 작은 CPU
    - 제어 정보를 위해 control register, status register를 가짐
        - 이 register는 CPU에서 해야할 작업들에 대한 내용이 담기는 것
    - local buffer
        - • I/O의 실행결과에 대한 데이터들을 저장하거나 출력을 위한 데이터를 저장하는 저장소

## ****DMA Controller(direct memory access controller)****

- 메모리에 직접적으로 접근할 수 있는 컨트롤러
- I/O 장치의 빈번한 인터럽트를 줄여주기 위한 장치
- I/O 장치의 저장된 데이터를 메모리에 직접 적재를 해주고, 모와서 CPU에 알려주는 역할
    - CPU는 계속 자기 일을 하고 있고, 중간중간 local buffer로 들어오는 내용이 작업이 끝났으면 DMA Controller가 직접 그 내용을 메모리에 복사까지 해줌
    - 작업이 다 끝나면 CPU한테 인터럽트를 또 걸어서 그 일(local buffer → memory 복사)까지 완료했다고 보고 직접 메모리를 접근할 수 있는 컨트롤러

## ****입출력(I/O)의 수행****

- 모든 입출력 명령은 특권 명령
- 사용자 프로그램이 I/O을 하는법
    1. 시스템 콜
        - 사용자 프로그램이 운영체제에 커널 함수를 호출하는 것
        - 일반적인 함수는 사용자프로그램이 자기가 할당된 메모리 내에서 주소를 바꿔가면서 실행이 되지만, I/O 요청은 사용자 프로그램 메모리를 벗어난 OS의 메모리에 요청을 보내야하기 때문에 직접적으로 점프를 하는것이 불가능
        - 사용자프로그램은 소프트웨어적으로, CPU에게 I/O 요청이 있음을 interrupt신호를 보냄
        - 이 interrupt 신호를 받은 CPU은 interrupt가 왔기 때문에 mode bit을 0으로 전환 후 OS모드 실행
    2. trap을 사용하여 인터럽트 벡터의 특정위치로 이동
    3. 제어권이 인터럽트 벡터가 가리키는 인터럽트 서비스 루틴으로 이동
    4. 올바른 I/O 요청인지 확인후 I/O 수행
    5. I/O 완료시 제어권을 시스템콜 다음명령으로 옮김

## ****인터럽트(Interrupt)****

- 인터럽트 당한 시점의 레지스터와 Program counter를 save한 후 CPU의 제어를 인터럽트 처리 루틴에 넘김
    - **Interrupt(넓은 의미)의 인터럽트**
        - Interruct (하드웨어 인터럽트): 하드웨어가 발생시킨 인터럽트
        - Trap (소프트웨어 인터럽트)
            - Exception: 프로그램이 오류를 범한 경우
            - System call: 프로그램이 커널 함수를 호출하는 경우

- 인터럽트 벡터 : 해당 인터럽트의 처리 루틴 주소를 가지고 있음
- 인터럽트 처리 루틴 (=interrupt Service Routine, 인터럽트 핸들러) : 해당 인터럽트를 처리하는 커널 함수
- 인터럽트 과정
    1. I/O 인터럽트 발생 ( 사용자 프로그램이 OS에 I/O요청)
    2. OS가 I/O device controller 에 처리명령 → tarp을 사용해 인터럽트 벡트의 특정 위치로 이동 
    3. CPU는 다른 프로그램으로 넘어가 올바른 요청인지 확인 후 수행
    4. 완료 후 하드웨어 인터럽트
    5. program 카운터에 저장된 다음 명령으로 제어권 옮김
- I/O를 하기 위해서 필요한 인터럽트는 무엇인지
    
    → I/O 요청을 할 때는 소프트웨어 인터럽트로 할 것이고, I/O가 다 끝나면 하드웨어 인터럽트를 사용 
    
    요청한 I/O 가 다 끝났다는 것을 알려주는 것은 I/O Controller가 인터럽트를 걸어서 알려주게 됨
