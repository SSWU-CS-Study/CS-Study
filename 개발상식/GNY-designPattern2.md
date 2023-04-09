# 디자인패턴2

분야: 개발상식
생성 일시: 2023년 4월 9일 오후 2:44

<aside>
💡 **면접 질문 정리**

- **디자인 패턴이란?**
    
    프로그램을 설계할 때 발생했던 문제점들을 객체 간의 상호 관계 등을 이용하여 해결할 수 있도록 하나의 **‘규약’** 형태로 만들어 놓은 것
    
- 옵저버 패턴을 어떻게 구현하나요?
    
    여러 가지 방법이 있지만 프록시 객체를 써서 하곤 합니다. 프록시 객체를 통해 객체의 속성이나 메서드 변화 등을 감지하고 이를 미리 설정해 놓은 옵저버들에게 전달하는 방법으로 구현합니다.
    
- ****SOLID를 설명하세요****
    
    단일 책임의 원칙, 개방 폐쇄의 원칙, 리스코브 치환의 법칙, 인터페이스 분리의 원칙, 의존성 역전의 원칙
    
- **전략 패턴이란?**
    
    어떤 동작을 하는 로직을 정의하고, 이것들을 하나로 묶어(캡슐화) 관리하는 패턴
    
- 옵저버 패턴이란?
    
    서로의 정보를 주고받는 과정에서 정보의 단위가 클수록, 객체들의 규모가 클수록 복잡성이 증가할 때  가이드라인을 제시해줄 수 있는 것
    
</aside>

## 0. 디자인 패턴 개요

<aside>
🧤 **디자인 패턴이란?**

프로그램 이나 어떠한 것을 개발하는 과정에서 나타나는 과제나 문제의 상황에 따라 간편하게 적용해서 쓸 수 있는 것을 정리하여 특정한 "규약"을 통해 패턴별로 나눈 것 입니다. 알고리즘과 같이 프로그램 코드로 바로 변환될 수 있는 형태는 아니지만, 특정한 상황에서 구조적인 문제를 해결하는 방식을 설명해줍니다.

즉 구현자들 간의 커뮤니케이션 효율성을 높임과 동시에 개발을  **좀 더 쉽고 편리하게** 사용할 수 있게 만든 특정한 방법들을 디자인 패턴이라고 함

디자인 패턴은 특정한 구현이 아닌 **아이디어**

프로젝트에 항상 적용해야 하는 것은 아니지만, 추후 재사용, 호환, 유지 보수시 발생하는 **문제 해결을 예방하기 위해 패턴을 만들어 둔 것**

</aside>

### ****Design Smells****

> design smell이란 나쁜 디자인을 나타내는 증상
> 
1. `Rigidity 경직성`
시스템이 변경하기 어렵다. 하나의 변경을 위해서 다른 것들을 변경 해야할 때 경직성이 높다. 경직성이 높다면 non-critical한 문제가 발생했을 때 관리자는 개발자에게 수정을 요청하기가 두려워진다.
2. `Fragility 취약성` 
취약성이 높다면 시스템은 어떤 부분을 수정하였는데 관련이 없는 다른 부분에 영향을 준다. 수정사항이 관련되지 않은 부분에도 영향을 끼치기 떄문에 관리하는 비용이 커지며 시스템의 credibility 또한 잃는다.
3. `Immobility 부동성`
부동성이 높다면 재사용하기 위해서 시스템을 분리해서 컴포넌트를 만드는 것이 어렵다. 주로 개발자가 이전에 구현되었던 모듈과 비슷한 기능을 하는 모듈을 만들려고 할 때 문제점을 발견한다.
4. `Viscosity 점착성`
점착성은 디자인 점착성과 환경 점착성으로 나눌 수 있다.
    
    시스템에 코드를 추가하는 것보다 핵을 추가하는 것이 더 쉽다면 디자인 점착성이 높다고 할 수 있다. 예를 들어 수정이 필요할 때 다양한 방법으로 수정할 수 있을 것이다. 어떤 것은 디자인을 유지하는 것이고 어떤 것은 그렇지 못할 것이다(핵을 추가).
    
    환경 점착성은 개발환경이 느리고 효율적이지 못할 때 나타난다. 예를들면 컴파일 시간이 매우 길다면 큰 규모의 수정이 필요하더라도 개발자는 recompile 시간이 길기 때문에 작은 규모의 수정으로 문제를 해결할려고 할 것이다.
    

