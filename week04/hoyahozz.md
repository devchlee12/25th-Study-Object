- 객체지향 설계 핵심은 역할, 책임, 협력이다.
- 역할, 책임, 협력 중 가장 중요한 것은 **책임**이다.
    - **책임이 할당되지 못한 상황에선 원활한 협력도 기대하기 어렵다.**
    - **역할은 책임의 집합이기에, 책임이 적절치 않으면 역할 역시 협력과 조화를 이루지 못한다.**
    - 즉, **책임이 곧 객체지향 애플리케이션의 전체 품질을 결정한다.**

> 객체지향 설계란 올바른 객체에게 올바른 책임을 할당하며, 낮은 결합도와 높은 응집도를 지닌 구조를 창조하는 활동이다.
> 
- **핵심은 책임**이며, **책임을 할당하는 작업이 곧 응집도와 결합도 같은 설계 품질과 깊이 연관**되어 있음을 의미한다.

- 설계는 변경을 대비하기 위해 존재하고, 어떤 방식으로든 비용이 발생한다.
- **훌륭한 설계란 합리적 비용 내에서 변경을 수용할 수 있는 구조를 만드는 것**이다.
    - 적합한 비용 내에서 쉽게 변경 가능한 설계는, 응집도가 높고 서로 느슨하게 결합된 요소로 구성된다.

- 결합도, 응집도를 합리적 수준으로 유지할 수 있는 중요 원칙이 있다.
    - **객체 상태가 아닌 객체 행동에 초점을 맞추는 것**이다.
    - 객체를 단순한 데이터 집합으로 바라보면, 내부 구현을 퍼블릭 인터페이스에 노출시키는 결과를 낳는다.
        - 즉, **설계가 변경에 취약**해진다.

- **객체의 책임에 초점을 맞추면, 객체와 객체 사이의 상호작용을 설계 중심으로 이동**시킬 수 있다.
    - 또한, **결합도가 낮고 응집도가 높으며 구현을 효과적으로 캡슐화하는 객체를 창조할 수 있는 기반**을 제공한다.

- 이번 장에서는, 상태를 표현하는 **데이터 중심의 나쁜 설계**를 살펴본다.
    - 객체지향 설계와 어떤 차이점이 있는지 살펴보며 통찰을 얻도록 한다.

## 01. 데이터 중심의 영화 예매 시스템

---

- 객체지향 설계에선 두 개의 방법을 이용해 시스템을 객체로 분할할 수 있다.
    1. **상태를 분할의 중심축으로 삼는 방법**
    2. **책임을 분할의 중심축으로 삼는 방법**

- **데이터 중심 관점**에서 객체는 자신이 포함하고 있는 **데이터를 조작하는 데 필요한 연산을 정의**한다.
    - 즉, 객체를 독립된 데이터 덩어리로 바라본다.
- 책임 중심의 관점에서 객체는 **다른 객체가 요청할 수 있는 연산을 위해 필요한 상태를 보관**한다.
    - **객체를 협력하는 공동체의 일원**으로 바라본다.

- 훌륭한 객체지향 설계는 데이터가 아닌 책임에 초점을 맞춰야 한다.
    - 이는 변경과 연관이 있다.
        - 객체 상태는 구현에 속하며, 구현은 불안정하기에 변경이 잦다.
    - 상태를 객체 분할의 중심축으로 삼으면, 구현에 관한 세부사항이 객체 내 인터페이스에 스며들게 되므로, **캡슐화의 원칙이 무너진다.**
        - 결과적으로 상태의 변경이 인터페이스의 변경을 초래한다.
        - 고로, 해당 인터페이스에 의존하는 모든 객체에게 변경의 영향이 퍼진다.
    
    **⇒ 따라서, 데이터 중심 설계는 변경에 매우 취약하다.**
    

- 반대로 책임을 객체 분할의 중심축으로 삼으면, 책임 수행에 필요한 상태를 캡슐화할 수 있게 된다.
    - 이로 인해 구현 변경에 대한 파장이 외부로 퍼져나가는 것을 방지한다.
    - 결과적으로 변경에 안정적인 설계를 얻을 수 있다.

### 데이터를 준비하자

- **객체가 내부에 저장해야 하는 데이터가 무엇인가**를 묻는 것으로, 데이터 중심 설계를 시작한다.

```kotlin
data class Movie(
    val title: String,
    val runningTime: Duration,
    val fee: Money,
    
    // 할인 조건 목록
    val discountConditions: List<DiscountCondition>,
    
    val movieType: MovieType,
    
    // 할인 금액
    val discountAmount: Money,
    
    // 할인 비율
    val discountPercent: Double,
)
```

- **`Movie` 에 저장될 데이터를 결정하면, 위와 같이 구현**할 수 있다.
    - 기존 책임 중심 설계와의 차이를 두 가지 발견할 수 있다.
        - **할인 조건 목록이 `Movie` 내에 변수로 포함되어 있다는 것**이다.
        - 또한 **할인 정책을 결정하기 위해 할인 금액과 할인 비율을 `Movie` 내에서 직접 정의**하고 있다.

- 할인 정책은 영화별로 하나만 지정할 수 있기에, **`discountAmount`**, **`discountPercent`** 중 하나만 사용될 수 있다.
    - 이 때 영화에 사용된 할인 정책의 종류를 파악하기 위해, **`MovieType`** 을 사용한다.

```kotlin
enum class MovieType {
    AMOUNT_DISCOUNT, // 금액 할인 정책
    PERCENT_DISCOUNT, // 비율 할인 정책
    NONE_DISCOUNT; // 미적용
}
```

- `movieType` 의 값에 따라, `Movie` 내에서 할인 정책이 결정되며 이에 따라 할인이 진행된다.

- **말그대로 데이터 중심 접근**으로, **`Movie` 가 할인액을 계산하는 데 필요한 데이터가 무엇인가?** 에 초점이 맞춰져 있다.
    - 금액 할인 정책의 경우 할인 금액이, 비율 할인 정책의 경우 할인 비율이 필요하다.
        - 이를 위해 각각 `discountAmount`, `discountPercent` 라는 값으로 표현한다.
    - 예매 가격을 계산하기 위해선 `Movie` 에 설정된 할인 정책을 파악해야 한다.
        - `MovieType` 을 정의하여 `Movie` 에 포함시킨다.

