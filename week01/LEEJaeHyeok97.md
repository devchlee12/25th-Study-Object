> 이론보다 실무가 먼저다.
설계에 관해 설명할 때 가장 유용한 도구는 이론으로 덕지덕지 치장된 개념과 용어가 아니라 ‘코드’ 그 자체다.
>

## 1. 티켓 판매 애플리케이션 구현하기

초대장 클래스 - Invitation

```json
public class Invitation {
		private LocalDateTime when;
}
```

티켓 클래스 - Ticket

```json
public class Ticket {
		private Long fee;

		public Long getFee() {
				return fee;
		}
}
```

관람객이 소지품을 보관할 Bag 클래스

```json
public class Bag {
		private Long amount;
		private Invitation invitation;
		private Ticket ticket;
		
		public boolean hasInvitation() {
				return invitation != null;
		}
		
		public boolean hasTicket() {
				return ticket != null;
		}
		
		.. set, minusAmount, plusAmount ..
```

이벤트에 당첨된 관람객의 가방 안에는 현금과 초대장이 들어있지만 이벤트에 당첨되지 않은 관람객의 가방 안에는 초대장이 들어있지 않을 것이다. 따라서 Bag 인스턴스의 상태는 현금과 초대장을 함게 보관하거나, 초대장 없이 현금만 보관하는 두 가지 중 하나일 것이다.

```json
public class Bag {
		public Bag(long amount) {
				this(null, amount);
		}
		
		public Bag(Invitation invitation, long amount) {
				this.invitation = invitation;
				this.amount = amount;
		}
}
```

소지품을 소지하는 관광객 클래스

```json
public class Audience {
		private Bag bag;
		
		public Audience(Bag bag) {
				this.bag = bag;
		}
		
		public Bag getBag() {
				return bag;
		}
}
```

매표소 클래스

```json
public class TicketOffice {
		private Long amount;
		private List<Ticket> tickets = new ArrayList<>();
		
		public TicketOffice(Long amount, Ticket ... tickets) {
				this.amount = amount;
				this.tickets.addAll(Arrays.asList(tickets));
		}
		
		public Ticket getTicket() {
				return tickets.remove(0);
		}
		
		public void minusAmount(Long amount) {
				this.amount -= amount;
		}
		
		public void plusAmount(Long amount) {
				this.amount += amount;
		}
}
```

매표소에서 초대장을 티켓으로 교환해 주거나 티켓을 판매하는 역할 - 판매원 클래스

```json
public class TicketSeller {
		private TicketOffice ticketOffice;
		
		public TicketSeller(TicketOffice ticketOffice) {
				this.ticketOffice = ticketOffice;
		}
		
		public TicketOffice getTicketOffice() {
				return ticketOffice;
		}
}
```

소극장 클래스

```json
public class Theater {
		private TicketSeller ticketSeller;
		
		public Theater(TicketSeller ticketSeller) {
				this.ticketSeller = ticketSeller;
		}
		
		public void enter(Audience audience) {
				if (audience.getBag().hasInvitation()) {
						Ticket ticket = ticketSeller.getTicketOffice().getTicket();
						audience.getBag().setTicket(ticket);
				} else {
						Ticket ticket = ticketSeller.getTicketOffice().getTicket();
						audience.getBag().minusAmount(ticket.getFee());
						ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
						audience.getBag().setTicket(ticket);
				}
		}
}
```

## 2. 무엇이 문제인가

> 모든 소프트웨어 모듈에는 세 가지 목적이 있다.
1. 실행 중에 제대로 동작하는 것
2. 변경을 위해 존재하는 것
3. 코드를 읽는 사함과 의사소통하는 것.(특별한 훈련 없이도)
>

### 예상을 빗나가는 코드

> 앞에서 살펴본 예제는 우리의 예상을 벗어난다.
이해 가능한 코드란 그 동작이 우리의 예상에서 크게 벗어나지 않는 코드다.
>

<aside>
💡

앞선 코드의 가장 심각한 문제는 Audience와 TicketSeller를 변경할 경우 Theater도 함께 변경해야 한다는 사실.

</aside>

### 변경에 취약한 코드

> 다른 클래스가 Audience의 내부에 대해 더 많이 알면 알수록 Audience를 변경하기 어려워진다.
>

<aside>
💡

우리의 목표는 애플리케이션의 기능을 구현하는데 필요한 최소한의 의존성만 유지하고 불필요한 의존성을 제거하는 것이다.

</aside>

*의존성이 과한 경우를 가리켜 결합도(coupling)가 높다고 말한다.

## 3. 설계 개선하기

> 관람객이 스스로 가방 안의 현금과 초대장을 처리하고 판매원이 스스로 매표소의 티켓과 판매 요금을 다루게 한다면 이 모든 문제를 해결할 수 있다.
→ 관람객과 판매원을 자율적인 존재로 만들면 된다.
>

### 자율성을 높이자

