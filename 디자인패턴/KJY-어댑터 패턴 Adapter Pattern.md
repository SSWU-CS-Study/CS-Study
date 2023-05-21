<aside>
💡 이미 제공되어 있는 것, 필요한 것 사이의 간격을 메우는 디자인 패턴

</aside>

![https://refactoring.guru/images/patterns/content/adapter/adapter-comic-1-ko.png](https://refactoring.guru/images/patterns/content/adapter/adapter-comic-1-ko.png)

- 두 가지 종류의 Adapter 패턴
    - 상속(inheritance)을 이용한 Adapter 패턴
    - 위임(delegation)을 이용한 Adapter 패턴

## 문제점과 해결법

주식 시장 모니터링 앱을 만들고 있고, 이 앱은 여러 소스에서 주식 데이터를 XML 형식으로 다운로드한 후 사용자에게 보기 좋은 차트들과 다이어그램들을 표시한다고 상상해본다.

어느 시점에  타사의 스마트 분석 라이브러리를 통합하여  앱을 개선하기로 결정했다. 그런데 함정이 있다. 이 분석 라이브러리는 JSON 형식의 데이터로만 작동한다는 것이다.

![https://refactoring.guru/images/patterns/diagrams/adapter/problem-ko.png](https://refactoring.guru/images/patterns/diagrams/adapter/problem-ko.png)

 이 라이브러리를 XML과 작동하도록 변경할 수 있으나, 그러면 라이브러리에 의존하는 일부 기존 코드가 손상될 수 있다. 또 처음부터 타사의 라이브러리 소스 코드에 접근하는 것이 불가능하여 위의 해결 방식을 사용하지 못할 수도 있다.

이때 *어댑터*를 만들 수 있다. 어댑터는 한 객체의 인터페이스를 다른 객체가 이해할 수 있도록 변환하는 특별한 객체다.

어댑터는 변환의 복잡성을 숨기기 위하여 객체 중 하나를 래핑(포장)한다. 래핑된 객체는 어댑터를 인식하지도 못한다. ~~예를 들어 미터 및 킬로미터 단위로 작동하는 객체를 모든 데이터를 피트 및 마일과 같은 영국식 단위로 변환하는 어댑터로 래핑할 수 있다.~~

어댑터는 데이터를 다양한 형식으로 변환할 수 있을 뿐만 아니라 다른 인터페이스를 가진 객체들이 협업하는 데에도 도움을 줄 수 있으며, 대략 다음과 같이 작동한다:

1. 어댑터는 기존에 있던 객체 중 하나와 호환되는 인터페이스를 받습니다.
2. 이 인터페이스를 사용하면 기존 객체는 어댑터의 메서드들을 안전하게 호출할 수 있습니다.
3. 호출을 수신하면 어댑터는 이 요청을 두 번째 객체에 해당 객체가 예상하는 형식과 순서대로 전달합니다.

때로는 양방향으로 호출을 변환할 수 있는 양방향 어댑터를 만드는 것도 가능하다.

다시 주식 시장 앱을 살펴보면 형식이 호환되지 않는 문제를 해결하기 위해 코드와 직접 작동하는 분석 라이브러리의 모든 클래스에 대한 XML->JSON 변환 어댑터를 만든다. 그 후 이러한 어댑터들을 통해서만 해당 라이브러리와 통신하도록 코드를 조정한다. 어댑터는 호출을 받으면 들어오는 XML 데이터를 JSON 구조로 변환한 후 해당 호출을 래핑된 분석 객체의 적절한 메서드들에 전달한다.

## UML

### 상속 UML

![Adapter-inheritance.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/03d9639b-bc92-4cfd-bff3-30206c38f1fc/Adapter-inheritance.png)

### 위임UML

![Adapter-de.PNG](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/71ab08ef-1068-49c3-b730-6d70c4015904/Adapter-de.png)

### 시퀀스 다이어그램

## 역할

### Target(대상)

- 필요한 메서드를 제공하는 역할
- Print Interface/Class
- 직류 12볼트

### Client(의뢰자)

- Target 역할의 메서드를 이용하는 역할
- Main Class
- 12볼트로 동작하는 노트북

### Adaptee(적용 대상자)

- 이미 준비되어 있는 메서드를 제공하는 역할
- Baner Class
- 교류 100볼트 전원

### Adpater(적용자)

- Target 역할을 실제로 충족시키는 역할
- PrintBanner Class

## 심층 이해

### 어떤 경우에 사용하는가?

- 기존의 클래스를 수정하지 않고도 필요한 클래스로 만들 때 사용
    - 기존 클래스를 사용하고 싶지만 그 인터페이스가 나머지 코드와 호환되지 않을 때
    - 부모 클래스에 추가할 수 없는 어떤 공통 기능들이 없는 여러 기존 자식 클래스들을 재사용하려는 경우
- 기존의 클래스가 충분히 테스트 되어 있을 때 더욱 좋음

### 비록 소스가 없더라도

- 기존의 클래스를 수정하지 않고, 새로운 인터페이스에 기존의 클래스를 적용 가능
- 기존 클래스의 소스 코드 없이, 메소드의 프로타입만 알면 어댑터 패턴 적용 가능
- API가 다른 두 개의 사이에 들어가 그 사이를 메우는 Adpater패턴

### 상속 VS 위임

- 위임을 사용하는 것이 문제가 적음
    - 상위 클래스의 내부 동작을 자세히 모르면, 상속을 효과적으로 사용하기 어려운 경우가 많음

## 장단점

### 장점

- *단일 책임 원칙*
    - 프로그램의 기본 비즈니스 로직에서 인터페이스 또는 데이터 변환 코드를 분리할 수 있다.
- *개방/폐쇄 원칙*
    - 클라이언트 코드가 클라이언트 인터페이스를 통해 어댑터와 작동하는 한, 기존의 클라이언트 코드를 손상시키지 않고 새로운 유형의 어댑터들을 프로그램에 도입할 수 있다.

### 단점

- 다수의 새로운 인터페이스와 클래스들을 도입해야 하므로 코드의 전반적인 복잡성이 증가한다. 때로는 코드의 나머지 부분과 작동하도록 서비스 클래스를 변경하는 것이 더 간단하다.

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
