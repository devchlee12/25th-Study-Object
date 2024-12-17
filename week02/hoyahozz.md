## 01. 영화 예매 시스템

---

### 요구사항 살펴보기

![image](https://github.com/user-attachments/assets/a32568b4-5c65-40d2-a0d9-48ef3dce168f)

- **사용자는 영화 예매 시스템을 이용해 쉽고 빠르게 보고 싶은 영화를 예매**할 수 있다.
    - **‘영화’** - 제목, 상영 시간, 가격 정보와 같은 영화가 가지고 있는 기본 정보를 가리킬 때 해당 단어를 사용한다.
    - **‘상영’** - 실제로 관객들이 영화를 관람하는 사건, 즉 상영 일자, 시간, 순번 등을 가리키기 위해 사용한다.
        
        ⇒ 두 용어의 차이가 중요한 이유는, **사용자가 실제 예매하는 대상이 ‘영화’ 가 아닌 ‘상영’ 이기 때문**이다.
        
        - 영화를 예매한다고 표현하지만, 실제로는 **특정 시간에 상영되는 영화의 관람 권리를 구매**하기 위해 돈을 지불하는 것이다.
    
- 요금 할인과 관련해선, 두 가지 정책이 존재한다.
    1. **할인 조건**
    2. **할인 정책**

- **할인 조건**은 가격 할인 여부를 결정하며, 또 다시 두 개의 종류로 나눠진다.
    1. **순서 조건**
        - **상영 순번을 이용해 할인 여부를 결정하는 규칙**이다.
        - 순번이 10번인 경우, 매일 10번째로 상영되는 영화를 예매한 사용자에게 할인 혜택을 제공한다.
    2. **기간 조건**
        - **영화 상영 시작 시간을 이용해 할인 여부를 결정**한다.
        - 요일, 시작 시간, 종료 시간 세 부분으로 구성되며, 영화의 시작 시간이 해당 기간 내에 포함될 경우 할인 혜택을 제공한다.
        - 월요일, 시작 시간 오전 10시, 종료 시간 오후 1시인 기간 조건을 사용하면, 해당 시간 내 상영되는 모든 영화에 대해 할인 혜택을 제공한다.
    - **영화별로 여러 개의 할인 조건을 함께 지정할 수 있으며, 순서 및 기간 조건을 혼용하여 사용 가능**하다.

- **할인 정책**은 할인 요금을 결정하고, 마찬가지로 두 가지 정책이 존재한다.
    1. 금액 할인 정책
        - 예매 요금에서 일정 금액을 할인해준다.
    2. 비율 할인 정책
        - 정가에서 일정 비율의 요금을 할인해준다.
    - **영화별 하나의 할인 정책을 할당하거나, 지정하지 않을 수 있다.**

![image](https://github.com/user-attachments/assets/abce2b02-95e7-4d7d-ba73-435171f0cf04)


- **할인 적용을 위해선, 할인 조건과 할인 정책을 조합하여 사용**한다.
    - 먼저 사용자 예매 정보가 할인 조건 중 하나라도 만족하는지 검사한다.
    - 만족할 경우 할인 정책을 활용하여 할인 요금을 계산한다.

![image](https://github.com/user-attachments/assets/198390ca-2f47-4cd3-bd7d-0fefe5526071)

- **위 표의 ‘아바타’를 예매한다고 가정**한다.
    1. **아바타의 할인 조건은 순번 조건 2개, 기간 조건 2개로 구성**되어 있다.
    2. 이 조건을 만족하는 영화를 예매할 경우, 원래 가격인 10,000원에서 800원만큼 할인받을 수 있다.
    3. 결과적으로 9200원에 예매할 수 있다.
    - 단, 할인 정책은 1인 기준으로 책정되기 떄문에, 예약 인원이 두 명이라면 1,600원의 요금을 할인받을 수 있다.

## 02. 객체지향 프로그래밍을 향해

---

### 협력, 객체, 클래스

- **객체지향에 익숙한 경우, 설계 시 어떤 클래스가 필요한지 먼저 고민**한다.
    - 이후 어떤 속성과 메소드가 필요한지 고민한다.
        
        **⇒ 그러나, 이는 객체지향의 본질과는 거리가 멀다.**
        
- **객체지향은 말그대로 객체를 지향하는 것**이다.
    - **클래스가 아닌 객체에 초점을 맞춰야만, 진정한 객체지향 패러다임의 전환**이라고 할 수 있다.

- **객체지향을 위해선 다음 두 가지에 집중**해야 한다.
    1. **어떤 클래스가 필요한지 고민하기 전, 어떤 객체들이 필요한지 고민해야 한다.**
        - 클래스는 **공통 상태 및 행동을 공유하는 객체를 추상화**한 것이다.
        - 고로, 클래스 윤곽을 잡기 위해선 **어떤 객체들이 어떤 상태와 행동을 가지는지 먼저 결정**해야 한다.
        - 객체 중심 접근법은, 설계를 단순하고 깔끔하게 만든다.
    2. **객체를 독립적 존재가 아닌, 기능 구현을 위한 협력 공동체의 일원으로 보아야 한다.**
        - 객체는 홀로 존재하지 않고, 다른 객체에게 도움을 주거나 의존하며 살아간다.
        - **객체를 협력 공동체의 일원으로 보는 것은, 설계의 유연성 및 확장성을 증진**시킨다.
- **객체들의 모양, 윤곽이 잡히면 공통 특성과 상태를 가진 객체들을 타입으로 구분**한다.
    - 그리고, 이 타입을 기반으로 클래스를 구현한다.
    - **훌륭한 협력이 훌륭한 객체를 낳고, 훌륭한 객체가 훌륭한 클래스를 낳는다.**

### 도메인의 구조를 따르는 프로그램 구조

- **소프트웨어는 어떠한 문제를 해결**하기 위해 만들어진다.
    - 영화 예매 시스템의 목적은, 영화를 좀 더 빠르고 쉽게 예매하려는 사용자의 문제를 해결한다.
    - **이처럼, 문제 해결을 위해 사용자가 프로그램을 사용하는 분야를 도메인**이라고 부른다.

![image](https://github.com/user-attachments/assets/0726952a-0866-4180-b15b-78814d2b6d09)

- 객체지향 패러다임이 강력한 이유는, **요구사항을 분석하는 초기 단계부터 프로그램을 구현하는 마지막 단계까지 ‘객체’ 라는 동일한 추상화 기법을 사용**할 수 있기 때문이다.
    - **요구사항과 프로그램을 객체라는 동일한 관점**에서 바라볼 수 있다.
    - **고로, 도메인을 구성하는 개념들이 프로그램의 객체와 클래스로 매끄럽게 연결**될 수 있다.

- 위 이미지는, 영화 예매 도메인을 구성하는 개념과 관계를 표현한 이미지로, 하기 정보를 파악할 수 있다.
    1. 영화는 여러 번 상영될 수 있고, 상영은 여러 번 예매될 수 있다.
    2. 영화엔 할인 정책을 할당하지 않거나, 하더라도 하나만 할당할 수 있다.
    3. 할인 정책이 존재하는 경우, 반드시 하나 이상의 할인 조건이 존재한다.

- **일반적으로 클래스명은 대응되는 도메인의 이름과 동일하거나 적어도 유사**하게 지어야 한다.
- **클래스 사이 관계 역시, 최대한 도메인 개념 사이에 맺어진 관계와 유사**하게 만들어야 한다.
    
    → 이렇게 하면, **프로그램의 구조를 쉽게 이해하고 예상**할 수 있다.
    
![image](https://github.com/user-attachments/assets/12921268-8efa-41fe-a825-2ebf5dea079c)


- 원칙에 따라 클래스로 구현하면, 위 이미지와 같은 형태가 만들어진다.
    - **클래스의 구조는, 도메인 구조와 유사한 형태를 띠어야 한다.**

### 클래스 구현하기

```kotlin
class Screening(
    private val movie: Movie,
    private val sequence: Int,
    private val whenScreened: LocalDateTime,
) {
    fun isSequence(sequence: Int) = this.sequence == sequence
    fun getMovieFee(): Money = movie.fee
}
```

- `Screening` 클래스는, 사용자들이 예매하는 대상인 **‘상영’을 구현**한다.
    - 영화, 순번, 상영 시작 시간을 인스턴스 변수로 포함한다.

- 위 클래스에서 주목할 점은 인스턴수 변수 가시성은 **`private`**, 메소드 가시성은 **`public`** 이라는 것이다.
    - 클래스 구현 시 가장 중요한 것은, 클래스의 경계를 구분짓는 것이다.
    - 클래스는 내부, 외부로 구분되며 **훌륭한 클래스 설계를 위한 핵심은 어떤 부분을 외부에 공개하고 감출 것인지 결정하는 것**이다.
    - **외부에서는 객체 속성에 접근할 수 없도록 막고**, **적절한 `public` 메소드를 통해서만 내부 상태를 변경**할 수 있도록 해야한다.

- 클래스 내부, 외부를 구분해야 하는 이유는 다음과 같다.
    1. **경계의 명확성이 객체의 자율성을 보장**한다.
    2. **프로그래머에게 구현의 자유를 제공**한다.

**자율적인 객체**

- 우선 두 가지 중요한 사실을 알아야 한다.
    1. **객체는 상태, 행동을 함께 가지는 복합적 존재**이다.
    2. **객체는 스스로 판단하고 행동하는 자율적 존재**이다.

- 기존 패러다임에서는 데이터, 기능을 독립적 존재로 엮어 프로그램을 구성했다.
- **객체지향에서는, 객체라는 단위 내 데이터와 기능을 묶는다.**
    - **데이터, 기능을 객체 내부로 함께 묶는 것을 캡슐화**라고 한다.

- 대부분의 객체지향 프로그래밍들은, **외부에서의 접근을 통제할 수 있는 접근 제어 메커니즘도 제공**한다.
    - `public`, `private` 과 같은 접근 수정자를 제공한다.
    - 이는 **외부의 간섭을 최소화하여 최대한 객체를 자율적 존재로 만들기 위함**이다.
    - 외부에선 객체가 어떤 상태에 놓여있는지 알아선 안된다.
    - 외부에서 내부 객체의 결정에 직접적으로 개입해서도 안된다.
    - **오로지 원하는 것을 요청하고, 객체가 스스로 최선의 방법을 결정할 수 있음을 믿고 기다려야 한다.**

- 캡슐화와 접근 제어는 객체를 두 부분으로 나눈다.
    1. **외부에서 접근 가능한 부분으로, 퍼블릭 인터페이스(public interface)**라고 부른다.
    2. **외부에서 접근 불가능한 부분으로, 구현(implementation)** 이라고 부른다.
    - **인터페이스와 구현의 분리 원칙**은, 좋은 객체지향 프로그램을 만들기 위해 따라야 하는 핵심 원칙이다.

**프로그래머의 자유**

- 프로그래머의 역할을 **클래스 작성자**와 **클라이언트 프로그래머**로 구분하는 것이 유용하다.
    - 작성자는 새 데이터 타입을 프로그램에 추가한다.
    - 클라이언트는 작성자가 추가한 데이터 타입을 사용한다.

- 클라이언트 프로그래머는, 필요 클래스들을 엮어 애플리케이션을 빠르고 안정적으로 구축한다.
- **클래스 작성자는 클라이언트에게 필요한 부분만 공개하고, 나머지는 꽁꽁 숨긴다.**
    
    ⇒ 이로 인해, **클라이언트 프로그래머에 대한 영향을 생각하지 않고 내부 구현을 쉽게 변경**할 수 있다.
    
    - **이를, 구현 은닉(implementation hiding) 이라고 부른다.**

- **구현 은닉은 모두에게 유용한 개념**이다.
    - 클라이언트는 내부 구현에 대한 의존없이 인터페이스만 알고 있어도 클래스 사용이 가능하므로, 머릿속에 담아둬야 할 지식의 양이 줄어든다.
    - 작성자는 내부 구현은 마음대로 변경할 수 있다.

- **설계가 필요한 이유는, 변경을 관리하기 위함**이다.
    - 객체지향 언어는 객체 사이 의존성을 적절히 관리하여 변경에 대한 파급효과를 제어할 수 있다.
    - **변경될 여지가 있는 세부 구현 내용을 `private` 하게 감춤으로써, 변경으로 인한 혼란을 최소화**할 수 있다.

### 협력하는 객체들의 공동체

```kotlin
class Screening(
    private val movie: Movie,
    private val sequence: Int,
    private val whenScreened: LocalDateTime,
) {
    val startTime get() = whenScreened

    fun isSequence(sequence: Int): Boolean = this.sequence == sequence
    fun getMovieFee(): Money = movie.fee
    private fun calculateFee(audienceCount: Int): Money = 
        movie.calculateMovieFee(this).times(audienceCount.toDouble())

    fun reserve(customer: Customer, audienceCount: Int): Reservation =
        Reservation(customer, this, calculateFee(audienceCount), audienceCount)
}
```

- 영화 예매 기능을 구현하는 메소드를 살펴보자.
    - `reserve` 메소드는 영화 예매 후 예매 정보를 담고 있는 `Reservation` 인스턴스를 생성 후 반환한다.

```kotlin
@JvmInline
value class Money(val amount: BigDecimal) {
    companion object {
        @JvmStatic
        val ZERO = Money.wons(0)

        @JvmStatic
        fun wons(amount: Long): Money =
            Money(BigDecimal.valueOf(amount))
    }

    fun plus(amount: Money) = Money(this.amount.add(amount.amount))
    fun minus(amount: Money) = Money(this.amount.subtract(amount.amount))
    fun times(percent: Double) = 
		    Money(this.amount.multiply(BigDecimal.valueOf(percent)))
    fun isLessThan(other: Money) = amount < other.amount
    fun isGreaterThanOrEqual(other: Money) = amount >= other.amount
}

```

- **`Money` 는 금액과 관련된 다양한 계산을 구현하는 클래스**이다.
    - 이전엔 금액 구현을 위해 **`Long` 타입을 사용**했었다.
        - 변수 크기, 연산자 종류와 관련된 구현 관점 제약은 표현 가능하다.
        - 하지만, **`Money` 타입처럼 저장한 값이 금액과 관련되어 있다는 의미를 전달할 수는 없다.**
        - 그리고 금액 관련 로직이 다른 곳에 중복되어 구현되는 것을 제약할 수 없다.

- **객체지향은 객체를 활용해 도메인의 의미를 풍부하게 표현할 수 있다는 것이 장점**이다.
    - 고로, 의미를 더 명시적이고 분명하게 표현할 수 있다면 객체를 이용하여 개념을 구현하는 것이 좋다.
    - 개념이 오로지 하나의 인스턴스 변수만 포함하더라도, **개념을 명시적으로 표현하는 것이 전체 설계의 명확성과 유연성을 높이는 첫 걸음**이다.

```kotlin
class Reservation(
    private val customer: Customer,
    private val screening: Screening,
    val fee: Money, 
    private val audienceCount: Int,
)
```

- `Reservation` 클래스는 고객, 상영 정보, 예매 요금, 인원 수를 속성으로 포함시킨다.

![image](https://github.com/user-attachments/assets/7a9edfbd-60d0-4c5e-bd77-775647587e1c)

- 영화 예매를 위해 `Screening`, `Movie`, `Reservation` 인스턴스들은 **서로의 메소드를 호출하며 상호작용**한다.
    - 이처럼 **시스템의 어떤 기능을 구현하기 위해 객체들 사이에 발생하는 상호작용**을 **‘협력’**이라고 부른다.
- 객체지향 프로그램 작성 시엔 **먼저 협력의 관점에서 어떤 객체가 필요한지 결정**한다.
    - 이후 객체들의 공통 상태 및 행위를 구현하기 위해 클래스를 작성한다.

### 협력에 대한 짧은 이야기

- 객체 내부 상태는 외부에서 접근하지 못하도록 감추고, 대신 **외부에 공개하는 퍼블릭 인터페이스를 통해 내부 상태에 접근할 수 있도록** 해야 한다.
    - **객체는 다른 객체의 인터페이스에 행동을 수행하도록 요청**할 수 있다.
    - **요청을 받은 객체는, 자신만의 방법에 따라 요청을 처리한 후 응답**한다.

- **객체가 다른 객체와 상호작용할 수 있는 유일한 방법은, 메시지를 전송하는 것뿐이다.**
- **다른 객체에게 요청이 도착할 때 해당 객체가 메시지를 수신했다고 이야기한다.**
    - 그리고 **스스로의 결정에 따라 자율적으로 메시지를 처리**한다.
    - 이처럼 **수신 메시지를 처리하기 위한 자신만의 방법을 메소드(method)**라고 부른다.

- 메시지와 메소드의 구분은 매우 중요하다.
    - 메시지와 메소드의 구분에서부터, **다형성**의 개념이 시작된다.

- `Screening` 은 `Movie` 의 메소드를 호출하는 것이 아니라, **‘`calculate` 라는 메시지를 전송한다’로 표현**하는 것이 더 적절하다.
    - `Screening` 은 `Movie` 내에 `calculateMovieFee` 가 존재하는지도 모른다.
    - 그저, **메시지에 응답할 수 있다고 믿고 메시지를 전송**할 뿐이다.

- **`Movie` 는 응답을 위해 적절한 메소드를 선택**한다.
    - 즉, **메시지를 처리하는 방법을 결정하는 것은 오로지 `Movie` 스스로의 문제**다.
    - 결국, 객체는 메시지 처리 방법을 자율적으로 결정할 수 있다.

## 03. 할인 요금 구하기

---

```kotlin
class Movie(
    private val title: String,
    private val runningTime: Duration,
    private val fee: Money,
    private val discountPolicy: DiscountPolicy
) {
    fun calculateMovieFee(screening: Screening): Money = 
        fee.minus(discountPolicy.calculateDiscountAmount(screening))
}
```

- `Movie` 는 제목과 상영 시간, 기본 요금, 할인 정책을 속성으로 가진다.
- `calculateMovieFee` 메소드는, `discountPolicy` 에 `calculateDiscountAmount` 메시지를 전송해 할인 요금을 반환받는다.
    - 중요한 것은, 어떤 할인 정책을 사용할 것인지 결정하는 코드가 존재하지 않는다는 점이다.
    - 오로지 `discountPolicy` 에 메시지를 전송할 뿐이다.

- 이 코드엔, **객체지향의 주요 키워드인 상속과 다형성이라는 개념**이 숨겨져 있다.
    - 그리고 그 기반엔 **추상화(abstraction)**이라는 원리가 숨겨져 있다.

### 할인 정책과 할인 조건

- 정책은 **금액 할인 정책, 비율 할인 정책**으로 구분된다.
    - 각각 **`AmountDiscountPolicy`**, **`PercentDiscountPolicy`** 라는 클래스로 구현한다.
    - 두 클래스는 대부분의 코드가 유사하고, 할인 요금 계산 방식만 다르다.
    - 고로, **두 클래스 사이의 중복 코드를 제거하기 위해 공통 코드를 보관할 장소가 필요**하다.

```kotlin
abstract class DiscountPolicy {
    abstract val conditions: List<DiscountCondition>

    fun calculateDiscountAmount(screening: Screening): Money {
        if (conditions.any { each -> each.isSatisfiedBy(screening) })
            return getDiscountAmount(screening)

        return Money.ZERO
    }

    protected abstract fun getDiscountAmount(screening: Screening): Money
}
```

- 부모 클래스인 **`DiscountPolicy`** 안에 중복 코드를 두고, 두 클래스가 이를 **상속**받게 한다.
    - `DiscountPolicy` 의 **인스턴스는 생성될 필요가 없으므로, 추상 클래스**로 구현한다.

- `conditions` 의 존재로 인해, 하나의 할인 정책이 여러 개의 할인 조건을 포함할 수 있음을 알 수 있다.
    - `Screening` 이 할인 조건을 하나라도 만족시킬 경우, **`getDiscountAmount` 메소드를 호출해 할인 요금을 계산**한다.

- **`DiscountPolicy` 는 할인 요금과 요금 계산에 필요한 전체 흐름은 정의하지만, 실제 요금 계산부는 추상 메소드인 `getDiscountAmount` 메소드에 위임**한다.
    - **부모 클래스에 기본 알고리즘 흐름을 구현하고, 도중 필요한 처리를 자식 클래스에게 위임하는 디자인 패턴을 `TEMPLATE METHOD` 패턴**이라고 부른다.

```kotlin
interface DiscountCondition {
    fun isSatisfiedBy(screening: Screening): Boolean
}
```

- **`DiscountCondition`** 은 **인터페이스**로 선언되어 있다.
    - `isSatisfiedBy` 는 `screening` 이 할인이 가능한 경우 `true` 를 리턴한다.

- 이번엔 **순번 조건, 기간 조건의 두 가지 할인 조건을 구현**해보자.
    - 각각 **`SequenceCondition`**, **`PeriodCondition`** 이라는 클래스로 구현한다.

```kotlin
class SequenceCondition(
    private val sequence: Int
) : DiscountCondition {
    override fun isSatisfiedBy(screening: Screening): Boolean =
        screening.isSequence(sequence)
}

class PeriodCondition(
    private val dayOfWeek: DayOfWeek,
    private val startTime: LocalTime,
    private val endTime: LocalTime
) : DiscountCondition {
    override fun isSatisfiedBy(screening: Screening): Boolean =
        screening.startTime.dayOfWeek.equals(dayOfWeek) &&
                startTime <= screening.startTime.toLocalTime() &&
                endTime >= screening.startTime.toLocalTime()
}
```

- **`SequenceCondition`** 은 순번(**`sequence`**) 을 인스턴스 변수로 포함한다.
    - `Screening` 의 상영 순번과 일치할 경우 할인 가능한 것으로 판단한다.
- **`PeriodCondition`** 은 상영 시작 시간이 특정 기간 내 포함되는지 여부를 판단해 할인 여부를 결정한다.
    - 상영 요일이 같고, 상영 시작 시간이 `startTime`, `endTime` 사이에 있는 경우 할인 가능한 것으로 판단한다.

- 이제 할인 정책을 구현한다.

```kotlin
class AmountDiscountPolicy(
    private val discountAmount: Money,
    override val conditions: List<DiscountCondition> = emptyList(),
) : DiscountPolicy() {
    override fun getDiscountAmount(screening: Screening): Money = discountAmount
}

class PercentDiscountPolicy(
    private val percent: Double,
    override val conditions: List<DiscountCondition> = emptyList(),
) : DiscountPolicy() {
    override fun getDiscountAmount(screening: Screening): Money = 
        screening.getMovieFee().times(percent)
}
```

- `AmoiuntDiscountPolicy` 는 단순히 할인 요금인 `discountAmount` 를 리턴한다.
- `PerecentDiscountPolicy` 는, 고정 금액이 아닌 일정 비율을 차감한 값을 리턴한다.

![image](https://github.com/user-attachments/assets/e17c4b8f-d69c-43e0-bb4e-73c4ae0828ab)

- 모든 클래스 사이의 관계를 위와 같은 다이어그램으로 표현할 수 있다.

### 할인 정책 구성하기

- 이전에 영화에 대해 하나의 할인 정책, 다수의 할인 조건을 적용할 수 있다고 이야기했었다.

```kotlin
class Movie(
    private val title: String,
    private val runningTime: Duration,
    private val fee: Money,
    private val discountPolicy: DiscountPolicy
) { /* . . . */ }
```

- `Movie`, `DiscountPolicy` 의 생성자는 이러한 제약을 강제한다.
    - `Movie` 는 오로지 하나의 `DiscountPolicy` 인스턴스만 받을 수 있도록 선언되어 있다.

```kotlin
abstract class DiscountPolicy {
    abstract val conditions: List<DiscountCondition>
  }
}
```

- 반면 `DiscountPolicy` 는 여러 `DiscountCondition` 인스턴스의 사용을 허용한다.

- 이와 같이, **생성자 파라미터 목록을 이용해 초기화에 필요한 정보를 전달하도록 강제**시키면, **올바른 상태를 가진 객체의 생성을 보장**할 수 있다.

```kotlin
val avatar = Movie(
    title = "아바타",
    runningTime = Duration.ofMinutes(120),
    fee = Money.wons(10000),
    discountPolicy = AmountDiscountPolicy(
        discountAmount = Money.wons(800),
        conditions = listOf(
            SequenceCondition(1),
            SequenceCondition(10),
            PeriodCondition(DayOfWeek.MONDAY, LocalTime.of(10, 0), LocalTime.of(11, 59)),
            PeriodCondition(DayOfWeek.THURSDAY, LocalTime.of(10, 0), LocalTime.of(20, 59)),
        )
    )
)
```

- **‘아바타’에 대한 할인 정책과 할인 조건을 설정한 예제**이다.
    - **두 가지 정보를 파악**할 수 있다.
        1. 할인 정책으로 금액 할인 정책이 적용된다.
        2. 두 개의 순서 조건, 두 개의 기간 조건을 이용해 할인 여부를 판단한다.

## 04. 상속과 다형성

---

- **`Movie` 클래스 내부엔 할인 정책이 무엇인지 판단하는 로직이 전혀 없다.**
    - 내부에 할인 정책을 결정하는 조건문이 없음에도, 어떻게 영화 요금 계산 시 할인 정책과 비율 할인 정책을 선택할 수 있을까?
    - 이 질문에 답하기 위해선, **상속 및 다형성**에 대해 알아보아야 한다.

### 컴파일 시간 의존성과 실행시간 의존성

![image](https://github.com/user-attachments/assets/8e2c7ca8-59d3-458d-a239-87ba62bfa4c5)

- `Movie`, `DiscountPolicy` 계층 사이 관계를 클래스 다이어그램으로 표현하면 위와 같다.
    - 하기 조건을 만족할 경우, **두 클래스 사이엔 의존성이 존재**한다고 이야기한다.
        1. **어떤 클래스가 다른 클래스에 접근할 수 있는 경로를 가진 경우**
        2. **해당 클래스 객체의 메소드를 호출할 경우**

- 중요한 것은, **`Movie` 는 오로지 추상 클래스인 `DiscountPolicy` 에만 의존한다는 점**이다.
    - 하지만 실제 요금 계산을 위해선 구현체인 `AmountDiscountPolicy`, `PercentDiscountPolicy` 가 필요하다.
    - 그렇다면 **코드 작성 시점에는 알지 못했던 `AmountDiscountPolicy`, `PercentDiscountPolicy` 의 인스턴스와 실행 시점엔 협력이 가능한 이유**는 무엇일까?

```kotlin
val avatar = Movie(
    title = "아바타",
    runningTime = Duration.ofMinutes(120),
    fee = Money.wons(10000),
    discountPolicy = AmountDiscountPolicy(
        // . . .
    )
)
```

- 의문을 풀기 위해선, **`Movie` 의 인스턴스를 생성하는 코드**를 살펴보아야 한다.
    - 실제 요금 계산 시 **금액 할인 정책**을 적용하고 싶다면, 인자로 **`AmountDiscountPolicy` 의 인스턴스**를 전달하면 된다.
        
        ![image](https://github.com/user-attachments/assets/ad583cf3-9007-4aaf-a19d-ab98296e0f4e)
        
    - 고로, 실행 시 `Movie` 의 인스턴스는 `AmountDiscountPolicy` 의 인스턴스에 의존하게 된다.
- 반대로 비율 할인 정책을 적용하고 싶다면, 인자로 `PercentDiscountPolicy` 의 인스턴스를 전달하면 된다.

- 분명 **코드 상 `Movie` 는 `DiscountPolicy` 에 의존**한다.
    - 하지만, **실행 시점에는 `AmountDiscountPolicy`, `PercentDiscountPolicy` 의 인스턴스에 의존**하게 된다.

- 중요한 것은, **코드의 의존성과 실행 시점의 의존성이 서로 다를 수 있다는 것**이다.
    - 다시 말해, **클래스 사이 의존성과 객체 사이 의존성**은 동일하지 않을 수 있다.
    - **유연하고 쉽게 재사용할 수 있으며, 확장 가능한 객체지향 설계**가 가지는 특징은 **코드의 의존성과 실행 시점의 의존성이 다르다는 점**이다.

- 단, **코드의 의존성, 실행 시점의 의존성이 다를수록 코드를 이해하기 어려워진다.**
    - 객체를 생성하고, 연결하는 부분을 찾아야 하기 때문이다.
    - 그 대신, 코드는 더 유연해지고 확장 가능해진다.
    - **이와 같이 의존성의 양면성은, 설계가 트레이드오프의 산물**이라는 사실을 보여준다.

- **`Movie` 인스턴스가 실제 어떤 객체**에 의존하고 있을까?
    - 이를 확인하기 위해선 실제 의존성을 연결하는 부분을 찾아야만 한다.
    - **`Movie` 인스턴스의 생성부를 찾아, 전달되는 인자가 어떤 정책의 인스턴스인지 확인**해야 한다.

- **설계가 유연해질수록, 코드를 이해하고 디버깅하기 점점 더 어려워진다.**
- **반면 유연성을 억제하면 코드를 이해하고 디버깅하긴 쉬워지나 재사용성과 확장성은 낮아진다.**
- 이 중 무엇도 정답이 아니므로, **항상 유연성과 가독성 사이에서 고민**해야 한다.

### 차이에 의한 프로그래밍

- 마지막으로, `Movie` 에서 `DiscountPolicy` 클래스로의 의존성이 **어떻게 실행 시점에는 `AmountDiscountPolicy`, `PercentDiscountPolicy` 인스턴스에 대한 의존성**으로 바뀐걸까?
    - 이는, 상속과 연관되어 있다.

- **클래스를 작성하려 하는데, 그 클래스가 기존에 작성했던 클래스와 구조가 매우 흡사**하다고 가정해보자.
    - 클래스 코드를 그대로 가져와 약간만 추가하거나, 수정할 수 있다.
    - 하지만 이보다 **클래스 코드 수정없이 오로지 재사용하는 것**이 가장 좋다.
    - 이를 가능하게 해주는 것이 바로 **상속**이다.

- **상속**은 **코드 재사용 시 가장 널리 사용되는 기법**이다.
    - 클래스 사이 관계를 설정해주는 것만으로, **기존 클래스가 가지고 있는 모든 속성과 행동을 새 클래스에 포함**시킬 수 있다.
    - 또한 **부모 클래스 구현은 공유하면서, 행동이 다른 자식 클래스를 쉽게 추가**할 수 있다.
        - 이전에 `getDiscountAmount` 메소드를 각 클래스에서 오버라이딩하여 다른 행동을 구현한 것을 확인할 수 있다.

- 이처럼, **부모 클래스와 다른 부분만을 추가하여 새 클래스를 쉽고 빠르게 만드는 방법을 ‘차이에 의한 프로그래밍(programming by difference)** 라고 부른다.

### 상속과 인터페이스

- **상속의 가치는, 부모 클래스가 제공하는 모든 인터페이스를 자식 클래스가 물려받는 것**에 있다.
    - 이는 상속을 바라보는 일반 인식과는 거리가 있는데, **대부분은 상속의 목적이 메소드와 인스턴스 변수를 재사용하는 것이라고 생각하기 때문**이다.

- **인터페이스는 객체가 이해할 수 있는 메시지의 목록을 정의**한다.
    - **상속을 통해 자식 클래스는 자신의 인터페이스에 부모 클래스의 인터페이스를 포함**하게 된다.
    - **결과적으로 자식 클래스는 부모 클래스가 수신할 수 있는 모든 메시지를 수신할 수 있다.**
        - 고로, 외부 객체는 자식 클래스를 부모 클래스의 동일한 타입으로 간주할 수 있다.

```kotlin
class Movie() {
    fun calculateMovieFee(screening: Screening): Money = 
        fee.minus(discountPolicy.calculateDiscountAmount(screening))
}
```

- `Movie` 는 `DiscountPolicy` 인터페이스에 정의된 **`calculateMovieFee`** 메시지를 전송하고 있다.
    - **`AmountDiscountPolicy`, `PercentDiscountPolicy`** 의 인터페이스에도, 이 오퍼레이션이 포함되어 있다.
- **`Movie` 는 오로지 `calculateMovieFee` 메시지를 수신할 수 있다는 사실만이 중요**하다.
    - 메시지를 이해할 수만 있다면, **메시지를 수신하는 것이 어떤 클래스의 인스턴스인지는 전혀 상관하지 않는다.**
    - 따라서, 메시지를 수신할 수 있는 **`AmountDiscountPolicy`, `PercentDiscountPolicy` 모두** `DiscountPolicy` 를 대신하여 `Movie` 와 협력할 수 있다.

![image](https://github.com/user-attachments/assets/046a617c-adba-497c-9a07-54287c159aaf)

- 이처럼 **자식 클래스가 부모 클래스를 대신하는 것을 업캐스팅**이라고 한다.
    - 아래에 위치한 자식 클래스가 위에 위치한 부모 클래스로 자동적으로 타입 캐스팅되는 것처럼 보이기에, 업캐스팅이라는 용어를 사용한다.

### 다형성

- **다시 한 번 강조하지만, 메시지와 메소드는 매우 다른 개념**이다.
    - `Movie` 는 `DiscountPolicy` 인스턴스에 `calculateMovieFee` **’메시지’**를 전송한다.
    - 하지만 실행되는 **‘메소드’**는 연결된 객체의 클래스가 무엇인가에 따라 달라진다.

- 코드 상에선 분명 **`DiscountPolicy`** 클래스에 메시지를 전송하지만, **실행 시점에 실제 실행되는 메소드는 메시지 수신 클래스에 따라 항상 달라질 수 있다.**
    - 이를, **다형성**이라고 부른다.

- 다형성은 객체지향 프로그램의 컴파일 시간 의존성과 실행 시간 의존성이 다를 수 있다는 사실을 기반으로 구현된다.
- **다형성**이란 **동일 메시지 수신 시 객체 타입에 따라 다르게 응답할 수 있는 능력**을 의미한다.
    - 고로, **다형적 협력에 참여하는 객체들은 모두 같은 메시지를 이해**할 수 있어야 한다.
        - 이는, 다른말로 인터페이스가 동일해야 한다는 것이다.
    - **`AmountDiscountPolicy`, `PercentDiscountPolicy`** 가 다형적 협력에 참여 가능한 이유눈, `DiscountPolicy` 로부터 동일 인터페이스를 물려받았기 때문이다.
        - 이 **두 클래스의 인터페이스를 통일하기 위해 사용한 구현법이, 바로 상속**이다.

- 다형성 구현 방법은 매우 다양하나, **메시지에 응답하기 위해 실행될 메소드를 컴파일 시점이 아닌, 실행 시점에 결정한다는 공통점**이 있다.
    - 다시 말해, **메시지 및 메소드를 실행 시점에 바인딩**한다는 것이다.
        - 이를 지연 바인딩(lazy binding), 혹은 동적 바인딩(dynamic binding) 이라고 부른다.
    - 이와 반대로 전통적인 함수 호출은 컴파일 시점에 실행될 함수나 프로시저를 결정했다.
        - 이를 초기 바인딩(early binding), 정적 바인딩(static binding) 이라고 부른다.

- 다만 클래스를 상속받는 것만이 다형성을 구현할 수 있는 유일한 방법은 아니다.
    - 이 책을 읽으면 앞으로는 다형성이란 그저 추상적인 개념이고, 이를 구현할 수 있는 방법이 다양하다는 사실을 알게될 것이다.

### 구현 상속과 인터페이스 상속

- 상속을 **구현 상속**과 **인터페이스 상속**으로 분류할 수 있다.
    - 흔히 **구현 상속**을 **서브 클래싱**, **인터페이스 상속**을 **서브타이핑**이라고 부른다.
        - 순수하게 코드 재사용 목적으로 상속을 사용하는 것을 구현 상속이라고 부른다.
        - **다형적 협력**을 위해 부모 클래스 및 자식 클래스가 인터페이스를 공유할 수 있도록 상속을 이용하는 것을 인터페이스 상속이라 부른다.

- **상속은 구현 상속이 아닌 인터페이스 상속을 위해 사용해야 한다.**
    - **코드 재사용은 상속의 주된 목적이 아니다.**
    - 인터페이스 재사용 목적이 아닌, 구현 재사용 목적으로 상속을 이용하면 변경에 취약한 코드를 낳게 될 확률이 높다.

### 인터페이스와 다형성

- **`DiscountPolicy` 는 추상 클래스로 구현하여, 자식 클래스들이 인터페이스와 내부 구현을 모두 상속**받도록 만들었다.
    - 하지만, **구현은 공유할 필요 없이 순수 인터페이스만 공유하고 싶은 경우, `Interface`** 를 사용할 수 있다.
    - `C#`, `Java`, `Kotlin` 에서 인터페이스라는 프로그래밍 요소를 제공한다.

![image](https://github.com/user-attachments/assets/03b116a9-d26d-4717-ad1e-7bcfe1985873)

- 추상 클래스를 활용했던 할인 정책과 달리, **할인 조건은 구현을 공유할 필요가 없다.**
    - 고로, **자바 인터페이스를 활용**하여 타입 계층을 구현하였다.
    - `SequenceCondition`, `PeriodCondition` 은 동일한 인터페이스를 공유하며, 다형적 협력에 참여할 수 있다.
- 클라이언트측에서 두 구현체는 **`isSatisfiedBy`** 메시지를 이해할 수 있기에, **`DiscountCondition`** 를 사용하는 것과 아무런 차이가 없다.
    - 이 경우에도 업캐스팅이 적용되며, 협력은 다형적이다.

## 05. 추상화와 유연성

---

### 추상화의 힘

- **할인 정책은 구체적인 금액 할인 정책과, 비율 할인 정책을 포괄하는 추상적 개념**이다.
    - 즉, `DiscountPolicy` 는 `AmountDiscountPolicy`, `PercentDiscountPolicy` 보다 추상적이다.
- **할인 조건도 마찬가지, 더 구체적인 순번 조건과 기간 조건을 포괄하는 추상적 개념**이다.
    - 즉, `DiscountCondition` 은 `SequenceCondition`, `PeriodCondition` 보다 추상적이다.

- 두 클래스가 더 추상적인 이유는, **인터페이스에 초점**을 맞추기 때문이다.
    - **`DiscountPolicy` 는 모든 할인 정책들이 수신 가능한 `calculateDiscountAmount` 메시지를 정의**한다.
    - **`DiscountCondition` 는 모든 할인 조건들이 수신 가능한 `isSatisfiedBy` 메시지를 정의**한다.
    - 둘 모두 **같은 계층에 소속된 클래스들이 공통으로 가질 수 있는 인터페이스를 정의**한다.
    - 또한 구현의 일부(추상 클래스), 또는 전체(자바 인터페이스)를 자식 클래스가 결정할 수 있도록 **결정권을 위임**한다.

![image](https://github.com/user-attachments/assets/9b11dfd6-6c5f-4713-985d-3faf97257d29)

- 위 그림은 추상화 사용 시 두 장점을 보여준다.
    1. **추상화 계층만 떼어 놓고 보면, 요구사항의 정책을 높은 수준에서 서술할 수 있다.**
    2. **추상화 이용 시 설계가 더 유연해진다.**

- 첫 장점부터 자세히 보면, **위 다이어그램을 하나의 문장으로 정리**하면 다음과 같이 정리할 수 있다.
    
    > **“영화 예매 요금은 최대 하나의 ‘할인 정책’, 다수의 ‘할인 조건’을 이용해 계산할 수 있다”**
    > 
    - 할인 정책, 할인 조건이라는 **추상적 개념들을 사용하여 문장을 서술**하였다.
    - 즉, **세부 내용을 무시**한 채 상위 정책을 쉽고 간단하게 표현한 것이다.
    - 어떨 땐 세부 사항을 설명해야 하는 경우도 있긴 하겠지만, 어쨌든 **필요에 따라 표현의 수준을 조정**하는 것이 가능하게 해준다.

- 추상화를 이용해 상위 정책을 기술한다는 것은, **기본 애플리케이션의 협력 흐름을 기술함을 의미**한다.
    - 영화 예메 가격을 계산하기 위한 흐름은 `Movie` 에서 `DiscountPolicy` 로, 그리고 다시 `DiscountCondition` 을 향해 흐른다.
    - 이후 **할인 정책, 할인 조건의 새 자식 클래스들은 추상화를 이용해 정의한 상위 협력 흐름을 그대로 따라야 한다.**
    - **매우 중요한 개념**인데, 이유는 **재사용 가능 설계의 기본**을 이루는 **디자인 패턴**이나 **프레임워크** 모두 **추상화를 이용해 상위 정책을 정의하는 객체지향 메커니즘**을 활용하고 있기 때문이다.

### 유연한 설계

- 두 번째 장점은 첫 번째 장점으로부터 쉽게 유추할 수 있다.
    - 추상화를 이용해 상위 정책을 표현하면, **기존 구조를 수정하지 않고도 기능을 쉽게 추가할 수 있다.**
    - 즉, **설계가 유연**해진다.

- 다른 영화와 달리 ‘스타워즈’의 할인 정책은 전혀 없다.
    - 즉, 할인 요금 계산 없이 영화 내 설정된 기본 금액을 그대로 사용하면 된다.

```kotlin
class Movie() {
    fun calculateMovieFee(screening: Screening): Money {
	    if (discountPolicy == null)
			   return fee
	    
		  return fee.minus(discountPolicy.calculateDiscountAmount(screening))
    }       
}
```

- 위와 같이 간단하게 구현할 수 있다.
    - 하지만, **이 문제점은 할인 정책이 없는 경우를 예외 케이스로 취급한다는 점**이다.
        - 이로 인해, **지금까지 일관성 있던 협력 방식이 무너진다.**
        - 기존엔 할인 금액을 계산하는 책임이 **`DiscountPolicy` 의 자식 클래스**에 있었다.
        - 하지만, 할인 정책이 없으니 **할인 금액이 0원임을 결정하는 책임이 `Movie` 쪽으로 옮겨져버렸다.**
    - **책임의 위치를 결정하기 위해 조건문을 사용하는 것은, 협력의 설계 측면에서 대부분의 경우 좋지 않은 선택**이다.
        - 항상 예외 케이스를 최소화하고, 일관성을 유지할 방법을 찾아야 한다.

```kotlin
class NoneDiscountPolicy(
    override val conditions: List<DiscountCondition> = emptyList(),
) : DiscountPolicy() {
    override fun getDiscountAmount(screening: Screening): Money =
        Money.ZERO
}
```

- 일관성을 유지할 수 있는 방법은, **0원이라는 할인 요금을 계산할 책임을 그대로 `DiscountPolicy` 계층에 유지시키는 것**이다.

```kotlin
val starWars = Movie(
        title = "스타워즈",
        runningTime = Duration.ofMinutes(210),
        fee = Money.wons(10000),
        discountPolicy = NoneDiscountPolicy()
    )
```

- 이전과 같이 `Movie` 인스턴스에 `NoneDiscountPolicy` 인스턴스를 연결하면, 할인되지 않는 영화를 생성할 수 있다.

- 중요점은, **`Movie`, `DiscountPolicy` 의 변경점없이 `NoneDiscountPolicy` 라는 새 클래스를 추가하여 애플리케이션 기능을 확장했다는 점**이다.
    - 이처럼, **추상화 중심으로 코드 구조를 설계하면 유연하고 확장 가능한 설계를 구축**할 수 있다.

![image](https://github.com/user-attachments/assets/5f517661-3659-4e58-ba1e-dbde138f53cc)

- 추상화가 유연한 설계를 가능케 하는 이유는, **설계가 구체적 상황에 결합되는 것을 방지**하기 때문이다.
    - `Movie` 는 특정 할인 정책에 묶여있지 않다.
    - `DiscountPolicy` 만 상속받는 클래스라면, 누구와도 협력이 가능하다.
    - 이는 **컨텍스트 독립성(context independency)**이라고 불리며, 프레임워크와 같이 유연한 설계가 필수적인 분야에서 진가가 발휘된다.

- 결론은 간단하다. **유연성이 필요한 곳에 추상화를 사용**하라.

### 추상 클래스와 인터페이스 트레이드오프

- `NoneDiscountPolicy` 의 코드를 자세히 살펴보면, **`getDiscountAmount` 가 어떤 값을 반환하더라도 상관이 없음을 알 수 있다.**
    - **`DiscountPolicy` 에서 할인 조건이 없을 경우, `getDiscountAmount` 메소드를 호출하지 않기 때문이다.**
    - 이는, **부모 클래스인 `DiscountPolicy` 와 자식 클래스인 `NoneDiscountPolicy` 를 개념적으로 결합**시킨다.
        - `NoneDiscountPolicy` 의 개발자는, `getDiscountAmount` 가 호출되지 않을 경우 부모 클래스인 `DiscountPolicy` 가 0원을 반환할 것이라는 사실을 가정하고 있기 때문이다.

- 해당 문제를 해결하기 위해선, **`DiscountPolicy` 를 인터페이스**로 바꿔야 한다.
    - 그리고 **`NoneDiscountPolicy` 가 `getDiscountAmount` 메소드가 아닌 `calculateDiscountAmount` 메소드를 오버라이딩하도록 변경**한다.

![image](https://github.com/user-attachments/assets/07e3b152-cfb7-44d3-8c2a-3027746747d5)

- 어떤 설계가 더 좋을까?
    - 이상적으로는, **인터페이스를 사용하도록 변경한 설계가 더 좋을 것**이다.
    - 현실적으론, `NoneDiscountPolicy` 만을 위해 인터페이스를 추가한 것이 과하단 생각이 들 수 있다.
    - 중요한 것은 **구현과 관련된 모든 것들이 트레이드오프의 대상이 될 수 있다는 점**이다.
        - **작성하는 모든 코드엔, 합당한 이유가 있어야 한다.**
        - 사소한 결정이라도, 트레이드오프를 통해 얻어진 결론과 아닌 결론엔 차이가 크다.

### 코드 재사용

- 코드를 재사용하기 위해 가장 널리 사용되는 방법은 분명 상속이다.
    - 하지만, **코드 재사용을 위해선 상속보단 합성이 더 나은 방법**이라는 이야기가 많다.

- **합성(composition)은 다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함하여 재사용하는 방법**을 말한다.
    - `Movie` 가 `DiscountPolicy` 의 코드를 재사용하는 방법이 바로 합성이다.

![image](https://github.com/user-attachments/assets/41b4c07e-39eb-40c4-a612-e2dd8ba6681b)

- 해당 설계를 위와 같이 **상속을 사용하도록 변경**할 수도 있다.
    - **`Movie` 를 직접 상속받아 기존 기능을 그대로 구현**시키면 된다.
    - 완벽히 동일하게 작동하지만, 그럼에도 상속 대신 합성이 선호되는 이유는 무엇일까?

### 상속

- 상속은 두 가지 관점에서 설계에 안좋은 영향을 미친다.
    1. **상속이 캡슐화를 위반한다.**
    2. **설계를 유연하지 못하게 만든다.**

- **가장 큰 문제는 캡슐화를 위반**한다는 것이다.
    - 상속 이용을 위해선, **부모 클래스의 내부 구조를 잘 알고 있어야 한다.**
    - `AmountDiscountMovie` 를 구현하는 개발자는, 부모 클래스인 **`Movie` 의 `calculateMovieFee` 메소드 내에서 추상 메소드인 `getDiscountAmount` 메소드를 호출**함을 알고 있어야 한다.
- **결과적으로 부모 클래스의 구현이 자식 클래스에 노출되므로 캡슐화가 약화**된다.
    - 이는 결국 자식 클래스가 부모 클래스에 강하게 결합되도록 만들기에, **부모 클래스 변경 시 자식 클래스도 함께 변경될 가능성을 높인다.**
    - **상속의 과도한 사용은, 코드를 변경하기 어렵게 만든다.**

- **두 번째 단점은, 설계가 유연하지 않다는 것**이다.
    - 상속에선 부모 클래스, 자식 클래스의 관계를 컴파일 시점에 결정한다.
    - **즉, 실행 시점에 객체 종류를 변경하는 것이 불가능하다.**

- 금액 할인 정책인 영화를 비율 할인 정책으로 바꾼다고 가정하자.
    - 상속 사용 시엔 다음과 같이 전개된다.
        - **`AmountDiscountMovie` 인스턴스를 `PerecentDiscountMovie` 인스턴스로 변경**해야 한다.
        - 이미 생성된 객체의 클래스는 변경할 수 없으므로, **최선은 `PerecentDiscountMovie` 의 인스턴스 생성 후, `AmountDiscountMovie` 의 상태를 복사하는 것**뿐이다.

```kotlin
class Movie(
    private var discountPolicy: DiscountPolicy
) {
    fun changeDiscountPolicy(discountPolicy: DiscountPolicy) {
        this.discountPolicy = discountPolicy
    }
}

val avatar = Movie(
    title = "아바타",
    runningTime = Duration.ofMinutes(120),
    fee = Money.wons(10000),
    discountPolicy = AmountDiscountPolicy(
        discountAmount = Money.wons(800),
        conditions = // . . .
    )
)

avatar.changeDiscountPolicy(PercentDiscountPolicy())
```

- 반면 기존 방법을 사용하면, **실행 시점에 할인 정책을 간단히 변경**할 수 있다.
    - 해당 예제를 통해 상속보다 인스턴스 변수로 관계를 연결한 기존 설계가 더 유연하다는 사실을 알 수 있다.

### 합성

- **`Movie` 는 요금을 계산하기 위해 `DiscountPolicy` 의 코드를 재사용**한다.
    - 상속은 부모 클래스, 자식 클래스의 코드를 컴파일 시점에 하나의 단위로 결합한다.
    - 하지만, **이 방법은 `DiscountPolicy` 인터페이스를 통해 약하게 결합**된다.
        - `Movie` 는 `DiscountPolicy` 가 `calculateDiscountAmount` 메소드를 제공한다는 사실만 알고 있다.
        
        → **이처럼, 인터페이스에 정의된 메시지를 통해서만 코드를 재사용하는 기법을 ‘합성’** 이라고 한다.
        
- 합성은 상속의 두 가지 문제를 모두 해결한다.
    - **인터페이스 내 정의된 메시지를 통해서만 재사용이 가능하기에, 구현을 효과적으로 캡슐화**한다.
    - 또한 **의존하는 인스턴스를 교체하는 것이 쉬우므로 설계를 유연**하게 만든다.
    - 상속은 클래스를 통해 강한 결합이 발생함에 비해, **합성은 메시지를 통해 느슨하게 결합**된다.
        
        **⇒ 고로, 코드 재사용을 위해선 상속보단 합성을 선호하는 것이 훨씬 좋다.**
        
- 그럼에도 상속을 반드시 사용하지 말라는 것은 아니다.
    - 대부분의 설계에선 상속, 합성을 함께 사용해야 한다.
        - `Movie`, `DiscountPolicy` 는 합성 관계이다.
        - `DiscountPolicy` 와 그 자식 클래스는 상속 관계이다.
    - 다형성을 위해 인터페이스를 재사용하는 경우, 상속 및 합성을 함께 조합해서 사용할 수 밖에 없다.

---

- 대부분의 사람들은 객체지향 프로그래밍 과정을 클래스 내에 속성과 메소드를 채워넣는 작업, 혹은 상속을 이용해 코드를 재사용하는 방법이라고 생각한다.
    - 물론 프로그래밍 관점에서 클래스, 상속은 매우 중요하다.
    - 하지만, 너무 프로그래밍 관점에 치우쳐 객체지향을 바라보면, 본질을 놓치기 쉽다.

- 객체지향 패러다임의 중심에는 객체가 존재한다.
    - **각 객체를 따로 떼어놓고 보는 것은 무의미**하다.
    - 가장 중요한 것은, **애플리케이션 기능을 구현하기 위해 협력에 참여하는 객체들 사이의 상호작용**이다.
    - 객체들은 협력에 참여하기 위해 역할을 부여받고, 역할에 적합한 책임을 수행한다.
- 객체지향 설계의 핵심은, 적절한 협력을 식별하고 협력에 필요한 역할을 정의한 후 역할을 수행할 수 있는 적절한 객체에게 적절한 책임을 할당하는 것이다.