```java
public class Theater {
		private TicketSeller ticketSeller;
		
		public Theater(TicketSeller ticketSeller) {
				this.ticketSeller = ticketSeller;
		}
		
		public void enter(Audience audience) {
				이곳 코드를 sellTo 메소드로 이동
			}
	}
	
	public class TicketSeller {
			private TicketOffice ticketOffice;
			
			public TicketSeller(TicketOffice ticketOffice) {
					this.ticketOffice = ticketOffice;
			}
			
			public void sellTo(Audience audience) {
				if (audience.getBag().hasInvitation()) {
						Ticket ticket = ticketSeller.getTiketOffice().getTicket();
						audience.getBag().setTicket(ticket);
				} else {
					Ticket ticket = tiketSeller.getTicketOffice().getTicket();
					audience.getBag().minusAmount(ticket.getFee());
					ticketSeller.getTicketOffice().plusAmount(ticket.getFee());
					audience.getBag().setTicket(ticket);
				}
			}
```

> 개념적이나 물리적으로 객체 내부의 세부적인 사항을 감추는 것을 캐슐화라고 부른다.
캡슐화의 목적은 변경하기 쉬운 객체를 만드는 것이다.
>

Theater의 변경된 enter 메서드

```java
public void enter(Audience audience) {
		ticketSeller.sellTo(audience);
}
```

> Theater 는 오직 TicketSeller의 인터페이스에만 의존한다. TicketSeller가 TicketOffice 인스턴스를 포함하고 있다는 사실은 구현의 영역에 속한다.
”객체를 인터페이스와 구현으로 나누고 인터페이스만을 공개하는 것은 객체 사이의 결합도를 낮추고 변경하기 쉬운 코드를 작성하기 위해 따라야 하는 가장 기본적인 설계 원칙이다.”
>

변경된 Audience 클래스

```java
public class Audience {
	private Bag bag;
	
	public Audience(Bag bag) {
			this.bag = bag;
	}
	
	public Long buy(Ticket ticket) {
			if (bag.hasInvitation()) {
					bag.setTicket(ticket);
					return 0L;
			} else {
				bag.setTicket(ticket);
				bag.minusAmount(ticket.getFee());
				return ticket.getFee();
			}
	}
}
```

티켓 셀러가 but 메서드를 호출하도록 코드를 변경하면 된다.

```java
public class TicketSeller {
		private TicketOffice ticketOffice;
		
		public void sellTo(Audience audience) {
				ticketOffice.plusAmount(audience.buy(ticketOffice.getTicket()));
		}
}
```

### 무엇이 개선됐는가

Audience나 TicketSeller의 내부 구현을 변경하더라도 Theater를 함께 변경할 필요가 없어졌다.

### 어떻게 한 것인가

> 자기 자신의 문제를 스스로 해결하도록 코드를 변경했다.
우리는 우리의 직관을 따랐고 그 결과로 코드는 변경이 용이하고 이해 가능하도록 수정됐다.
이해하기 쉽고 유연한 설계를 얻었다.
>

### 캡슐화와 응집도

외부의 간섭을 최대한 배제하고 메시지를 통해서만 협력하는 자율적인 객체들의 공동체를 만드는 것이 훌륭한 객체지향 설계를 얻을 수 있는 지름길인 것이다.

### 절차지향과 객체지향

> 데이터와 프로세스가 동일한 모듈 내부에 위치하도록 프로그래밍 하는 방식을 객체지향 프로그래밍이라고 부른다.
훌륭한 객체지향 설계의 핵심은 캡슐화를 이용해 의존성을 적절히 관리함으로써 객체 사이의 결합도를 낮추는 것이다.
>

### 책임의 이동

> 객체는 자신을 스스로 책임진다.
자율적인 객체들이 낮은 결합도와 높은 응집도를 가지고 협력하도록 최소한의 의존성만을 남기는 것이 훌륭한 객체지향 설계다.
>

### 더 개선할 수 있다

> 작은 메서드로 코드를 작게 분리하는 것이 얼마나 유용한지 실감할 수 있다.
>

> 설계는 균형의 예술이다. 훌륭한 설계는 적절한 트레이드오프의 결과물이라는 사실을 명심하라. 이러한 트레이드오프 과정이 설계를 어려우면서도 흥미진진한 작ㅇ버으로 만드는 것이다.
>

### 그래 거짓말이다!

> 훌륭한 객체지향 설계란 소프트웨어를 구성하는 모든 객체들이 자율적으로 행동하는 설계를 가리킨다.
쉬운 코드를 작성하고 싶다면, 한 편의 애니메이션을 만든다고 생각하라.
>

## 4. 객체지향 설계

### 설계가 왜 필요한가

> 설계란 코드를 배치하는 것이다
좋은 설계란 오늘 요구하는 기능을 온전히 수행하면서 내일의 변경을 매끄럽게 수용할 수 있는 설계다.
>

### 객체지향 설계

> 훌륭한 객체지향 설계란 협력하는 객체 사이의 의존성을 적절하게 관리하는 설계다.
객체 간의 의존성은 애플리케이션을 수정하기 어렵게 만드는 주범이다.
데이터와 프로세스를 하나의 덩어리로 모으는 것은 훌륭한 객체지향 설계로 가는 첫걸음일 뿐이다.
의존성을 적절하게 조절함으로써 변경에 용이한 설계를 만드는 것이다.
>