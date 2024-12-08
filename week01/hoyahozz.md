- 실무가 먼저일까? 이론이 먼저일까?
    - 일반적으로, **초기 단계에서는 이론 정립보단 실무 관찰이 우선**이다.
    - 실무 관찰을 바탕으로 이론을 정립하는 것이 최선이다.
    - 소프트웨어 업계는 역사가 짧으므로 **이론보다 실무가 더 앞서있다.**

- 마찬가지로 **객체지향 패러다임을 설명함에 있어 버거운 용어와 난해한 개념들, 즉 이론을 중심으로 설명하는 것은 적절하지 않다.**
    - 이 책에서는 **추상적 개념과 이론이 아닌, 개발자가 가장 잘 이해할 수 있는 언어인 ‘코드’를 활용해 객체지향의 다양한 측면을 설명**할 것이다.

---

## 01 - 티켓 판매 애플리케이션 구현하기

- **연극을 공연할 수 있는 소극장을 공연**한다고 가정한다.
    - 특이사항으로, 추첨을 통해 선정된 관람객은 공연을 무료로 관람할 수 있는 초대장을 가지고 있다.
    - 즉, **관람객 입장 전 이벤트 당첨 여부를 확인하고, 미당첨자의 경우 티켓 판매 후 입장**시켜야 한다.

```kotlin
// 초대장
class Invitation(
    private val date: LocalDateTime
)

// 티켓
class Ticket(
    val fee: Long
)

// 가방
class Bag(
    private var amount: Long,
    private val invitation: Invitation?,
    var ticket: Ticket?,
) {
    // 초대장 없이 현금만 가지고 있는 경우
    constructor(amount: Long) : this(amount, null, null)

    // 초대장을 가지고 있는 경우
    constructor(amount: Long, invitation: Invitation) : this(amount, invitation, null)

    val hasInvitation: Boolean
        get() = invitation != null

    val hasTicket: Boolean
        get() = ticket != null

    fun minusAmount(amount: Long) {
        this.amount -= amount
    }

    fun plusAmount(amount: Long) {
        this.amount += amount
    }
}

// 관람객
class Audience(
    val bag: Bag,
)

// 매표소
class TicketOffice(
    private var amount: Long,
    private val tickets: MutableList<Ticket> = mutableListOf(),
) {
    fun getTicket(): Ticket = tickets.removeAt(0)

    fun minusAmount(amount: Long) {
        this.amount -= amount
    }

    fun plusAmount(amount: Long) {
        this.amount += amount
    }
}

// 판매원
class TicketSeller(
    val ticketOffice: TicketOffice,
)
```

- 요구사항에 따라 초대장, 티켓, 가방, 관람객, 매표소, 판매원을 위와 같이 구현할 수 있다.

```kotlin
// 소극장
class Theater(private val ticketSeller: TicketSeller) {

    fun enter(audience: Audience) {
        val bag = audience.bag
        val ticket = ticketSeller.ticketOffice.getTicket()

        if (bag.hasInvitation) {
            bag.ticket = ticket
        } else {
            bag.minusAmount(ticket.fee)
            ticketSeller.ticketOffice.plusAmount(ticket.fee)
            bag.ticket = ticket
        }
    }
}
```

- 이후 소극장을 구현하고, 관람객을 맞이할 수 있도록 `enter` 메소드를 구현하였다.
- 정상적으로 잘 동작하는 것을 확인할 수 있다.

---

## 02 - 무엇이 문제인가

- 로버트 마틴은 모든 소프트웨어 모듈에는 세 가지 목적이 있다고 이야기한다.
    1. 실행 중 제대로 동작하는 것
    2. 변경을 위해 존재하는 것
    3. 코드를 읽는 사람과 의사소통 하는 것
    
    ⇒ **즉, 모든 모듈은 제대로 실행되어야 하고, 변경에 용이해야 하며, 이해하기 쉬워야 한다.**
    

