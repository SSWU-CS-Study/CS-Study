## 선형(순차) 탐색

<aside>
💡 맨 앞이나, 맨 뒤에서부터 순서대로 하나씩 찾아보는 알고리즘

</aside>

- 가장 단순하고 간단한 탐색 알고리즘
- 정렬되지 않은 리스트에 많이 사용

### 방법

1. 배열과 값을 인자로 받는다
2. 배열을 루핑하면서 현재 배열의 요소가 값과 일치하는지를 확인한다
3. 만약 일치한다면, 찾은 요소의 인덱스를 반환한다
4. 만약 값을 찾을 수 없다면 -1을 반환한다

### 시간 복잡도

- O(N)
    - 순차 탐색 알고리즘이 탐색에 성공할 경우, 탐색을 위한 평균 비교 횟수는 다음과 같습니다.
        
        (1+2+3+⋯+n)/n=(n+1)/2(1+2+3+⋯+n)/n=(n+1)/2
        
        탐색에 실패할 경우에는 n번 비교하게 되기에 순차 탐색의 시간 복잡도는 O(n)입니다
        
- 최고 케이스 : O(1)
- 최악 케이스 : O(n)
- 평균 케이스 : O(n)

### 코드

```cpp
#include <stdio.h>
 
#define MAX_SIZE 10
int list[MAX_SIZE];
 
// 순차 탐색
int seq_search(int key, int low, int high) {
	int i;
 
	for(i = low; i <= high; i++) {
		if (list[i] == key) 
			return i; // 탐색에 성공하면 키 값의 인덱스 반환
	}
	return -1; // 탐색에 실패하면 -1 반환
}
 
// 개선된 순차 탐색
int seq_search2(int key, int low, int high) {
	int i;
	
	list[high + 1] = key;
	for(i = low; list[i] != key; i++) // 키값을 찾으면 종료
		;
	if (i == (high + 1)) return -1; // 탐색 실패
	else return i; // 탐색 성공
}
```

## 이진 탐색

<aside>
💡 중간 지점을 기준으로 데이터를 반씩 나눠서 탐색하는 알고리즘

</aside>

- 매우 빠른 알고리즘
    - 한 번에 하나씩 요소를 지워나가는 것이 아니라 남은 요소들의 절반을 지워나감
- 정렬된 배열에서만 작동

### 방법

1. 정렬된 배열과 값을 인자로 받는다
2. 배열의 첫 시작점을 왼쪽 포인터로, 배열의 끝을 오른쪽 포인터로 정한다
3. 왼쪽 포인터가 오른쪽 포인터로 올 때까지 또는 값을 찾을 때까지 아래 로직을 반복한다
    1. 가운데 점을 가리키는 포인터를 만든다
    2. 만약 원하는 값을 찾았다면 해당 인덱스를 반환한다
    3. 만약 값이 너무 작다면, 왼쪽 포인터의 값을 올린다 → 우측으로 상향
    4. 만약 값이 너무 크다면, 오르쪽 포인터의 값을 내린다 → 좌측으로 하향
4. 만약 값을 찾을 수 없다면 -1을 반환한다

### 시간 복잡도

