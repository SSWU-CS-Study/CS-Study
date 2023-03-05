# Process Synchronization

# **Process Synchronization**

<aside>
📌 면접 질문

- 임계 구역(Critical Section)이란?
    - 프로세스 간 공유 자원을 접근하는 데 있어 문제가 발생하지 않도록 한 번에 하나의 프로세스만 이용해 다른 프로세스들의 접근을 제한하는 영역
- 임계 구역 문제를 해결하기 위한 방법
    - **상호 배제(Mutual Exclution)**
        - 하나의 프로세스가 임계 구역에 들어가 있다면 다른 프로세스는 들어갈 수 없어야 한다.
    - **진행(Progress)**
        - 들어가려는 프로세스가 여러 개라면 어느 것이 들어갈지 결정해주어야 한다.
    - **한정된 대기(Bounded Waiting)**
        - 다른 프로세스의 기아(Starvation)를 막기 위해 한 번 임계 구역에 들어간 프로세스는 다음번 임계 구역 접근에 제한이 생겨야 한다.
- 스핀락 (Spinlock) 이란?
    - **특정한 자원을 획득(Lock) 또는 해제(Unlock)를 통해 공유 자원에 대한 접근 권한을 관리하는 방법**
</aside>

### ★  **데이터의 접근**

> 컴퓨터 시스템에서 데이터 연산은 저장 공간과 실행 공간이 아래와 같은 시퀀스로 동작하면서 이루어진다.
> 
1. 저장 공간에 데이터가 있다.
2. 연산할 데이터를 실행 공간으로 가져온다.
3. 실행 공간에서 연산한다.
4. 연산 결과를 저장 공간에 반영한다.

추상적인 표현으로 저장 공간, 실행 공간이라는 말을 썼는데 실행 공간은 CPU나 프로세스, 컴퓨터 내부 등이 있고, 저장 공간은 메모리나 해당 프로세스의 주소 공간, 디스크 등이 있다

**→ 💡 데이터를 읽고 쓰기 때문에 데이터의 동기화가 필요하다.**

### ★ **Race Condition**

> 공유하는 하나의 주체에 여러 주체가 마치 경쟁하듯 접근하려 하는 것을 경쟁 상태, 즉 **race condition**이라고 한다. (저장 공간을 공유하는 실행 공간이 여러 개 있는 경우)
> 

여러 프로세스가 하나의 데이터를 공유하고, 연산을 하려고 할 때 경쟁상태가 생길 수 있다  

예를 들어 메모리에 *count* 변수가 있고 서로 다른 CPU가 각각 증가 연산, 감소 연산을 한다고 가정해보자. 정상적인 동작을 한다면 CPU A가 count 변수를 가져와 1 증가시키고 다시 메모리에 반영하고, CPU B가 count 변수를 가져와 1 감소시키고 다시 메모리에 반영하여 count 변수 값의 변화가 없을 것이다. 하지만 CPU A에서 증가 연산을 하는 동안 CPU B가 메모리에 있는 count 변수를 가져가서 연산한다면 결과는 *count - 1* 이 저장될 것이다.

<aside>
💡 **운영체제에서 race condition이 발생하는 상황**

1. kernel 수행 중 인터럽트 발생 시
2. 프로세스가 시스템콜을 호출하여 kernel mode로 수행 중 context switch가 일어나는 경우
3. multi-processor에서 공유 메모리 내의 커널 데이터
</aside>

<aside>
💡 **OS에서의 race condition**

1. interrupt handler vs kernel
    
    
    ![스크린샷 2023-03-05 13.21.28.png](Process%20Synchronization%208f2d2d68fc8d4716a1daf85daf4a13ce/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-05_13.21.28.png)
    
    kernel address space를 공유하기 때문에 증감 연산 후 *count* 값이 그대로 유지되는 것이 아니라 1 증가한다. 이와 같이 중요한 변수의 값을 건드리는 동안에는 **인터럽트가 발생해도 연산이 끝나고 수행될 수 있게끔 disable** 해준다.
    