- 하지만 **기존 코드는 변경도 불편하고, 읽는 사람이 이해하기 어렵다는 특징**이 있다.

### 예상을 빗나가는 코드

- `enter` 메소드 내에서 발생하는 일들은 다음과 같다.
    1. 소극장은 관람객 가방을 열어 초대장이 들어 있는지 살핀다.
    2. 가방 내 초대장이 있으면, 판매원은 매표소 내 티켓을 관람객 가방으로 이동시킨다.
    3. 가방 내 초대장이 없다면, 관람객 가방에서 티켓 비용만큼 현금을 꺼내 매표소에 적립시키고, 티켓을 관람객 가방 안으로 옮긴다.

- 스스로가 저 세계의 관람객이라고 생각해보자.
    - 소극장이라는 녀석이 허락도 없이 가방을 뒤져보고, 돈을 가져가고, 돈을 넣어주고 있다.
- 판매원이라고 생각해도 이상하다.
    - 소극장이 매표소 내 티켓과 현금에 마음대로 접근하고 있다.

⇒ **즉, ‘사람’인 우리가 보았을 때 예상할 수 없는, 즉 이해하기 어려운 코드이다.**

- 코드 이해가 어려운 데에는 또 다른 이유가 있다.
    - 이는, **`enter` 메소드를 이해하기 위해 여러 세부 내용을 알고 있어야 하기 때문**이다.
        1. 관람객은 가방을 가지고 있다.
        2. 가방 내엔 현금, 티켓이 있다
        3. 판매자는 매표소에서 티켓을 판매한다.
        4. 매표소 내엔 돈과 티켓이 보관되어 있다.

⇒ **즉, 한 클래스나 메소드에서 너무 많은 세부사항을 다루고 있다.**

- 이는, 코드 작성자, 조회자 모두에게 부담을 준다.

### 변경에 취약한 코드

- 더욱 심각한 문제는, **`Audience`, `TicketSeller` 변경 시 `Theater` 도 함께 변경된다는 점**이다.
    1. 관람객이 가방을 들고 있지 않다면?
    2. 관람객이 현금이 아닌 신용카드로 결제한다면?
    3. 판매원이 매표소 외부에서 티켓을 판매해야 한다면?
    ⇒ 모든 변경사항에 따라 `Theater` 내 `enter` 메소드를 수정해야 한다.

- 이처럼, **클래스가 다른 클래스의 내부에 대한 지식이 많아지면, 코드 변경이 어려워진다.**
    - 이는 객체 사이의 의존성(dependency) 관련 문제이다.
    - 의존성을 지닌다는 것은, 그 객체가 변경되면 자신 역시 변경될 수 있다는 사실이 내포되어 있다.
- 물론 객체 사이 의존성을 모두 제거해야 하는 것은 아니다.
    - **객체지향 설계는 서로 의존하며 협력하는 객체들의 공동체를 구축하는 것이 목적**이다.
    - 고로, 애플리케이션을 구현하는 데 필요한 최소한의 의존성만 유지해야 한다.

---

## 03 - 설계 개선하기

- **이제 변경하기 쉽고, 이해하기도 쉬운 코드로 변경**해보자.
    - **소극장에서 관람객과 판매자에 대해 너무 많은 부분을 알지 못하도록 정보를 차단**하면, 모든 것이 해결된다.
    - 소극장은 오로지, 관람객이 소극장에 입장하는 것만 알고 있으면 된다.

- 관람객은 **스스로** 가방의 현금과 초대장을 처리한다.
- 판매원은 **스스로** 매표소 내 티켓과 판매 요금을 다룬다.
- **즉, 관람객과 판매원을 자율적인 존재로 만든다.**

### 자율성을 높이자

