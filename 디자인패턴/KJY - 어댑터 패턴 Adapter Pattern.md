<aside>
💡 이미 제공되어 있는 것, 필요한 것 사이의 간격을 메우는 디자인 패턴

</aside>

- 두 가지 종류의 Adapter 패턴
    - 상속(inheritance)을 이용한 Adapter 패턴
    - 위임(delegation)을 이용한 Adapter 패턴

## UML

### 상속 UML

![Adapter-inheritance.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03d9639b-bc92-4cfd-bff3-30206c38f1fc/Adapter-inheritance.png)

### 위임UML

![Adapter-de.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71ab08ef-1068-49c3-b730-6d70c4015904/Adapter-de.png)

### 시퀀스 다이어그램

## 역할

### Target(대상)

- 원소를 하나씩 끄집어낼 때 사용할 공통된 메소드를 선언한 인터페이스
- hasNext()와 next() 메소드 선언
- Print Interface/Class

### Client(의뢰자)

- Iterator 인터페이스를 실제로 구현하는 클래스
- Main Class

### Adaptee(적용 대상자)

- Iterator를 만들어내는 인터페이스를 제공
- iterator() : 내가 가지고 있는 각 원소들을 차례로 검색해 줄 사람을 만들어내는 메소드
- Baner Class

### Adpater(적용자)

- Aggregate 인터페이슬 구현하는 클래스
- PrintBanner Class

## 심층 이해

### 어떤 경우에 사용하는가?

- 기존의 클래스를 수정하지 않고도 필요한 클래스로 만들 때 사용
- 기존의 클래스가 충분히 테스트 되어 있을 때 더욱 좋음

### 비록 소스가 없더라도

- 기존의 클래스를 수정하지 않고, 새로운 인터페이스에 기존의 클래스를 적용 가능
- 기존 클래스의 소스 코드 없이, 메소드의 프로타입만 알면 어댑터 패턴 적용 가능
- API가 다른 두 개의 사이에 들어가 그 사이를 메우는 Adpater패턴

### 상속 VS 위임

- 위임을 사용하는 것이 문제가 적음
    - 상위 클래스의 내부 동작을 자세히 모르면, 상속을 효과적으로 사용하기 어려운 경우가 많음

## 예제

### 상속

Banner 클래스라는 기존의 클래스를 이용해서, Print 인터페이스를 충족시키는 클래스를 만들자 → Banner 클래스의 메소드를 재활용하는 Adapter 클래스(PrintBanner 클래스)를 생성

### Banner Class

- 두 메소드를 이미 제공되어 있는 것으로 가정
- showWithParen()
- showWithAster()

```java
public class Banner{
	private String string;

	public Banner(String string){
		this.string = string;
	}

	public void showWithParen(){
		System.out.println("(" + string + ")");
	}

	public void showWithAster(){
		System.out.println("*" + string + "*");
	}
}
```

### Print Interface

- 두 메소드를 필요한 것으로 가정
- printWeak()
- printStrong()

```java
public Interface Print{
	public abstract void printWeak();
	public abstract void printStrong();
}
```

### PrintBanner Class

- 
- 

```java
public class PrintBanner extends Banner implements Print{
	public PrintBanner(String string){
		super(string);
	}

	@Override
	public void printWeak(){
		showWintParen();
	}

	@Override
	public void printStrong(){
		showWithAster();
	}
}
```

### Main

- 
- 

```java
public class Main{
	public static void main(String[] args){
		Print p = new PrintBanner("Hello");
		p.printWeak();
		p.printStrong();
	}
}
```

```
(Hello)
*Hello*
```

### 위임

PrintBanner는 자신이 할 일을 Banner 클래스의 인스턴스에게 맡김

### Print Class

- 
- 

```java
public abstract class Print{
	public abstract void printWeak();
	public abstract void printStrong();
}
```

### PrintBanner Class

- 
- 

```java
public class PrintBanner extends Print{
	private Banner banner;

	public PrintBanner(String string){
		this.banner = new Banner(string);
	}

	@Override
	public void printWeak(){
		banner.showWintParen();
	}

	@Override
	public void printStrong(){
		banner.showWithAster();
	}
}
```
