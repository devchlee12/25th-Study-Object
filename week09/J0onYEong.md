## 내용정리

### 계방 패쇄 원칙

확장에는 열려있고 수정에 대해서는 닫혀 있어야 한다.

기존의 코드 수정없이 런타임에 협력할 객체를 선택할 수 있도록 해야한다.

여기서 주의할 점은 추상화를 했다고 해서 모든 수정에 대해 설계가 패쇄되는 것은 아니라는 것이다.

변하는 것과 변하지 않는 것이 무엇인지 정확히 이해하고 추상화하는 것이 중요하다.

추상화가 수정에 닫혀있을 수 있는 이유는 `주의 깊게 추상화 대상을 선택했기 때문이다.`

### 순수 가공물 패턴

- FACTORY추가하기
    
    생성 책임을 클라이언트로 옮김 배경에는 클라이언트는 객체 생성에 의존해도 상관없다는 취지에서 시작
    
    하지만, 클라이언트에서 최대한 해당 책임을 분리시키는 방법이 존재한다.
    
    객체 생성과 관련된 책임만 전담하는 별도의 객체를 추가하고 클라이언트는 이 객체를 사용하도록 만들 수 있다.
    

모든 책임을 도메인 모델에게만 전담시키게될 경우 낮은 응집도, 높은 결합도, 재사용성 저하와 같은 문제가 발생할 확률이 높다. 이 경우 도메인과 무관한 인공적인 객체를 만들어 문제를 해결해야한다.

펙토리를 사용한 책임분리의 경우 도메인 모델을 통해 책임을 할당하는 정보 정분가 패턴과는 다르다.

팩토리를 사용하는 것은 수순한 기술적 결정이다.

※ 순수 가공물 패턴은 정보 전문가 패턴을 사용한 책임분해의 결과가 바람직하지 않을 경우 사용

### 의존성 주입

런타임 의존성을 객체 외부에서 확정하여 전달하는 패턴이다.

방법은 아래와 같다.

- 생성자를 통한 인스턴스 전달
- setter매서드를 사용한 설정
- 매서드의 인자로 의존할 객체를 전달
    - 거리 구하기 알고리즘

### 서비스 로케이터 패턴과 단점

서비스 로케이터는 런타임의존성을 해결해줄 것을 요청받는 객체이다.

해당 패턴은 서비스를 사용하는 클라이언트로 부터 구체적인 객체가 무엇인지를 가린다.

가장 큰 문제는 의존성을 감춘다는 것이다.

의존성과 관련된 문제를 런타임에만 발견할 수 있다는 문제가 발생한다.

- 의존성을 숨기는 코드는 단위테스트가 어렵다.
    
    모든 의존성이 동일한 서비스 로케이터의 상태(어떤 인스턴스를 들고 있는지)에 의존하기에, 각각의 단위테스트가 고립되어야한다는 원칙을 어기게된다.
    
    `숨겨진 의존성이 캡슐화를 위반하게 된 경우`
    

깊은 호출 계층에 걸쳐 객체를 계속 전달해야 하는 고통을 견디기 어려운 경우에는 어쩔 수 없이 서비스 로케이터 패턴을 사용하는 것을 고려해볼 수 있다.

서비스 로케이터 사용시 주의할 점

- 필요한 객체를 인자로 넘겨줄 수 없는지 생각
- 겹겹히 쌓여있는 스택에 인스턴스를 계속전달하는 것도 좋지 않음

### 의존성 역전 원칙

구체클래스에 대한 의존성은 결합도를 높이고 특정 객체의 재사용, 유연성이 저해된다.

어떤 협력에서 중요한 정책이나 의사결정, `비즈니스의 본질을 담고 있는 것은 상위 수준의 클래스`다.

영화 티켓의 판매 가격을 결정하는 서비스에서 중요한 것은 가격을 결정하는 것이지, 할인율을 계산하는 것은 세부사항이다.

가격을 결정하는 Movie클래스는 DiscountPolicy객체와의 협력을 통해 가격을 결정한다. 

이 경우 상위 수준의  Movie는 직접적으로 가격을 산출하는 책임을 가지지만 후자는 그렇지 않다.

하지만 상위 수준의 객체는 하위수준의 객체에 의존하기에 큰 영향을 받고 있다고 말할 수 있다.

하위 수준(세부사항)의 객체에 의해 상위 수준(중대한 로직)이 영향을 받는 것은 상위 객체의 재사용성을 저하시킨다.

### 추상화를 통한 해결

모든 의존성의 방향을 추상 클래스나 인터페이스와 같은 추상적인 것으로 향하게한다.

이렇게 하면 상위 수준의 클래스를 재사용할 때 하위수준의 구체 클래스에 얽메이지 않게된다.

<aside>

구체 클래스는 의존성의 목적지가 아니라 의존성의 시작점이 되야한다.

</aside>

### 의존성 역전 원칙과 패키지

역전은 의존성의 방향뿐만아니라 인터페이스의 소유권에도 적용된다는 것이다.

어떤 구성요소의 소유권을 결정하는 것은 모듈이다.

의존성 역전에 사용된 인터페이스가 상위 객체의 모듈에 포함되면 해당 패키지는 수정되는 경우가 감소한다.

이 경우 읜존하는 객체가 추가되어도 구체 타입이 인터페이스를 포함하는 모듈을 의존하게 되기 때문에, 새로운 협력객체의 추가가 상위 객체의 재빌드를 요구하지 않게된다.

<aside>

추상화(인터페이스)를 별도의 독립적인 패키지가 아니며 클라이언트(의존의 주체)에 속한 패키지에 포함해야한다.

</aside>

### 유연한 설계에 대한 조언

- 유연한 설계는 필요할 때만 옳다.
    
    변경하기 쉽고 확장하기 쉬운 구조를 만들기 위해서는 단숨함과 명확함을 잃게되는 트레이드 오프가 발생한다.
    

- 유연한 설계의 이면은 복잡한 설계이다.
    
    미래에 변경이 일어날지도 모른다는 막연한 불안감은 불필요하게 복잡한 설계를 낳는다.
    
    유연성은 항상 복잡성을 수반하는데, 대표적인 예시로 코드상의 객체와 런타임 의존객체가 다를 수 있다는 것이다.
    
    따라서 복잡성이 필요한 이유와 합리적인 근거를 생각하는 과정이 반드시 필요하다.
    

### 객체의 협력과 책임은 즁요하다

객체의 생성하는 방법에 대한 결정은 모든 책임이 자리를 잡은 후 가장 마지막 시점에 내리는 것이 적절하다.

필자는 설계를 할 때, 객체가 무엇이 되고 싶은지를 알게 될 때까지 객체들을 어떻게 인스턴스화할 것인지 전혀 신경쓰지 않았다는 것이다.

관계에 맞게 객체를 생성할 수 있을 것이라고 추축하고 설계를 진행한다.

생성에 대한 관심을 멀리하고 관계에 주목해야 생산성이 높다. 머리속에 넣을 수 있는 생각의 수는 한정적임으로 모든 것을 고려할 수는 없다.

하여튼 중요한 것은 객체의 책임을 가장 우선적으로 생각하는 것이다.