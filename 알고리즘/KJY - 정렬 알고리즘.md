정렬 알고리즘은 컴퓨터 공학에서 중요시되는 문제 중 하나로, 어떤 데이터셋이 주어졌을 때 이를 정해진 순서대로 나열하여 재배치하는 문제이다.

어떤 데이터가 정렬되어 있지않다면 순차적으로 하나씩 데이터를 봐가면서 탐색해야하지만, 데이터가 이미 정렬되어 있다면 이진탐색과 같은 강력한 알고리즘을 사용할 수 있기에 정렬은 중요하다.

대표적인 정렬 알고리즘으로 선택정렬, 삽입정렬, 병합정렬, 퀵정렬, 버블정렬이 있다.

## 선택 정렬 Selection Sort

현재 위치에 들어갈 값을 찾아 정렬하는 배열

현재 위치에 저장될 값의 크기가 작냐, 크냐에 따라 최소 선택 정렬(Min-Selection Sort)와 최대 선택 정렬(Max-Selection Sort)로 구분할 수 있다.

최소 선택 정렬은 오름차순으로 정렬되고, 최대 선택 정렬은 내림차순으로 정렬된다.

### 로직

1. 정렬되지 않은 인덱스의 맨 앞에서부터, 이를 포함한 그 이후의 배열값 중 가장 작은 값을 찾아간다.
2. 가장 작은 값을 찾으면, 그 값을 현재 인덱스의 값과 바꿔준다.
3. 다음 인덱스에서 위 과정을 반복해준다.

