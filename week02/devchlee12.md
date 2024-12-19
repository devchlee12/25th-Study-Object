# 2장 : 객체지향 프로그래밍

### 영화 예매 시스템은 어떻게 설계해야할까?

- 요구 사항
    - 영화는 제목, 상영시간, 가격정보를 가짐
    - 사용자가 실제로 예매하는 대상은 상영
    - 할인은 할인 조건과 할인 정책으로 나뉨
        - 할인 조건
            - 순서 조건
            - 기간 조건
        - 할인 정책
            - 금액 할인 정책
            - 비율 할인 정책
        - 할인 정책은 하나지만, 할인 조건은 여러개 설정 가능

### 객체 지향 프로그래밍을 향해

- 대부분 객체지향 프로그램을 작성할 때 클래스를 결정한 후, 어떤 속성과 메서드가 필요한지 고민함
    - 그런데 이렇게 하면 객체지향의 본질과 거리가 멀어짐
    - 클래스가 아닌 객체에 초점을 맞춰야함
    - 어떻게?
        - 어떤 객체가 필요한지 고민
        - 객체를 보는 방식을 제대로
            - 독립적인 존재 아니고, 협력적인 존재임 → 설계를 유연하고, 확장 가능하게 만듬
- 도메인의 구조를 따르는 프로그램 구조
    - 객체 지향 패러다임은 요구사항 분석부터 프로그램 구현까지 객체라는 동일한 추상화 기법 사용 가능
    - 영화 예매 시스템 도메인 구조
        
        ![image](https://github.com/user-attachments/assets/68f47638-46a7-4706-b247-14cc8c932e4a)

        
    - 영화 예매 시스템 클래스 구조
        
        ![image 1](https://github.com/user-attachments/assets/d04c9cb8-5677-4e44-973a-f0e6693309c2)

        
- 자율적인 객체
    - 객체지향 프로그래밍 언어는 상태와 행동을 캡슐화하고, 접근 제어 메커니즘을 제공함
        - 왜일까?
            - 객체를 자율적인 존재로 만들기 위해 외부 간섭 최소화
            - 인터페이스와 구현의 분리
- 프로그래머의 자유
    - 객체지향 설계를 이해하기 위해서 프로그래머의 역할을 두가지로 나누는 것이 유용하다
        - 클래스 작성자 : 데이터 타입 추가
        - 클라이언트 프로그래머 : 데이터 타입 사용
    - 이렇게 하면 접근 제어 매커니즘을 통해 내부와 외부를 명확하게 경계지어야하는 이유가 명확해짐
        - 클라이언트 프로그래머에 대한 영향 걱정 안해도 되기에
    - 구현 은닉은 두 역할에게 모두 유용
        - 클래스 작성자는 구현 바꿀 수 있고
        - 클라이언트 프로그래머는 내부 구현 몰라도 되기 때문에 내부 지식 부족해도 됨
- 협력하는 객체들의 공동체
    - 영화를 예매하기 위해 Screening과 Movie, Reservation 인스턴스들은 서로의 메서드를 호출하며 상호작용함
        
        ![image 2](https://github.com/user-attachments/assets/9a620c40-8c58-4e8e-9d16-99c344357afb)

        
    - 여담이지만, 금액을 구현할 때 Long 타입을 사용하는 것은 객체지향적으로 좋은가?
        - Money 타입을 추가하면 금액과 관련있다는 의미를 전달 가능하다
        - 의미를 명시적이고 분명하게 표현할 수 있다면 객체를 사용하는 것이 좋음
    - 객체 지향 프로그램 작성하기
        1. 협력 관점에서 어떤 객체 필요한지 결정
        2. 객체들의 공통 상태와 행위 구현하기 위해 클래스 작성
- 협력에 관한 짧은 이야기
    - 메시지와 메서드는 구분해야함
    - 객체끼리 상호작용할 수 있는 유일한 방법은 메시지 전송
    - 수신한 메시지를 처리하기 위한 객체 자신만의 방법이 메서드
    - 메시지와 메서드를 구분에서 다형성 개념이 출발함

### 할인 요금 구하기

- 할인 요금 계산을 위한 협력
    - 할인 요금 계산할 때 어떤 정책 사용할지 결정하는 코드가 없다
        - 상속과 다형성을 이용했기 때문
- 할인 정책과 할인 조건
    - 할인 정책을 구현한 방법 → Template method 패턴
        - DiscountPolicy에 할인량은 추상메서드로 놓음
        - 구체 클래스들이 추상메서드만 구현하면 됨
    - 할인 조건을 구현한 방법
        - DiscountCondition을 인터페이스로 놓고, isSatisFiedBy 메서드를 구체클래스에서 구현
    - 다이어 그램으로 표현하면?
        
        ![image 3](https://github.com/user-attachments/assets/71b5bb23-0405-4ed0-9ecc-3b4ed1ac7558)

        
- 할인 정책 구성하기
    - 하나의 정책에 여러 조건이 들어갈 수 있도록 구현

### 상속과 다형성

- 컴파일 시간 의존성과 실행시간 의존성
    - 우선 의존성이란?
        - 어떤 클래스가 다른 클래스에 접근할 수 있는 경로를 가자거나 해당 클래스 객체의 메서드를 호출하는 경우 의존성이 있다고함
    - 아래 그림은 누구에게 의존하고 있는 것인가?하지만,
        
        ![image 4](https://github.com/user-attachments/assets/36c0d085-dee6-4b68-8f5c-be079cb477a3)

        
    - 코드 수준에서는 DIscountPolicy
    - 하지만 실행 시점에는 구체 클래스와 협력함
    - 코드의 의존성과 실행시점의 의존성은 다를 수 있다.
        - 확장 가능하고 유연한 객체지향 설계일수록 코드와 실행시점의 의존성이 다름
        - 하지만, 그러면 코드 이해가 힘들어짐
        - 결국 트레이드오프임
- 차이에 의한 프로그래밍
    - 상속을 통해 기존 클래스의 속성과 행동을 물려받을 수 있음
    - 이를 통해 다른 부분만 추가해서 빠르게 클래스 만들기 가능
- 상속과 인터페이스
    - 상속이 가치있는 이유는 부모의 인터페이스를 모두 물려받을 수 있기 때문
    - 일반적으로 상속의 가치를 코드재사용으로 알고 있지만, 아님
        - 코드 재사용 목적으로 상속을 사용하면 변경에 취약한 코드 낳을 가능성 높아짐
    - 부모 대신 협력이 가능하다는 것이 상속의 가치
- 다형성
    - 동일한 메시지를 전송하지만 하는 객체의 클래스에 따라 실행 메서드 달라짐
        - 동적 바인딩
    - 동일한 인터페이스 공유하는 클래스들을 하나의 타입계층으로 묶기 가능
    - 컴파일 시간 의존성과 실행시간 의존성을 다르게 만들 수 있기에 다형성이 가능
    - 다형성은 꼭 상속으로만 구현할 수 있는 것은 아님
        - 다양한 방법으로 다형성 구현 가능
- 인터페이스와 다형성
    - 자바의 인터페이스를 통해 순수하게 인터페이스만 공유가 가능
    - 이를 통해 다형성 실현 가능

### 추상화와 유연성

- 추상화의 힘
    
    ![image 5](https://github.com/user-attachments/assets/46c257bf-8a8b-4379-bcf4-1ac6f1adcdc9)

    
    - 추상화를 통해 요구 사항을 높은 수준에서 서술 가능
    - 설계 유연해짐
- 유연한 설계
    - Movie에게 할인 정책 존재 여부를 결정시키는 것은 일관성있는 협력이라고 보기 힘듬
        - DiscountPolicy에서 결정하게 하자
    
    ![image 6](https://github.com/user-attachments/assets/46e87be4-de49-4033-a449-61956bfda749)

    
    - 추상화를 기반으로 유연한 설계를 했기 때문에 쉽게 확장이 가능했음
- 추상 클래스와 인터페이스 트레이드 오프
    - 인터페이스를 사용하여 개념적인 혼란과 결합을 제거가 가능하다
        
        ![image 7](https://github.com/user-attachments/assets/4f81f8b2-32c2-498e-8a4d-63563d452546)

        
    - 하지만, 이러면 과하게 코드가 복잡해진다는 생각이 들 수 있다
    - 결국 모든것은 트레이드 오프이다.
- 코드 재사용
    - 상속은 코드를 재사용하기 위해 널리 사용되지만, 가장 좋은 방법은 아니다
    - 코드 재사용을 위해서는 합성을 사용하는 것이 더 좋다
        - 합성이란, 다른 객체의 인스턴스를 자신의 인스턴스 변수로 포함해서 재사용하는 것
    - 왜 상속보다 합성이 더 좋을까?
        - 상속으로 할인정책을 구현한다면?
            
            ![image 8](https://github.com/user-attachments/assets/8009f457-e657-4a86-9fc7-09f088f63396)

            
        - 하지만 상속은 캡슐화를 위반한다
            - 부모 클래스의 내부 구조를 알아야하므로
            - 부모클래스의 구현이 자식에게 노출
        - 또한, 설계를 유연하지 못하게 만드는 문제도 있다
            - 부모 클래스와 자식 클래스 사이 관계를 컴파일 시점에 결정
            - 상속에서 할인 정책 바꾸려면 인스턴스 새로 생성해서 복사해야함
            - 그런데 합성은 간단히 변수 바꿔서 바꾸기 가능
- 합성
    - 합성은 상속과는 다르게 인터페이스를 통해 약하게 결합된다
    - 인터페이스에 정의된 메시지를 통해서만 코드 재사용
    - 합성이 상속에 비해 좋은 점
        - 구현을 효과적으로 캡슐화 가능
        - 인스턴스 교체가 쉬워서 설계 유연해짐