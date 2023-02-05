## 메모리

### 메인 메모리

CPU가 직접 접근할 수 있는 기억 장치로, 프로세스가 실행되려면 프로그램 코드를 메인 메모리에 적재해 두어야 한다. 

그런데, 만약 프로그램 용량이 메인 메모리보다 크면 어떤 일이 벌어질까?

### 가상 메모리

실제 물리 메모리 개념과 사용자의 논리 메모리 개념을 분리한 것이다. 메모리의 공간은 한정적이므로 사용자에게 더 많은 메모리를 제공하기 위해 가상 주소를 사용한다. 메모리 관리 장치는 가상 주소를 이용해 실제 데이터가 담겨 있는 주소로 변환해 준다.

여기서 가상 주소 공간은 하나의 프로세스가 메모리에 저장되는 논리적인 모습을 가상 메모리에 구현한 공간이며, 가상 주소는 해당 공간을 가리키는 주소이다.

**필요성**

- 물리 메모리의 한계
    - 모든 프로그램 코드를 물리 메모리에 올릴 수가 없다. 그렇다고, 프로그램을 교체하면서 올리면 메모리 교체 성능 문제가 발생한다.
- 가상 메모리의 장점
    - 프로그램 용량이 실제 물리 메모리보다 커도 된다.
    - 전체 프로그램이 물리 메모리에 올라와 있지 않아도 된다.
    - 더 많은 프로그램을 동시에 실행할 수 있다. (응답 시간은 유지. CPU 이용률과 처리율은 증가)
    - 즉 다중 프로그래밍을 실현하기 위해 물리 메모리의 제약을 보완하고 프로세스 전체를 메모리에 올리지 않고도 실행할 수 있도록 해 준다.

**구현**

운영 체제는 물리 메모리의 제약을 갖고 있는 주 기억 장치를 보조하기 위해 디스크를 보조 기억 장치 (Paging Space)로 사용한다.

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fr3Uq3%2FbtrmLA0TKZj%2FXz5IKdc2II7W8RkSpBXtB1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fr3Uq3%2FbtrmLA0TKZj%2FXz5IKdc2II7W8RkSpBXtB1%2Fimg.png)

즉, 메인 메모리 (주 기억 장치)와 디스크의 페이징 스페이스 (보조 기억 장치)를 묶어 하나의 메모리처럼 동작하게 하며, 이를 통해 메인 메모리의 한계를 넘는 메모리 사용을 가능하게 하는 가상 메모리를 구현한다.

- 스와핑 (swapping)
    
    CPU 할당 시간이 끝난 프로세스의 메모리를 보조 기억 장치를 내보내고 (swap-out) 다른 프로세스의 메모리를 불러오는 (swap-in) 작업을 Swap이라고 한다. 이러한 Swap 작업에는 디스크 전송 시간이 들기 때문에 메모리 공간이 부족할 때 Swapping이 이루어 진다.
    

## 메모리 관리

다중 프로그래밍 시스템에 여러 프로세스를 수용하기 위해 주 기억 장치 (RAM)을 동적 분할하는 메모리 관리 작업이 필요하다. 즉, 하드 디스크에 있는 프로그램을 어떻게 메인 메모리에 적재할 것인지 판단해야 한다.

### 연속 메모리 관리

프로그램 전체가 하나의 커다란 공간에 연속적으로 할당되어야 한다.

- 고정 분할 기법 : 주 기억 장치가 고정된 파티션으로 분할 → 내부 단편화 발생
- 동적 분할 기법 : 파티션들이 동적 생성되며 자신의 크기와 같은 파티션에 적재 → 외부 단편화 발생

이렇게 연속 메모리 관리 기법을 사용할 경우, 단편화 현상이 발생한다.

**단편화**

기억 장치의 빈 공간 또는 자료가 여러 조각으로 나뉘는 현상이다. 프로세스들이 메모리에 적재되고 제거되는 일이 반복되면, 프로세스들이 차지하는 메모리 틈 사이에 사용하지 못할 만큼의 자유 공간이 늘어나게 된다. 이러한 단편화는 2가지로 나뉜다.

