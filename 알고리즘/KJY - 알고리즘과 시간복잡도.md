## 알고리즘과 복잡도

> 어떤 목적을 달성하거나 결과물을 만들어내기 위해 거쳐야 하는 일련의 과정들
> 

알고리즘 문제를 풀다 보면 문제에 대한 해답을 찾는 것이 가장 중요하다. 그러나 그에 못지않게, 효율적인 방법으로 문제를 해결했는지도 중요하다. 효율적인 방법을 고민한다는 것은 시간 복잡도를 고민한다는 것과 같은 말이다.

알고리즘의 실행시간은 컴퓨터가 알고리즘 코드를 실행하는 속도에 의존한다. 이 속도는 컴퓨터의 처리속도, 사용된 언어종류, 컴파일러의 속도등 에 달려있다.

알고리즘을 평가할 때 시간 복잡도와 공간 복잡도를 사용한다.

- 공간복잡도 : 알고리즘이 실행될 때 사용하는 메모리의 양을 나타낸다.
    - 요즘에는 데이터를 저장할 수 있는 메모리의 발전으로 중요도가 낮아졌다.
- 시간복잡도 : 입력된 N의 크기에 따라 실행되는 조작의 수를 나타낸다.
    - ‘입력값의 변화에 따라 연산을 실행할 때, 연산 횟수에 비해 시간이 얼마만큼 걸리는가?’
    - 왜 실행시간이 아닌 연산수치로 판별할까?  명령어의 실행시간은 컴퓨터의 하드웨어 또는 프로그래밍 언어에 따라 편차가 크게 달라지기 때문에 명령어의 실행 횟수만을 고려하는 것이다.
    - 시간 복잡도 표기법에는 크게 3가지가 있다.
        - 최상의 경우 : 오메가 표기법 (Big-Ω Notation)
        - 평균의 경우 : 세타 표기법 (Big-θ Notation)
        - 최악의 경우 : 빅오 표기법 (Big-O Notation)
    - 평균인 세타 표기를 하면 가장 정확하고 좋겠지만 알고리즘이 복잡해질수록 평가하기가 까다롭다.
    - 그래서 최악의 경우인 **빅오표기법**을 사용하는데 최악의 경우가 발생하지 않기를 바라며 시간을 계산하는 것보다 최악의 경우를 판단하면 평균과 가까운 성능으로 예측하기 쉽기 때문이다.

## 빅오(Big-O) 표기법

빅오 표기법은 불필요한 연산을 제거하여 알고리즘 분석을 쉽게 할 목적으로 사용된다.

연산 횟수가 다항식으로 표현될 경우, 최고차항을 제외한 모든 항과 최고차항의 계수를 제외시켜 나타낸다.

예를 들어 입력 크기가 n이라고 했을 때 다음과 같이 표기합니다.

- T(n) = n^2  + 2n + 1 = O(n^2) : 최고차항만 나타냄.
- T(n) = 2n = O(n) : 최고차항의 계수는 제외함.

```cpp
int func (int n) {
  int sum = 0;     // 대입연산 1회
  int i = 0;       // 대입연산 1회
   
  for(i=0; i < n; i++) {  // 반복문 n+1회
    sum += i;             // 덧셈 연산 n회
  }
  for(i=0; i < n; i++) {  // 반복문 n+1회
    sum += i;             // 덧셈 연산 n회   
  }
  return sum;       // 리턴 1회
}
```

T(n) = 1 + 1 + (n + 1) + n + (n + 1) + n + 1

       = 4n + 5

       = O(n)

 