![https://blog.kakaocdn.net/dn/blSBFD/btqPem0zr5m/hh4Q5i81KoUXHUwC8TyKik/img.gif](https://blog.kakaocdn.net/dn/blSBFD/btqPem0zr5m/hh4Q5i81KoUXHUwC8TyKik/img.gif)

### 시간/공간 복잡도

선택 정렬 알고리즘은 n-1개, n-2개, n-3개, …, 1개씩 비교를 반복한다.

따라서 배열이 어떻게 되어있던지간에 전체 비교를 진행하므로 시간복잡도는 **O(n^2)**이다.

공간복잡도는 단 하나의 배열에서만 진행하므로 O(1)이다.

### 코드

```python

```

```cpp
//C++
void selectionSort(vector<int> v){
	for(int i = 0; i < v.size()-1; i++)
		int tmp = i;
		for(int j = i+1; j < v.size(); j++){
			if(v[tmp] >= v[j]) 
				tmp = j;
		}
		swap(v[i], v[tmp]);
	}
}
```

```java

```

## 삽입 정렬

현재 위치에서, 그 이하의 배열들을 비교하여 자신이 들어갈 위치를 찾아, 그 위치에 삽입하는 배열 알고리즘

### 로직

1. 두 번재 인덱스부터 시작해서 현재 인덱스는 별도의 변수에 저장해주고, 비교 인덱스를 현재 인덱스-1로 잡는다.
2. 별도로 저장해 둔 삽입을 위한 변수와, 비교 인덱스의 배열 값을 비교한다.
3. 삽입 변수의 값이 더 작으면 현재 인덱스로 비교 인덱스의 값을 저장해주고, 비교 인덱스를 -1하여 비교를 반복한다.
4. 만약 삽입 변수가 더 크면, 비교 인덱스 +1에 삽입 변수를 저장한다.

![https://blog.kakaocdn.net/dn/begzyV/btqO71XwCp9/anVaLURwpwx2Qms15QHRS1/img.gif](https://blog.kakaocdn.net/dn/begzyV/btqO71XwCp9/anVaLURwpwx2Qms15QHRS1/img.gif)

### 시간/공간 복잡도

최악의 경우엔 n-1개, n-2개, n-3개, …, 1개씩 비교를 반복하여 시간복잡도는 O(n^2)이고, 이미 정렬되어 있는 경우에는 한번씩 밖에 비교를 하지 않아 시간 복잡도는 O(n)이다.

하지만 상한을 기준으로 하는 Big-O notation은 최악의 경우를 기준으로 평가하므로 삽입 정렬의 시간복잡도는 **O(n^2)**이다.

공간 복잡도는 단 하나의 배열에서만 진행하므로 O(1)이다.

### 코드

```python

```

```cpp
//C++
void insertionSort(vector<int> v){
	for(int i = 1; i < v.size()-1; i++)
		int key = v[i], j = i-1;
		while(j >= 0 && key < v[j]){
			swap(v[j], v[j+1]);
		}
		v[j+1] = key;
	}
}
```

```java

```

## 합병/병합 정렬 Merge Sort

두 개의 배열을 비교하여, 기준에 맞는 값을 다른 배열에 저장해 나간다.

오름차순의 경우 배열A, 배열B를 비교하여 A에 있는 값이 더 작다면 새 배열에 저장해주고, A인덱스를 증가시킨 후 A,B의 반복을 진행한다. 이를 A나 B 중 하나가 모든 배열값들을 새 배열에 저장할 때까지 반복하며, 전부 다 저장하지 못한 배열의 값들은 모두 새 배열의 값에 저장해준다.

큰 문제를 반으로 쪼개 배열의 크기가 1보다 작거나 같을 때까지 분할을 반복하는 분할 정복 방식으로 설계되었다.

### 로직

**********************분할과정**********************

1. 현재 배열을 반으로 쪼갠다. 배열의 시작 위치와, 종료 위치를 입력받아 둘을 더한 후 2를 나눠 그 위치를 기준으로 나눈다.
2. 이를 쪼갠 배열의 크기가 0이거나 1일 때까지 반복한다.

**********************합병과정**********************

1. 두 배열 A, B의 크기를 비교한다. 각각의 배열의 현재 인덱스를 i, j로 가정한다.
2. i에는 A배열의 시작 인덱스를 저장하고, j에는 B배열의 시작 주소를 저장한다.
3. A[i]와 B[j]를 비교한다. 오름차순의 경우 이중에 작은 값을 새 배열 C에 저장한다. A[i]가 더 컸다면 A[i]의 값을 배열 C에 저장해주고, i의 값을 하나 증가시켜준다.
4. 이를 i나 j 둘 중 하나가 각자 배열의 끝에 도달할 때까지 반복한다.
5. 끝까지 저장을 못한 배열의 값을, 순서대로 전부 다 C에 저장한다.
6. C 배열을 원래의 배열에 저장해준다.

![https://blog.kakaocdn.net/dn/bA5bcq/btqO6gHzdBG/lx43EvHWXDaKBrhjz4zVa0/img.gif](https://blog.kakaocdn.net/dn/bA5bcq/btqO6gHzdBG/lx43EvHWXDaKBrhjz4zVa0/img.gif)

### 시간/공간 복잡도

분할 과정은 크기가 N인 배열을 분할하면, 한 번 분할하면, N/2, N/2 2개, 그다음 분할하면 N/4, N/4, N/4, N/4 4개로 분할되므로 총 O(lgN)만큼 일어난다.

합병 과정은 두 배열 A, B를 정렬하기 때문에, A배열의 크기를 N1, B배열의 크기를 N2라고 할 경우 O(n1 + n2)와 같다. 배열 A와 배열 B는 하나의 배열을 나눈 배열들이기 때문에 전체 배열의 길이가 N이라고 할 경우 N = N1 + N2이므로 O(N)이다.

따라서, 각 분할별로 합병을 진행하므로, 합병정렬의 시간 복잡도는 **O(NlogN)**이다.

사용하는 공간은 정렬을 위한 배열을 하나 더 생성하므로 **O(N)**이다.

### 코드

```python

```

```cpp
//C++
//합병
void merge(vector<int>& v, int s, int e, int m){
	vector<int> ret;
	int i = s, j = m+1, copy = 0;
	
//결과를 저장할 배열에 하나씩 비교하여 저장한다.
	while(i <= m && j <= e){
		if(v[i] < v[j])
			ret.push_back(v[i++]);
		else if(v[i] > v[j])
			ret.push_back(v[j++]);
	}

//남은 값들을 뒤에 채워넣어준다.
	while(i <= m) ret.push_back(v[i++]);
	while(j <= e) ret.push_back(v[j++]);

//원래 배열에 복사해준다.
	for(int k = s; k <= e; k++){
		v[k] = ret[copy++];
	}
}

//합병 정렬
void mergeSort(vector<int>& v, int s, int e){
	if(s < e){
		int m = (s+e) / 2;
		
		//분할
		mergeSort(v, s, m);     //s부터 m까지
		mergeSort(v, m+1, e);   //m+1부터 e까지
		//합병
		merge(v, s, e, m);
	}
}
```

```java

```

## 퀵 정렬

pivot point라고 기준이 되는 값을 하나 설정하여 이 값을 기준으로 작은 값은 왼쪽, 큰 값은 오른쪽으로 옮기는 방식으로 정렬을 진행한다. 이를 반복하여 분할된 배열의 크기가 1이 되면 배열이 모두 정렬된다.

퀵정렬도 분할 정복을 이용하여 정렬을 수행하는 알고리즘이다.

### 로직

1. pivot point로 잡을 배열의 값 하나를 정한다. 보통 맨 앞이나 맨 뒤, 혹은 전체 배열 값 중 중간값이나 랜덤 값으로 정한다.
2. 분할을 진행하기에 앞서, 비교를 진행하기 위해 가장 왼쪽 배열의 인덱스를 저장하는 left변수, 가장 오른쪽 배열의 인덱스를 저장한 right변수를 생성한다.
3. right부터 비교를 진행한다. 비교는 right가 left보다 클 때만 반복하며, 비교한 배열값이 pivot point보다 크면 right를 하나 감소시키고 비교를 반복한다. pivot point보다 작은 배열 값을 찾으면, 반복을 중지한다.
4. 그 다음 left부터 비교를 진행한다. 비교는 right가 left보다 클 때만 반복하며, 비교한 배열값이 pivot point보다 작으면 left를 하나 증가시키고 비교를 반복한다. pivot point보다 큰 배열 값을 찾으면, 반복을 중지한다.
5. left 인덱스의 값과 right 인덱스의 값을 바꿔준다.
6. 3, 4, 5 과정을 left < right가 만족할 때까지 반복한다.
7. 위 과정이 모두 끝나면 left의 값과 pivot point를 바꿔준다.
8. 맨 왼쪽부터 left-1까지, left+1부터 맨 오른쪽까지 나눠 퀵 정렬을 반복한다.

![https://blog.kakaocdn.net/dn/sVgeg/btqO6hT6nVq/APxheX0siKi3SJRHxvx3n0/img.gif](https://blog.kakaocdn.net/dn/sVgeg/btqO6hT6nVq/APxheX0siKi3SJRHxvx3n0/img.gif)

### 시간/공간 복잡도

각 정렬은 배열의 크기 N만큼 비교하며, 이를 총 분할 깊이인 lgN만큼 진행하므로 총 비교횟수는 NlgN, 즉 시간 복잡도는 O(NlgN)이다.

다만 퀵 정렬에는 최악의 경우가 존재하는데, 이는 배열이 이미 정렬이 되어있는 경우이다. 이 경우 분할이 N만큼 일어나므로 시간 복잡도는 O(N^2)이다.

이를 방지하기 위해 앞서 언급했던 전체 배열 값 중 중간값이나 랜덤 값으로 pivot point를 정하는 방법을 쓰기도 한다.

최악의 경우 때문에 합병 정렬보다 느린 알고리즘이라 생각하기 쉽지만, 발생하기 쉽지 않은 경우이고, 일반적으로 퀵정렬이 합병정렬보다 20%이상 빠르다고 한다.

공간 복잡도는 O(lgN)이다.

### 코드

```python

```

```cpp
//C++
//합병
void merge(vector<int>& v, int s, int e, int m){
	int pivot = v[s];
	int bs = s, be = e;
	while(s < e){
		while(pivot <= v[e] && s < e) 
			e--;
		if(s > e) break;
		while(pivot >= v[s] && s < e) 
			s++;
		if(s > e) break;
		std::swap(v[s], v[e]);
	}
	std::swap(v[bs], v[s]);
	if(bs < s)
		quickSort(v, bs, s-1);
	if(be > e)
		quickSort(v, s+1, be);
}
```

```java

```

## 버블 정렬 Bubble Sort

매번 연속된 두개 인덱스를 비교하여, 정한 기준의 값을 뒤로 넘겨 정렬하는 알고리즘

오름차순으로 정렬하고자 할 경우, 비교시마다 큰 값이 뒤로 이동하여, 1바퀴 돌 시 가장 큰 값이 맨 뒤에 저장된다. 맨 마지막에는 비교하는 수들 중 가장 큰값이 저장되기 때문에, (전체 배열의 크기 - 현재까지 순환한 바퀴 수)만큼만 반복해 주면 된다.

### 로직

1. 두번 째 인덱스부터 시작해서 현재 인덱스 값과, 바로 이전의 인덱스 값을 비교한다.
2. 만약 이전 인덱스가 더 크면, 현재 인덱스와 바꿔준다.
3. 현재 인덱스가 더 크면, 교환하지 않고 다음 두 연속된 배열값을 비교한다.
4. 이를 (전체 배열의 크기 - 현재까지 순환한 바퀴 수)만큼 반복한다.

![https://blog.kakaocdn.net/dn/kTpcI/btqO13hKM3O/hsZY59VnJYPiQVKikxw4N0/img.gif](https://blog.kakaocdn.net/dn/kTpcI/btqO13hKM3O/hsZY59VnJYPiQVKikxw4N0/img.gif)

### 시간/공간 복잡도

1부터 비교를 시작하여, n-1개, n-2개, n-3개, …, 1개씩 비교를 반복하며, 선택 정렬과 같이 배열이 어떻게 되어있던지간에 전체 비교를 진행하므로 시간복잡도는 **O(n^2)**이다.

공간 복잡도는 단 하나의 배열에서만 진행하므로 O(1)이다.

### 코드

```python

```

```cpp
//C++
void bubbleSort(vector<int> v){
	for(int i = 1; i < v.size()-1; i++)
		for(int j = 1; j < v.size()-i; j++){
			if(v[j-1] > v[j])
				swap(v[j-1], v[j]);
}
```
