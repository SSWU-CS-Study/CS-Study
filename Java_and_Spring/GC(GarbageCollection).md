# GC(Garbage Collection)

## 예상 면접 질문

- 가비지 컬렉션이란?
    
    → 메모리 관리를 위해 더 이상 사용되지 않는 객체를 삭제하는 것
    
- 가비지 컬렉션이 실행되는 시점은?
    
    → Heap 영역 내 특정 메모리 공간이 다 찼을 때 실행된다.
    
- Stop-the-world란?
    
    → Garbage Collection을 실행하기 위해 애플리케이션 실행을 멈추는 것을 말한다.
    

## GC(Garbage Collection)

JVM의 동적으로 할당되는 메모리 영역 `Heap` 에서 필요 없게 된 메모리 영역을 주기적으로 삭제하는 프로세스를 말한다.
* Heap 구조
<img width="510" alt="Screen Shot 2022-10-09 at 4 40 17 AM" src="https://user-images.githubusercontent.com/68562176/194755489-3d97ba94-bd47-46e0-8a11-6dd28c9610e3.png">    

### [장점]

메모리 관리, 메모리 누수 문제를 개발자가 관리하지 않아도 돼서 오롯이 개발에만 집중할 수 있다.

### [단점]

1. 가비지 컬렉션이 수행되는 정확한 시점을 알 수 없다.
2. 가비지 컬렉션이 동작하는 동안에는 다른 동작을 멈추기 때문에 오버헤드가 발생한다.

## Stop-the-world

- Garbage Collection을 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것을 말한다.
- GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다.
- 어떤 GC 알고리즘을 사용하더라도 Stop-the-world는 발생한다.
<details>
<summary>Garbage Collection 알고리즘</summary>
<div markdown="1">
    1. Serial GC<br>
    2. Parallel GC<br>
    3. Parallel Old GC<br>
    4. CMS(Concurrent Mark Sweep) GC<br>
    5. G1(Garbage First) GC<br>
</div>
</details>

## GC 동작 과정
https://user-images.githubusercontent.com/68562176/194756730-a6fcce07-2d7c-45fb-9b2e-c4199a691aee.mov

### 1. Marking
Root 객체에서부터 시작하여 참조되고 있는 객체와 그렇지 않은 객체를 구분한다.
<img width="510" alt="Screen Shot 2022-10-09 at 1 26 13 AM" src="https://user-images.githubusercontent.com/68562176/194755821-ff18cd7c-6290-43d5-a5e1-4cc9d0fec9d8.png">.   

<details>
<summary>Marking 과정 상세</summary>
<div markdown="1">
<img width="510" alt="Screen Shot 2022-10-08 at 10 20 05 PM" src="https://user-images.githubusercontent.com/68562176/194755944-ac023e5f-2e77-4421-8d8f-0b670ad081c6.png">     
 
객체들을 실질적으로 Heap 영역에서 생성되고 Method Area 나 Stack Area 등 Root Area에서는 생성된 객체의 주소만 참조한다.
```        
- GC Root가 될 수 있는 것들
    1. 실행중인 쓰레드 (Active Thread)
    2. 정적 변수 (Static Variable)
    3. 로컬 변수 (Local Variable)
``` 
메서드 종료와 같은 이벤트 후에 객체를 참조하고 있는 변수가 사라진다면, 해당 객체는 Unreachable한 상태이므로, 가비지 컬렉션의 대상이 된다.
```
- Reachable: 객체가 참조되고 있는 상태
- Unreachable: 객체가 참조되고 있지 않은 상태
```
</div>
</details>

### 2. Deletion(= Sweep)
참조되고 있지 않는 상태의 객체의 메모리를 해제한다(객체를 삭제한다.)
<img width="426" alt="Screen Shot 2022-10-09 at 1 26 35 AM" src="https://user-images.githubusercontent.com/68562176/194756205-7ef6ba80-bb6e-498e-8026-7db6b70237df.png">

### 3. Deletion with Compacting
성능 향상을 위해 삭제(Deletion)후 압축(Compacting) 과정을 추가로 진행할 수 있다. 삭제 후 남아있는 객체를 압축시킨다. 
→ 그렇게 되면, 새로 메모리를 할당할 때, 더 쉽고 빠르게 진행 가능하다.        
<img width="404" alt="Screen Shot 2022-10-09 at 1 29 02 AM" src="https://user-images.githubusercontent.com/68562176/194756217-b359058f-6478-48d6-9b52-4e68a2999195.png">


## Minor GC vs Major GC
<img width="501" alt="Screen Shot 2022-10-09 at 4 40 17 AM" src="https://user-images.githubusercontent.com/68562176/194756270-cb0e8cf5-f516-449a-9ad1-43faa574fa77.png">

- Minor GC: Young Generation 안에서 일어나는 GC
- Major GC: Tenured Generation 안에서 일어나는 GC
    

## Permanent Generation

- 클래스와 메서드를 사용하기 위해 JVM으로부터 요구되는 메타데이터가 포함되어 있다.
- 응용 프로그램에서 사용하는 클래스를 기반으로 런타임에 JVM에 의해 채워진다.
- Java SE 라이브러리 클래스 및 메서드가 여기에 저장될 수 있다.

## 참고

- GC > Heap 구조
    
    [[Java] 자바 JVM 내부 구조와 메모리 구조에 대하여](https://coding-factory.tistory.com/828)

    
- GC > [장점], [단점], 사진
    
    [[Java] 가비지 컬렉션(GC, Garbage Collection) 총정리](https://coding-factory.tistory.com/829)
    
- GC 동작 과정 > 1. Mark, 2. Deletion, 3. Deletion with Compacting
    [Java Garbage Collection Basics](https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html)