![https://blog.chulgil.me/content/images/2019/02/Screen-Shot-2019-02-07-at-2.31.54-PM-1.png](https://blog.chulgil.me/content/images/2019/02/Screen-Shot-2019-02-07-at-2.31.54-PM-1.png)

<aside>
💡 **O(1) > O(logn) > O(n) > O(n^2) > O(2^n)>O(n!)**

</aside>

### O(1)

![https://hanamon.kr/wp-content/uploads/2021/07/O1-1280x719.png](https://hanamon.kr/wp-content/uploads/2021/07/O1-1280x719.png)

**일정한 복잡도**라고 하며, **입력값이 증가하더라도 시간이 늘어나지 않고 일정**하다.

다시 말해 입력값의 크기와 관계없이, 즉시 출력값을 얻어낼 수 있다는 의미이다.

ex) 스택의 push, pop

```cpp
function O_1_algorithm(arr, index) {
  return arr[index];
}

let arr = [1, 2, 3, 4, 5];
let index = 1;
let result = O_1_algorithm(arr, index);
console.log(result); // 2
```

이 알고리즘에선 입력값의 크기가 아무리 커져도 즉시 출력값을 얻어낼 수 있다.예를 들어 arr의 길이가 100만이라도, 즉시 해당 index에 접근해 값을 반환할 수 있다.

### O(log n)

![https://hanamon.kr/wp-content/uploads/2021/07/Olog-n-1280x719.png](https://hanamon.kr/wp-content/uploads/2021/07/Olog-n-1280x719.png)

**로그 복잡도(logarithmic complexity)**라고 부르며, Big-O표기법중 **O(1) 다음으로 빠른 시간 복잡도**를 가진다. 

ex) 이진 탐색, 재귀가 순기능으로 이루어지는 경우

### O(n)

![https://hanamon.kr/wp-content/uploads/2021/07/On-1280x719.png](https://hanamon.kr/wp-content/uploads/2021/07/On-1280x719.png)

**선형 복잡도(linear complexity)**라고 부르며, **입력값이 증가함에 따라 시간 또한 같은 비율로 증가**하는 것을 의미한다.

ex) 1차원 for문

```cpp
function O_n_algorithm(n) {
  for (let i = 0; i < n; i++) {
    // do something for 1 second
  }
}

function another_O_n_algorithm(n) {
  for (let i = 0; i < 2n; i++) {
    // do something for 1 second
  }
}
```

`O_n_algorithm` 함수에선 입력값(n)이 1 증가할 때마다 코드의 실행 시간이 1초씩 증가한다.즉 입력값이 증가함에 따라 `같은 비율로` 걸리는 시간이 늘어나고 있다. 그렇다면 함수 `another_O_n_algorithm` 은 어떨까?입력값이 1 증가할때마다 코드의 실행 시간이 2초씩 증가한다.이것을 보고, “아! 그렇다면 이 알고리즘은 O(2n) 이라고 표현하겠구나!” 라고 생각할 수 있다.그러나, 사실 이 알고리즘 또한 Big-O 표기법으로는 O(n)으로 표기한다. 입력값이 커지면 커질수록 계수(n 앞에 있는 수)의 의미(영향력)가 점점 퇴색되기 때문에, 같은 비율로 증가하고 있다면 2배가 아닌 5배, 10배로 증가하더라도 O(n)으로 표기한다.

### O(n^2)

![https://hanamon.kr/wp-content/uploads/2021/07/On2-1280x719.png](https://hanamon.kr/wp-content/uploads/2021/07/On2-1280x719.png)

**2차 복잡도(quadratic complexity)**라고 부르며, **입력값이 증가함에 따라 시간이 n의 제곱수의 비율로 증가**하는 것을 의미한다.

ex) 이중 for문, 삽입정렬, 거품정렬, 선택정렬

```cpp
function O_quadratic_algorithm(n) {
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      // do something for 1 second
    }
  }
}

function another_O_quadratic_algorithm(n) {
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {
      for (let k = 0; k < n; k++) {
        // do something for 1 second
      }
    }
  }
}
```

2n, 5n 을 모두 O(n)이라고 표현하는 것처럼, n3과 n5 도 모두 O(n2)로 표기한다. n이 커지면 커질수록 지수가 주는 영향력이 점점 퇴색되기 때문에 이렇게 표기한다.

### O(2^n)

![https://hanamon.kr/wp-content/uploads/2021/07/O2n-1280x719.png](https://hanamon.kr/wp-content/uploads/2021/07/O2n-1280x719.png)

**기하급수적 복잡도(exponential complexity)**라고 부르며, Big-O 표기법 중 **가장 느린 시간 복잡도**를 가진다.

구현한 알고리즘의 시간 복잡도가 O(2^n)이라면 다른 접근 방식을 고민해 보는 것이 좋다.

ex) 피보나치 수열, 재귀가 역기능을 할 경우

```cpp
function fibonacci(n) {
  if (n <= 1) {
    return 1;
  }
  return fibonacci(n - 1) + fibonacci(n - 2);
}
```

재귀로 구현하는 피보나치 수열은 O(2n)의 시간 복잡도를 가진 대표적인 알고리즘이다. 브라우저 개발자 창에서 n을 40으로 두어도 수초가 걸리는 것을 확인할 수 있으며, n이 100 이상이면 평생 결과를 반환받지 못할 수도 있다.

### 시간복잡도를 구하는 요령

각 문제의 시간복잡도 유형을 빨리 파악할 수 있도록 아래 예를 통해 빠르게 알아 볼수 있다.

- 하나의 루프를 사용하여 단일 요소 집합을 반복 하는 경우 : O (n)
- 컬렉션의 절반 이상 을 반복 하는 경우 : O (n / 2) -> O (n)
- 두 개의 다른 루프를 사용하여 두 개의 개별 콜렉션을 반복 할 경우 : O (n + m) -> O (n)
- 두 개의 중첩 루프를 사용하여 단일 컬렉션을 반복하는 경우 : O (n²)
- 두 개의 중첩 루프를 사용하여 두 개의 다른 콜렉션을 반복 할 경우 : O (n * m) -> O (n²)
- 컬렉션 정렬을 사용하는 경우 : O(n*log(n))

## 정렬 알고리즘 비교

| 정렬 알고리즘 | 최적 | 평균 | 최악 | 메모리 | 동일값 순서 유지 | 비고 |
| --- | --- | --- | --- | --- | --- | --- |
| 버블 정렬 | O(n) | O(n^2) | O(n^2) | O(1) | yes |  |
| 삽입 정렬 | O(n) | O(n^2) | O(n^2) | O(1) | yes |  |
| 선택 정렬 | O(n^2) | O(n^2) | O(n^2) | O(1) | yes |  |
| 힙 정렬 | O(n log n) | O(n log n) | O(n log n) | O(1) | yes |  |
| 병합 정렬 | O(n log n) | O(n log n) | O(n log n) | O(n) | yes |  |
| 퀵 정렬 | O(n log n) | O(n log n) | O(n^2) | O(log n) | No | 추가 내용 |
| 셀 정렬 | O(n log n) | 간격 순서에 영향 | O(n (log n)^2) | O(1) | yes |  |
| 계수 정렬 | O(n + r) | O(n + r) | O(n + r) | O(n + r) | yes | r - 배열 내 가장 큰 수 |
| 기수 정렬 | O(n * k) | O(n * k) | O(n * k) | O(n + k) | yes | k - 키 값의 최대 길이 |

## 자료구조 비교

|  | 평균 |  |  |  | 최악 |  |  |  |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
|  | 접근 | 검색 | 삽입 | 삭제 | 접근 | 검색 | 삽입 | 삭제 |
| 배열(Array) | O(1) | O(n) | O(n) | O(n) | O(1) | O(n) | O(n) | O(n) |
| 스택(Stack) | O(n) | O(n) | O(1) | O(1) | O(n) | O(n) | O(1) | O(1) |
| 큐(Queue) | O(n) | O(n) | O(1) | O(1) | O(n) | O(n) | O(1) | O(1) |
| 단일연결리스트(Singly-Linked List) | O(n) | O(n) | O(1) | O(1) | O(n) | O(n) | O(1) | O(1) |
| 이중연결 리스트(Doubly-Linked List) | O(n) | O(n) | O(1) | O(1) | O(n) | O(n) | O(1) | O(1) |
| 해쉬 테이블(Hash Table) | - | O(1) | O(1) | O(1) | - | O(n) | O(n) | O(n) |
| 이진 탐색 트리(Binary Search Tree) | O(log n) | O(log n) | O(log n) | O(log n) | O(n) | O(n) | O(n) | O(n) |
| 비(B)-트리 | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) |
| 레드-블랙 트리 | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) |
| AVL 트리 | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) | O(log n) |

