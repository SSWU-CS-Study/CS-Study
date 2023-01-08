## 7.1 경쟁상태

공유 자원에 대해 여러 프로세스가 동시에 접근을 시도할 때, 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태

동시에 접근할 때 자료의 일관성을 해치는 결과가 나타날 수 있다.

### 7.1.1 발생

1. 커널 작업을 수행 중에 **인터럽트가 발생**할 때
    
    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbNwSKo%2FbtqucnG897W%2FXvaqS7vyG8kTtPilBVUIuk%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbNwSKo%2FbtqucnG897W%2FXvaqS7vyG8kTtPilBVUIuk%2Fimg.png)
    
2. 프로세스가 system call을 하여 커널 모드로 진입하여 작업을 수행하는 도중 **문맥 교환이 발생**할 때
    
    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcgKHsD%2FbtqufcRX7DX%2FBEg19ysc7CrUG0PQGTyVnk%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcgKHsD%2FbtqufcRX7DX%2FBEg19ysc7CrUG0PQGTyVnk%2Fimg.png)
    
3. 멀티 프로세서에서 **공유 메모리 내의 커널 데이터에 접근**할 때
    
    ![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwdEsp%2FbtqubSUYy5q%2FJbVEnNWTLSKkKMpsbl55R1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FwdEsp%2FbtqubSUYy5q%2FJbVEnNWTLSKkKMpsbl55R1%2Fimg.png)
    

## 7.2 임계 구역(Critical Section)

OS에서 여러 프로세스가 데이터를 공유하면서 수행될 때, **각 프로세스에서 공유 데이터를 엑세스하는 프로그램 코드 부분**

공유 자원의 독점을 보장해주는 역할을 수행한다.

### 7.2.1 프로그램적으로 해결하는 방법의 충족 조건