![image](https://github.com/user-attachments/assets/99eab79c-dde1-4c99-a31a-0766ca3acd32)

- 소극장에서 매표소에 접근하는 모든 코드를, 판매원으로 옮기는 작업을 먼저 실행하였다.
    - 결과적으로, `TicketOffice` 는 판매원, 즉 `TicketSeller` 만 접근 가능하도록 수정된 것을 확인할 수 있다.
    - 이처럼 객체 내부의 세부 사항을 숨기는 것을 **캡슐화**라고 부른다.
    - **객체 사이의 결합도를 낮춰 설계를 좀 더 쉽게 변경**할 수 있게 되었다.

- 이제, 소극장은 매표소에 접근할 수 없게 되었음은 물론, 판매원이 매표소를 알고 있다는 사실조차 모르게 되었다.
    - **`Theater` 는 오로지 `TicketSeller` 라는 인터페이스(interface)에만 의존**한다.
    - **`TicketSeller` 가 `TicketOffice` 의 인스턴스를 포함하고 있는 것은 구현(implementation) 영역**에 속한다.
    
    ⇒ **객체를 인터페이스, 구현으로 나누고 인터페이스만을 공개하는 것은, 객체 사이 결합도를 낮추고 변경이 쉬운 코드를 작성하기 위해 따라야 하는 가장 기본 설계 원칙**이다.
    

![image](https://github.com/user-attachments/assets/809ae02b-9317-49b1-87dd-4a4d47bf857c)

- 관람객 영역도 위와 같이 캡슐화 할 수 있다.
    - 이제, **관람객은 자신의 가방 안에 초대장이 있는지 “직접” 확인**한다.
    - 그리고 외부에서는 관람객이 가방이 존재하는지 조차 알지 못한다.

- 결과적으로, **판매자와 관람객 사이의 결합도가 크게 낮아졌다.**
    - 내부 구현이 캡슐화되었으므로, **`Audience`** 내 구현에 변경사항이 발생하더라도, **`TicketSeller`** 엔 어떠한 영향도 주지 않는다.
- **이제 판매자, 관람객 모두 자율적인 존재**가 되었다.

### 무엇이 개선됐는가

- **판매자, 관람객은 자신이 가지고 있는 소지품을 스스로 관리**한다.
    - 이는, **개발자의 예상과 정확히 일치**함을 의미하므로 확실히 개선되었다고 볼 수 있다.

- 또한, 판매자, 관람객의 **내부 구현을 변경**하더라도 소극장에는 **아무런 영향을 미치지 않도록 수정**되었다.
    - 관람객이 가방이 아닌 지갑을 소지하더라도 아무 상관 없다!
    - 즉, **변경 용이성도 크게 개선**됐다고 볼 수 있다.

### 어떻게 한 것인가

- **객체 각각이 자신의 문제를 스스로 해결하도록 코드를 변경했을 뿐**이다.
    - 더 이상 소극장은 판매자, 관람객의 내부에 직접 접근하지 않는다.
    - 즉, **객체의 자율성을 높이는 방향으로 설계**했고, 결과적으로 **이해하기 쉽고 유연한 설계**를 만들 수 있었다.

### 캡슐화와 응집도

- **핵심은, 객체 내부 상태는 캡슐화한다는 점**이다.
    - **객체간 상호작용은 오로지 메시지를 통해서만 할 수 있도록** 만든다.

- **밀접하게 연관된 작업만 수행하고, 연관없는 작업은 다른 객체에게 위임하는 객체는 응집도(cohesion)가 높다**고 말한다.
    - **자율적인 객체를 만들면, 결합도를 낮추고 응집도를 높일 수 있다.**
    - 객체는, **자신의 데이터를 ‘스스로’ 처리**하는 자율적 존재여야만 한다.
    - 외부 간섭을 배제하고, 메시지를 통해서만 협력하는 **자율적인 객체들의 공동체를 만드는 것이 객체지향 설계의 목표**이다.

### 절차지향과 객체지향

- 수정 전에는, **`Theater` 의 `enter` 메소드 내에서 모든 객체에 접근해 데이터를 가져왔다.**
    - `Audience`, `TicketSeller` 등은 관람객을 입장시키는 데 필요한 모든 정보를 제공한다.
    - 그리고 이에 대한 처리는 모두 `enter` 메소드 내에 존재했다.
- 해당 관점에서 **`Theater` 의 `enter` 는 프로세스**이다.
- **`Audience`, `TicketSeller`, `Bag` 은 데이터**이다.
    - 이와 같이 프로세스, 데이터를 별도 모듈에 위치시키는 방식을 **절차지향 프로그래밍**이라고 한다.

- **절차지향 프로그래밍에선 관람객, 판매원 모두 수동적 존재**이다.
    - 개발자, 즉 사람의 예상을 벗어나기에 코드를 이해하기 어렵다.
    - 그리고, 데이터에 변경이 생기면 프로세스 역시 변경이 발생하는 어려움이 존재한다.

- **변경하기 쉬운 설계는, 한 번에 하나의 클래스만 변경할 수 있는 설계**이다.
- **객체 스스로가 자신의 데이터를 처리**하도록 프로세스의 적절한 단계를 `Audience`, `TicketSeller` 로 이동시킨다.
    - 이처럼, **데이터 및 프로세스가 동일한 모듈 내에 존재하도록 프로그래밍하는 것을 객체지향 프로그램**이라고 부른다.

- **훌륭한 객체지향 설계는, 캡슐화를 이용해 의존성을 관리하여 객체 사이의 결합도를 낮추는 것**이다.

### 책임의 이동

![image](https://github.com/user-attachments/assets/54013b50-6669-459e-a766-eb7f35a9ed9e)

![image](https://github.com/user-attachments/assets/9ce70ba2-e6be-4606-ada2-363247e839fb)


- 두 방식 사이의 근본적 차이는 **책임의 이동**이다.
    - ‘책임’은 기능을 가리키는 객체지향의 용어이다.

- 위 그림을 보면 알 수 있듯이, **작업의 흐름이 `Theater` 에 의해 제어된다는 사실**을 알 수 있다.
    - **즉, 책임이 `Theater` 에 집중**되어 있다.
- 하지만 **객체지향의 경우 제어 흐름이 각 객체에 적절히 분산**되어 있다.
    - 즉, 하나의 기능을 완성하는 데 필요한 책임이 여러 객체에 분산되어 있다.
    - **각 객체는 스스로를 책임**지고 있다.

- **데이터와 데이터를 사용하는 프로세스가 동일한 객체가 아니라면 절차지향 프로그래밍일 가능성**이 높다.
- **동일한 객체 내에 위치해 있다면, 객체지향 프로그래밍일 가능성**이 높다.

### 더 개선할 수 있다

- 관람객은 이제 분명 자율적인 존재지만, 관람객 속의 가방은 자율적인 존재가 아니다.
    - `Bag` 객체 역시 자율적인 존재로 바꿀 수 있다.

```kotlin
// 관람객
class Audience(
    private val bag: Bag,
) {
    fun buy(ticket: Ticket): Long {
        return bag.hold(ticket)
    }
}

// 가방
class Bag(
    private var amount: Long,
    private val invitation: Invitation?,
    private var ticket: Ticket?,
) {
    fun hold(ticket: Ticket): Long {
        this.ticket = ticket

        if (hasInvitation) {
            minusAmount(ticket.fee)
            return 0
        } else {
            return ticket.fee
        }
    }

    val hasInvitation: Boolean
        get() = invitation != null

    val hasTicket: Boolean
        get() = ticket != null

    private fun minusAmount(amount: Long) {
        this.amount -= amount
    }

    private fun plusAmount(amount: Long) {
        this.amount += amount
    }
}
```

- `Bag` 은 상태와 행위를 모두 가지는 응집도 높은 클래스가 되었다.

```kotlin
// 판매원
class TicketSeller(
    private val ticketOffice: TicketOffice,
) {
    fun sellTo(audience: Audience) {
        ticketOffice.sellTicketTo(audience)
    }
}

// 매표소
class TicketOffice(
    private var amount: Long,
    private val tickets: MutableList<Ticket> = mutableListOf(),
) {
    fun sellTicketTo(audience: Audience) {
        plusAmount(audience.buy(getTicket()))
    }
    
    private fun getTicket(): Ticket = tickets.removeAt(0)

    private fun plusAmount(amount: Long) {
        this.amount += amount
    }
}
```

- 판매원 역시 매표소에 있는 티켓을 자기 마음대로 꺼내 관람객에게 팔고 있으므로, 자율권을 침해한다.
    - 매표소의 자율권 역시 찾아줄 수 있다.

- 하지만, `TicketOffice` 내 `Audience` 에 대한 의존성이 생기게 되었다.
    - 의존성의 추가는 높은 결합도를 의미하고, 이는 즉 변경하기 어려운 설계임을 의미한다.
    - 자율성은 높였으나, 전체 설계에선 결합도가 상승한 케이스이다.

- `Audience` 에 대한 결합도와 `TicketOffice` 에 대한 자율성 모두 만족시킬 수 있는 방법이 있을까?
    - 즉, **트레이드오프의 시점**이 다가온 것이다.
- 여기서 두 가지 사실을 알 수 있다.
    1. **어떤 기능을 설계하는 방법은 한 가지 이**상일 수 있다.
    2. 그렇기에 **결국 설계는 트레이드오프의 산물**이다.
    - **어떤 경우에도 모든 사람을 만족시킬 수 있는 설계를 만들 순 없다.**

### 그래, 거짓말이다!

- 앞에서, 실제 세계의 관람객, 판매자와 같이 변경되었기 때문에 코드를 이해하기 쉬워졌다고 이야기했다.
    - 하지만, **`Theater`, `Bag`, `TicketOffice` 는 실세계에선 자율적 존재가 아니다.**

- **그러나 객체지향 세계에선 모든 것이 능동적이고 자율적인 존재**로 바뀐다.
    - 이를 **의인화**라고 부르는데, **실세계에선 생명이 없는 수동적 존재라도 객체지행 세계에선 지능을 가진 존재**로 태어나는 것이다.
    - 한 편의 애니메이션을 만든다고 생각하자.

## 04 - 객체지향 설계

### 설계가 왜 필요한가

> **설계란 코드를 배치하는 것이다. - Metz12**
> 
- 설계가 코드를 작성하는 것보다 높은 차원의 창조 행위라고 보는 경우가 많다.
    - 하지만, **설계와 구현은 떨어질 수 없는 존재**이다.
    - 설계는 코드 작성의 일부이고, 코드없인 검증할 수 없다.

- 좋은 설계는 두 가지 요구 사항을 만족시켜야 한다.
    1. **요구 기능을 온전히 수행**한다.
    2. **내일의 변경을 매끄럽게 수용**할 수 있어야 한다.

### 객체지향 설계

- 위 두 요구사항을 만족시키는 것은, 결국 **변경에 유연하게 대응할 수 있는 코드**이다.
    - **객체지향 프로그래밍은 의존성을 효율적으로 통제할 수 있는 여러 방법을 제공하므로, 변경에 수월하게 대응**할 수 있다.

- **변경이 쉬운 코드란 이해하기 쉬운 코드를 의미**한다.
    - 이해하기 쉬운 코드는, 변경하기도 두렵게 만든다.
    - **객체지향 패러다임은 사람이 세상을 바라보는 방식대로 코드를 작성**할 수 있도록, 즉 이해하기 쉬운 코드를 작성할 수 있도록 돕는다.

- **훌륭한 객체지향 설계는, 객체 사이 의존성을 적절하게 관리하는 것**이다.
 