- 객체 책임을 결정하기 전, **필요한 데이터가 무엇인지 계속 고민**한다면, 이러한 데이터 중심 설계에 매몰되어 있을 가능성이 크다.
    - 데이터 중심 설계에선 아래 두 가지 인스턴스를 흔히 볼 수 있다.
        - `MovieType` 과 같이 객체 종류를 저장하는 인스턴스 변수
        - `discountAmount`, `discountPercent` 와 같이 인스턴스 종류에 따라 배타적으로 사용되는 인스턴스 변수

```kotlin
data class Movie(
    val title: String,
    val runningTime: Duration,
    var fee: Money,
    var discountConditions: List<DiscountCondition>,

    var movieType: MovieType,
    var discountAmount: Money,
    var discountPercent: Double,
)
```

- 데이터를 준비했고, 캡슐화를 준수하기 위해 내부 데이터가 객체의 엷은 막을 빠져나가 외부 다른 객체를 오염시키는 것을 막아야 한다.
    - 이를 위해 **내부 데이터를 반환하는 접근자(accessor)와 데이터를 변경하는 수정자(modifier)를 추가**한다.
    - (코틀린에서는 단순히 `var` 로 설정해주면, 접근 및 수정이 모두 가능하다.)

- 필요한 데이터도 모두 결정했고, 내부 데이터를 캡슐화하는 데도 성공했으므로 이제 **할인 조건을 구현**한다.
    - 도메인에는 순번 조건, 기간 조건이라는 두 가지 종류의 할인 조건이 존재한다.
    - **할인 조건을 구현하는 데 필요한 데이터는 무엇인가?**

```kotlin
enum class DiscountConditionType {
    SEQUENCE, // 순번 조건
    PERIOD; // 기간 조건
}
```

- 우선 할인 조건의 타입을 저장할 `enum` 클래스를 선언한다.

```kotlin
data class DiscountCondition(
    var type: DiscountConditionType,
    
    // 상영 순번
    var sequence: Int,
    var dayOfWeek: DayOfWeek,
    var startTime: LocalTime,
    var endTime: LocalTime,
)
```

- `DiscountCondition` 을 통해 할인 조건을 구현한다.
    - `type` 이 추가되었고, 순번 조건에서만 사용되는 상영 순번, 기간 조건에서만 사용되는 시간 데이터를 포함한다.

```kotlin
// 상영
data class Screening(
    var movie: Movie,
    var sequence: Int,
    var whenScreened: LocalDateTime,
)
```

- `Screening` 클래스 역시 어떤 데이터를 포함해야 하는지를 우선적으로 결정한다.

```kotlin
data class Reservation(
    var customer: Customer,
    var screening: Screening,
    var fee: Money,
    var audienceCount: Int,
)
```

- 영화 예매 시스템의 목적은 결국 영화를 예매하는 것이기에, `Reservation` 클래스를 추가한다.

```kotlin
data class Customer(
    var id: String,
    var name: String,
)
```

- 유저 데이터를 저장할 `Customer` 도 구현한다.

