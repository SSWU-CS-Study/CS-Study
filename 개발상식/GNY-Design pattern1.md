# 디자인패턴

분야: 개발상식
생성 일시: 2023년 3월 26일 오후 2:24

<aside>

💡 **면접 질문 정리**

- **디자인 패턴이란?**
    
    프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 **‘규약’** 형태로 만들어 놓은 것
    
- **싱글톤 패턴의 정의**
    
    하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
    
- **싱글톤 패턴의 장단점**
    
    하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어드는 장점이 있지만 의존성이 높아진다는 단점이 있습니다
    
- **의존성 주입이란?**
    
    모듈 간의 결합을 조금 더 느슨하게 만들어주는 것
    
- **의존성 주입의 원칙은?**
    
    상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 하며 둘 다 추상화에 의존해야 한다.  이때 추상화는 세부 사항에 의존하지 말아야 한다.
    
- **팩토리 패턴의 정의**
    
    객체를 만드는 부분을 Sub class에 맡기는 패턴
    
- **팩토리 패턴을 사용하는 이유는?**
    
    상위 클래스와 하위 클래스가 분리되기 때문에 느슨한 결합을 가지며 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 갖기 때문
    
- **MVC 패턴이란?**
    
    MVC란 Model View Controller의 약자로 개발을 할 때 이 3가지 형태로 역할을 나누어 개발하는 방법론 입니다. 비지니스 처리 로직과 사용자 인터페이스 요소들을 각각이 가진 특징들을 이해해 분리시켜 구성 요소들이 할 일만 구현하면 다른 요소를 수정하지 않으면서 유지 보수가 비교적 편한  조직화된 프로그래밍을 작성한다. 
    
- MVC 패턴의 **흐름을 설명하시오**
    
    사용자가 Controller를 조작하면 Controller는 Model을 통해서 데이터를 가져오고 그 정보를 바탕으로 시각적인 표현을 담당하는 View를 제어해서 사용자(User)에게  전달하게 됩니다.
    
- MVC 패턴을 구성하는 구성요소의 각 **역할**과 **규칙**
    
    Model은 무엇을 할지 정의하며, 비즈니스 로직에서의 알고리즘이나 데이터 등의 기능을 관리합니다. View는 무엇을 화면에 보여주는 역할을 합니다. Controller는 어떻게 할 지를 정의하며, 요청을 받아서 화면과 Model과 View를 연결해 주는 다리 역할을 합니다.
    
</aside>

# 디자인 패턴이란?

<aside>
🧤 **디자인 패턴이란?**

프로그램 이나 어떠한 것을 개발하는 과정에서 나타나는 과제나 문제의 상황에 따라 간편하게 적용해서 쓸 수 있는 것을 정리하여 특정한 "규약"을 통해 패턴별로 나눈 것 입니다. 알고리즘과 같이 프로그램 코드로 바로 변환될 수 있는 형태는 아니지만, 특정한 상황에서 구조적인 문제를 해결하는 방식을 설명해줍니다.

즉 구현자들 간의 커뮤니케이션 효율성을 높임과 동시에 개발을  **좀 더 쉽고 편리하게** 사용할 수 있게 만든 특정한 방법들을 디자인 패턴이라고 합니다.  디자인 패턴 중에는  스트래티지 패턴, 옵저버 패턴 등등 정말 여러가지가 있고 그 중에 하나가 바로 오늘 학습할  MVC패턴입니다.

</aside>

## 1. 싱글톤 패턴 singleton pattern

> 하나의 클래스에 오직 하나의 인스턴스만 가지는 패턴
> 

즉 인스턴스가 필요할 때, 똑같은 인스턴스를 만들지 않고 기존의 인스턴스를 활용하는 것

생성자가 여러번 호출되도, 실제로 생성되는 객체는 하나이며 최초로 생성된 이후에 호출된 생성자는 이미 생성한 객체를 반환시키도록 만드는 것이다.

- `**장점`**
    - 하나의 인스턴스를 만들어 놓고 해당 인스턴스를 다른 모듈들이 공유하며 사용하기 때문에 인스턴스를 생성할 때 드는 비용이 줄어든다. → 메모리낭비 저하
    - 싱글톤 인스턴스는 전역으로 사용되는 인스턴스이기 때문에 다른 클래스의 인스턴스들이 접근하여 사용할 수 있다 → **데이터 공유 용이**