### ****SOLID (객체지향 설계 원칙)****

> Robejt C. Martin은 5가지 Software design principles을 정의하였고 앞글자를 따서 SOLID라고 부른다.
> 
1. **`Single Responsibility Principle`** 
클래스는 오직 하나의 이유로 수정이 되어야 한다는 것을 의미 하나의 클래스는 하나의 역할만 해야 함. 
    
    
    ![Register 클래스가 Student 클래스에 dependency를 가지고 있는 모습](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.43.21.png)
    
    Register 클래스가 Student 클래스에 dependency를 가지고 있는 모습
    
    `SRP 위반 사례`
    
    ![어떤 클래스가 Student를 다양한 방법으로 정렬을 하고 싶을 때 Register 클래스는 어떠한 변경도 일어나야하지 않지만 Student 클래스가 바뀌어서 Register 클래스가 영향을 받는다. 정렬을 위한 변경이 관련없는 Reigster 클래스에 영향을 끼쳤기 때문에 SRP를 위반한다.](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.43.50.png)
    
    어떤 클래스가 Student를 다양한 방법으로 정렬을 하고 싶을 때 Register 클래스는 어떠한 변경도 일어나야하지 않지만 Student 클래스가 바뀌어서 Register 클래스가 영향을 받는다. 정렬을 위한 변경이 관련없는 Reigster 클래스에 영향을 끼쳤기 때문에 SRP를 위반한다.
    
    `해결 방안`
    
    ![각각의 정렬 방식을 가진 클래스를 새로 생성하고 Client는 새로 생긴 클래스를 호출](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.44.19.png)
    
    각각의 정렬 방식을 가진 클래스를 새로 생성하고 Client는 새로 생긴 클래스를 호출
    
2. **Open - Close Principle:**  확장 (상속)에는 열려있고, 수정에는 닫혀 있어야 함.
    
    
    `OCP 위반 사례`
    
    ![스크린샷 2023-04-09 15.47.53.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.47.53.png)
    
    ```java
    void incAll(Employee[] emps) {
        for (int i=0; i<emps.size(); i++) {
            if(emps[i].empType == FACULTY)
                incFacultySalary((FACULTY)emps[i])
            else if(emps[i].empType == STAFF)
                incStaffSalary((STAFF)emps[i])
            else if(emps[i].empType == SECRETARY)
                incSecretarySalary((SECRETARY)emps[i])
        }
    }
    ```
    
    Rigid - 새로운 employee type이 계속 요구된다.
    
    Fragile - 많은 if/lese 구문과 코드를 찾기 어렵다
    
    `해결 방안`
    
    ![스크린샷 2023-04-09 15.49.10.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.49.10.png)
    
    incAll() 함수를 통해서 문제 해결
    
3. **Liskov Substitution Principle:** 자식이 부모의 자리에 항상 교체될 수 있어야 함, base 클래스에서 파생된 클래스는 base 클래스를 대체해서 사용할 수 있어야한다.

1. **Interface Segregation Principle:** 인터페이스가 잘 분리되어서, 클래스가 꼭 필요한 인터페이스만 구현, 사용하지 않는 메소드에 의존하면 안된다.
    
    ![클래스에 맞는 interface를 만들어서 제공해야 함](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.53.28.png)
    
    클래스에 맞는 interface를 만들어서 제공해야 함
    
2. **Dependency Inversion Property:** 상위 모듈이 하위 모듈에 의존하면 안됨, 둘 다 추상화에 의존하며, 추상화는 세부 사항에 의존하면 안됨, 자신(high level module)보다 변하기 쉬운 모듈(low level modeul)에 의존해서는 안된다