- O(logN)
    - n개의 원소 중 특정 원소를 탐색하고자 할 때, 탐색 범위가 1이 될 때의 탐색 횟수를 k라고 하면 n/(2^k)=1이 된다.
        
        따라서 k=log2n 이다. 
        
        그렇기에 이진 탐색의 시간 복잡도는 (O(log2n)이다.
        
- 최고 케이스 : O(1)
- 최악 케이스 : O(logN)
- 평균 케이스 : O(logN)

### 이진 탐색 트리

- 모든 원소의 키는 유일한 키를 가진다.
- 왼쪽 서브 트리의 키들은 루트 키보다 작다.
- 오른쪽 서브 트리의 키들은 루트의 키보다 크다.
- 왼쪽과 오른쪽 서브 트리도 이진 탐색 트리이다.

찾고자 하는 키 값이 이진 트리의 루트 노드의 키값과 비교했을 때 루트 노드보다 작으면 왼쪽 서브트리에 있고, 루트 노드보다 크면 오른쪽 서브트리에 있음을 알 수 있습니다. 이러한 이진 탐색 트리의 성질을 이용해 이진 트리 알고리즘을 쉽게 사용할 수 있습니다.

```cpp
#include <stdio.h>
#include <time.h>
 
#define MAX_ELEMENTS 10000000L
int list[MAX_ELEMENTS];
 
// 순환 호출 방법 
int search_binary(int key, int low, int high) {
	int middle;
 
	if (low <= high) {
		
		middle = (low + high) / 2;
		if (key == list[middle]) // 탐색 성공
			return middle;
		else if (key < list[middle]) // 왼쪽 부분 리스트 탐색
			return search_binary(key, low, middle - 1);
		else // 오른쪽 부분 리스트 탐색
			return search_binary(key, middle + 1, high);
	}
	return -1; // 탐색 실패
}
 
// 반복적인 방법 
int search_binary2(int key, int low, int high)
{
	int middle;
 
	while (low <= high) {       // 아직 숫자들이 남아 있으면
		middle = (low + high) / 2;
		if (key == list[middle])
			return middle;
		else if (key > list[middle])
			low = middle + 1;
		else
			high = middle - 1;
	}
	return -1;   // 발견되지 않음
}
```

## 해시 탐색

<aside>
💡 값과 index를 미리 연결해둠으로써 짧은 시간에 탐색할 수 있는 알고리즘

</aside>

- 함수를 사용하여 데이터를 보관한 후에 보관하는데 사용된 함수를 사용하여 한 번에 데이터를 탐색
    - 해시 함수 : 어떤 값이 주어진 경우, 그 값을 대표하는 숫자를 계산하는 함수
    - 해시값 : 해시 함수의 계산으로 산출된 값
    - 해시 : 원래의 숫자를 모양이 변할 정도로 가공하여, 전혀 다른 값을 생성한다
- 저장과 검색 알고리즘이 필요
- 해시값 충돌 : 분류 저장하는 작업에서 해시 함수에 의한 해시 값이 겹치는 경우
    - 해시값 충돌이 자주 일어날 수록 질이 안 좋다고 판단된다.
    - 일반적으로 입력의 크기에 2배로 해시 값 배열을 준비하지만, 다른 입력에 대해서 해시 값이 겹치는 경우, 배열의 다음 인덱스들을 훑으면서 비어있는 인덱스에 저장해야함
    - 해시 값에 해당하는 요소들을 연결 리스트로 만들어두는 방법도 있다. 이 글에서는 인덱스를 증가시키면서 비어있는 인덱스에 저장하는 방법을 사용
        - 번지 계열 : 해시 충돌 발생시 조사하게 되는 대체 칸
        - 클러스터 : 번지 계열이 겹쳐서 충돌이 일어나는 경우
    - 요소를 탐색할 때도 처음에는 해시값을 이용해서 빠르게 요소를 탐색할 수 있지만, 해시값 충돌로 인해 다른 인덱스에 저장되어있을 수 있기 때문에 원하는 값과 일치하지 않는다면 결국 찾던 인덱스로부터 순차 탐색 해보아야한다. 따라서 해시 탐색은 해시 함수의 질이 좋을 때만 효율적으로 작동한다.
    
    [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdmiCxD%2FbtqHvJw4JpL%2FGLNrros3rV3PKuTlVbKjnk%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdmiCxD%2FbtqHvJw4JpL%2FGLNrros3rV3PKuTlVbKjnk%2Fimg.png)
    

### 시간 복잡도

- 상수의 시간복잡도
- 함수의 질이 좋지 않다면, 더 복잡해짐
- 기존 입력의 2배 이상의 배열이 필요하므로 공간 복잡도 측면에서 좋지 않음

[](https://serzhul.io/algorithm/탐색-알고리즘(searching-algorithms)/)

[[알고리즘] 탐색 알고리즘 (선형, 이분, 해시, BST)](https://bba-dda.tistory.com/21)
