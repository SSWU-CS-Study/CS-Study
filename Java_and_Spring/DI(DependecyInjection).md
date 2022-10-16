## 예상 면접 질문

- DI란?
    
    의존 관계를 주입하는 것으로, 객체를 외부에서 생성한 후 주입시키는 것입니다.
    
- DI의 장점
    
    첫째, 의존성이 줄어들어, 특정 클래스가 바뀔 때 다른 클래스가 영향을 덜 받게 됩니다.
    
    둘째, 재사용성이 높은 코드가 됩니다. DI를 사용하지 않는 경우가 일체형이라면, DI를 사용하는 경우를 조립식이라고 할 수 있습니다. 객체가 분리되어 있어서 다른 객체에서 사용하기도 쉽고, 변경도 수월해집니다.
    
    셋째, 각각의 클래스를 분리하여 테스트를 진행할 수 있습니다.
    
- IoC 컨테이너란?
    
    IoC란 Inversion of Control의 약어로 제어권이 역전되었다는 것을 뜻합니다. IoC 컨테이너는 개발자 대신 객체의 생명주기와 의존성을 관리해주는 역할을 합니다.
    

## 의존성이란?

- 의존성이란 클래스 간에 의존 관계가 있다는 것을 뜻한다.
- 클래스 간에 의존 관계가 있다는 것은 한 클래스가 바뀔 때 다른 클래스가 영향을 받는다는 것을 뜻한다.
- 인터페이스를 이용하여 특정 클래스에 대한 의존성을 해결할 수 있다.



## DI(Dependency Injection)

- DI란 의존 관계 주입 기능으로, 객체를 직접 생성하는 게 아니라 외부에서 생성한 후 주입 시켜주는 방식이다. (= 약한 결합)
- DI를 통해서 모듈 간의 결합도가 낮아지고 유연성이 높아진다.
<img width="510" alt="Screen Shot 2022-10-14 at 9 38 32 PM" src="https://user-images.githubusercontent.com/68562176/196028295-e425fb45-6282-4743-b708-b0567c864476.png">
<img width="510" alt="Screen Shot 2022-10-15 at 12 02 29 AM" src="https://user-images.githubusercontent.com/68562176/196028289-b6563ad4-72dd-48e5-95d2-72c3f2b9bce4.png">

    
### [DI의 장점]
- 의존성이 줄어든다.
- 재사용성이 높은 코드가 된다.
- 테스트하기 좋은 코드가 된다.
- 가독성이 높아진다.

## Spring을 통한 의존성 주입
<img width="510" alt="Screen Shot 2022-10-14 at 9 39 25 PM" src="https://user-images.githubusercontent.com/68562176/196028293-8d3db2f3-e09c-4519-affe-93b1e86d3a6e.png">

1. Field 주입
    
    외부에서 접근할 수 없다는 단점이 있다. 필드 주입은 DI 프레임워크가 존재해야하므로 사용을 지양하는 것이 좋다.
    
    ```java
    public class TeamProject {
    
        @Autowired
        public NewBackendEngineer backendEngineer;
        ...
    
        public void develop() {
            ...
            // 1. Backend 개발자가 API 개발
            backendEngineer.developApi();
            ...
        }
    
    }
    ```
    
2. Setter 주입
    
    주입받는 객체가 변경될 가능성이 있을 때 사용한다. 실제로 변경이 필요한 경우는 드물다.
    
    ```java
    public class TeamProject {
    
        public NewBackendEngineer backendEngineer;
        ...
    
        @Autowired
        public void setBackendEngineer(NewBackendEngineer backendEngineer) {
            this.backendEngineer = backendEngineer;
        }
    
        public void develop() {
            ...
            // 1. Backend 개발자가 API 개발
            backendEngineer.developApi();
            ...
        }
    }
    ```
    
3. Constructor(생성자) 주입
    - 생성자가 1개이고 그 생성자로 주입받을 객체가 빈으로 등록되어 있다면, `@Autowired` 생략 가능
    
    ```java
    public class TeamProject {
    
        public NewBackendEngineer backendEngineer;
        ...
    
        public TeamProject(NewBackendEngineer backendEngineer, FrontendEngineer frontendEngineer) {
            this.backendEngineer = backendEngineer;
        }
    
        public void develop() {
            ...
            // 1. Backend 개발자가 API 개발
            backendEngineer.developApi();
            ...
        }
    }
    ```
    