- 상호 배제 Multi Exclusion
    - 프로세스1이 임계 영역 부분을 수행 중이면 다른 모든 프로세스들은 그 임계 영역에 접근할 수 없음
    - 뮤텍스, 세마포어 같은 lock 메커니즘을 통해 구현
    
    ![https://junhyunny.github.io/images/race-condition-2.JPG](https://junhyunny.github.io/images/race-condition-2.JPG)
    
- 진행 Progress
    - 아무도 임계 영역에 있지 않다면, 들어가고자 하는 프로세스를 들어가게 해줘야 함
    
    ![https://junhyunny.github.io/images/race-condition-3.JPG](https://junhyunny.github.io/images/race-condition-3.JPG)
    
- 유한대기 Bounded Waiting
    - 프로세스가 임계 영역에 들어가기 위해 무한정으로 기다리는 현상(=starvation)이 있어서는 안됨
    
    ![https://junhyunny.github.io/images/race-condition-4.JPG](https://junhyunny.github.io/images/race-condition-4.JPG)
    

### 7.1.2 피터슨 알고리즘(Peterson’s Algorithm)

```cpp
do {
	frag[i] = true;                       // My intention is to enter
		turn = j;                           // Set to his turn
	while(flag[j] && trun == j);          // Wait...

	//critical section
	
	flag[i] = false;
	//remainder section
} while(1);
```

프로세스 간 메시지를 전송하거나, 공유메모리를 통해 공유된 자원에 여러 개의 프로세스가 동시에 접근하면 임계 영역 문제가 발생할 수 있다.

이를 해결하기 위해 데이터를 한 번에 하나의 프로세스만 접근할 수 있도록 제한을 두는 **동기화 방식**을 취해야 한다.

동기화 도구에는 대표적으로 **뮤텍스(Mutex)**와 **세마포어(Semaphore)**가 있다.

이들은 모두 공유된 자원의 데이터를 여러 스레드/프로세스가 접근하는 것을 막는 역할을 한다.

## 7.3 셰마포어

**멀티 프로그래밍 환경에서 공유된 자원에 대한 접근을 제한하는 방법**

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft2sHF%2FbtqZJ6mQJzl%2FTcm2kfZtxXHWvnNY8t3xb1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft2sHF%2FbtqZJ6mQJzl%2FTcm2kfZtxXHWvnNY8t3xb1%2Fimg.png)

- ~~공유자원의 상태를 나타낼 수 있는 **카운터**로 생각할 때,~~
    - ~~사용하고 있는 스레드/프로세스의 수를 공통으로 관리하는 하나의 값을 이용해 상호배제를 달성한다.~~
    - ~~운영체제 또는 커널의 한 지정된 저장장치 내의 값~~
    - ~~일반적으로 비교적 긴 시간을 확보하는 리소스에 대한 이용~~
    - ~~유닉스 프로그래밍에서 세마포어는 운영체제의 리소스를 경쟁적으로 사용하는 다중 프로세스에서 행동을 조정하거나 또는 동기화하는 기술~~
- 공유 자원에 접근할 수 있는 프로세스의 최대 허용치만큼 동시에 사용자가 접근할 수 있다.
- 각 프로세스는 세마포어의 값을 확인하고 변경할 수 있다.
- 자원을 사용하지 않는 상태가 될 때, 대기하던 프로세스가 즉시 자원을 사용한다.
- 이미 다른 프로세스에 의해 사용중이라는 사실을 알게 되면, 재시도 전에 일정시간 대기해야 한다.
- 세마포어를 사용하는 프로세스는 그 값을 확인하고, 자원을 사용하는 동안에는 그 값을 변경함으로써 다른 세마포어 사용자들이 대기하도록 해야 한다.
- 세마포어는 이진수를 사용하거나 추가적인 값을 가질 수 있다.

> 세마포어는 손님이 화장실을 좀 더 쉽게 이용할 수 있는 레스토랑이다. 세마포어를 이용하는 레스토랑의 화장실에는 여러 개의 칸이 있다. 그리고 화장실 입구에는 현재 화장실의 빈 칸 개수를 보여주는 전광판이 있다.
> 

![https://cdn-images-1.medium.com/max/1600/1*ZJXrQu8rFhSQxW6LVAI-GA.png](https://cdn-images-1.medium.com/max/1600/1*ZJXrQu8rFhSQxW6LVAI-GA.png)

> 만약 당신이 화장실에 가고 싶다면 입구에서 빈 칸의 개수를 확인하고 빈 칸이 1개 이상이라면 빈칸의 개수를 하나 뺀 다음에 화장실로 입장해야 한다. 그리고 나올 때 빈 칸의 개수를 하나 더해준다.
> 

![https://cdn-images-1.medium.com/max/1600/1*lFNABipdkdtxFvW9UZmCaw.png](https://cdn-images-1.medium.com/max/1600/1*lFNABipdkdtxFvW9UZmCaw.png)

> 든 칸에 사람이 들어갔을 경우 빈 칸의 개수는 0이 되며 이때 화장실에 들어가고자 하는 사람이 있다면 빈 칸의 개수가 1로 바뀔 때까지 기다려야 한다.
> 

![https://cdn-images-1.medium.com/max/1600/1*wP9yqG6QBuS7A8i_kKTyRA.png](https://cdn-images-1.medium.com/max/1600/1*wP9yqG6QBuS7A8i_kKTyRA.png)

> 사람들은 나오면서 빈 칸의 개수를 1씩 더한다. 그리고 기다리던 사람은 이 숫자에서 다시 1을 뺀 다음 화장실로 돌진한다.
> 

![https://cdn-images-1.medium.com/max/1600/1*36aMopAPHO3e80YYADmY6w.png](https://cdn-images-1.medium.com/max/1600/1*36aMopAPHO3e80YYADmY6w.png)

> 이처럼 세마포어는 공통으로 관리하는 하나의 값을 이용해 상호배제를 달성한다.
> 

> 세마포어의 화장실이 공유자원이며 사람들이 쓰레드, 프로세스이다. 그리고 화장실 빈칸의 개수는 현재 공유자원에 접근할 수 있는 쓰레드, 프로세스의 개수를 나타낸다.
> 

### Busy-Wait 방식

프로세스P가 자원이 모두 사용 중이라면, wait하는 방식

만약 자원의 여유가 생기면 s—를 통해 자원을 획득한다. 그리고 자원을 모두 사용했다면 s++를 통해 자원을 반납한다.

```cpp
P(s){
	while(s <= 0) do wait
	s--;
}

V(s){
	s++;
}
```

### Block-Wakeup 방식

```cpp
type struct{
	int value;                  // Semaphore
	struct process *Q;          // Wait Queue
}semaphore;
```

먼저 자원의 값을 감소시킨다. 만약 자원의 개수가 부족해 사용할 수 없다면 현재 프로세스를 Wait Queue에 추가시킨 후, block()을 통해 suspend시킨다.

만약 다른 프로세스가 작업을 완료해 자원의 값을 증가시키며 반납을 했을 때, 자원의 수가 없다면 이는 현재 자원을 기다리는 프로세스가 존재한다는 뜻이다. 따라서 Wait Queue에서 프로세스를 꺼내와 wakeup()을 통해 깨우게 되는 것이다.

```cpp
P(s){
	s.value--;
	if(s.value < 0){
		// add this process to s.queue;
		block();
	}
}

V(s){
	s.value++;
	if(s.value <= 0){
		// remove a process P from s.queue;
		wakeup(P);
	}
}
```

임계 구역의 길이가 긴 경우는 Block-wakeup 방식이 유리하다. 하지만 임계 구역이 짧다면 잦은 문맥 교환으로 인해 오버헤드가 증가하게 된다. 따라서 반대의 경우에는 Busy-wait 방식이 유리하다.

- 모니터(Monitor)
    
    동기화 문제를 해결하기 위해서 셰마포어라는 방법을 사용했지만, 아래와 같은 문제점을 가지고 있다.
    
    - 코딩하기 어렵다
    - 정확성의 입증이 힘들다
    - 자발적 협력이 필요하다
    - 한 번의 실수가 모든 시스템에 치명적 영향을 끼친다
    
    모니터 내에서는 한 번에 하나의 프로세스만 활동이 가능하다. 프로그래머가 동기화 제약 조건을 명시적으로 코딩할 필요가 없다. 또한 프로세스가 모니터 안에서 기다릴 수 있도록 Condition Variable을 사용한다. 이는 wait/signal 연산에 의해서만 접근이 가능하다.
    
    - wait() : 다른 프로세스가 signal을 하기 전까지 suspend된다.
    - signal() : 정확하게 하나의 suspend된 프로세스를 깨운다.

## 7.4 뮤텍스

**동시 프로그래밍에서 공유 불가능한 자원의 동시 사용을 피하기 위해 사용하는 알고리즘**

![https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdfJ75G%2FbtqZJ43DWsg%2FOUjUDPalDipT8rkuM7aks1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdfJ75G%2FbtqZJ43DWsg%2FOUjUDPalDipT8rkuM7aks1%2Fimg.png)

- 임계구역(Critical Section)을 가진 스레드들의 실행시간(Running Time)이 서로 겹치지 않고 각각 단독으로 실행(**상호배제_Mutual Exclution**)되도록 하는 기술
- 한 프로세스에 의해 소유될 수 있는 Key를 기반으로 한 상호배제 기법
    - Key에 해당하는 어떤 객체(Object)가 있으며, 이 객체를 소유한 스레드/프로세스만이 공유자원에 접근할 수 있다.
- 다중 프로세스들의 공유 리소스에 대한 접근을 조율하기 위해 동기화(Synchronization) 또는 락(Lock)을 사용.
- 즉, 뮤텍스 객체를 두 스레드가 동시에 사용할 수 없다.

> 화장실이 하나뿐인 식당과 비슷하다. 화장실을 가기 위해서는 카운터에서 열쇠를 받아 가야 한다.
> 

> 당신이 화장실을 가려고 하는데 카운터에 키가 있으면 화장실에 사람이 없다는 뜻이고 당신은 그 열쇠를 이용해 화장실에 들어갈 수 있다.
> 

![https://cdn-images-1.medium.com/max/1600/1*6JKj81oYsxQlDhjAlMXQjg.png](https://cdn-images-1.medium.com/max/1600/1*6JKj81oYsxQlDhjAlMXQjg.png)

> 당신이 화장실에서 행복한 시간을 보내고 있는데 다른 테이블에 있는 어떤 남자가 화장실에 가고 싶어졌다. 이 남자는 아무리 용무가 급하더라도 열쇠가 없기 대문에 화장실에 들어갈 수 없다. 결국 남자는 당신이 용무를 마친 후 나올 때까지 카운터에서 기다려야 한다.
> 

![https://cdn-images-1.medium.com/max/1600/1*kbTbS09Yvah9ja7nozSAMA.png](https://cdn-images-1.medium.com/max/1600/1*kbTbS09Yvah9ja7nozSAMA.png)

> 곧이어 옆 테이블에 있는 남자도 화장실에 가고 싶어졌고 이 남자 또한 화장실에 들어가기 위해서는 카운터에서 대기해야한다.
> 

![https://cdn-images-1.medium.com/max/1600/1*CgUE8ByDUKnVkkrEbMPwCQ.png](https://cdn-images-1.medium.com/max/1600/1*CgUE8ByDUKnVkkrEbMPwCQ.png)

> 이제 당신이 화장실에서 나와 카운터에 키를 돌려놓았다. 이제 기다리던 사람들 중 맨 앞에 있던 사람은 키를 받을 수 있고 이를 이용해 화장실에 갈 수 있다.
> 

![https://cdn-images-1.medium.com/max/1600/1*dIIfI3ezb3Gt2YH1uWhauQ.png](https://cdn-images-1.medium.com/max/1600/1*dIIfI3ezb3Gt2YH1uWhauQ.png)

> 이것이 뮤텍스가 동작하는 방식이다. 화장실을 이용하는 사람은 프로세스 혹은 쓰레드이며 화장실은 공유자원, 화장실 키는 공유자원에 접근하기 위해 필요한 어떤 오브젝트이다.
> 

![https://cdn-images-1.medium.com/max/1600/1*CdLr52i_BZjnEf3uWZyRVQ.png](https://cdn-images-1.medium.com/max/1600/1*CdLr52i_BZjnEf3uWZyRVQ.png)

> 즉 뮤텍스는 key에 해당하는 어떤 오브젝트가 있으며 이 오브젝트를 소유한 쓰레드, 프로세스만이 공유자원에 접근할 수 있다.
> 

## 차이점

1. **동기화 대상의 갯수**
    1. Mutex는 동기화 대상이 only 1개일 때 사용
    2. Semaphore는 동기화 대상이 1개 이상일 때 사용
2. 세마포어는 뮤텍스가 될 수 있지만, 뮤텍스는 세마포어가 될 수 없다.
    1. Mutex는 0, 1로 이루어진 이진 상태를 가지므로 Binary Semaphore!
3. Mutex는 자원 소유 가능 + 책임을 가지는 반면, Semaphore는 자원 소유 불가
    1. 뮤텍스는 상태 0, 1 뿐이므로 Lock 가질 수 있음
4. Mutex는 소유하고 있는 스레드만이 이 Mutex를 해제할 수 있다.
    1. 반면, Semaphore는 Semaphore를 소유하지 않는 스레드가 Semaphore를 해제할 수 있다.
5. Semaphore는 시스템 범위에 걸쳐 있고, 파일 시스템 상의 파일로 존재한다.
    1. 반면, Mutex는 프로세스의 범위를 가지며 프로세스 종료될 때 자동으로 Clean up 된다.
    

뮤텍스와 세마포어는 모두 완벽한 기법은 아니므로, 데이터 무결성을 보장할 수 없으며 모든 교착 상태를 해결하지는 못한다.

하지만, 상호배제를 위한 기본적인 기법이며 여기에 좀 더 복잡한 매커니즘을 적용해 개선된 성능을 가질 수 있도록 하는 것이 중요하다.

## QUIZ

- 경쟁상태란 무엇인가요?
    
    공유 자원에 대해 여러 프로세스가 동시에 접근을 시도할 때, 타이밍이나 순서 등이 결과값에 영향을 줄 수 있는 상태로 동시에 접근할 때 자료의 일관성을 해치는 결과가 나타날 수 있습니다.
    
- 세마포어와 뮤텍스의 공통점과 차이점은 무엇인가요?
    
    둘 다 병행 처리를 위한 프로세스 동기화 기법으로 세마포어는 공유 자원에 세마포어의 변수만큼의 프로세스에 접근할 수 있습니다. 반면에 뮤텍스는 오직 1개만의 프로세스만 접근할 수 있습니다. 또한 현재 수행중인 프로세스가 아닌 다른 프로세스가 세마포어를 해제할 수 있지만 뮤텍스는 락을 획득한 프로세스가 반드시 그 락을 해제해야 합니다.
    
- 임계구역이란 무엇인가요?
    
    각 프로세스에서 공유 데이터를 접근하는 프로그램 코드 부분으로 한 프로세스가 임계 구역을 수행할 때는 다른 프로세스가 접근하지 못하도록 해야 한다.
    

[뮤텍스(Mutex)와 세마포어(Semaphore)의 차이](https://worthpreading.tistory.com/90)

[[OS] 4. 프로세스 동기화 (Process Synchronization)](https://hibee.tistory.com/297)

[경쟁 상태(Race Condition)](https://junhyunny.github.io/information/operating-system/junit/race-condition/)