- 내부 단편화
    - 프로세스가 사용하는 메모리 공간에 남는 부분
    - 프로세스가 요청한 양보다 더 많은 메모리를 할당할 때 발생하며, 메모리 분할 자유 공간과 프로세스가 사용하는 공간의 크기 차이를 의미한다.
    
    [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb2GvRX%2FbtrmSxVFvgJ%2Fxp6TfIJteQy8N790KJFyj1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb2GvRX%2FbtrmSxVFvgJ%2Fxp6TfIJteQy8N790KJFyj1%2Fimg.png)
    
- 외부 단편화
    - 메모리 공간 중 사용하지 못하게 되는 부분
    - 메모리 할당 및 해제 작업의 반복으로 작은 메모리가 중간 중간 존재할 수 있다. 이렇게 사용하지 않는 메모리가 존재해서 총 메모리 공간은 충분하지만 실제로 할당할 수 없는 상황이다.
    - 외부 단편화를 해결하기 위해 압축을 이용하여 프로세스가 사용하는 공간을 한쪽으로 몰 수 있지만, 작업 효율이 좋지는 않다.
    
    [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdN6x55%2FbtrmQ3VieHq%2FDp3nzzkzPYqWxSXpZLDTV0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdN6x55%2FbtrmQ3VieHq%2FDp3nzzkzPYqWxSXpZLDTV0%2Fimg.png)
    

### 불연속 메모리 관리

프로그램의 일부가 서로 다른 주소 공간에 할당될 수 있는 기법이다. 앞서 봤던 단편화 문제를 해결하기 위해 제시된 기법으로, 외부 단편화 해소를 위한 페이징과 내부 단편화 해소를 위한 세그멘테이션으로 나뉜다.

## 페이징

프로세스를 일정한 크기의 페이지로 분할해서 메모리에 적재하는 방식이다.

- 페이지 : 고정 사이즈의 가상 메모리 내 프로세스 조각
- 프레임 : 페이지 크기와 같은 주 기억 장치의 메모리 조각

하나의 프로세스가 사용하는 메모리 공간이 연속적이어야 한다는 제약을 없애는 메모리 관리 방법이다.

### 페이징 테이블

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbisAIZ%2FbtrmMggPQSL%2FngNzgHoxMnwSU5boElqrA0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbisAIZ%2FbtrmMggPQSL%2FngNzgHoxMnwSU5boElqrA0%2Fimg.png)

위 사진은 페이지 테이블의 매핑을 나타낸다. 앞서 봤듯이, 물리 메모리는 고정 크기의 프레임으로, 가상 메모리는 고정 크기의 페이지로 분리되어 있다. 개별 페이지는 순서에 상관 없이 물리 메모리에 있는 프레임에 매핑되어 저장된다.

즉, 모든 프로세스는 하나의 페이징 테이블을 가지고 있으며, 여기에는 메인 메모리에 적재되어 있는 페이지 번호와 해당 페이지가 위치한 메인 메모리의 시작 주소가 있다. 이를 통해 하나의 프로세스를 나눈 가상 메모리 페이지들이 각각 실제 메인 메모리의 어디 프레임에 적재되어 있는지 알아낼 수 있다.

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsntgC%2FbtrmRiq813s%2F5JpzqIPO4melxoEkgBrczK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsntgC%2FbtrmRiq813s%2F5JpzqIPO4melxoEkgBrczK%2Fimg.png)

위 PMT (Page Mapping Table, 페이징 테이블)에서는 P1 프로세스의 0번째 페이지가 메인 메모리의 5번째 프레임에 있는 것을 알 수 있다.

### 논리주소와 페이지테이블

앞서 메모리 관리 장치 (MMU, Memory Management Unit)는 가상 주소 (논리 주소)를 이용해 실제 데이터가 담겨 있는 주소로 변환해준다고 하였다.

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcFvXra%2FbtrmQ3A1UO8%2FtzbK62Q2uBQgxnui3wc1KK%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcFvXra%2FbtrmQ3A1UO8%2FtzbK62Q2uBQgxnui3wc1KK%2Fimg.png)

논리 주소 (Logical Address)는 <page, offset>과 같은 형태로 구성되는데, 이를 이용해 물리 주소로 변환해 주는 것이다.

[https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpCyca%2FbtrmKBsv1c2%2FuenUi303boQMKSwN0jV1C1%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FpCyca%2FbtrmKBsv1c2%2FuenUi303boQMKSwN0jV1C1%2Fimg.png)