![image](https://github.com/user-attachments/assets/6cfc6fa4-b8b9-4670-86bb-691c37882aae)

- 모든 데이터가 준비되었으니, 이제 영화를 예매하기 위한 절차를 구현하자.

### 영화를 예매하자

```kotlin
class ReservationAgency {
    fun reserve(
        screening: Screening,
        customer: Customer,
        audienceCount: Int,
    ): Reservation {
        val movie = screening.movie
        var discountable = false
        val fee: Money

        // 영화 할인 여부 판단
        for (condition in movie.discountConditions) {
            // 기간 조건이라면 기간을 이용해 적용 여부 판단
            if (condition.type == DiscountConditionType.PERIOD) {
                discountable =
                    screening.whenScreened.dayOfWeek.equals(condition.dayOfWeek)
                            && condition.startTime <= screening.whenScreened.toLocalTime()
                            && condition.endTime >= screening.whenScreened.toLocalTime()
            } else {
                // 순번 조건이라면 상영 순번을 이용해 적용 여부 판단
                discountable = condition.sequence == screening.sequence
            }

            if (discountable)
                break
        }

				// 만족하는 할인 조건이 있는 경우
        if (discountable) {
		        // 할인 정책에 따라 할인 요금 계산
            val discountAmount = when (movie.movieType) {
                MovieType.AMOUNT_DISCOUNT -> movie.discountAmount
                MovieType.PERCENT_DISCOUNT -> movie.fee * movie.discountPercent
                MovieType.NONE_DISCOUNT -> Money.ZERO
            }

            fee = movie.fee - discountAmount
        } else {
            fee = movie.fee
        }

        return Reservation(customer, screening, fee, audienceCount)
    }
}

```

- 영화 예매 시스템을 데이터 중심으로 설계하는 방법에 대해 알아보았다.
    - 이제, 이 설계를 책임 중심 설계 방법과 비교하며 두 방법의 장단점을 파악해보자.

## 02. 설계 트레이드오프

---

- 객체지향 커뮤니티에선 오랜 기간 좋은 설계의 특징을 판단할 수 있는 기준에 대해 논의해왔다.
    - 여기선 **캡슐화, 응집도, 결합도를 사용**한다.

### 캡슐화

- 상태, 행동을 하나의 객체 안에 모으는 이유는 **객체 내 구현을 외부로부터 감추기 위해서**다.
    - **구현**이란, **나중에 변경될 가능성이 높은 어떤 것을 의미**한다.
    - 객체를 사용하면 **변경 가능성이 높은 부분은 내부에 숨기고, 외부엔 상대적으로 안정적인 부분만 공개하여 변경의 여파를 통제**할 수 있다.

- **변경될 가능성이 높은 부분**을 **구현**, **상대적으로 안정적인 부분**을 **인터페이스**라 부른다는 사실을 기억하라.
    - 객체 설계의 기본 아이디어는, 변경 정도에 따라 두 부분을 분리하는 것이다.
    - 그리고 **외부에서는 인터페이스에만 의존하도록 관계를 조절**한다.

- 객체지향의 가장 중요한 원리는 **캡슐화**다.
    - 불안정한 구현 세부사항을 안정적인 인터페이스 뒤로 캡슐화할 수 있다.

> 복잡성을 다루기 위한 가장 효과적인 도구는 추상화다. 다양한 추상화 유형을 사용할 수 있지만, 객체지향 프로그래밍에서 복잡성을 취급하는 주요한 추상화 방법은 캡슐화다. 그러나 객체지향 언어를 사용한다고 해서 애플리케이션의 복잡성이 잘 캡슐화될 것이라고 보장할 수는 없다. 훌륭한 프로그래밍 기술을 적용하여 캡슐화를 향상시킬 순 있으나, **객체지향 프로그래밍을 통해 전반적으로 얻을 수 있는 장점은 오직 설계 과정 동안 캡슐화를 목표로 인식할 때만 달성**될 수 있다. [Wirfs-Brock89]
> 

- 설계가 필요한 이유는 요구사항이 변경되기 때문이고, **캡슐화가 중요한 이유는 불안정한 부분과 안정적인 부분을 분리하여 변경의 영향을 통제할 수 있기 때문**이다.
    - **변경될 수 있는 어떤 것이든 캡슐화해야 하고, 이 부분이 바로 객체지향 설계의 핵심**이다.

> 유지보수성이란 두려움, 주저함, 저항감 없이 코드를 변경할 수 있는 능력을 의미한다. 가장 중요한 동료는 캡슐화다. 우리는 시스템의 한 부분을 다른 부분으로 감춰 뜻 밖의 피해가 발생할 수 있는 가능성을 사전에 방지할 수 있다. **만약 시스템이 완전히 캡슐화된다면, 우리는 변경으로부터 완전히 자유로워질 것**이다.

만약 시스템의 캡슐화가 크게 부족하다면, 변경으로부터 자유로울 수 없고, 결과적으로 시스템은 진화할 수 없을 것이다. 응집도, 결합도, 중복 역시 변경가능한, 훌륭한 코드를 규정하는 데 핵심적인 품질인 것이 사실이긴 하나, **캡슐화는 우리를 제일 좋은 코드로 안내하기에 가장 중요한 제 1원칙**이다.
> 

### 응집도와 결합도

- **응집도**는 **모듈에 포함된 내부 요소들이 연관돼 있는 정도**를 나타낸다.
    - 묘듈 내 요소들이 한 목적을 위해 **긴밀히 협력**한다면, 그 모듈은 높은 응집도를 지닌다.
    - 객체지향 관점에서 **응집도는 객체 혹은 클래스에 얼마나 관련 높은 책임을 할당**했는지를 나타낸다.

- **결합도**는 **의존성의 정도를 나타내며, 다른 모듈에 대해 얼마나 많은 지식을 갖고 있는지를 나타낸 척도**이다.
    - 어떤 모듈이 다른 모듈에 대해 꼭 필요한 지식만 가지고 있다면, 두 모듈은 낮은 결합도를 가진다.
    - 객체지향 관점에서 **결합도는 객체 혹은 클래스가 협력에 필요한 적합한 수준의 관계만을 유지**하고 있는지를 나타낸다.

- 일반적으로 **좋은 설계란 높은 응집도와 낮은 결합도를 가진 모듈로 구성된 설계**를 의미한다.
    - 좋은 설계란 오늘의 기능을 수행하며 내일의 변경을 수용할 수 있는 설계다.
    - 응집도와 결합도는 결국, 변경과 관련된 것이다.

- **변경의 관점에서 응집도란, 변경이 발생할 때 모듈 내에서 발생하는 변경의 정도**로 측정할 수 있다.
    - 쉽게 이야기해 **한 변경의 수용**을 위해 모듈 전체가 함께 변경된다면, 응집도가 높은 것이다.
        - 모듈의 일부만 변경된다면, 응집도가 낮은 것이다.
    - 또한 **하나의 변경**에 대해 하나의 모듈만 변경된다면, 응집도가 높은 것이다.
        - 다수의 모듈이 함께 변경되어야 한다면, 응집도가 낮은 것이다.

![image](https://github.com/user-attachments/assets/a25cb9c1-a542-484a-b6d7-222c0a20a48c)

- 위 그림에서 음영으로 칠해진 부분은, 변경 발생 시 수정되는 영역을 표현한 것이다.
    - 응집도가 높은 설계에선 **한 요구사항의 변경을 반영하기 위해 오직 하나의 모듈만 수정**하면 된다.
    - 반면 낮은 설계에선 변경 부분이 다수 모듈에 분산되어 있기에, **여러 모듈을 동시에 수정**해야만 한다.

- **응집도가 높을수록 변경 대상과 범위가 명확**해지기에, **코드 변경이 쉬워진다.**
    - 변경으로 인해 수정되는 부분의 파악을 위해 여러 모듈을 동시에 수정할 필요가 없다.
    - 변경의 반영을 위해선 **오직 하나의 모듈만 수정**하면 된다.

- **변경의 관점에서 결합도란, 한 모듈이 변경되기 위해 다른 모듈의 변경을 요구하는 정도**로 측정할 수 있다.
    - 쉽게 이야기해 **한 모듈을 수정할 때 얼마나 많은 모듈을 함께 수정해야 하는지**를 나타낸다.
    - 따라서, 결합도가 높을수록 함께 변경해야 하는 모듈의 수가 늘어나기에, 변경이 어려워진다.

![image](https://github.com/user-attachments/assets/4e7606ab-e3c3-4f6e-90eb-76ed86ae0dd5)

- 위 그림에서 낮은 결합도를 가진 왼쪽 설계에선 **모듈 A를 변경했을 때 오직 하나의 모듈만 영향을 받는다는 사실**을 알 수 있다.
- 반면 높은 결합도의 오른쪽 설계에선 모듈 A 변경 시 **동시에 4개의 모듈을 변경**해야 함을 알 수 있다.

- 영향을 받는 모듈의 수 외에 **변경의 원인**을 이용해 결합도의 개념을 설명할 수도 있다.
    - **내부 구현을 변경했을 때 다른 모듈에 영향을 미치는 경우, 두 모듈 사이 결합도가 높다**고 표현한다.
    - 반면 **퍼블릭 인터페이스를 수정했을 때만 다른 모듈에 영향을 미친다면, 결합도가 낮다**고 표현한다.
    - 고로 클래스의 구현이 아닌 인터페이스에만 의존하도록 코드를 작성해야, 낮은 결합도를 얻을 수 있다.
        - **“이는 인터페이스에 대해 프로그래밍하라[GOF94]”** 라는 격언으로도 잘 알려져 있다.

- 물론 결합도가 높아도 상관없는 경우가 있다.
    - **변경될 확률이 매우 적은 안정적인 모듈에 의존하는 것은 전혀 문제가 되지 않는다.**
        - 자바의 `String`, `ArrayList` 와 같이 변경 확률이 매우 낮은 표준 라이브러리, 프레임워크에 의존하는 것은 문제가 없다.
    - 하지만 **직접 작성한 코드는, 불안정하고 언제라도 변경될 가능성이 높다.**
        - 코드 내 버그가 존재할 수도, 요구사항이 변경될 수도 있다.
        - **직접 작성한 코드는 최대한 낮은 결합도를 유지하려 노력**해야 한다.

- 다시 한 번 강조하지만, **응집도와 결합도는 변경과 관련이 깊다.**
    - 응집도와 결합도를 변경의 관점에서 바라보면, 설계에 대한 시각을 크게 변화시킬 수 있다.

- 마지막으로 **캡슐화의 정도가 응집도, 결합도에 영향을 미친다는 사실**도 잊지 말자.
    - 캡슐화를 지키면 모듈 내 응집도는 높아지고, 모듈 사이의 결합도는 낮아진다.
    - 따라서 **응집도, 결합도를 생각하기 전에 먼저 캡슐화를 향상시키기 위해 노력**해야 한다.

## 03. 데이터 중심의 영화 예매 시스템의 문제점

---

- 기능적인 측면만 보면, 데이터 중심 설계와 책임 중심 설계는 완전히 동일하다.
    - 하지만 설계 관점에서 보았을 때, 캡슐화를 다루는 방식은 매우 다르다.

- 데이터 중심 설계는 캡슐화를 위반하고, 객체 내부 구현을 인터페이스의 일부로 만든다.
- 반면 책임 중심 설계는 객체의 내부 구현을 안정적인 인터페이스 뒤로 **캡슐화**한다.

- 데이터 중심 설계는 캡슐화를 위반하기 쉽기에, **응집도가 낮고 결합도가 높은 객체들을 양산하게 될 가능성이 크다.**

### 캡슐화 위반

```kotlin
data class Movie(
    val title: String,
    val runningTime: Duration,
    var fee: Money,
    var discountConditions: List<DiscountCondition>,

    var movieType: MovieType,
    var discountAmount: Money,
    var discountPercent: Double,
)
```

- **`Movie`** 클래스를 보면, 오직 **메소드를 통해서만 객체 내부 상태에 접근**할 수 있음을 알 수 있다.
    - (코틀린에선 프로퍼티를 활용한다.)
- 직접 내부에 접근할 수 없기에 캡슐화를 지키는 것처럼 보이나, 실상은 그렇지 못하다.
    - `getFee`, `setFee` 메소드는 내부에 **`Money` 타입의 `Fee` 라는 이름의 변수가 있음을 퍼블릭 인터페이스에 노골적으로 드러낸다.**
    - 이는 객체가 수행할 책임이 아닌, 내부에 저장할 데이터에 초점을 맞췄기 때문이다.

- 객체에게 중요한 것은 **책임**이며, **구현을 캡슐화할 수 있는 적절한 책임은 협력이란 문맥을 고려**할 때만 얻을 수 있다.
    - 설계 시 협력에 대해 고민하지 않으면, 캡슐화를 위반하는 접근자, 수정자를 가지게 되는 경향이 있다.
    - **객체가 사용될 문맥을 추측할 수밖에 없으면, 결국 개발자는 어떤 상황에서도 객체가 최대한 사용될 수 있게 많은 접근자 메소드를 추가**하게 되는 것이다.

- 이와 같이 접근자, 수정자에 과도하게 의존하는 설계 방식을 **추측에 의한 설계(design-by-guessing strategy)** 라고 부른다.
    - 객체가 사용될 협력을 고려하지 않고, 다양한 상황에서 사용될 것이라는 막연한 추측을 기반으로 설계를 진행한다.
    - 결과적으로 내부 구현이 퍼블릭 인터페이스에 노출되며, 캡슐화를 위반하게 된다.

### 높은 결합도

- **캡슐화를 위반하니, 결과적으로 객체 내부 구현을 변경하면 많은 부분에 변경이 생길 것**임을 추측할 수 있다.

```kotlin
class ReservationAgency {
    fun reserve(
        screening: Screening,
        customer: Customer,
        audienceCount: Int,
    ): Reservation {
		    // . . .
        val fee: Money

				// 만족하는 할인 조건이 있는 경우
        if (discountable) {
						// . . .
            fee = movie.fee - discountAmount
        } else {
            fee = movie.fee
        }

				// . . .
    }
}

```

- **`ReservationAgency`** 는, 한 명의 예매 요금을 계산하기 위해 **`Movie` 의 `getFee` 메소드를 호출**하고, 결과를 **`Money` 타입의 `fee` 에 저장**한다.
    - 이 때 `fee` 의 타입을 변경한다면 다음과 같은 현상이 발생한다.
        1. `getFee` 메소드의 반환 타입도 함께 수정해야 한다.
        2. `ReservationAgency` 의 구현도 변경 타입에 맞게 함께 수정해야 한다.

- `fee` 의 타입 변경으로 인해 협력 클래스가 변경되기에, **`getFee` 는 결국 `fee` 를 정상적으로 캡슐화하지 못한다.**
    - 결국 `getFee` 메소드의 사용은, 인스턴스 변수 `fee` 의 가시성을 `public` 으로 변경한 것과 거의 동일하다.
    - 이처럼 **데이터 중심 설계는 객체의 캡슐화를 약화시키기에, 클라이언트가 객체 구현에 강하게 결합**된다.

![image](https://github.com/user-attachments/assets/034c16e5-44cb-4566-a267-19321938c2ea)

- 결합도 측면에서 바라보았을 땐, **여러 데이터 객체들을 사용하는 제어 로직이 특정 객체 안에 집중되는 것**이 문제다.
    - 하나의 제어 객체가 다수의 데이터 객체에 강하게 결합되고, 이 결합도로 인해 어떤 데이터 객체를 변경하더라도 제어 객체 역시 함께 변경될 수밖에 없다.
    - 위 그림을 보면, **결국 시스템 내에서 하나라도 변경이 일어나면 `ReservationAgency` 의 변경을 유발**함을 볼 수 있다.

- **데이터 중심 설계는, 전체 시스템을 하나의 거대한 의존성 덩어리로 만들어 버린다.**
    - 어떤 변경이라도 발생하고 나면, 결국 시스템 전체가 요동칠 수밖에 없다.

### 낮은 응집도

- **서로 다른 이유로 변경되는 코드가 한 모듈 내에 공존할 때, 모듈의 응집도가 낮다고 표현**한다.
    - 고로 각 모듈의 응집도를 살펴보기 위해선 코드를 수정하는 이유가 무엇인지 봐야 한다.

- **`ReservationAgency`** 를 예로 변경, **응집도 사이 관계**를 살펴보자.
    - **다음과 같은 수정사항이 발생하면, `ReservationAgency` 코드를 수정**할 것이다.
        1. 할인 정책이 추가될 경우
        2. 할인 정책별 할인 요금을 계산하는 방법이 변경될 경우
        3. 할인 조건이 추가되는 경우
        4. 할인 조건별로 할인 여부를 판단하는 방법이 변경될 경우
        5. 예매 요금을 계산하는 방법이 변경될 경우

- 낮은 응집도는 두 가지 측면에서 설계에 문제를 일으킨다.
    1. **변경의 이유가 서로 다른 코드들을 한 모듈 내에 뭉쳐놓았기에, 변경과 아무 상관 없는 코드들이 영향을 받게 된다.**
        - `ReservationAgency` 내에 할인 정책, 할인 조건 관련 코드가 함께 존재했다.
            - 이로 인해, 새 할인 정책을 추가하는 작업이 할인 조건에도 영향을 미칠 수 있다.
        - 코드 수정 후 아무 상관없던 코드에 문제가 발생하는 것은, 모듈 응집도가 낮을 때 발생하는 대표적 증상이다.
    2. **한 요구사항의 변경을 반영하기 위해, 동시에 여러 모듈을 수정해야만 한다.**
        - 새 할인 정책을 추가한다고 가정하면 정말 많은 일이 발생한다.
            - `MovieType`, `ReservationAgency`, `DiscountConditionType` 모두 변경이 발생한다.
    
- 현재 설계는 새 할인 정책을 추가하거나 할인 조건을 추가하기 위해 **하나 이상의 클래스를 동시에 수정**해야만 한다.
    - 이는 **응집도가 낮다는 증거**다.

## 04. 자율적인 객체를 향해

---

### 캡슐화를 지켜라

- **캡슐화는 설계의 제 1원리**다.
    - 객체는 **자신이 어떤 데이터를 가지고 있는지를 캡슐화하고, 외부에 공개해선 안된다.**
    - 객체는 **스스로의 상태를 책임져야 하며, 외부에선 인터페이스에 정의된 메소드를 통해서만 상태에 접근**할 수 있어야 한다.

- 여기서 메소드는 단순히 속성 하나의 값을 반환 혹은 변경하는 접근자, 수정자를 의미하는게 아니다.
    - **의미 있는 메소드는, 객체가 책임져야 하는 무언가를 수행하는 메소드**다.

```kotlin
data class Rectangle(
    private var left: Int,
    private var top: Int,
    private var right: Int,
    private var bottom: Int,
)
```

- **`Rectangle`** 을 예로 좀 더 쉽게 이해해보자.
    - 사각형의 너비, 높이를 증가시키는 코드가 필요하다고 가정해보자.

```kotlin
class AnyClass {
    fun anyMethod(rectangle: Rectangle, multiple: Int) {
        rectangle.right *= multiple
        rectangle.bottom *= multiple
    }
}
```

- 위 코드에는 많은 문제점이 있다.
    1. **‘코드 중복’이 발생할 확률이 높다.**
        - 다른 곳에서도 사각형 너비, 높이를 증가시켜야 한다면 `getRight`, `getBottom` 메소드를 호출한 후, 수정자 메소드를 이용해 값을 설정할 것이다.
        - 코드 중복은 악의 근원이므로, 중복을 초래할 수 있는 모든 원인을 제거하는 것이 좋다.
    2. **‘변경에 취약’하다.**
        - `right`, `bottom` 대신 `length`, `height` 를 이용해 사각형을 표현하도록 수정한다면 어떨까?
        - 결과적으로 `getRight`, `setRight` 등의 메소드를 모두 `getLength`, `setLength` 등으로 변경해야 한다.
        - 기존 접근자 메소드를 사용하던 모든 코드에 영향을 미친다.

```kotlin
data class Rectangle(
    private var left: Int,
    private var top: Int,
    private var right: Int,
    private var bottom: Int,
) {
    fun enlarge(multiple: Int) {
        right *= multiple
        bottom * multiple
    }
}
```

- 해결을 위해, **캡슐화를 강화**해보자.
- `Rectangle` 내부에 너비, 높이를 조절하는 로직을 캡슐화하면 두 문제를 해결할 수 있다.
    - **`Rectangle` 의 변경 주체를 외부 객체에서, `Rectangle` 로 이동**시켰다.
    - 즉, 자신의 크기를 스스로 증가시키도록 **책임을 이동시킨 것**이다.
    
    ⇒ **이것이 바로, 객체가 자기 스스로를 책임진다는 말의 의미다.**
    

### 스스로 자신의 데이터를 책임지는 객체

- 상태, 행동을 객체라는 한 단위로 묶는 이유는 결국 **객체 스스로 자신의 상태를 처리할 수 있게 하기 위함**이다.
    - 객체는 단순 데이터 제공자가 아니다.
    - 객체 내 저장되는 데이터보단, **객체가 협력에 참여하면서 수행할 책임을 정의하는 연산이 더 중요**하다.

- 고로, 객체 설계 시 **“이 객체가 어떤 데이터를 포함하는가?”** 라는 질문은 다음 두 개의 개별 질문으로 분리해야만 한다.
    - **이 객체가 어떤 데이터를 포함해야 하는가?**
    - **이 객체가 데이터에 대해 수행해야 하는 연산은 무엇인가?**
- 두 질문을 조합하면, **객체 내 상태를 저장하는 방식과 저장된 상태에 대해 호출 가능한 연산 집합**을 얻을 수 있다.

- 다시 영화 예매 시스템으로 돌아가, **`ReservationAgency` 로 새어나간 데이터에 대한 책임을 실제 데이터를 포함하고 있는 객체**로 옮겨보자.

```kotlin
data class DiscountCondition(
    var type: DiscountConditionType,
    var sequence: Int,
    var dayOfWeek: DayOfWeek,
    var startTime: LocalTime,
    var endTime: LocalTime,
)
```

- 할인 조건을 표현하는 `DiscountCondition` 에서 시작한다.
    - 첫 질문인, 어떤 데이터를 관리해야 하는지는 이미 이전에 위와 같이 데이터를 결정해놓았다.

- 두 번째 질문은 **이 데이터에 대해 수행할 수 있는 오퍼레이션**이 무엇인지 묻는 것이다.
    - 할인 조건에선 순번 조건, 기간 조건의 두 종류가 존재함을 기억하자.

```kotlin
fun isDiscountable(dayOfWeek: DayOfWeek, time: LocalTime): Boolean {
    require(type == DiscountConditionType.PERIOD)

    return this.dayOfWeek == dayOfWeek
            && this.startTime <= time
            && this.endTime >= time
}

fun isDiscountable(sequence: Int): Boolean {
    require(type == DiscountConditionType.SEQUENCE)
    return this.sequence == sequence
}
```

- 두 가지 할인 조건을 판단할 수 있도록, **`isDiscountable`** 메소드가 필요하다.
    - `type` 을 이용해 현재 할인 조건 타입에 맞는 적합한 메소드가 호출됐는지 판단한다.

```kotlin
data class Movie(
    val title: String,
    val runningTime: Duration,
    var fee: Money,
    var discountConditions: List<DiscountCondition>,

    var movieType: MovieType,
    var discountAmount: Money,
    var discountPercent: Double,
)
```

- `Movie` 역시 어떤 데이터를 가지고 있어야 하는지는 이미 알고 있다.
    - 그렇다면 해당 데이터를 처리하기 위해 어떤 연산이 필요할까?
        - 영화 요금을 계산하는 연산
        - 할인 여부를 판단하는 연산

- 요금 계산 연산부터 구현하자.
    - 계산을 위해선 할인 정책을 염두에 두어야 한다.
    - 할인 정책에는 금액 할인, 비율 할인, 할인 미적용, 세 가지 타입이 존재한다.

```kotlin
data class Movie(
		// . . .
    var movieType: MovieType,
    // . . .
) {
    fun calculateAmountDiscountedFee(): Money {
        require(movieType == MovieType.AMOUNT_DISCOUNT)
        return this.fee.minus(discountAmount)
    }

    fun calculatePercentDiscountedFee(): Money {
        require(movieType == MovieType.PERCENT_DISCOUNT)
        return this.fee.minus(fee.times(discountPercent))
    }

    fun calculateNoneDiscountedFee(): Money {
        require(movieType == MovieType.NONE_DISCOUNT)
        return fee
    }
}
```

- 따라서 할인 정책의 타입을 반환하는 `getMovieType` 메소드와, 정책별로 요금을 계산하는 세 가지 메소드를 구현해야 한다.

```kotlin
fun isDiscountable(whenScreened: LocalDateTime, sequence: Int): Boolean {
    for (condition in discountConditions) {
		    // 기간 조건
        if (condition.type == DiscountConditionType.PERIOD) {
            if (condition.isDiscountable(whenScreened.dayOfWeek, whenScreened.toLocalTime())) {
                return true
            }
        } else {
		        // 순번 조건
            if (condition.isDiscountable(sequence)) {
                return true
            }
        }
    }

    return false
}
```

- `DiscountCondition` 목록을 포함하고 있기에, 할인 여부를 판단하는 연산 역시 포함시켜야 한다.
    - 기간 조건을 판단하기 위해 필요한 `dayOfWeek`, `whenScreen` 를 파라미터로 전달한다.
    - 순번 조건의 만족 여부를 판단하기 위해 `sequence` 를 파라미터로 전달한다.

```kotlin
data class Screening(
    var movie: Movie,
    var sequence: Int,
    var whenScreened: LocalDateTime,
) {
    fun calculateFee(audienceCount: Double): Money {
        return when (movie.movieType) {
            MovieType.AMOUNT_DISCOUNT ->
                movie.calculateAmountDiscountedFee().times(audienceCount)

            MovieType.PERCENT_DISCOUNT -> {
                if (movie.isDiscountable(whenScreened, sequence)) {
                    return movie.calculatePercentDiscountedFee().times(audienceCount)
                } else {
                    return movie.calculateAmountDiscountedFee().times(audienceCount)
                }
            }

            MovieType.NONE_DISCOUNT ->
                movie.calculateNoneDiscountedFee().times(audienceCount)
        }
    }
}
```

- `Screening` 은 `Movie` 가 금액 할인 정책이나 비율 할인 정책을 지원할 경우, `Moive` 의 `isDiscountable` 메소드를 호출한다.
    - 이후 할인이 가능한지 여부를 판단하고, 적합한 `Movie` 메소드를 호출해 요금을 계산한다.

```kotlin
class ReservationAgency {
    fun reserve(
        screening: Screening,
        customer: Customer,
        audienceCount: Int,
    ): Reservation {
        val fee = screening.calculateFee(audienceCount)
        return Reservation(customer, screening, fee, audienceCount)
    }
}
```

- `ReservationAgency` 에서는 `Screening` 의 `calculateFee` 메소드로 예매 요금을 계산한다.
- 계산된 요금을 바탕으로 `Reservation` 을 생성한다.

![image](https://github.com/user-attachments/assets/0d38a6ad-4748-43c4-98ee-4fe48f4222e7)

- 두 번째 설계가 종료되었다.
    - **최소한 결합도 측면에서 `ReservationAgency` 에 몰려있던 첫 설계보단 개선된 것**으로 보인다.
    - 이는, **두 번째 설계가 첫 번째 설계보다 내부 구현을 더 면밀히 캡슐화하고 있기 때문**이다.
        - 즉, 데이터를 처리하는 데 필요한 메소드를 데이터를 가지고 있는 객체 스스로 구현 중이다.
        - 고로, **이 객체들은 스스로를 책임진다**고 말할 수 있다.

## 05. 하지만 여전히 부족하다

---

- 캡슐화 관점에서 개선된 것은 사실이나, 만족스럽진 못하다.
    - 본질적으론 두 번째 설계 역시 **데이터 중심의 설계 방식**에 속하고, **문제는 여전히 발생**한다.

### 캡슐화 위반

```kotlin
data class DiscountCondition(
    var type: DiscountConditionType,
    var sequence: Int,
    var dayOfWeek: DayOfWeek,
    var startTime: LocalTime,
    var endTime: LocalTime,
) {
    fun isDiscountable(dayOfWeek: DayOfWeek, time: LocalTime): Boolean {
			// . . .
    }

    fun isDiscountable(sequence: Int): Boolean {
			// . . .
    }
}
```

- 분명 수정된 객체들은 **자신의 데이터를 스스로 처리**한다.
    - 객체지향이 자신의 상태를 스스로 관리하는 자율적 객체를 지향하는 것이라고 하면, 이에 부합하는 것처럼 보인다.

- 하지만 위 코드에서 두 개의 `isDiscountable` 메소드를 자세히 보면, 이상한 점이 눈에 띈다.
    - 기간 조건을 판단하는 첫 번째 `isDiscountable` 의 시그니처를 살펴보면, 아래 두 가지 정보를 파라미터로 받고 있다.
        - `DiscountCondition` 내 속성으로 포함된 `DayOfWeek` 타입의 요일 정보
        - `LocalTime` 타입의 시간 정보
    - 이는 **결국 두 정보가 인스턴스 변수로 포함되어 있다는 사실을 인터페이스를 통해 외부로 노출**하고 있는 것이다.

- 두 번째 메소드 역시 객체가 **`int` 타입의 순번 정보를 포함**하고 있음을 외부에 노출하고 있다.
- `getType` 메소드 역시 내부에 **`DiscountConditionType`** 을 포함하고 있다는 정보를 노출하고 있다.

- **`DiscountCondition` 의 속성을 변경해야 한다**면 어떻게 될까?
    - 두 **`isDiscountable` 메소드의 파라미터를 수정하고, 이 메소드를 사용하는 클라이언트 역시 모두 수정**해야 한다.

- **즉, 내부 구현의 변경이 외부로 퍼져나가는 파급 효과는 캡슐화가 부족하다는 명백한 증거다.**
    - 따라서 자기 자신을 스스로 처리한다는 점에서 개선된 것은 맞으나, **여전히 내부 구현을 캡슐화하는 데엔 실패**했다.

```kotlin
data class Movie(
		// . . .
    var movieType: MovieType,
    // . . .
) {
    fun calculateAmountDiscountedFee(): Money {
			// . . .
    }

    fun calculatePercentDiscountedFee(): Money {
			// . . .
    }

    fun calculateNoneDiscountedFee(): Money {
			// . . .
    }
}
```

- **`Movie`** 의 요금 계산 메소드들은 파라미터나 반환 값으로 내부에 포함된 속성에 대한 어떠한 정보도 노출하지 않는다.
    - 이로 인해, **캡슐화 원칙을 지키고 있다고 판단**할 수 있다.

- 하지만, **`Movie` 역시 내부 구현을 인터페이스에 노출시키고 있다.**
    - 노출시키는 것은 할인 정책의 종류다.
    - 세 개의 메소드는 각각 금액 할인 정책, 비율 할인 정책, 미적용의 세 가지가 존재한다는 사실을 만천하에 드러내고 있다.

- 새 할인 정책이 추가되거나 제거된다면, **역시 메소드에 의존하는 모든 클라이언트가 영향**을 받을 것이다.
    - 따라서, **`Movie` 는 세 가지 할인 정책을 포함하고 있다는 내부 구현을 성공적으로 캡슐화하지 못한다.**

> **캡슐화의 진정한 의미**

위 예제는 **캡슐화가 단순히 객체 내부 데이터를 외부로부터 감추는 것 이상의 의미를 가진다는 사실**을 잘 보여준다.

사실 **캡슐화는 변경될 수 있는 어떤 것이라도 감추는 것을 의미**한다. 내부 속성을 외부로부터 감추는 것은 ‘데이터 캡슐화’ 라고 불리는 캡슐화의 한 종류일 뿐이다.

**내부 구현의 변경으로 인해 외부의 객체가 영향을 받는다면, 캡슐화를 위반한 것**이다. 설계에서 변하는 것이 무엇인지 고려하고, 변하는 개념을 캡슐화해야만 한다.

**정리하면 캡슐화란 변하는 어떤 것이든 감추는 것이다. 그것이 무엇이든, 구현과 관련된 것이라면 말이다.**
> 

### 높은 결합도

- **캡슐화 위반으로 `DiscountCondition` 의 내부 구현이 외부로 노출**됐다.
    - 결국, `Movie` - `DiscountCondition` 사이의 결합도는 높을 수 밖에 없다.

```kotlin
data class Movie(
	// . . .
) {
		// . . .
    fun isDiscountable(whenScreened: LocalDateTime, sequence: Int): Boolean {
        for (condition in discountConditions) {
            if (condition.type == DiscountConditionType.PERIOD) {
                if (condition.isDiscountable(
                        whenScreened.dayOfWeek,
                        whenScreened.toLocalTime()
                    )
                ) {
                    return true
                }
            } else {
                if (condition.isDiscountable(sequence)) {
                    return true
                }
            }
        }

        return false
    }
}
```

- 위 메소드를 보면서 두 객체 사이의 높은 결합도가 어떤 문제를 초래하는지 고민해보자.
    - `DiscountCondition` 에 구현된 두 `isDiscountable` 중 적절한 것을 호출하고 있다.

- 중요한 것은 결합도이므로 **`DiscountCondition` 에 대한 어떤 변경이 `Movie` 에게까지 영향을 미치는지** 살펴보아야 한다.
    1. `DiscountCondition` 의 기간 할인 조건 명칭이 `PERIOD` 에서 다른 값으로 변경된다면 `Movie` 를 수정해야 한다.
    2. `DiscountCondition` 의 종류가 추가되거나 삭제된다면, `Movie` 내 `if, else` 구문을 수정해야 한다.
    3. 각 `DiscountCondition` 의 만족 여부를 판단하는 데 필요한 정보가 수정된다면, `Movie` 의 `isDiscountable` 메소드가 받는 파라미터도 변경해야 한다.
        - 결과적으로 이 메소드에 의존하는 `Screening` 역시 변경이 발생한다.

- 해당 요소들이 **`DiscountCondition` 의 구현**에 속한다는 사실에 주목하자.
    - `DiscountCondition` 의 인터페이스가 아닌 **‘구현’ 을 변경하는 경우에도, `Movie` 를 변경해야 한다는 것은 두 객체 사이의 결합도가 높다는 것을 의미**한다.
- 더 심각한 것은 **변경의 여파**가 두 객체 사이뿐 아니라, **시스템을 구성하는 모든 객체에게 간다는 것이다.**

- **모든 문제의 원인은 캡슐화 원칙을 지키지 않은 것이다.**
    - 유연한 설계를 차옺하기 위해선 캡슐화를 설계의 첫 번째 목표로 삼아야 한다.

### 낮은 응집도

```kotlin
data class Screening(
	// . . .
) {
    fun calculateFee(audienceCount: Double): Money {
        return when (movie.movieType) {
            MovieType.AMOUNT_DISCOUNT ->
                movie.calculateAmountDiscountedFee().times(audienceCount)

            MovieType.PERCENT_DISCOUNT -> {
                if (movie.isDiscountable(whenScreened, sequence)) {
                    return movie.calculatePercentDiscountedFee().times(audienceCount)
                } else {
                    return movie.calculateAmountDiscountedFee().times(audienceCount)
                }
            }

            MovieType.NONE_DISCOUNT ->
                movie.calculateNoneDiscountedFee().times(audienceCount)
        }
    }
}
```

- 이번엔 **`Screening`** 을 살펴본다.

- 앞서 이야기했던 것처럼, `Screening` 은 `DiscountCondition` 이 할인 여부를 판단하는 데 필요한 정보가 수정된다면 `Movie` 에 이어서 함께 수정되어야만 한다.
    
    
- 결과적으로 **할인 조건을 변경하기 위해선 `DiscountCondition`, `Movie`, `Movie` 를 사용하는 `Screening` 을 함께 수정**해야 한다.
    - **한 변경을 위해 코드의 여러 곳을 동시에 수정해야 함은 설계 응집도가 낮다는 증거**다.

- 응집도가 낮은 것 역시, **캡슐화를 위반**하며 발생한 문제다.
    - `Movie` 의 내부 구현이 인터페이스에 그대로 노출되어 있고, `Screening` 은 노출된 구현에 직접적으로 의존하고 있다.
    - 이는 원래 `Movie` 에 위치해야 하는 로직이 `Screening` 으로 새어나왔기 때문이다.

- 두 번째 설계 역시 데이터 중심 설계가 가지는 문제로 인해 몸살을 앓고 있다.
    - 그렇다면 왜 데이터 중심 설계는 이러한 문제점을 유발할까?

## 06. 데이터 중심 설계의 문제점

---

- 두 번째 설계가 변경에 유연하지 못한 이유눈, 결국 캡슐화를 위반했기 때문이다.
    - 캡슐화를 위반한 설계를 구성한 요소들이 높은 응집도, 낮은 결합도를 가질 확률은 매우 낮다.
    - **고로 변경에 취약할 수밖에 없다.**

- 변경에 취약한 이유는 아래 두 가지다.
    1. 데이터 중심 설계는 **본질적으로 너무 이른 시기에 데이터에 관해 결정하도록 강요**한다.
    2. 데이터 중심 설계에선 **협력이란 문맥을 고려하지 않고, 객체를 고립시킨 채 연산을 결정**한다.

### 데이터 중심 설계는 객체의 행동보다는 상태에 초점을 맞춘다

- 이 설계를 했을 때 던진 첫 질문은, **“이 객체가 포함해야 하는 데이터가 무엇인가?”** 다.
    - **데이터는 구현의 일부란 사실을 명심**하자.
    - 처음부터 데이터에 관해 결정하도록 강요하기에, 너무 이른 시기에 내부 구현에 초점을 맞추게 된다.

- 데이터 중심 설계 방식에 익숙한 개발자들은 일반적으로 데이터, 기능을 분리하는 **절차적 프로그래밍 방식**을 따른다.
    - 이는 상태, 행동을 한 단위로 캡슐화하는 객체지향 패러다임에 반한다.
- 데이터 중심 관점에서 **객체는 그저 데이터의 집합체**일 뿐이다.
    - 이로 인해 접근자, 수정자를 과도하게 추가하게 된다.
    - 또한 객체를 사용하는 절차를 분리된 별도 객체 안에 구현하게 된다.
- 앞서 설명한 것처럼, **접근자 및 수정자는 `public` 속성과 큰 차이가 없기에 캡슐화는 무너질 수밖에 없다.**
    
    ⇒ 이것이 첫 설계가 실패한 이유다.
    
- 데이터를 처리하는 작업과 데이터를 같은 객체에 두더라도, **데이터에 초점이 맞춰져 있다면 만족스러운 캡슐화를 얻기 어렵다.**
    - **데이터를 먼저 결정하고, 데이터를 처리하는 데 필요한 연산을 나중에 결정해선 안된다.**
        - 이는 데이터에 관한 지식이 객체의 인터페이스에 드러나도록 하는 원인이다.
- 결과적으로 **객체의 인터페이스는 구현을 캡슐화하는 데 실패하고, 코드는 변경에 취약**해진다.
    
    ⇒ 이거이 두 번째 설계가 실패한 이유다.
    
- 데이터 중심 설계는 너무 이른 시기에 데이터에 대해 고민하기에 캡슐화에 실패하게 된다.
    - 객체 내부 구현이 인터페이스를 어지럽히고, 객체 응집도 및 결합도에 나쁜 영향을 미친다.
    - 고로 변경에 취약해진다.

### 데이터 중심 설계는 객체를 고립시킨 채 오퍼레이션을 정의하도록 만든다

- **객체지항 애플리케이션 구축은 곧 협력하는 객체들의 공동체를 구축한다는 것**을 의미한다.
    - 따라서 **협력이란 문맥 내에서 필요한 책임을 결정**해야 한다.
    - 그리고, 이를 수행할 적절한 객체를 결정하는 것이 가장 중요하다.

- 올바른 객체지향 설계의 무게 중심은 **항상 객체 내부가 아닌 외부에 맞춰져 있어야 한다.**
    - 객체 내부가 어떤 상태를 가지고, 어떻게 상태를 관리하는지는 부가적 문제다.
    - **중요한 것은 객체가 다른 객체와 협력하는 방법**이다.

- 데이터 중심 설계에서의 초점은 객체 외부가 아닌 내부로 향한다.
    - 실행 문맥에 대한 깊이있는 고민 없이 곧바로 객체가 관리할 데이터의 세부 정보를 먼저 결정한다.
    - **객체의 구현이 이미 결정된 상태에서 다른 객체와 협력 방법을 고민**하기에, 이미 구현된 객체의 인터페이스를 억지로 끼워맞출 수 밖에 없다.

- 두 번째 설계가 변경에 민감한 것은 바로 이 때문이다.
    - 객체의 인터페이스에 구현이 노출돼 있었기에, 협력이 구현 세부사항에 종속되어 있었다.
    - 이에 따라 객체 내부 구현이 변경됐을 때 협력하는 객체 모두가 영향을 받을 수 밖에 없었다.