Spring framework에서는 생성자를 통한 주입을 권장한다.

→ 필수적으로 사용해야하는 의존성 없이는 인스턴스를 만들지 못하기 때문이다.

→ 순환 참조 문제를 컴파일 단계에서 찾아낼 수 있다. (순환 참조: 객체들이 서로를 참조하고 있는 상태)

## **@Autowired**

- DI를 할 때 사용하는 어노테이션이다.
- 스프링 서버가 올라갈 때 @Component, @Service, @Controller와 같은 어노테이션을 이용하여 등록한 빈을 생성하고, @Autowired가 붙은 위치에 의존 관계를 주입한다.

## lombok

해당 빈들이 생성자들을 통해 주입되는 시점에 불변성을 보장하도록 final 키워드를 붙여주는 것이 좋다.

## IoC(Inversion of Control)

IoC란 “제어의 역전”이라는 의미로, 말 그대로 메서드나 객체의 호출 작업을 개발자가 결정하는 것이 아니라 외부에서 결정되는 것을 의미한다.

- 스프링에서는 다음과 같은 순서로 객체가 만들어지고 실행된다
    1. 객체 생성
    2. 의존성 객체 주입
    3. 의존성 객체 메서드 호출

제어의 흐름을 스프링에 맡겨 작업을 처리하게 된다.

- Bean들을 담고 있는 스프링 IoC 컨테이너는 두 가지 중 하나를 사용한다.
    - **BeanFactory**
        
        Bean의 생성 및 의존성 주입, 생명주기 관리 등의 기능 제공
        
    - **ApplicationContext**
        
        BeanFactory Interface를 상속받고 있기 때문에, BeanFactory에서 제공하는 모든 기능들을 지원한다.
        
        추가적으로 AOP, 이벤트 처리, 메세지 처리 등의 기능을 제공한다.
    

- IoC 컨테이너에서 의존성 주입은 반드시 Bean으로 등록된 객체들 끼리만 가능하다.

### Bean

- Spring DI 컨테이너가 관리하는 객체를 Bean이라고 한다.
- Spring DI 컨테이너를 Bean들을 관리한다는 의미로 BeanFactory라고 부른다.
- Spring 컨테이너는 DI작업보다 더 많은 일을 한다. DI를 위한 BeanFactory에 여러가지 기능을 추가한 것을 Application Context 라고 한다.
- IoC 컨테이너는 IoC를 구현하는 프레임워크의 일반적 특성인데, Spring에서는 Application Context를 IoC Container라고 할 수 있다.

- Bean으로 등록하는 방법
    
    다음과 같은 Annotation을 활용한다.
    
    - @Component
    - @Controller
    - @Service 등

서블릿 객체는 싱글톤 패턴으로, 모든 스레드에서 공유할 수 있는 하나의 객체로 생성되어 실행된다.

## 참고

- 의존성이란?
[의존성 주입이란 무엇이며 왜 필요한가?](https://kotlinworld.com/64)
    
- DI > 정의, 이미지    
[[Spring] DI, IoC 정리](https://velog.io/@gillog/Spring-DIDependency-Injection)
    
- IoC > 이미지
[[Spring] Spring IoC와 DI란?](https://steady-coding.tistory.com/600)
    
- 스프링 의존성 주입 방법, @Autowired
[[Spring] 의존성 주입(DI, Dependency Injection)의 세가지 방법](https://atoz-develop.tistory.com/entry/Spring-%EC%9D%98%EC%A1%B4%EC%84%B1-%EC%A3%BC%EC%9E%85DI-Dependency-Injection%EC%9D%98-%EC%84%B8%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)
[[Spring] 다양한 의존성 주입 방법과 생성자 주입을 사용해야 하는 이유 - (2/2)](https://mangkyu.tistory.com/m/125)
    
- 참고하면 좋을 글
[DI는 IoC를 사용하지 않아도 된다](https://jwchung.github.io/DI%EB%8A%94-IoC%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EC%A7%80-%EC%95%8A%EC%95%84%EB%8F%84-%EB%90%9C%EB%8B%A4)