가상 주소를 물리 주소로 변환하는 전체 과정이다.

### 장단점

- 장점
    - 논리 메모리는 물리 메모리에 저장될 때 연속되어 저장될 필요가 없고, 물리 메모리의 남는 프레임에 적절히 배치되기 때문에 외부 단편화가 생기지 않는다.
- 단점
    - 내부 단편화 문제가 발생할 수 있다. 페이지 단위를 작게하면 해결할 수 있지만, 페이지 매핑 과정이 복잡해져 오히려 비효율적이다.
    
    [https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbfJMnj%2FbtrmLByKsGA%2FOwYaGZe36XOm25M49iRrz0%2Fimg.png](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbfJMnj%2FbtrmLByKsGA%2FOwYaGZe36XOm25M49iRrz0%2Fimg.png)
    

## 세그먼테이션

세그먼트는 가상 메모리를 서로 크기가 다른 논리적 단위로 분할한 것을 의미한다. 세그멘테이션은 프로세스를 물리적 단위인 페이지가 아닌 논리적 단위인 세그먼트로 분할해서 메모리에 적재하는 방식이다.

돼지를 도축할 때, 페이징은 돼지를 같은 크기로 잘라서 보관하는 것이라면 세그멘테이션은 부위 별로 잘라서 보관한다고 이해하면 된다. 이렇듯 세그먼트는 의미가 같지 않는 논리적 내용을 기준으로 프로그램을 분할하기 때문에 크기가 같지 않다.

### 세그먼트 테이블

분할 방식을 제외하면, 페이징과 세그멘테이션이 동일하기 때문에 매핑 테이블의 동작 방식도 동일하다. 다만, 논리 주소의 앞 비트들은 페이징 번호가 아니라 세그먼트 번호가 될 것이다. 즉, <segment, offser> 형태로 구성되며, 세그먼트 번호를 통해 세그먼트의 기준 (세그먼트의 시작 물리 주소)와 한계 (세그먼트의 길이)를 파악할 수 있다.

### 장단점

- 장점
    - 내부 단편화 문제가 해소된다.
    - 보호와 공유 기능을 수행할 수 있다. 프로그램의 중요한 부분과 중요하지 않은 부분을 분리하여 저장할 수 있고, 같은 코드 영역은 한 번에 저장할 수 있다.
- 단점
    - 외부 단편화 문제가 생길 수 있다.

## ~~페이징 vs  세그먼테이션~~

## Q

- 페이징 또는 세그멘테이션을 사용하는 이유는?
    
    프로그램을 실행하기 위해 코드를 디스크에서 메인 메모리로 적재하는 과정에서 단편화가 생길 수 밖에 없다. 이렇게 단편화가 많이 발생하면 사용하지 못하는 메모리 공간이 많아져 낭비가 되므로 최대한 피해야 한다. 최초 적합, 최적 적합, 압축 등의 방식을 통해 단편화를 해결할 수도 있지만, 메모리 계산의 비용이 적은 페이징 또는 세그멘테이션을 주로 사용한다.
    
- 페이징의 특징은?
    
    페이징은 프로그램을 실행할 때, 코드를 메모리에 적재하기 위해 사용하는 기법이며, 불연속 메모리 관리 기법이라는 특징이 있다. 다시 말해 프로그램 전체가 메모리에 연속적으로 올라가 있는 것이 아니라 페이지라는 고정된 크기로 분할되어 올라가 있다. 또한, 페이징은 외부 단편화 문제를 해결할 수 있다.
    
- 페이징과 세그멘테이션의 차이는?
    
    페이징과 세그멘테이션 모두 프로그램을 실행하기 위해 디스크에 있는 내용을 분할하여 메모리에 적재하는 불연속 메모리 관리 기법이다. 둘의 차이는 프로그램을 분할하는 방식이다. 페이징의 경우 프로그램을 같은 크기의 페이지로 분할하는 데에 비해, 세그멘테이션은 논리적 의미를 기준으로 세그먼트를 분할한다.
    

## 참고

[https://bellog.tistory.com/m/158](https://bellog.tistory.com/m/158)

https://steady-coding.tistory.com/524