2. preempt a process running in kernel
    
    
    ![스크린샷 2023-03-05 13.22.09.png](Process%20Synchronization%208f2d2d68fc8d4716a1daf85daf4a13ce/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-05_13.22.09.png)
    
    프로세스 A가 시스템콜을 호출하여 kernel mode에서 작업을 처리하는 도중에 CPU 할당 시간이 끝나 *context switch* 가 발생했다. 이후 프로세스 B가 CPU를 할당받아 A와 동일하게 시스템콜을 호출했고, count 변수를 1 증가시켰다. 그리고 다시 context switch가 발생하여 프로세스 A가 CPU를 잡아 count 변수를 1 증가시켰다.
    
    count 변수 값이 2 증가해야 하는데 결과는 그렇지 않다. 프로세스 A는 프로세스 B가 증가 연산을 하기 전의 count 값을 갖고 있었고, CPU를 다시 할당받았을 때 그 시점의 context를 가지고 값을 증가시켰기 때문에 1만 증가한 것처럼 보인다.
    
    이를 해결하기 위해 **커널 모드에서 수행 중일 때는 CPU 할당 시간이 끝나도 선점하지 않고** 다시 사용자 모드로 돌아갔을 때 선점한다.
    
3. multi-processor
    
    CPU가 메모리에서 데이터를 가져오기 전에 **`lock`**을 걸어 다른 CPU가 같은 데이터에 접근하는 것을 막아준다. 연산이 끝난 후 데이터를 다시 메모리에 저장할 때 lock을 풀어 줌으로써 그제서야 다른 CPU에서 접근할 수 있게 해준다.
    
    - 방법 1
        - 한 번에 하나의 CPU만 커널에 들어갈 수 있게 하는 방법
            - 커널 전체에 lock을 걸기 때문에 비효율적
    - 방법 2
        - 커널 내부에 있는 각 공유 데이터에 접근할 때마다 해당 데이터에 대한 lock을 거는 방법

</aside>

### ★ **Process Synchronization**

공유 데이터의 동시 접근은 데이터의 불일치 문제를 발생시킬 수 있다. 그래서 일관성을 유지하기 위해 협력 프로세스 간의 실행 순서를 정해주는 메커니즘이 필요함

### ★****Critical Section 임계영역****

> 공유 데이터를 접근하는 코드
> 
- Problem
    - 하나의 프로세스가 *critical section* 에 있을 때 다른 모든 프로세스는 *critical section* 에 들어갈 수 없어야 함
- ****Critical Section 문제를 해결하기 위한 충족 조건****
    - **Mutual Exclusion 상호배제**
        - 뮤텍스 (Mutex)
            
            > **MUT**ual **EX**clusion으로 상호 배제라고도 한다.
            > 
            - 획득(Lock), 해제(Unlock) 상태가 있으며 스핀락과 같이 접근 권한을 획득할 때까지 **BusyWating 상태에 머무르지 않고 Sleep 상태**가 되며 Wakeup이 되면 권한 획득을 시도한다.뮤텍스의 경우엔 Locking 메커니즘으로 오직 하나의 스레드만이 동일 시점에 뮤텍스를 얻어 임계 구역(Critical Section)에 접근할 수 있다.(마찬가지로 획득, 해제의 주체는 동일해야 한다.)
            - 어떤 프로세스가 critical section 부분을 수행 중이면 다른 모든 프로세스들은 그들의 critical section에 들어가면 안됨
        - 스핀락 (Spinlock)
            
            > **특정한 자원을 획득(Lock) 또는 해제(Unlock)를 통해 공유 자원에 대한 접근 권한을 관리**
            > 
            - 권한을 획득하기 전까지 CPU는 무의미한 코드를 수행하는 Busy Waiting 상태로 대기하고 있다가 접근 권한을 얻게되면 내부 코드를 수행하고 종료 후 권한을 해제한다.
                
                상태가 획득, 해제 뿐이기 때문에 공유 영역에서는 하나의 컴포넌트만 접근이 가능하다.(획득, 해제의 주체는 동일해야 한다.)
                
                **하나의 작업이 빠르게 수행**될 수 있다는 장점이 있지만, 선**점 기간 동안에는 다른 프로세스의 작업이 지연될 수 있는 오버 헤드가 존재**한다.그래서 짧게 수행할 수 있는 작업에 주로 사용된다.
                
        
    - **Progress 진행 : 비어있을 때는 사용하게 해줘야 한다.**
        - 아무도 critical section에 있지 않은 상태에서 들어가고자 하는 프로세스가 있으면 critical section에 들어가게 해주어야 함
    - **Bounded Waiting 유한대기**
        - 프로세스가 critical section에 들어가려고 요청한 후부터 해당 요청이 허용될 때까지 다른 프로세스들이 critical section에 들어가는 횟수는 한계가 있어야 함
        - ex) 세 개의 프로세스가 있을 때 두 개의 프로세스만 번갈아가며 critical section에 들어가는 것은 *bounded waiting* 조건을 만족하지 못한 것
        
    
    <aside>
    ✅ 위 조건은 모든 프로세스의 수행 속도는 0보다 크며, 프로세스 간의 상대적인 수행 속도는 가정 하지 않는다
    
    </aside>
    