## Trade-off

시간 복잡도와 공간 복잡도 간의 거래 관계

효율적인 알고리즘을 사용한다고 가정했을 때 보통 시간 복잡도와 공간 복잡도는 일종의 거래 관계가 성립한다.

메모리를 더 많이 사용하는 대신에 반복되는 연산을 생략하거나 더 많은 정보를 관리하면서 계산의 복잡도를 줄일 수 있다. 이때 메모리를 더 많이 사용해서 시간을 비약적으로 줄이는 방법으로 메모이제이션(Memoization) 기법이 있다.

### 메모이제이션 Memoization

- 기억되어야 할 것이라는 뜻의 라틴어
- 컴퓨터 프로그램이 동일한 계산을 반복적으로 해야 할 때, 이전에 계산한 값을 메모리에 저장하여 중복적인 계산을 제거하여 전체적인 실행 속도를 빠르게 해주는 기법
- 동적 계획법의 핵심 기술
- 피보나치수열로 알아보는 재귀와 메모이제이션
    - 피보나치 수를 구하는 알고리즘에서 fibo(n)의 값을 계산하고 저장해 놓으면 다음 계산에서 필요할 대 값만 가져와서 사용하면 되므로 실행 시간을 줄일 수 있다.
    - 재귀 함수를 사용한 피보나치 수열 알고리즘
        
        ```python
        def fibo(n):
        	if n < 2:
        		return n
        	else:
        		return fibo(n-1) + fibo(n-2)
        ```
        
    - 메모이제이션 기법을 사용한 피보나치수열 알고리즘
        
        ```python
        def memoi(n):
        	global memo
        	if n >= len(memo):
        		memo.append(fibo(n-1) + fibo(n-2))
        	return memo[n]
        
        memo = [0, 1]
        ```
        

## Quiz

- 정렬 알고리즘의 시간 복잡도를 말해보세요
- 이 코드의 시간 복잡도는 어떻게 되나요?
- 본인 코드의 시간 복잡도를 구해보세요

[[알고리즘] Time Complexity (시간 복잡도) - 하나몬](https://hanamon.kr/%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-time-complexity-%EC%8B%9C%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84/)

[시간 복잡도(Time Complexity) 및 공간 복잡도(Space Complexity)](https://yoongrammer.tistory.com/79)

[[Algorithm] 시간 복잡도, 공간 복잡도](https://velog.io/@cha-suyeon/Algorithm-%EC%8B%9C%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84-%EA%B3%B5%EA%B0%84-%EB%B3%B5%EC%9E%A1%EB%8F%84)

[https://wondytyahng.tistory.com/entry/memoization-메모이제이션](https://wondytyahng.tistory.com/entry/memoization-%EB%A9%94%EB%AA%A8%EC%9D%B4%EC%A0%9C%EC%9D%B4%EC%85%98)

[https://brunch.co.kr/@jihyun-um/41](https://brunch.co.kr/@jihyun-um/41)