<aside>
🌻 **디자인 패턴의 분류**
1. ****`생성 패턴` (Creational) : 객체의 생성 방식 결정
2.`구조 패턴` (Structural) : 객체간의 관계를 조직
3. `행위 패턴` (Behavioral): 객체의 행위를 조직, 관리, 연합**

</aside>

---

## 1. **전략 패턴(strategy pattern)**

> 어떤 동작을 하는 로직을 정의하고, 이것들을 하나로 묶어(캡슐화) 관리하는 패턴
> 

![스크린샷 2023-04-09 14.54.43.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_14.54.43.png)

- 객체의 행위를 바꾸고 싶은 경우 ‘직접’ 수정하지 않고 전략이라고 부르는 **‘캡슐화한 알고리즘’**을 [컨텍스트](https://www.notion.so/2-141443d11e34477883633de5973adeb6) 안에서 바꿔주면서 상호 교체가 가능하게 만드는 패턴
- 새로운 로직을 추가하거나 변경할 때, 한번에 효율적으로 변경이 가능하다.
- 로직을 사용하는 객체들은 자기의 입맛에 맞게 로직을 효율적으로 수정할 수 있다. 새로운 로직을 추가하거나 변경할 때 객체의 종류 수 만큼 반복하지 않고, 단 한번으로 반영할 수 있다.

- `**장점`**
    - Strategy Pattern을 활용하면 로직을 독립적으로 관리하는 것이 편해진다. 로직에 들어가는 '행동'을 클래스로 선언하고, 인터페이스와 연결하는 방식으로 구성하는 것!

<aside>
🥣 예제로 보는 전략패턴
****

![스크린샷 2023-04-09 15.05.36.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.05.36.png)

- 설계
    
    ![스크린샷 2023-04-09 15.03.51.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.03.51.png)
    

> 상속은 무분별한 소스 중복이 일어날 수 있으므로, 컴포지션을 활용한다. (인터페이스와 로직의 클래스와의 관계를 컴포지션하고, 유닛에서 상황에 맞는 로직을 쓰게끔 유도하는 것)
> 
- **미사일을 쏘는 것과 폭탄을 사용하는 것을 캡슐화**
    
    ShootAction과 BombAction으로 인터페이스를 선언하고, 각자 필요한 로직을 클래스로 만들어 implement한다.
    
- **전투기와 헬리콥터를 묶을 Unit 추상 클래스를 만들자**
    
    Unit에는 공통적으로 사용되는 메서드들이 들어있고, 미사일과 폭탄을 선언하기 위해 variable로 인터페이스들을 선언한다.
    
- 전투기와 헬리콥터는 Unit 클래스를 상속받고, 생성자에 맞는 로직을 정의

```java
class Fighter extends Unit {
    private ShootAction shootAction;
    private BombAction bombAction;
    
    public Fighter() {
        shootAction = new OneWayMissle();
        bombAction = new SpreadBomb();
    }
}
```

</aside>

<aside>
🥣 자바에서의 전략패턴
****

> 우리가 어떤 것을 살 때 네이버페이, 카카오페이 등 다양한 방법으로 결제하듯이 어떤 아이템을 살 때 LUNACard로 사는 것과 KAKAOCard로 사는 것을 구현
> 

```java
import java.text.DecimalFormat;
import java.util.ArrayList;
import java.util.List;
interface PaymentStrategy { 
    public void pay(int amount);
}

class KAKAOCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;
    
    public KAKAOCardStrategy(String nm, String ccNum, String cvv, String expiryDate) {
        this.name = nm;
        this.cardNumber = ccNum;
        this.cvv = cvv;
        this.dateOfExpiry = expiryDate;
    }

    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using KAKAOCard.");
    }
} 

class LUNACardStrategy implements PaymentStrategy {
    private String emailId;
    private String password;
    
    public LUNACardStrategy(String email, String pwd) {
        this.emailId = email;
        this.password = pwd;
    }
  
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using LUNACard.");
    }
}

class Item { 
    private String name;
    private int price; 
    public Item(String name, int cost) {
        this.name = name;
        this.price = cost;
    }

    public String getName() {
        return name;
    }

    public int getPrice() {
        return price;
    }
} 

class ShoppingCart { 
    List<Item> items;
    
    public ShoppingCart() {
        this.items = new ArrayList<Item>();
    }
    
    public void addItem(Item item) {
        this.items.add(item);
    }
    
    public void removeItem(Item item) {
        this.items.remove(item);
    }
    
    public int calculateTotal() {
        int sum = 0;
        for (Item item : items) {
            sum += item.getPrice();
        }
        return sum;
    }
    
    public void pay(PaymentStrategy paymentMethod) {
        int amount = calculateTotal();
        paymentMethod.pay(amount);
    }
}  

public class HelloWorld {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        
        Item A = new Item("kundolA", 100);
        Item B = new Item("kundolB", 300);
        
        cart.addItem(A);
        cart.addItem(B);
        
        // pay by LUNACard
        cart.pay(new LUNACardStrategy("kundol@example.com","pukubababo"));
        
        // pay by KAKAOCard
        cart.pay(new KAKAOCardStrategy("Ju hongchul", "123456789","123","12/01"));
    }
}
/*
400 paid using LUNACard.
400 paid using KAKAOCard.
*/
```

결제 방식의 ‘전략’만 바꿔서 두 가지 방식으로 결제하는 것을 구현

</aside>

<aside>
🥣 passport의 전략 패턴

> 전략 패턴을 활용한 라이브러리
> 

![스크린샷 2023-04-09 15.00.26.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.00.26.png)

passport는 Node.js에서 인증 모듈을 구현할 때 쓰는 미들웨어 라이브러리로, 여러 가지 ‘전략’을 기반으로 인증할 수 있게 합니다. 서비스 내의 회원가입된 아이디와 비밀번호를 기반으로 인증하는 LocalStrategy 전략과 페이스북, 네이버 등 다른 서비스를 기반으로 인증하는 OAuth 전략 등을 지원합니다.

```jsx
var passport = require('passport')
    , LocalStrategy = require('passport-local').Strategy;

passport.use(new LocalStrategy(
    function(username, password, done) {
        User.findOne({ username: username }, function (err, user) {
          if (err) { return done(err); }
            if (!user) {
                return done(null, false, { message: 'Incorrect username.' });
            }
            if (!user.validPassword(password)) {
                return done(null, false, { message: 'Incorrect password.' });
            }
            return done(null, user);
        });
    }
));
```

</aside>

---

## 2. 옵저버 패턴(observer pattern)

> 상태를 가지고 있는 주체 객체 & 상태의 변경을 알아야 하는 관찰 객체
> 

(1 대 1 or 1 대 N 관계)

서로의 정보를 주고받는 과정에서 정보의 단위가 클수록, 객체들의 규모가 클수록 복잡성이 증가하게 된다. 이때 가이드라인을 제시해줄 수 있는 것이 '옵저버 패턴’

주체가 어떤 객체(subject)의 상태 변화를 관찰하다가 상태 변화가 있을 때마다 메서드 등을 통해 옵저버 목록에 있는 옵저버들에게 변화를 알려주는 디자인 패턴입니다.

옵저버 패턴은, 한 객체의 상태가 바뀌면 그 객체에 의존하는 다른 객체들에게 연락이 가고, 자동으로 정보가 갱신되는 1:N 관계(혹은 1대1)를 정의한다.

`주체`: 객체의 상태 변화를 보고 있는 관찰자
`옵저버`: 이 객체의 상태 변화에 따라 전달되는 메서드 등을 기반으로 ‘추가 변화 사항’이 생기는 객체

![스크린샷 2023-04-09 15.13.27.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.13.27.png)

- `**특징**`
    - Java에는 옵저버 패턴을 적용한 것들을 기본적으로 제공해줌
    - Observable은 클래스로 구현되어 있기 때문에 사용하려면 상속을 해야 함. 따라서 다른 상속을 함께 이용할 수 없는 단점 존재
    - 인터페이스를 통해 연결하여 **느슨한 결합성**을 유지하며, Publisher와 Observer 인터페이스를 적용한다.
    - 안드로이드 개발시, OnClickListener와 같은 것들이 옵저버 패턴이 적용된 것 
    (버튼(Publisher)을 클릭했을 때 상태 변화를 옵저버인 OnClickListener로 알려주로독 함)
    

<aside>
🥣 예시로 보는 옵저버 패턴

> 트위터
> 

![스크린샷 2023-04-09 15.16.12.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.16.12.png)

내가 어떤 사람인 주체를 ‘팔로우’했다면 주체가 포스팅을 올리게 되면 알림이 ‘팔로워’에게 가는 구조

> 잡지사와 구독자간의 관계
> 

구독자, 고객들은 정보를 얻거나 받아야 하는 주체와 관계를 형성하게 된다. 관계가 지속되다가 정보를 원하지 않으면 해제 가능

이때, 객체와의 관계를 맺고 끊는 상태 변경 정보를 Observer에 알려줘서 관리하는 것을 말한다.

![스크린샷 2023-04-09 15.18.54.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.18.54.png)

- ****Publisher 인터페이스****
    
    > Observer들을 관리하는 메소드를 가지고 있음
    > 
    > 
    > 옵저버 등록(add), 제외(delete), 옵저버들에게 정보를 알려줌(notifyObserver)
    > 
    
    ```jsx
    public interface Publisher { 
        public void add(Observer observer); 
        public void delete(Observer observer); 
        public void notifyObserver(); 
    }
    ```
    
- ****Observer 인터페이스****
    
    > 정보를 업데이트(update)
    > 
    
    ```jsx
    public interface Observer { 
        public void update(String title, String news); }
    ```
    
- ****NewsMachine 클래스****
    
    > Publisher를 구현한 클래스로, 정보를 제공해주는 퍼블리셔가 됨
    > 
    
    ```jsx
    public class NewsMachine implements Publisher {
        private ArrayList<Observer> observers; 
        private String title; 
        private String news; 
    
        public NewsMachine() {
            observers = new ArrayList<>(); 
        }
        
        @Override public void add(Observer observer) {
            observers.add(observer);
        }
        
        @Override public void delete(Observer observer) 	{ 
            int index = observers.indexOf(observer);
            observers.remove(index); 
        }
        
        @Override public void notifyObserver() {
            for(Observer observer : observers) {
               observer.update(title, news); 
            }
        } 
        
        public void setNewsInfo(String title, String news) { 
            this.title = title; 
            this.news = news; 
            notifyObserver(); 
        }
    public String getTitle() { return title; } 		public String getNews() { return news; }
    }
    ```
    
- ****AnnualSubscriber, EventSubscriber 클래스****
    
    > Observer를 구현한 클래스들로, notifyObserver()를 호출하면서 알려줄 때마다 Update가 호출됨
    > 
    
    ```jsx
    public class EventSubscriber implements Observer {
        
        private String newsString;
        private Publisher publisher;
        
        public EventSubscriber(Publisher publisher) {
            this.publisher = publisher;
            publisher.add(this);
        }
        
        @Override
        public void update(String title, String news) {
            newsString = title + " " + news;
            display();
        }
        
        public void withdraw() {
            publisher.delete(this);
        }
        
        public void display() {
            System.out.println("이벤트 유저");
            System.out.println(newsString);
        }
        
    }
    ```
    

> MVC 패턴
> 

![스크린샷 2023-04-09 15.16.47.png](%E1%84%83%E1%85%B5%E1%84%8C%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%AB%E1%84%91%E1%85%A2%E1%84%90%E1%85%A5%E1%86%AB2%20141443d11e34477883633de5973adeb6/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2023-04-09_15.16.47.png)

주체라고 볼 수 있는 모델(model)에서 변경 사항이 생겨 update() 메서드로 옵저버인 뷰에 알려주고 이를 기반으로 컨트롤러(controller) 등이 작동

</aside>

---

컨텍스트(context): 프로그래밍에서의 컨텍스트는 상황, 맥락, 문맥을 의미하며 개발자가 어떠한 작업을 완료하는 데 필요한 모든 관련 정보를 말한다.