## ★ ****임계 구역 문제 해결 알고리즘****

<aside>
1️⃣ **Algorithm 1**

**Synchronization variable** 

```c
int turn; 
initially turn = 0; // 몇 번 프로세스가 들어갈 수 있는지를 알려주는 turn 변수
```

**Process P0**

> turn 변수가 0이 아닌 동안 while문을 계속 돌면서 자기 차례를 기다린다.
> 

```c
do {
    while (turn != 0)
    critical section
    turn = 1;
    remainder section
} while (1);
```

**Mutual Exclusiong은 만족, Progress는 만족 X : 위의 알고리즘은 상대방이 사용해야지만 자신이 한번 사용할 수 있는 알고리즘이다.**

 반드시 한 번씩 교대로 임계 구역에 들어가야 하기 때문이다. 만약 프로세스 0은 임계 구역에 5번 들어가야 하고 프로세스 1은 한 번만 들어가면 된다고 가정해 보자. 프로세스 1은 한 번 임계 구역에 들어가고 나오면 끝나지만, 프로세스 0은 프로세스 1이 다시 들어갈 때까지 본인도 못 들어가므로 progess 조건이 성립하지 않는다. (과잉 양보)

</aside>

<aside>
2️⃣ **Algorithm 2**

 **Synchronization variable**

```c
boolean flag[2];
initially flag[모두] = false; // critical section에 들어가고자 하는 의사 표시
```

**Process Pi**

> 상대방이 임계 영역에서 빠져 나왔을 때  진입
> 

```c
do {
    flag[i] = true;
    while (flag[j]);
    critical section
    flag[i] = false;
    remainder section
} while (1);
```

**mutual exclusion은 만족하지만 progress는 만족하지 않는다.**

프로세스 A가 임계 구역에 들어가려고 flag를 true로 바꾼 상황에서 context switch가 발생하여 프로세스 B에게 CPU가 넘어갔다고 하자. 이때 프로세스 B도 flag를 true로 바꿨는데 다시 context switch가 발생하여 프로세스 A가 CPU를 잡는다면 그 누구도 임계 구역에 들어갈 수 없다.

</aside>

<aside>
💡 ★**Algorithm 3 *(Peterson's Algorithm)***

> **Combined synchronization variables of algorithm 1 and algorithm 2**
> 

```c
do {
    flag[i] = true; // 내가 임계 구역에 들어가고 싶다고 알림
    turn = j; // 자신의 다음 차례를 프로세스 j로 바꿔 줌
    while (flag[i] && turn == j);
    critical section
    flag[i] = false;
    remainder section
} while (1);
```

상대방이 임계 구역에 들어가 있지 않고, 들어갈 준비도 하지 않는다면 내가 들어간다.

세 조건을 모두 만족하지만, 계속 CPU와 메모리를 쓰면서 기다리기 때문에 busy waiting (spin lock)이 발생한다. 쉽게 말해 임계 구역에 들어가려면 상대방이 CPU를 잡고 flag 변수를 false로 바꿔주어야 하는데, 내가 CPU를 잡고 있는 상황에서 의미 없이 while문을 돌며 CPU 할당 시간을 낭비해야 한다.

```c
#include <iostream>
#include <thread>

using namespace std;

int cnt;
bool flag[2] = { false, false };
int turn = 0;

void func0() {
    for (int i = 0; i < 10000; i++) {
        flag[0] = true;
        turn = 1;
        while (flag[1] == true && turn == 1) {}

        cnt++;
        printf("cnt1 :: %d\n", cnt);

        flag[0] = false;
    }
}
void func1() {
    for (int i = 0; i < 10000; i++) {
        flag[1] = true;
        turn = 0;
        while (flag[0] == true && turn == 0) {}

        cnt++;
        printf("cnt2 :: %d\n", cnt);

        flag[1] = false;
    }
}

int main() {
    thread t1(func0);
    thread t2(func1);

    t1.join();
    t2.join();

    cout << "cnt : :" << cnt << endl;

    return 0;
}
```

1. flag[0] = true로 설정하여 0번 스레드가 임계 영역 진입을 하고 싶다고 표시한다.
2. 이때 turn = 1으로 설정하여, 1번 프로세스가 먼저 임계 영역에 들어가라고 양보한다.
3. 만약 이때 context switch가 되지 않았다면 while문에 갇히게 된다.
4. 1번 프로세스가 모든 작업을 끝내면 turn = 0, flag[1] = false가 되므로 0번 프로세스가 임계 구역에 진입할 수 있다.
</aside>