- `**단점**`
    - 의존성이 높아짐
        - 모듈 간의 결합을 강하게 만듦 → 이때 **[의존성 주입](https://www.notion.so/a63023f996da40d499fc18635d783f8c)**(DI, Dependency Injection)을 통해 모듈 간의 결합을 조금 더 느슨하게 만들어 해결
        - 싱글톤 패턴을 구현하는 코드 자체가 많이 필요하다.
        - 테스트에서의 어려움
            - TDD(Test Driven Development)를 단위 테스트를 주로 하는데, **단위 테스트는 테스트가 서로 독립적이어야 하며 테스트를 어떤 순서로든 실행할 수 있어야 한다**. 하지만 싱글톤 패턴은 미리 생성된 하나의 인스턴스를 기반으로 구현하는 패턴이므로 각 테스트마다 ‘**독립적인’** 인스턴스를 만들기가 어려움.
            
- `**활용**`
    - 데이터베이스 연결 모듈
    - 스레드풀, 캐시, 로그 기록 객체 등

<aside>
🥣 **자바스크립트로 보는 싱글톤**

> 자바스크립트에서는 리터럴 {} 또는 new Object로 객체를 생성하게 되면 다른 어떤 객체와도 같지 않기 때문에 이 자체만으로 싱글톤 패턴을 구현할 수 있다.
> 

```jsx

const obj = { a: 27 }
const obj2 = {  a: 27 }
console.log(obj === obj2) // false
```

```jsx
class Singleton {
    constructor() {
        if (!Singleton.instance) {
            Singleton.instance = this
        }
        return Singleton.instance
    }
    getInstance() {
        return this.instance
    }
}
const a = new Singleton()
const b = new Singleton() 
console.log(a === b) // true
```

</aside>

<aside>
🥣 **데이터베이스에서의 싱글톤**

> 인스턴스 생성 비용 절약 가능
> 

```jsx

const URL = 'mongodb://localhost:27017/kundolapp'
const createConnection = url => ({"url" : url})
class DB {
    constructor(url) {
        if (!DB.instance) { 
            DB.instance = createConnection(url)
        }
        return DB.instance
    }
    connect() {
        return this.instance
    }
}
const a = new DB(URL)
const b = new DB(URL)
console.log(a === b) // true
```

- mysql
    
    ```jsx
    // 메인 모듈
    const mysql = require('mysql');
    const pool = mysql.createPool({
        connectionLimit: 10,
        host: 'example.org',
        user: 'kundol',
        password: 'secret',
        database: '승철이디비'
    });
    pool.connect();
    
    // 모듈 A
    pool.query(query, function (error, results, fields) {
        if (error) throw error;
        console.log('The solution is: ', results[0].solution);
    });
    
    // 모듈 B
    pool.query(query, function (error, results, fields) {
        if (error) throw error;
        console.log('The solution is: ', results[0].solution);
    });
    ```
    
</aside>

<aside>
🥣 **자바에서의 싱글톤 패턴**

```java
public class Singleton {

    private static Singleton instance = new Singleton();
    
    private Singleton() {
        // 생성자는 외부에서 호출못하게 private 으로 지정해야 한다.
    }

    public static Singleton getInstance() {
        return instance;
    }

    public void say() {
        System.out.println("hi, there");
    }
}
```

</aside>

---

## 2. **팩토리 패턴 factory pattern**

> 객체를 만드는 부분을 Sub class에 맡기는 패턴
> 

객체를 사용하는 코드에서 객체 생성 부분을 떼어내 추상화한 패턴이자 상속 관계에 있는 두 클래스에서 상위 클래스가 중요한 뼈대를 결정하고, 하위 클래스에서 객체 생성에 관한 구체적인 내용을 결정한다. 

- **왜 팩토리 패턴일까?**
    
    
    ![스크린샷 2023-03-26 15.17.43.png](%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-26_15.17.43.png)
    
    팩토리 클래스에서는 `getInstance()` 메소드를 이용하여 인스턴스를 반환한다. 이제, User 클래스 내부에서는 Product 객체를 직접 생성하지 않는다. 팩토리 클래스에 인스턴스를 요청하고, 생성된 인스턴스를 반환받으면 된다.(객체의 생성을 위임했다고 말 할 수 있다.)
    
    만약 Product의 생성자가 변경 된다면?? → Factory에 `getInstance()` 메소드 내부에 있는 Product 생성자만 변경 시켜주면 된다.
    
- `**장점**`
    - 객체간의 [결합도](https://www.notion.so/a63023f996da40d499fc18635d783f8c)를 낮출 수 있다.
    - 단일 책임 원칙을 따른다. 프로그램의 코드에서 생성자 코드를 분리함으로써 코드를 더욱 간결하게 만들 수 있다.
    - 개방 폐쇄 원칙을 따른다. 기존 client의 코드를 파괴하지 않고 새로운 타입을 추가 할 수 있다.
    - 상위 클래스와 하위 클래스가 분리되기 때문에 **느슨한 결합을** 가진다
    - 상위 클래스에서는 인스턴스 생성 방식에 대해 전혀 알 필요가 없기 때문에 더 많은 유연성을 가짐
    - 유지 보수성이 증가됨
- `**단점**`
    - 패턴을 구현할 많은 서브 클래스를 도입합으로써 코드가 복잡 해 질 수 있다.
    

<aside>
🥣 **자바 스크립트 에서의 팩토리 패턴**

```jsx
const num = new Object(42)
const str = new Object('abc')
num.constructor.name; // Number
str.constructor.name; // String
```

> 커피 팩토리를 기반으로 라떼 등을 생산하는 코드
> 

`CoffeeFactory`라는 **상위 클래스**가 중요한 뼈대를 결정하고 **하위 클래스**인 `LatteFactory`가 구체적인 내용을 결정

`CoffeeFactory`를 보면 `static`으로 `createCoffee()` 정적 메서드를 정의한 것을 알 수 있는데, 정적 메서드를 쓰면 클래스의 인스턴스 없이 호출이 가능하여 **메모리를 절약**할 수 있고 **개별 인스턴스에 묶이지 않으며** **클래스 내의 함수를 정의**할 수 있다.

```jsx
class Latte {
    constructor() {
        this.name = "latte"
    }
}
class Espresso {
    constructor() {
        this.name = "Espresso"
    }
}

class LatteFactory {
    static createCoffee() {
        return new Latte()
    }
}
class EspressoFactory {
    static createCoffee() {
        return new Espresso()
    }
}
const factoryList = { LatteFactory, EspressoFactory }

class CoffeeFactory {
    static createCoffee(type) {
        const factory = factoryList[type]
        return factory.createCoffee()
    }
}   
const main = () => {
    // 라떼 커피를 주문한다.
    const coffee = CoffeeFactory.createCoffee("LatteFactory")
    // 커피 이름을 부른다.
    console.log(coffee.name) // latte
}
main()
```

</aside>

## 3. MVC 패턴

> Model View Controller의 약자로 개발을 할 때 이 3가지 형태로 역할을 나누어 개발하는 방법론
> 

![https://mblogthumb-phinf.pstatic.net/MjAxNzAzMjVfMTM0/MDAxNDkwNDQyNDI5OTAy.MUksll6Y9SzelJjmGW6zXOlPebJKOft3OhcnmhrcmTgg.4g4FxlhwEpgxp8kGXJVLf2LHlrRJhP7NqR7LJew8tL0g.PNG.jhc9639/ModelViewControllerDiagram.png?type=w800](https://mblogthumb-phinf.pstatic.net/MjAxNzAzMjVfMTM0/MDAxNDkwNDQyNDI5OTAy.MUksll6Y9SzelJjmGW6zXOlPebJKOft3OhcnmhrcmTgg.4g4FxlhwEpgxp8kGXJVLf2LHlrRJhP7NqR7LJew8tL0g.PNG.jhc9639/ModelViewControllerDiagram.png?type=w800)

- `특징`
    - 비지니스 처리 로직과 사용자 인터페이스 요소들을 각각이 가진 특징들을 이해해 분리시켜 구성 요소들이 할 일만 구현하면 다른 요소를 수정하지 않으면서 유지 보수가 비교적 편한  조직화된 프로그래밍을 작성
    - 사용자가 Controller를 조작하면 Controller는 Model을 통해서 데이터를 가져오고 그 정보를 바탕으로 시각적인 표현을 담당하는 View를 제어해서 사용자(User)에게  전달

<aside>
<img src="https://www.notion.so/icons/checkmark_pink.svg" alt="https://www.notion.so/icons/checkmark_pink.svg" width="40px" /> **모델 Model**

- 데이터를 가진 객체
- 어플리케이션이 **“무엇”** 을 할 것인지를 정의
- 컨트롤러에게 데이터를 전달
- 데이터베이스와 소통

```모델**의 규칙**`

- 사용자가 편집하길 원하는 **모든 데이터**를 가지고 있어야 함
- 뷰나 컨트롤러에 대해 어떠한 정보도 알지 말아야 함
- 변경이 일어나면, 변경 통지에 대한 처리 방법을 구현
</aside>

<aside>
<img src="https://www.notion.so/icons/checkmark_green.svg" alt="https://www.notion.so/icons/checkmark_green.svg" width="40px" /> **뷰 view**

- 데이터를 받아서 보여주는 역할
- 유저가 보는 화면을 보여줌
- 컨트롤러에게 엑션이나 데이터를 전달

`**뷰의 규칙**`

- 모델이 가지고 있는 정보를 **따로** 저장해서는 안됨
- 모델이나 컨트롤러와 같이 **다른 구성요소들을 몰라야 함**
- 변경이 일어나면 **변경통지에** 대한 처리방법을 구현.
</aside>

<aside>
<img src="https://www.notion.so/icons/checkmark_orange.svg" alt="https://www.notion.so/icons/checkmark_orange.svg" width="40px" /> **컨트롤러 Controller**

- 모델에게 받은 데이터를 가공해서 뷰에게 전달
- 뷰에서 엑션과 이벤트에 대한 인풋 값을 받음
- 모델이 “어떻게” 처리할지를 알려줌

`````````컨트롤러**의 규칙**`

- **모델이나 뷰에** 대해 알고 있어야 함
- 모델이나 뷰의 변경을 **모니터**링 해야 함
</aside>

- **왜 MVC 패턴일까?**
    
    사용자가 보는 페이지, 데이터처리, 그리고 이 2가지를 중간에서 제어하는 컨트롤, 이 3가지로 구성되는 하나의 애플리케이션을 만들면 각각 맡은바에만 집중을 할 수 있게 된다.
    
    서로 분리되어 각자의 역할에 집중할 수 있게끔하여 개발을 하고 그렇게 애플리케이션을 만든다면, 유지보수성, 애플리케이션의 확장성, 그리고 유연성이 증가하고, 중복코딩이라는 문제점 또한 사라지게 되기 때문에 사용
    

---

<aside>
💭 **의존성 주입이란?**

![스크린샷 2023-03-26 14.46.50.png](%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-03-26_14.46.50.png)

메인 모듈(main mudule)이 ‘직접’ 다른 하위 모듈에 대한 의존성을 주기보다는 중간에 **의존성 주입자(dependency injector**)가 이 부분을 가로채 메인 모듈이 **‘간접’적**으로 의존성을 주입하는 방식

- `**의존성 주입 원칙`(OCP, Open Closed Principle)**
    - 상위 모듈은 하위 모듈에서 어떠한 것도 가져오지 않아야 한다.
    - 추상화는 세부 사항에 의존하지 않아야 한다.
- `**장점`**
    - 모듈들을 쉽게 교체할 수 있는 구조가 되어 테스팅과  마이그레이션에 용이.
    - 애플리케이션 의존성 방향이 일관됨
    - 모듈 간의 관계들이 조금 더 명확해짐
- `**단점`**
    - 클래스 수가 늘어나 복잡성이 증가됨
    - 런타임 페널티가 생성됨
</aside>

결합도 : 결합도는 한 클래스에 변경점이 얼마나 다른 클래스에 영향을 주는가를 의미함
