- 이전에 살펴보았던 **데이터 중심 설계는, 캡슐화를 위반하기 쉽고 결합도가 높아지며 코드의 변경이 어려워진다**는 특징이 있었다.
- 이를 해결할 수 있는 기본 방법은 데이터가 아닌 책임에 초점을 맞추는 것이다.

- 책임 위주 설계 시 직면하는 가장 큰 어려움은 **어떤 객체에 어떤 책임을 할당할지 결정이 쉽지 않다는 것**이다.
    - 책임 할당 과정은 일종의 트레이드오프 활동이며, 동일 문제를 해결할 수 있는 다양한 책임 할당법이 존재한다.
    - 어떤 방법이 최선인진 상황, 문맥에 따라 달라진다.
    - 고로, **올바른 책임의 할당을 위해선 다양한 관점에서 설계를 평가**할 수 있어야 한다.

- 이번장에선 **`GRASP` 패턴을 통해 책임 할당의 어려움을 해소**해본다.
    - **응집도, 결합도, 캡슐화와 같은 다양한 기준에 따라 책임을 할당**하고, 결과를 트레이드오프 할 수 있는 기준을 배울 수 있다.

- 2장에선 책임 중심으로 설계된 객체지향 코드의 대략적 모양을 살펴보았다.
- 3장에선 역할, 책임, 협력이 객체지향적 코드를 작성하기 위한 핵심이란 사실을 배웠다.
- 4장에선 데이터에 초점을 맞출 때 어떤 문제점이 발생하는지에 관해 살펴봤다.
- 이번 장에선, 2장에서 소개한 **코드의 설계 과정을 한 걸음씩 따라가 보면서 객체에 책임을 할당하는 기본적인 원리**를 살펴본다.

---

## 01. 책임 주도 설계를 향해

- **데이터 중심 설계에서 책임 중심 설계의 전환**을 위해선 다음 두 가지 원칙을 따라야 한다.
    - **데이터보다 행동을 먼저 결정**하라.
    - **협력이란 문맥 안에서 책임을 결정**하라.

### 데이터보다 행동을 먼저 결정하라

- **객체에게 중요한 것은 데이터가 아닌, 외부에 제공하는 행동**이다.
    - 클라이언트 관점에서 **객체의 행동은 곧 객체의 책임을 의미**한다.
    - **객체는 협력에 참여하기 위해 존재하며, 협력 내에서 수행하는 책임이 객체의 존재 가치를 증명**한다.

- 데이터는 오로지 **객체가 책임을 수행하는 데 필요한 재료를 제공**할 뿐이다.
    - 이른 시기에 데이터에 초점을 맞추면 캡슐화가 약화되고, 이로 인해 낮은 응집도와 높은 결합도를 가진 객체들로 넘쳐나게 된다.
    - 이로 인해 얻게되는 것은 변경에 취약한 설계다.

- 객체의 데이터에서 행동으로 무게 중심을 옮기기 위한 기본 방법은, **객체 설계를 위한 질문의 순서를 바꾸는 것**이다.
    - **데이터 중심 설계**
        - “이 객체가 포함해야 하는 데이터가 무엇인가?” 를 우선적으로 결정한다.
        - 그리고 후에 “데이터를 처리하는 데 필요한 연산은 무엇인가?” 를 결정한다.
    - **책임 중심 설계**
        - “이 객체가 수행해야 하는 책임은 무엇인가?” 를 우선적으로 결정한다.
        - 그리고 후에 “이 책임을 수행하는 데 필요한 데이터는 무엇인가”를 결정한다.
    - 다시 말해 **책임 중심 설계에선 객체의 행동, 즉 책임을 먼저 결정한 후 객체 상태를 결정**한다.

- 객체지향 설계에서 가장 중요한 것은 **객체에 적절한 책임을 할당하는 것**이다.
    - 그렇다면 객체에게 어떤 책임을 할당해야 할까?
    - 이에 대한 실마리를 협력에서 찾을 수 있다.

### 협력이란 문맥 안에서 책임을 결정하라

- **객체에 할당된 책임의 품질은 협력에 적합한 정도로 결정**된다.
    - 책임이 협력에 어울리지 않는다면, 그 책임은 나쁜 것이다.
- 객체 입장에서 책임이 조금 어색해 보이더라도, 협력에 적합하다면 그 책임은 좋은 것이다.
    - 책임은 객체 입장이 아닌, 객체가 참여하는 협력에 적합해야 한다.

- 협력을 시작하는 주체는 메시지 전송자이기에, **협력에 적합한 책임이란 메시지 수신자가 아닌 메시지 전송자에게 적합한 책임을 의미**한다.
- 협력에 적합한 책임을 수확하기 위해선, 객체 결정 후 메시지를 선택해선 안된다.
    - **메시지를 결정한 후에 객체를 선택**해야 한다.
    - 메시지가 존재하기에 그 메시지를 처리할 객체가 필요한 것이다.
    - **객체가 메시지를 선택하는 것이 아닌, 메시지가 객체를 선택하게 해야한다.**

> 클래스 결정 후 클래스의 책임을 찾는 대신, 메시지를 결정하고 메시지를 누구에게 전송할지 찾아보게 되었다.

메시지 기반 설계 관점은 클래스 기반 설계 관점보다 훨씬 유연한 애플리케이션을 만들 수 있게 해준다.
”이 클래스가 필요한건 알겠는데 이 클래스는 무엇을 해야하지?” 보단,
**”메시지를 전송해야 하는데 누구에게 전송해야 하지?”** 라고 질문하는 것.

설계의 핵심 질문을 이렇게 바꾸는 것이 메시지 기반 설계로 향하는 첫 걸음이다.
객체를 가지고 있기에 메시지를 보내는 것이 아니다. 메시지를 전송하기에 객체를 갖게 된 것이다.
[Metz12].
> 

- **메시지가 클라이언트의 의도를 표현한다는 사실에 주목**하자.
- 그리고 **객체를 결정하기 전 객체가 수신할 메시지를 먼저 결정한다는 점 역시 주목**하자.
- 클라이언트는 어떤 객체가 메시지를 수신할지 알지 못한다.
    - 오로지 어떤 임의의 객체가 메시지를 수신할 것이라 믿고 자신의 의도를 표현할 메시지를 전송할 뿐이다.
    - **메시지를 수신하는 객체는 메시지를 처리할 ‘책임’을 할당**받게 된다.

- 메시지를 먼저 결정하기에 **메시지 송신자는 수신자에 대한 어떤 가정도 할 수 없다.**
    - 즉, 메시지 전송자 관점에서 메시지 수신자가 깔끔하게 캡슐화된다.
    - 이렇게 하면, **협력이란 문맥 내에서 메시지에 집중하게 되기에 캡슐화를 지키기 훨씬 쉬워진다.**
    - **책임 중심 설계가 응집도가 높고, 결합도가 낮으며 변경이 쉬운 이유**가 여기에 있다.

- 정리해보자.
    1. 객체에 적합한 책임을 할당하기 위해선 **협력이란 문맥을 고려**해야 한다.
    2. **협력이란 문맥 내에서 책임이란 클라이언트 관점에서 적절한 책임을 의미**한다.
    3. 올바른 객체지향 설계는 **클라이언트가 전송할 메시지를 결정한 후에야 객체 상태를 저장하는 데 필요한 내부 데이터에 관해 고민**한다.
    
    ⇒ 결론적으로 **책임 중심 설계에선 협력이란 문맥 내에서 객체가 수행할 책임에 초점**을 맞춘다.
    

### 책임 주도 설계

- 3장에서 설명한 책임 주도 설계의 흐름은 다음과 같다.
    1. 시스템이 사용자에게 제공해야 하는 기능인 시스템 책임을 파악한다.
    2. 시스템 책임을 더 작은 책임으로 분할한다.
    3. 분할된 책임을 수행할 적절한 객체 혹은 역할을 찾아 책임을 할당한다.
    4. 객체가 책임을 수행하는 도중 다른 객체의 도움이 필요한 경우, 이를 책임질 객체 혹은 역할을 찾는다.
    5. 해당 객체 혹은 역할에게 책임을 할당함으로써, 두 객체가 협력하도록 한다.

- 핵심은 결국 **책임을 결정한 후, 책임을 수행할 객체를 결정하는 것**이다.
    - **협력에 참여하는 객체들의 책임이 적절히 정리되기 전까진 객체 내부 상태에 관심을 가지지 않는다.**

---

## 02. 책임 할당을 위한 `GRASP` 패턴

- 대중적으로 널리 알려진 책임 할당 기법으로 **`GRASP` 패턴**이 있다.
    - **`General Responsibility Assignment Software Pattern`**
    - 일반적인 책임 할당을 위한 소프트웨어 패턴의 약자다.
    - **객체에 책임을 할당할 때 지침으로 삼을 수 있는 원칙들의 집합**을 패턴 형식으로 정리한 것이다.

- 영화 예매 시스템을 책임 중심으로 설계하는 과정을 따라가보자.

### 도메인 개념에서 출발하기

- 설계 시작 전, **도메인에 대한 개략적 모습을 그려보는 것이 유용**하다.
    - 도메인 안엔 무수히 많은 개념이 존재하며, 이 도메인 개념들을 책임 할당의 대상으로 사용하면 코드에 도메인의 모습을 투영하기 좀 더 수월해진다.
    - **따라서 책임 할당 시 가장 먼저 고민해야 하는 유력 후보는 도메인 개념**이다.

![image](https://github.com/user-attachments/assets/19f2ee1e-0847-4f91-8f49-2bca20097296)

- 위 그림은 도메인 개념과 개념 사이의 관계를 대략적으료 표현한 것이다.
    - 아래 개념들을 파악할 수 있다.
        - 한 영화는 여러 번 상영 가능하고, 한 상영은 여러 번 예약될 수 있다는 사실을 알 수 있다.
        - 영화는 다수 할인 조건을 가질 수 있으며, 할인 조건엔 순번 조건, 기간 조건이 존재한다.
        - 영화는 금액, 비율에 따라 할인 가능하나 동시에 두 가지 할인 정책을 표현할 수 없다.

- 설계 시작 단계에선 개념들의 의미, 관계가 정확하거나 완벽할 필요는 없다.
    - 단지 출발점이 필요할 뿐이다.
    - 책임을 할당받을 객체들의 종류, 관계에 대해 유용한 정보를 제공할 수 있다면 충분하다.
    
- **중요한 것은 설계 시작이며, 도메인 개념들을 완벽히 정리하는 것이 아니다.**
    - 도메인 개념을 정리하는 데 너무 많은 시간을 들이지 말고, **빠르게 설계와 구현을 진행**하자.

### 올바른 도메인 모델이란 존재하지 않는다

![image](https://github.com/user-attachments/assets/99ae2895-4669-4219-b9e8-318c1216bc05)

- 그림 5.1과 2장에서 사용했던 그림 2.3은 서로 다르다.
    - 그림 2.3에선 할인 정책이란 개념이 독립적 개념으로 분리되어 있었다.
    - 그림 5.1에선 영화의 종류로 표현되어 있다.

- **두 도메인 모델 모두 올바른 구현을 이끌어낼 수만 있다면, 둘 모두 올바른 도메인 모델**이다.

- 도메인 모델은 구현과 무관하다고 생각하는 사람들이 많지만, 그건 오해다.
    - 도메인 모델은 도메인을 개념적으로 표현한 것이지만, **그 안에 포함된 개념, 관계는 구현의 기반**이 되어야 한다.
        - 이는 **도메인 모델이 구현을 염두에 두고 구조화되는 것이 바람직함**을 의미한다.
    - 반대로 코드의 구조가 도메인을 바라보는 관점을 바꾸기도 한다.

- 실제 이번 장의 구현 코드는 2장과는 다를 것이다.
    - 이는 도메인 모델 구조가 코드 구조에 영향을 끼치기 때문이다.
- 하지만 마지막엔 2장과 동일하게 변경된다.
    - 유연성, 재사용성 등과 같이 **실제 코드를 구현하며 얻게되는 통찰이 역으로 도메인에 대한 개념을 바꾸기 때문**이다.

- 이는 올바른 도메인 모델이란 존재하지 않는단 사실을 잘 보여준다.
    - **필요한 것은 도메인을 그대로 투영한 모델이 아닌, 구현에 도움이 되는 모델**이다.
    - 즉, **실용적이면서 유용한 모델**이 정답이다.

### 정보 전문가에게 책임을 할당하라

- 책임 주도 설계 방식의 첫 단계는 애플리케이션의 제공 기능을 애플리케이션의 책임으로 생각하는 것이다.
    - 해당 책임을 애플리케이션에 대해 전송된 메시지로 간주한다.
    - 그리고, 이 메시지를 책임질 첫 객체를 선택하는 것으로 설계를 시작한다.

- 사용자에게 제공해야 할 기능은 영화 예매다.
    - 즉, 애플리케이션은 영화를 예매할 책임이 있다.
    - 다음으로 책임 수행에 필요한 메시지를 결정한다.
    - 메시지는 전송할 객체의 의도를 반영하여 결정한다.

- 따라서 첫 질문은 다음과 같다.
    
    > **메시지를 전송할 객체는 무엇을 원하는가?**
    > 
    - 협력을 시작할 객체는 미정이나 원하는 것은 분명하게 “영화 예매”다.

![image](https://github.com/user-attachments/assets/6f9e88ef-ee6b-4a39-a009-d2fbbc0c2877)

- 고로 첫 메시지의 이름으론 “예매하라”가 적절해 보인다.

- 메시지를 결정했으므로 이제 메시지에 적합한 객체를 선택해야 하는데, 이에 따른 두 번째 질문은 다음과 같다.
    
    > **메시지를 수신할 적합한 객체는 누구인가?**
    > 
    
- 위 질문에 답하기 위해선 객체가 상태, 행동을 통합한 캡슐화 단위란 사실에 집중해야 한다.
    - 객체는 자신의 상태를 스스로 처리하는 존재이며, 책임과 책임을 수행하는 데 필요한 상태는 동일 객체 내에 존재해야 한다.
    - 고로 객체에게 책임을 할당하는 첫 원칙은, **책임을 수행할 정보를 아는 객체에게 책임을 할당하는 것**이다.
        
        ⇒ **GRASP에선 이를 INFORMATION EXPERT(정보 전문가) 패턴이라고 부른다.**
        

- **정보 전문가 패턴은 객체가 자율적 존재**여야 한다는 사실을 다시금 상기시킨다.
    - 정보를 알고 있는 객체만이 책임을 어떻게 수행할지 스스로 결정할 수 있기 때문이다.
    - 이 패턴을 따르면 정보, 행동을 최대한 가까운 곳에 위치시키기에 캡슐화를 유지할 수 있다.
    - 결과적으로 **필요한 정보들을 가진 객체들로 책임이 분산**된다.
        
        ⇒ 즉, 높은 응집도가 낮은 결합도를 유지하여 이해하기 쉽고 유지보수하기 쉬운 시스템 구축이 가능하다.
        
- 정보 전문가 패턴은 객체가 자신이 소유하고 있는 정보와 관련된 작업을 수행한다는 일반적 직관을 표현한 것이다.
    - 여기서의 정보는 데이터와는 다르다.
    - **책임 수행 객체가 정보를 ‘알고’ 있다고 해서, 그 정보를 ‘저장’ 할 필요는 없다.**
        - 정보를 제공할 다른 객체를 알고 있거나, 필요한 정보를 계산해서 제공할 수도 있다.
    
    ⇒ 중요한 것은 **정보 전문가가 데이터를 반드시 저장할 필요는 없다는 사실을 이해하는 것**이다.
    
![image](https://github.com/user-attachments/assets/f6a7126f-9023-4c8a-961e-fe5e36608efe)

- 정보 전문가 패턴에 따르면, **예매하는 데 필요한 정보를 가장 많이 알고 있는 객체**에 “예매하라” 메시지를 처리할 책임을 할당해야 한다.
    - 이는 ‘상영’ 이라는 도메인 개념이 가장 적합해보인다.
    - 영화에 대한 정보, 상영 시간, 상영 순번과 같이 영화 예매에 필요한 정보를 알고 있다.
    - **영화 예매를 위한 정보 전문가이므로, `Screening` 에 예매를 위한 책임을 할당**한다.

- “예매하라” 메시지 수신 시 `Screening` 이 수행할 작업의 흐름에 대해 고민해보자.
    - **이제부턴 `Screening` 내부로 들어가 메시지를 처리하기 위한 절차, 구현을 고민**해본다.
    - 지금은 대략적으로 객체들의 책임을 결정하는 단계이므로, 세세한 부분은 고민하지 않는다.
    - 그저 책임을 수행하는 데 필요한 작업을 구상해보고, 스스로 할 수 없는 작업이 무엇인지 가릴정도면 된다.

- 스스로 처리할 수 없는 작업이 있다면 **외부에 도움을 요청**해야 한다.
    - **요청이 곧 외부에 전송할 새로운 메시지가 되고, 메시지가 곧 새로운 객체의 책임으로 할당**된다.
    - **이와 같은 연쇄적인 메시지 전송, 수신을 통해 협력 공동체가 구성**된다.

- “예매하라” 메시지를 완료하기 위해선 예매 가격을 계산해야 한다.
    - 영화 한 편의 가격을 계산한 금액에 예매 인원수를 곱한 값으로 구할 수 있다.
    - 즉, 영화 한 편의 가격을 알아야 하지만 `Screening` 은 이 정보를 알 수 없다.
    - 고로, 외부의 객체에 도움을 요청하여 가격을 얻어야 한다.

![image](https://github.com/user-attachments/assets/24829992-c8b0-4c4e-b3fa-bbaa04dff678)

- “가격을 계산하라” 라는 새로운 메시지를 통해 가격을 얻도록 한다.

![image](https://github.com/user-attachments/assets/cbc4638f-7d39-4a94-9c25-a1298db37e43)

- 해당 메시지를 책임질 객체를 선택하기 위해, 정보 전문가에게 책임을 할당한다.
    - 영화 가격 계산과 관련된 정보를 알고 있는 전문가는 당연히 `Movie` 다.
    - `Movie` 는 앞으로 영화 가격을 계산할 책임을 지게 된다.

- `Movie` 는 가격을 계산하기 위해 무엇을 해야할까?
    - 영화가 할인 가능한지 판단 후 할인 정책에 따라 할인 요금을 제외한 금액을 계산하면 된다.
    - 하지만 여기서 할인 조건에 따라 영화가 할인 가능한지 판단하는 일은 `Movie` 가 스스로 할 수 없다.

![image](https://github.com/user-attachments/assets/ebbbd57e-69e0-4ed1-bd76-60f589e41050)

- 고로 다음은 “할인 여부를 판단하라” 라는 메시지를 전송하여 외부의 도움을 요청한다.

![image](https://github.com/user-attachments/assets/6c5231f9-ed9c-4c4c-9c33-58434f8a4719)

- 이 메시지에 대한 정보 전문가는 `DiscountCondition` 이다.
    - 할인 여부를 판단하는 데 필요한 모든 정보를 알고 있으므로, 외부 도움 없이 스스로 할인 여부를 판단할 수 있다.
    - 따라서 외부에 메시지를 전송하지 않는다.

- 이와 같이 **정보 전문가 패턴은 객체에 책임을 할당할 때 가장 기본이 되는 책임 할당 원칙**이다.
- 정보 전문가 패턴은 객체란 상태, 행동을 함께 가지는 단위라는 객체지향의 가장 기본적 원리를 책임 할당 관점에서 표현한다.
    - **정보 전문가 패턴을 따르는 것만으로 자율성이 높은 객체들로 구성된 협력 공동체를 구축할 가능성이 높아진다.**

### 높은 응집도와 낮은 결합도

- 설계는 트레이드오프 활동으로, 동일 기능을 구현하는 매우 많은 설계가 존재한다.
    - 고로 설계를 진행하다 보면 **몇 가지 설계 중 한 가지를 선택해야 하는 경우가 빈번히 발생**한다.
    - 이 경우엔 정보 전문가 패턴 외 다른 책임 할당 패턴들을 함께 고려할 필요가 있다.

![image](https://github.com/user-attachments/assets/b245de56-e889-42d7-ba09-c1114e7076e0)

- 이전엔 `Movie` 가 **`DiscountCondition` 에 할인 여부를 판단하라 메시지를 전송**한다.
    - 그럼 설계 대안으로 `Movie` 대신 `Screening` 이 직접 `DiscountCondition` 과 협력하게 하는 것은 어떨까?

- 이 설계는 기능적 측면에선 이전 설계와 동일하다.
    - 차이는 그저 `DiscountCondition` 과 협력하는 객체가 `Screening` 이라는 것뿐이다.

- `Movie`, `DiscountCondition` 이 협력하는 방법을 선택한 이유는, **응집도와 결합도**에 있다.
    - 책임을 할당할 수 있는 다양한 대안이 있다면, 응집도와 결합도 측면에서 나은 대안을 선택하는게 좋다.
        
        ⇒ `GRASP` 에선 이를 **LOW COUPLING(낮은 결합도) 패턴**과 **HIGH COHESION(높은 응집도 패턴)**이라고 부른다.
        

- `DiscountCondition` 이 `Movie` 와 협력하는 것이 좋은 이유는 **결합도**에 있다.
    - `Movie` 는 `DiscountCondition` 목록을 이미 속성으로 포함하고 있고, 이미 결합되어 있다.
    - 고로 이 둘을 서로 협력하게 하면, **설계 전체적으로 결합도를 추가하지 않고 협력의 완성이 가능**하다.

- `Screening` 이 `DiscountCondition` 과 협력할 경우 둘 사이에 새로운 결합도가 추가된다.
    - **낮은 결합도 패턴의 관점에선 `Movie` ↔ `DiscountCondition` 사이의 협력이 더 나은 설계 대안**이다.

- **높은 응집도** 패턴의 관점에서도 설계 대안들을 평가할 수 있다.
    - **`Screening`** 의 가장 중요한 책임은 **예매 생성**이다.
    - 하지만 `DiscountCondition` 와 협력한다면, 영화 요금 계산과 관련된 책임 일부를 떠맡아야 한다.
    - `Screening` 은 `DiscountCondition` 이 할인 여부를 판단할 수 있다는 사실과 `Movie` 가 할인 여부를 알아야 한다는 사실 역시 알고 있어야 한다.

- 즉, **예매 요금 계산 방식이 변경될 경우 `Screening` 역시 변경되어야 하는 것**이다.
    - 결과적으로 `Screening` 과 `DiscountCondition` 이 협력하게 되면, `Screening` 은 서로 다른 이유로 변경되는 책임을 짊어지게 되므로 응집도가 낮아진다.

- `Movie` 는 영화 요금을 계산하는 것이 주된 책임이므로, `DiscountCondition` 과 협력하는 것이 응집도에 아무런 해도 끼치지 않는다.
    - 따라서 **높은 응집도 패턴의 관점에서 `Movie` ↔ `DiscountCondition` 사이의 협력이 더 나은 설계 대안**이다.

- 책임을 할당하고 코드를 작성하는 순간마다 **낮은 결합도, 높은 응집도의 관점에서 전체 설계 품질을 검토하면, 단순하면서도 재사용 가능하고 유연한 설계**를 얻을 수 있다.

### 창조자에게 객체 생성 책임을 할당하라

- 영화 예매 협력의 최종 결과물은 `Reservation` 인스턴스를 생성하는 것이다.
    - 이는 협력에 참여하는 어떤 객체에겐 `Reservation` 인스턴스를 생성할 책임을 할당해야 함을 의미한다.

- `GRASP` 의 CREATOR(창조자) 패턴은 이 경우 사용할 수 있는 책임 할당 패턴이다.
    - **객체를 생성할 책임을 어떤 객체에 할당할지에 대한 지침을 제공**한다.

### CREATOR 패턴

- **객체 `A` 생성 시 어떤 객체에게 객체 생성 책임을 할당**해야 하는가?
- 아래 조건을 최대한 많이 만족하는 **`B` 에게 객체 생성 책임을 할당**하라.
    1. **`B` 가 `A` 객체를 포함하거나 참조**한다.
    2. **`B` 가 `A` 객체를 기록**한다.
    3. **`B` 가 `A` 객체를 긴밀하게 사용**한다.
    4. **`B` 가 `A` 객체를 초기화하는 데 필요한 데이터**를 가지고 있다. **(`B` 는 `A` 에 대한 정보 전문가다.)**

- 창조자 패턴은 **어떤 방식으로든 생성된 객체와 연결되거나 관련될 필요가 있는 객체에 해당 객체를 생성할 책임을 맡기는 것**이다.
    - 생성될 객체에 대해 잘 알고 있어야 하거나 그 객체를 사용해야 하는 객체는 **어떤 방식으로든 생성 객체와 연결될 것**이다.
    - 다시 말해 두 객체는 서로 결합된다.

- 이미 결합된 객체에게 생성 책임을 할당하는 것은, 설계의 전체 결합도에 영향을 미치지 않는다.
    - 결과적으로 **창조자 패턴은 이미 존재하는 객체 사이 관계를 이용하기에, 낮은 결합도를 유지**하게 만들어준다.

![image](https://github.com/user-attachments/assets/ae43ea91-bd85-49bf-bc3f-3d8e37e4678b)

- `Reservation` 을 잘 알거나, 긴밀하게 사용하거나, 초기화에 필요한 데이터를 가지고 있는 객체는 무엇일까?
    - 이는 바로 **`Screening`** 이다.
    - 예매 정보를 생성하는 데 필요한 영화, 상영 시간, 상영 순번 등의 정보에 대한 전문가이다.
    - 또한 예매 요금 계산에 필요한 `Movie` 도 잘 알고 있다.
    - 따라서 **`Reservation` 의 창조자**로 손색이 없어보인다.

- 대략적으로 영화 예매에 필요한 책임을 객체들에 할당했다.
    - 현재까지 책임 분배는 그저 설계 시작을 위한 대략적 스케치에 불과하다.
    - **실제 설계는 코드 작성 때 이뤄진다.**
- **협력, 책임이 제대로 동작하는지 확인할 수 있는 유일한 방법은 코드를 작성하고 실행해 보는 것**뿐이다.
    - **올바르게 설계하고 있는지 궁금하다면, 코드를 작성**하라.

---

## 03. 구현을 통한 검증

```kotlin
class Screening {
    fun reserve(customer: Customer, audienceCount: Int): Reservation {
        
    }
}
```

- `Screening` 은 **“에매하라” 메시지에 응답**할 수 있어야 하므로, **메시지를 처리할 메소드를 구현**한다.

```kotlin
class Screening(
    private val movie: Movie,
    private val sequence: Int,
    private val whenScreened: LocalDateTime,
) {
    fun reserve(customer: Customer, audienceCount: Int): Reservation {
        
    }
}
```

- 책임이 결정되었으므로 **책임 수행에 필요한 인스턴스 변수를 결정**해야 한다.
    - 상영 시간, 상영 순번을 인스턴스 변수로 포함한다.
    - **`Movie` 에 “가격을 계산하라” 메시지를 전송해야 하므로 `Movie` 에 대한 참조도 포함**한다.

```kotlin
class Screening(
    private val movie: Movie,
    private val sequence: Int,
    private val whenScreened: LocalDateTime,
) {
    fun reserve(customer: Customer, audienceCount: Int): Reservation =
        Reservation(customer, this, calculateFee(audienceCount), audienceCount)

    fun calculateFee(audienceCount: Int): Money =
        movie.calculateMovieFee(this).times(audienceCount)
}
```

- 영화 예매를 위해선 `movie` 에게 **“가격을 계산하라” 메시지를 전송**하여 계산된 영화 요금을 반환받아야 한다.
    - `calculateFee` 메소드는 반환된 요금에 예매 인원 수를 곱해 전체 예매 요금을 계산한다.

- 구현 과정에서 **`Movie` 에 전송하는 메시지 시그니처를 `calculateMovieFee` 로 선언했다는 사실에 주목**하자.
    - 이 메시지는 수신자인 `Movie` 가 아닌, 송신자인 `Screening` 의 의도를 표현한다.
    - 중요한 점은 **`Movie` 의 내부 구현에 대한 어떠한 지식도 없이 전송 메시지를 결정했다는 점**이다.
    - 이처럼 **구현의 고려 없이 필요한 메시지를 결정하면, `Movie` 의 내부 구현을 깔끔히 캡슐화**할 수 있다.

- **`Screening`, `Movie` 사이의 유일한 연결고리는 이제 메시지**뿐이다.
    - 따라서 **메시지가 변경되지 않는 한, `Movie` 에 어떤 수정을 하더라도 `Screening` 엔 영향이 없다.**
    - 이처럼 메시지가 객체를 선택하도록 **책임 주도 설계 방식을 따르면, 캡슐화와 낮은 결합도란 목표를 손쉽게 달성**할 수 있다.

```kotlin
class Movie {
    fun calculateFee(screening: Screening): Money {
        
    }
}
```

- 이제 `Movie` 는 메시지에 응답하기 위해 메소드를 구현한다.
    - 요금 계산을 위해 기본 금액과 할인 조건, 할인 정책 등의 정보를 알아야 한다.

```kotlin
class Movie(
    private val title: Stirng,
    private val runningTime: Duration,
    private val fee: Money,
    private val discountCondition: List<DiscountCondition>,
    private val movieType: MovieType,
    private val discountAmount: Money,
    private val discountPercent: Double
) {
    fun calculateFee(screening: Screening): Money {

    }
}
```

- 현 설계에서 할인 정책을 `Movie` 의 일부로 구현하기에, 할인 정책을 구성하는 할인 금액과 할인 비율을 인스턴스 변수로 선언한다.
    - 그리고 어떤 할인 정책이 적용된 영화인지 파악하기 위해 영화 종류를 인스턴스 변수로 포함한다.

```kotlin
enum class MovieType {
    AMOUNT_DISCOUNT, // 금액 할인 정책
    PERCENT_DISCOUNT, // 비율 할인 정책
    NONE_DISCOUNT // 미적용
}
```

- `MoneyType` 은 할인 정책 종류를 나열하는 단순한 열거형 타입이다.

```kotlin
class Movie(

) {
    fun calculateFee(screening: Screening): Money {
        if (isDiscountable(screening)) {
            return fee.minus(calculateDiscountAmount())
        }

        return fee
    }

    fun isDiscountable(screening: Screening): Boolean =
        discountConditions.stream()
            .anyMatch { condition ->
                condition.isSatisfiedBy(screening)
            }
}
```

- `Movie` 는 먼저 `discountConditions` 의 원소를 순회하며 `DiscountConditin` 인스턴스에게 `isSatisfiedBy` 메시지를 전송하여 할인 여부를 판단하도록 요청한다.
    - 할인 조건을 만족하는 경우 할인 요금의 계산을 위해 `calculateDiscountAmount` 메소드를 호출한다.
    - 만족하는 할인 조건이 없는 경우 기본 금액인 `fee` 를 반환한다.

```kotlin
private fun calculateDiscountAmount(): Money {
    when (movieType) {
        MovieType.AMOUNT_DISCOUNT -> calculateAmountDiscountAmount()
        MovieType.PERCENT_DISCOUNT -> calculatePercentDiscountAmount()
        MovieType.NONE_DISCOUNT -> calculateNoneDiscountAmount()
    }
}
```

- `calculateDiscountAmount` 메소드는 `movieType` 의 값에 따라 적합한 메소드를 호출한다.

```kotlin
class DiscountCondition {
    fun isSatisfiedBy(screening: Screening): Boolean
}
```

- `Movie` 는 각 `DiscountCondition` 에 “할인 여부를 판단하라” 메시지를 전송한다.
    - 메시지 처리를 위해 `isSatisfiedBy` 메소드를 구현해야 한다.

```kotlin
class DiscountCondition(
    private val type: DiscountConditionType,
    private val sequence: Int,
    private val dayOfWeek: DayOfWeek,
    private val startTime: LocalTime,
    private val endTime: LocalTime,
) {
    fun isSatisfiedBy(screening: Screening): Boolean {
        return if (type == DiscountConditionType.PERIOD) {
            isSatisfiedByPeriod(screening)
        } else {
            isSatisfiedBySequence(screening)
        }
    }

    private fun isSatisfiedByPeriod(screening: Screening): Boolean {
        val screeningTime = screening.getWhenScreened()
        return dayOfWeek == screeningTime.dayOfWeek &&
                startTime <= screeningTime.toLocalTime() &&
                endTime.isAfter(screeningTime.toLocalTime())
    }

    private fun isSatisfiedBySequence(screening: Screening): Boolean {
        return sequence == screening.getSequence()
    }
}
```

- `DiscountCondition` 은 기간 조건을 위한 요일, 시작 시간, 종료 시간, 순번 조건을 위한 상영 순번을 인스턴스 변수로 포함한다.
    - 추가적으로 할인 조건의 종류 역시 인스턴스 변수로 포함한다.

```kotlin
class Screening(
    private val movie: Movie,
    val sequence: Int,
    val whenScreened: LocalDateTime,
)
```

- `DiscountCondition` 은 할인 조건 판단을 위해 `Screening` 의 상영 시간, 상영 순번을 알아야 한다.
    - 고로 내부 프로퍼티의 접근 지정자를 수정한다. (`private` → `public`)

```kotlin
enum class DiscountConditionType {
    SEQUENCE,   // 순번 조건
    PERIOD,     // 기간 조건
}
```

- `DiscountConditionType` 은 단순히 할인 조건의 종류를 나열한다.

- 구현은 완료되었으나, 코드엔 몇 가지 문제점이 숨어 있다.

### DiscountCondition 개선하기

- 가장 큰 문제는 변경에 취약한 클래스를 포함하고 있다는 점이다.
    - 이는 **코드를 수정해야 하는 이유가 하나 이상 가지는 클래스가 있음을 의미**한다.

- **`DiscountCondition` 은 다음 세 가지 이유로 변경**될 수 있다.
    1. 새로운 할인 조건 추가
        - `isSatisfiedBy` 메소드 내 `if ~ else` 구문을 수정해야 한다.
        - 새로운 할인 조건이 새로운 데이터를 요구한다면, `DiscountCondition` 에 속성을 추가하는 작업도 필요하다.
    2. 순번 조건을 판단하는 로직 변경
        - `isSatisfiedBySequence` 메소드 내부 구현을 수정해야 한다.
        - 순번 조건을 판단하기 위한 데이터가 변경된다면, `DiscountCondition` 의 `sequence` 속성도 변경해야 한다.
    3. 기간 조건을 판단하는 로직이 변경되는 경우
        - `isSatisfiedByPeriod` 메소드의 내부 구현을 수정해야 한다.
        - 기간 조건을 판단하기 위한 데이터가 변경된다면, `DiscountCondition` 의  `dayOfWeek`, `startTime`, `endTime` 속성도 변경해야 한다.

- 위와 같이 여러 변경 이유를 가지기에 **`DiscountCondition` 클래스는 응집도가 낮다.**
    - 이는 **서로 연관없는 기능 혹은 데이터가 한 클래스 내에 뭉쳐져 있음을 의미**한다.
    - 문제 해결을 위해서는 변경의 이유에 따라 클래스를 분리해야 한다.

- 지금까지 살펴본 것처럼 **설계를 개선하는 작업은 변경의 이유가 하나 이상인 클래스를 찾는 것부터 시작하는 것**이 좋다.
    - 클래스 내에서 변경의 이유를 찾는 것은 입문자에게 생각보다 어렵다.
    - 하지만, **변경 이유가 하나 이상인 클래스엔 위험 징후를 또렷하게 나타내는 몇 가지 패턴이 존재**한다.

- 코드를 통해 변경 이유를 파악할 수 있는 첫 방법은, **인스턴스 변수가 초기화되는 시점을 살펴보는 것**이다.
    - **응집도가 높은 클래스의 경우 인스턴스 생성 시 모든 속성을 함께 초기화**한다.
    - 반면 **응집도가 낮은 클래스는 객체 속성 중 일부만 초기화하고, 일부는 초기화하지 않은 상태**로 남겨둔다.

- `DiscountCondition` 클래스의 경우, 순번 조건을 표현하는 경우 `sequence` 만 초기화된다.
    - `dayOfWeek`, `startTime`, `endTime` 은 초기화되지 않는다.
    - 이처럼 클래스 속성이 서로 다른 시점에 초기화되거나 일부만 초기화되는 것은 응집도가 낮다는 증거다.
    - **따라서, 함께 초기화되는 속성을 기준으로 코드를 분리**해야 한다.

- 두 번째 방법은 메소드들이 **인스턴스 변수를 사용하는 방식을 살펴보는 것**이다.
    - **모든 메소드가 객체의 모든 속성을 사용한다면, 클래스 응집도가 높다고 볼 수 있다.**
    - 반면 사용 속성에 따라 메소드의 그룹이 나뉜다면 클래스의 응집도가 낮다고 볼 수 있다.
    - `DiscountCondition` 클래스의 `isSatisfiedBySequence`, `isSatisfiedByPeriod` 메소드가 이 경우에 해당한다.

- 클래스가 다음과 같은 징후로 몸살을 앓고 있다면 클래스의 응집도는 낮은 것이다.
    1. **클래스가 하나 이상의 이유로 변경돼야 한다면 응집도가 낮은 것**이다. 
        - 변경의 이유를 기준으로 클래스를 분리하라.
    2. **클래스의 인스턴스를 초기화하는 시점에 경우에 따라 서로 다른 속성들을 초기화하고 있다면 응집도가 낮은 것**이다. 
        - 초기화되는 속성의 그룹을 기준으로 클래스를 분리하라.
    3. **메서드 그룹이 속성 그룹을 사용하는지 여부로 나뉜다면 응집도가 낮은 것**이다. 
        - 이들 그룹을 기준으로 클래스를 분리하라.

- `DiscountCondition` 클래스는 위 세 가지 징후가 모두 들어있으므로, 변경의 이유에 따라 여러 클래스로 분리해야 한다.

### 타입 분리하기

- `DiscountCondition` 의 가장 큰 문제는 **순번 조건, 기간 조건이란 두 개의 독립적 타입이 한 클래스 내에 공존하고 있다는 사실**이다.
    - 가장 먼저 떠오르는 해결 방법은, **두 타입을 `SequenceCondition`, `PeriodCondition` 으로 분리**하는 것이다.

```kotlin
class PeriodCondition(
    private val dayOfWeek: DayOfWeek,
    private val startTime: LocalTime,
    private val endTime: LocalTime,
) {
    fun isSatisfiedBy(screening: Screening): Boolean =
        dayOfWeek == screening.whenScreened.dayOfWeek &&
                startTime <= screening.whenScreened.toLocalTime() &&
                endTime >= screening.whenScreened.toLocalTime()
}

class SequenceCondition(
    private val sequence: Int,
) {
    fun isSatisfiedBy(screening: Screening): Boolean =
        sequence == screening.sequence
}
```

- 클래스를 분리하면 앞서 얘기했던 모든 문제점들이 해결된다.
    - 두 클래스는 **자신의 모든 인스턴스 변수들을 함께 초기화**할 수 있다.
    - **클래스에 있는 모든 메소드는 동일한 인스턴스 변수 그룹을 사용**한다.
        
        ⇒ 결과적으로 **개별 클래스들의 응집도가 향상**되었다.
        

![image](https://github.com/user-attachments/assets/12aee310-201b-497a-a571-5a7684d2da8e)

- 하지만 클래스 분리 후 문제가 한 가지 나타났다.
    - `Movie` 와 협력하는 클래스는 오직 `DiscountCondition` 이었다.
    - 하지만 수정 후엔 **`Movie` 가 `PeriodCondition`, `SequenceCondition` 이라는 서로 다른 두 클래스의 인스턴스 모두와 협력**할 수 있어야 한다.

```kotlin
class Movie(
    private val periodConditions: List<PeriodCondition>,
    private val sequenceConditions: List<SequenceCondition>
) {
    fun isDiscountable(screening: Screening): Boolean =
        checkPeriodConditions(screening) || checkSequenceConditions(screening)

    private fun checkPeriodConditions(screening: Screening): Boolean =
        periodConditions.any { it.isSatisfiedBy(screening) }

    private fun checkSequenceConditions(screening: Screening): Boolean =
        sequenceConditions.any { it.isSatisfiedBy(screening) }
}
```

- 문제 해결을 위해 가장 먼저 떠오르는 방법은, `Movie` 클래스 내에 각 클래스의 목록을 따로 유지하는 것이다.
    - 하지만 이 방법은 두 가지 문제를 야기한다.
        1. **`Movie` 가 `PeriodCondition`, `SequenceCondition` 클래스 양쪽 모두에게 결합**된다.
            - 클래스 분리 후 설계 관점에서 전체적인 결합도가 높아졌음을 의미한다.
        2. **새로운 할인 조건을 추가하기 더 어려워졌다.**
            - 새 할인 조건을 담기 위한 `List` 를 `Movie` 의 인스턴스 변수로 추가해야 한다.
            - 이 `List` 를 활용해 할인 조건을 만족하는지 여부를 판단하는 메소드도 추가해야 한다.
            - `isDiscountable` 메소드 역시 수정이 불가피하다.

- 분리 전엔 `DiscountCondition` 의 내부 구현만 수정하면 `Movie` 엔 아무 영향도 미치지 않았다.
    - 하지만 **수정 후에 할인 조건을 추가하려면, `Movie` 도 함께 수정**해야 한다.
- `DiscountCondition` 입장에선 응집도가 높아졌으나, **변경 및 캡슐화의 관점에선 전체적으로 설계의 품질이 나빠졌다.**

### 다형성을 통해 분리하기

- **`Movie` 입장에선 `PeriodCondition`, `SequenceCondition` 은 아무 차이가 없다.**
    - 그저 **할인 여부를 판단하는 동일한 책임을 수행할 뿐**이다.
    - `Movie` 입장에선 두 클래스의 내부 구현이 그다지 중요하지 않다.
    - 오로지 할인 가능 여부를 반환해주기만 하면, 어떤 인스턴스든 상관하지 않는다.

![image](https://github.com/user-attachments/assets/41c1a3ea-df15-4557-8552-4f6bbf8eb806)

- 여기서 자연스럽게 **역할**의 개념이 등장한다.
    - `Movie` 입장에서 두 클래스가 동일한 책임을 수행한다는 것은, 동일한 역할을 수행함을 의미한다.
    - 역할은 협력 내에서 대체 가능성을 의미하기에, `PeriodCondition`, `SequenceCondition` 에 역할의 개념을 적용하면 `Movie` 가 구체적인 클래스는 알지 못한다.
    - **오로지 역할에 대해서만 결합되도록 의존성을 제한**할 수 있다.

- **역할을 사용하면 객체의 구체적 타입을 추상화**할 수 있다.
    - 이 때 일반적으로 역할을 구현하기 위해 **추상 클래스, 인터페이스**를 사용한다.
    - 구현의 공유 여부에 따라 추상 클래스, 인터페이스를 선택하여 사용할 수 있다.

```kotlin
interface DiscountCondition {
    fun isSatisfiedBy(screening: Screening): Boolean
}
```

- 할인 조건의 경우 `PeriodCondition`, `SequenceCondition` 클래스가 **구현을 공유할 필요는 없다.**
    - **따라서 `DiscountCondition` 라는 이름을 지닌 인터페이스를 이용해 역할을 구분**한다.

```kotlin
class SequenceCondition(
    private val sequence: Int
) : DiscountCondition { . . . }

class PeriodCondition(
    private val dayOfWeek: DayOfWeek,
    private val startTime: LocalTime,
    private val endTime: LocalTime
) : DiscountCondition { . . . }
```

- 그리고 두 인스턴스가 **`DiscountCondition`** 을 실체화할 수 있도록 수정한다.

```kotlin
class Movie(
    private val fee: Money,
    private val discountConditions: List<DiscountCondition>
) {
    fun calculateMovieFee(screening: Screening): Money {
        return if (isDiscountable(screening)) {
            fee.minus(calculateDiscountAmount())
        } else {
            fee
        }
    }

    private fun isDiscountable(screening: Screening): Boolean {
        return discountConditions.any { it.isSatisfiedBy(screening) }
    }
}
```

- **`Movie` 는 더 이상 협력하는 객체의 구체적 타입을 몰라도 상관없다.**
    - 오로지 객체가 `DiscountCondition` 의 역할을 수행할 수 있고, `isSatisfiedBy` 메시지를 이해할 수 있다는 사실만 알고 있으면 된다.
    - `Movie` 가 전송한 메시지를 수신할 객체의 구체적 클래스에 따라 적절한 메소드가 실행된다.
        - 즉, `Movie`, `DiscountCondition` 사이의 협력은 **다형적**이다.

![image](https://github.com/user-attachments/assets/ba1c5ac2-7e4b-43e3-b3bd-808ddc22a80a)

- **`DiscountCondition`** 처럼, 객체의 암시적 타입에 따라 행동을 분기해야 한다면, **암시적 타입을 명시적 클래스로 정의하고 행동을 나눔으로써 응집도 문제를 해결**할 수 있다.
    - 다시 말해, **객체 타입에 따라 변하는 행동이 있다면 타입을 분리하고 변화하는 행동을 각 타입의 책임으로 할당**하라는 것이다.
    - `GRASP` 에서는 이를 **POLYMORPHISM(다형성) 패턴**이라고 부른다.

### POLYMORPHISM 패턴

- **객체의 타입에 따라 변하는 로직이 있을 때 변하는 로직을 담당할 책임**을 어떻게 할당해야 하는가?
    - **타입을 명시적으로 정의하고 각 타입에 다형적으로 행동하는 책임을 할당**하라.
- 조건에 따른 변화는 프로그램의 기본 논리다.
    - 프로그램을 `if ~ else` 또는 `switch ~ case` 등의 조건 논리를 사용해서 설계한다면 **새로운 변화가 일어난 경우 조건 논리를 수정**해야 한다.
    - 이것은 프로그램을 수정하기 어렵고 변경에 취약하게 만든다.
- 다형성 패턴은 객체의 타입을 검사해서 타입에 따라 여러 대안들을 수행하는 **조건적인 논리를 사용하지 말라고 경고**한다.
    - 대신 **다형성을 이용해 새로운 변화를 다루기 쉽게 확장하라고 권고**한다.

### 변경으로부터 보호하기

- `DiscountCondition` 의 두 서브 클래스는 서로 다른 이유로 변경될 수 있다.
    - `SequenceCondition` → 순번 조건의 구현 방법이 변경된 경우
    - `PeriodCondition` → 기간 조건의 구현 방법이 변경된 경우
    - **두 개의 서로 다른 변경이 두 개의 서로 다른 클래스 안으로 캡슐화**된다.

- **새로운 할인 조건이 추가**되면 어떨까?
    - **`DiscountCondition` 이 구체적인 타입의 존재를 캡슐화, 즉 감춘다는 사실**을 잊지말자.
    - 이는 **`Movie` 의 관점에서 새로운 타입이 추가되더라도 전혀 영향을 받지 않음을 의미**한다.
    - 오직 `DiscountCondition` 의 **인터페이스를 실체화하는 클래스를 추가하는 것만으로, 할인 조건 종류를 확장**시킬 수 있다.

- 이처럼 **변경을 캡슐화하도록 책임을 할당하는 것을 `GRASP` 에선 PROTECTED VARIATIONS(변경 보호) 패턴**이라고 부른다.

### **PROTECTED VARIATIONS 패턴**

- 객체, 서브 시스템, 그리고 시스템을 어떻게 설계해야 변화와 불안정성이 다른 요소에 나쁜 영향을 미치지 않도록 방지할 수 있을까?
    - **변화가 예상되는 불안정한 지점들을 식별하고 그 주위에 안정된 인터페이스를 형성하도록 책임을 할당**하라.
- 변경 보호 패턴은 책임 할당의 관점에서 캡슐화를 설명한 것이다.
    - **“설계에서 변하는 것이 무엇인지 고려하고 변하는 개념을 캡슐화하라[GOF94]”** 라는 객체지향의 오랜 격언은 PROTECTED VARIATIONS 패턴의 본질을 잘 설명해준다.
    - 우리가 캡슐화해야 하는 것은 변경이다.
    - **변경이 될 가능성이 높은가? 그렇다면 캡슐화**하라.

### Movie 클래스 개선하기

- **`Movie` 역시 금액 할인 정책 영화, 비율 할인 정책 영화라는 두 가지 타입을 한 클래스 안에 구현**하고 있다.
    - 고로, 하나 이상의 이유로 변경될 수 있다.
    - **즉, 응집도가 낮다.**

- 이전처럼 **역할의 개념을 도입하여 협력을 다형적으로 만들어 해결**할 수 있다.
    - 다형성 패턴을 사용해 서로 다른 행동을 타입별로 분리하면, 다형성의 혜택을 누릴 수 있다.
    - `Screening`, `Movie` 가 메시지를 통해서만 다형적으로 협력하기에, `Movie` 의 타입이 새롭게 추가되더라도 `Screening` 에 영향을 미치지 않도록 만들 수 있다.
    
    ⇒ 즉 **변경 보호 패턴을 이용해 타입의 종류를 안정적인 인터페이스 뒤로 캡슐화할 수 있음을 의미**한다.
    

- 코드를 다음과 같이 개선할 수 있다.
    - 금액 할인 정책과 관련된 인스턴스 변수, 메소드가 담긴 클래스 → `AmountDiscountMovie`
    - 비율 할인 정책과 관련된 인스턴스 변수, 메소드를 옮겨 담을 클래스 → `PercentDiscountMovie`
    - 할인 정책을 적용하지 않는 경우 → `NoneDiscountMovie`

```kotlin
abstract class Movie(
    private val title: String,
    private val runningTime: Duration,
    private val fee: Money,
    private val discountConditions: List<DiscountCondition>
) {

    fun calculateMovieFee(screening: Screening): Money {
        return if (isDiscountable(screening)) {
            fee.minus(calculateDiscountAmount())
        } else {
            fee
        }
    }

    private fun isDiscountable(screening: Screening): Boolean {
        return discountConditions.any { it.isSatisfiedBy(screening) }
    }

    protected abstract fun calculateDiscountAmount(): Money
}
```

- `DiscountCondition` 과 달리 `Movie` 의 경우 **구현을 공유할 필요**가 있다.
    - 따라서, **추상 클래스**를 활용할 수 있다.

- 변경 전과 비교했을 때, `discountAmount`, `discountPercent` 와 이 변수들을 사용하는 메소드가 삭제되었음을 알 수 있다.
    - 이 변수들과 메소드들을 `Movie` 역할을 수행하는 적합한 자식 클래스로 옮길 것이다.

```kotlin
class AmountDiscountMovie(
    override val title: String,
    override val runningTime: Duration,
    override val fee: Money,
    private val discountAmount: Money,
    override val discountConditions: List<DiscountCondition>
) : Movie() {

    override fun calculateDiscountAmount(): Money {
        return discountAmount
    }
}
```

- 우선 할인 정책 종류에 따라 할인 금액을 계산하는 로직이 달라져야 한다.
    - `calculateDiscountAmount` 를 상속하게 함으로써, 구현을 재사용할 수 있다.

```kotlin
class PercentDiscountMovie(
    title: String,
    runningTime: Duration,
    fee: Money,
    private val percent: Double,
    discountConditions: List<DiscountCondition>
) : Movie(title, runningTime, fee, discountConditions) {

    override fun calculateDiscountAmount(): Money {
        return fee * percent
    }
}

class NoneDiscountMovie(
    title: String,
    runningTime: Duration,
    fee: Money,
    discountConditions: List<DiscountCondition>
) : Movie(title, runningTime, fee, discountConditions) {

    override fun calculateDiscountAmount(): Money {
        return Money.ZERO
    }
}
```

- 비율 할인 정책은 `PercentDiscountMovie` 에서 구현한다.
- 할인 정책 미적용의 경우 `NoneDiscountMovie` 클래스를 사용한다.

![image](https://github.com/user-attachments/assets/6cdf7f25-2620-4f42-b2f4-1ab2a2d3fa91)

- 이제 모든 구현이 끝났다.
    - **모든 클래스의 내부 구현은 캡슐화되어 있고, 모든 클래스는 변경의 이유를 하나씩만 가진다.**
    - **각 클래스는 응집도가 높고, 다른 클래스와 최대한 느슨하게 결합**되어 있다.
    - **클래스는 작고, 오직 한 가지 일만 수행**한다.
        
        ⇒ **이것이 책임을 중심으로 협력을 설계할 때 얻을 수 있는 혜택**이다.
        
- **결론은 데이터가 아닌 책임을 중심으로 설계하라는 것**이다.
    - **객체에게 중요한 것은 상태가 아닌 행동**이다.
    - **객체지향 설계의 기본은 책임, 협력에 초점을 맞추는 것**이다.

### 도메인의 구조가 코드의 구조를 이끈다

- **그림 5.7의 구조가 그림 5.1의 도메인 모델의 구조와 유사하다는 것**도 눈여겨보자.
    - 앞에서 설명한 것처럼 **도메인 모델은 단순히 설계에 필요한 용어를 제공하는 것을 넘어 코드의 구조에도 영향**을 미친다.
- 여기서 강조하고 싶은 것은 변경 역시 도메인 모델의 일부라는 것이다.
    - 도메인 모델에는 도메인 안에서 변하는 개념과 이들 사이의 관계가 투영되어 있어야 한다.
    - 그림 5.1의 도메인 모델에는 할인 정책과 할인 조건이 변경될 수 있다는 도메인에 대한 직관이 반영돼 있다.
    - 그리고 **이 직관이 우리의 설계가 가져야 하는 유연성을 이끌어냈다.**
- **다시 한번 강조하지만 구현을 가이드할 수 있는 도메인 모델을 선택**하라.
    - **객체지향은 도메인의 개념과 구조를 반영한 코드를 가능**하게 만든다.
    - 때문에, 도메인의 구조가 코드의 구조를 이끌어 내는 것은 자연스러울뿐만 아니라 바람직한 것이다.

### 변경과 유연성

- 설계를 주도하는 것은 변경이며, 개발자로서 변경에 대응할 수 있는 두 가지 방법이 있다.
    1. 코드를 이해하고 수정하기 쉽도록 최대한 단순하게 설계한다.
    2. 코드를 수정하지 않고도 변경을 수용할 수 있도록 코드를 더 유연하게 만든다.

- 대부분 전자의 방법이 좋지만, **유사한 변경이 반복적으로 발생한다면 복잡성이 상승하더라도 유연성을 추가하는 두 번째 방법**이 좋다.

- 영화에 설정된 **할인 정책을 실행 중 변경**해야 한다면 어떻게 해야 할까?
    - 현재 설계에선 할인 정책의 구현을 위해 상속을 이용하고 있다.
        - 고로 **새로운 인스턴스를 생성한 후 필요 정보를 복사**해야 한다.
    - 또한 **변경 전후의 인스턴스가 개념적으로 동일한 객체를 가리키나, 물리적으론 서로 다른 객체**다.
        - 식별자의 관점에서 혼란스러울 수 있다.

- 새 할인 정책이 나올 때마다 인스턴스를 생성하고, 상태를 복사하고, 식별자를 관리하는 코드를 추가하는 일은 번거롭고 오류가 발생하기 쉽다.
    - 이 때는 **코드 복잡성이 증가하더라도 변경을 쉽게 수용할 수 있도록 코드를 유연하게 만드는 것이 좋다.**

![image](https://github.com/user-attachments/assets/5aa05754-6aa2-4539-a21b-2b2fd731b965)

- 해결법은, **상속 대신 합성을 사용하는 것**이다.
    - `Movie` 의 상속 계층 안에 구현된 할인 정책을 **독립적인 `DiscountPolicy` 로 분리**한다.
    - 이후 `Movie` 에 합성시키면 **유연한 설계가 완성**된다.

```kotlin
val avatar = Movie(
    title = "아바타",
    runningTime = Duration.ofMinutes(120),
    fee = Money.wons(10000),
    discountPolicy = AmountDiscountPolicy(
        discountAmount = Money.wons(800),
        conditions = // . . .
    )
)
```

- 이제 금액 할인이 적용된 영화를 비율 할인 정책으로 바꾸는 일은, 단순히 **`DiscountPolicy` 의 인스턴스를 변경하는 단순한 작업으로 교체**된다.
    - 새 할인 정책이 추가되더라도, 할인 정책을 변경하는 데 필요한 추가적 코드를 작성할 필요도 없다.
    - 새 클래스를 추가하고, 그 인스턴스를 `Movie` 의 `changeDiscountPolicy` 에 전달하면 된다.

- 이 예는 유연성에 대한 압박이 설계에 어떤 영향을 미치는지 잘 알려준다.
    - 실제로 유연성은 의존성 관리의 문제로, **요소들 사이 의존성의 정도가 유연성의 정도를 결정**한다.
    - **유연성의 정도에 따라 결합도를 조절할 수 있는 능력은, 객체지향 개발자가 갖춰야 하는 기술**이다.

### 코드의 구조가 도메인의 구조에 대한 새로운 통찰력을 제공한다

![image](https://github.com/user-attachments/assets/eb02c396-70c4-4eff-8c9c-c504f821c9f0)

- **코드 구조가 변경되면 도메인에 대한 관점도 함께 바뀐다.**
    - 할인 정책을 자유롭게 변경할 수 있다는 것은, 도메인에 포함된 주요 요구사항이다.
    - 이 요구사항의 수용을 위해 할인 정책이란 개념을 코드 상에 명시적으로 드러냈다면, **도메인 모델 역시 코드 관점에 따라 바뀌어야 한다.**
- 따라서, 도메인 모델은 코드 구조에 따라 위 그림과 같이 수정된다.
    - 이 도메인 모델은 도메인에 포함된 개념, 관계뿐 아니라 도메인이 요구하는 유연성도 명확히 반영한다.

- **도메인 모델은 단순하게 도메인 개념, 관계를 모아 놓은 것이 아니다.**
    - **도메인 모델은 구현과 밀접한 관계**를 맺어야 한다.
        - 코드에 대한 가이드를 제공할 수 있어야 하며, **코드의 변화에 발맞춰 함께 변화**해야만 한다.
        - 도메인 모델은 코드와 분리된 막연한 무엇이 아니다.

- 책임을 할당하고 유연성을 기반으로 설계를 리팩터링하며 결국 2장의 코드와 동일한 구조에 도달했다.
    - 이제 협력이란 문맥 안에서 적절한 책임을 적절한 객체에 할당하는 방법에 대한 윤곽을 잡았다.
- 하지만 여전히 책임을 올바르게 할당하는 것은 어렵고 난해하다.
    - 사실 객체지향 프로그래밍 언어로 절차형 프로그램을 작성하는 대부분의 이유가 이 책임 할당의 어려움에서 기인한다.

- 이번 장을 읽었음에도 책임 할당에 대한 어려움을 느낀다면, 앞으로 소개할 방법을 사용해보자.
    - 일단 절차형 코드로 프로그램을 빠르게 작성한 후, 코드를 객체지향적인 코드로 변경하는 것이다.

---

## 04. 책임 주도 설계의 대안

- 책임, 객체 사이에서 방황할 때 선택할 수 있는 방법은 **최대한 빠르게 목적한 기능을 수행하는 코드를 작성**하는 것이다.
    - 일단 실행되는 코드를 얻고 난 후, **코드에 명확히 드러나는 책임들을 올바른 위치로 이동시키는 것**이다.

- 주의할 점은 **코드를 수정한 후 겉으로 드러난 동작이 바뀌어선 안된다.**
    - 캡슐화를 향상시키고, 응집도를 높이고, 결합도를 낮춰야 하지만 동작은 유지되어야 한다.
    - 이처럼 외부 동작은 바꾸지 않은 채 내부 구조를 개선하는 작업을 **리팩터링**이라 부른다.

> 객체 디자인의 가장 기본 중 하나는 책임을 어디에 둘지 결정하는 것이다.
나는 십년 이상 객체를 가지고 일했지만, 처음 시작할 땐 여전히 적당한 위치를 찾지 못한다.
하지만 이럴 때, **리팩터링**을 사용하면 된다는 것을 알게 되었다. [Fowler 1999a].
> 

### 메서드 응집도

- 데이터 중심으로 설계된 영화 예매 시스템의 경우, 도메인 객체들은 단지 데이터의 집합일 뿐이었다.
- 또한 영화 예매를 처리하는 모든 절차는 `ReservationAgency` 에 집중되어 있었다.
    - 따라서 **`ReservationAgency` 의 로직들을 적절한 객체의 책임들로 분배**하면, 책임 주도 설계와 거의 유사한 결과를 얻을 수 있다.

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

- `reserve` 메소드는 길이도 길고, 이해하기도 어렵다.
- 긴 메소드는 아래와 같은 이유로 코드 유지보수에 부정적 영향을 끼친다.
    1. 어떤 일을 수행하는지 한 눈에 파악하기 어렵기에, 코드를 전체적으로 이해하는 데 너무 오랜 시간이 걸린다.
    2. 한 메소드 안에서 너무 많은 작업을 처리하기에 변경이 필요할 때 수정부를 찾기 어렵다.
    3. 메소드 내 일부 로직만 수정해도 나머지 부분에서 버그가 발생할 확률이 높다.
    4. 로직 일부만 재사용하는 것이 불가능하다.
    5. 코드를 재사용할 수 있는 유일한 방법은, 원하는 코드를 복사해서 붙여넣는 것뿐이므로 중복을 초래하기 쉽다.
        
        ⇒ 마이클 페더스는 이러한 메소드를 **몬스터 메소드**라고 부른다.
        

- 일반적으로 응집도가 낮은 클래스는 로직 흐름을 이해하기 위해 주석이 필요한 경우가 많다.
    - 메소드가 명령문들의 그룹으로 구성되고, 그 그룹에 주석을 달아야 한다면 메소드 응집도가 낮은 것이다.
    - **주석을 추가하는 대신 메소드를 작게 분해하여 각 메소드의 응집도를 높여라.**

- 클래스와 마찬가지로 **메소드의 응집도를 높이는 것도 변경과 관련이 깊다.**
    - **응집도 높은 메소드 역시 변경의 이유가 단 하나**여야만 한다.
        - 클래스가 작고 목적이 명확한 메소드로 구성되어 있다면 변경의 처리를 위해 어떤 메소드를 수정해야 하는지 쉽게 판단할 수 있다.
    - 메소드의 크기가 작고 목적이 분명하기에 재사용하기도 쉽다.
    - 작은 메소드는 주석을 나열한 것처럼 보이기에 코드를 이해하기도 쉽다.

> 나는 다음 이유로 짧고 이해하기 쉬운 이름의 메소드를 좋아한다. 

첫째, 메소드가 잘게 나눠졌을 때 다른 메소드에서 사용될 확률이 높아진다.
둘째, 고수준의 메소드를 볼 때 일련의 주석을 읽는 것 같은 느낌이 든다.
셋째, 메소드가 잘게 나눠져 있을 때 오버라이딩도 훨씬 쉽다.

큰 메소드에 익숙해져 있다면 잘게 나누는 데 시간이 걸릴 것이다. 작은 메소드는 이름을 잘 지었을 때만 진가가 드러나므로, 이름을 짓는 것에 신경써야 한다.

사람들은 한 메소드의 길이가 어느정도 되어야 할지 묻는다. 그러나 길이가 중요한 것이 아니다. 
**중요한 것은 메소드 이름과 메소드 몸체의 의미적 차이다.**

메소드를 뽑아내는 것이 코드를 더욱 명확하게 하면, 
새 메소드의 이름이 원래 코드 길이보다 길어져도 뽑아낸다.
[Fowler99a]
> 

```kotlin
class ReservationAgency {

    fun reserve(screening: Screening, customer: Customer, audienceCount: Int): Reservation {
        val discountable = checkDiscountable(screening)
        val fee = calculateFee(screening, discountable, audienceCount)
        return createReservation(screening, customer, audienceCount, fee)
    }

    private fun checkDiscountable(screening: Screening): Boolean {
        return screening.movie.discountConditions.any { condition -> 
            isDiscountable(condition, screening) 
        }
    }

    private fun isDiscountable(condition: DiscountCondition, screening: Screening): Boolean {
        return if (condition.type == DiscountConditionType.PERIOD) {
            isSatisfiedByPeriod(condition, screening)
        } else {
            isSatisfiedBySequence(condition, screening)
        }
    }

    private fun isSatisfiedByPeriod(condition: DiscountCondition, screening: Screening): Boolean {
        return condition.dayOfWeek == screening.whenScreened.dayOfWeek &&
               condition.startTime <= screening.whenScreened.toLocalTime() &&
               condition.endTime >= screening.whenScreened.toLocalTime()
    }

    private fun isSatisfiedBySequence(condition: DiscountCondition, screening: Screening): Boolean {
        return condition.sequence == screening.sequence
    }

    private fun calculateFee(screening: Screening, discountable: Boolean, audienceCount: Int): Money {
        return if (discountable) {
            screening.movie.fee.minus(calculateDiscountedFee(screening.movie))
                        .times(audienceCount)
        } else {
            screening.movie.fee.times(audienceCount)
        }
    }

    private fun calculateDiscountedFee(movie: Movie): Money {
        return when (movie.movieType) {
            MovieType.AMOUNT_DISCOUNT -> calculateAmountDiscountedFee(movie)
            MovieType.PERCENT_DISCOUNT -> calculatePercentDiscountedFee(movie)
            MovieType.NONE_DISCOUNT -> calculateNoneDiscountedFee(movie)
            else -> throw IllegalArgumentException()
        }
    }

    private fun calculateAmountDiscountedFee(movie: Movie): Money {
        return movie.discountAmount
    }

    private fun calculatePercentDiscountedFee(movie: Movie): Money {
        return movie.fee.times(movie.discountPercent)
    }

    private fun calculateNoneDiscountedFee(movie: Movie): Money {
        return Money.ZERO
    }

    private fun createReservation(screening: Screening, customer: Customer, audienceCount: Int, fee: Money): Reservation {
        return Reservation(customer, screening, fee, audienceCount)
    }
}
```

- 객체를 책임으로 분배할 때 가장 먼저 할 일은, 메소드를 응집도 있는 수준으로 분해하는 것이다.
    - **긴 메소드를 작고 응집도 높은 메소드로 분리하면, 각 메소드를 적합한 클래스로 이동하기 더 수월**해진다.

- `ReservationAgency` 클래스는 이제 **응집도가 높은 메소드**들로 구성되어 있다.
    - 클래스 길이는 길어졌지만, 일반적으로 **명확성의 가치가 클래스 길이보다 더 중요**하다.

```kotlin
fun reserve(screening: Screening, customer: Customer, audienceCount: Int): Reservation {
    val discountable = checkDiscountable(screening)
    val fee = calculateFee(screening, discountable, audienceCount)
    return createReservation(screening, customer, audienceCount, fee)
}
```

- 메소드를 분리하고 나면, `public` 메소드는 상위 수준의 명세를 읽는 것 같은 느낌이 든다.
    - 수정 후엔 메소드가 어떤 일을 하는지 한 눈에 알아볼 수 있게 되었다.
    - 주석을 모아놓은 것처럼 보이기도 한다.

- **작고 명확하며 한 가지 일에 집중하는 응집도 높은 메소드는, 변경 가능한 설계를 이끌어 내는 기반**이 된다.
    - 결과적으로 **이런 메소드들이 모여 응집도 높은 클래스**가 만들어진다.

- 하지만 아직 **`ReservationAgency` 의 응집도는 여전히 낮다.**
    - 클래스의 응집도를 높이기 위해 변경의 이유가 다른 메소드들을 적절한 위치로 분배해야 한다.
    - 적절한 위치란 바로 각 메소드가 사용하는 데이터를 정의하고 있는 클래스를 의미한다.

### 객체를 자율적으로 만들자

- 어떤 메소드를 어떤 클래스로 이동시켜야 할까?
    - **객체는 자율적 존재**란 사실을 잊지 말자.
    - **자신이 소유하고 있는 데이터를 자기 스스로 처리하도록 만드는 것이 자율적 객체를 만드는 지름길**이다.

- 어떤 데이터를 사용하는지 가장 쉽게 아는 방법은, **메소드 내에서 어떤 클래스의 접근자 메소드를 사용하는지 파악하는 것**이다.

```kotlin
class ReservationAgency {
    private fun isDiscountable(condition: DiscountCondition, screening: Screening): Boolean {
        return if (condition.type == DiscountConditionType.PERIOD) {
            isSatisfiedByPeriod(condition, screening)
        } else {
            isSatisfiedBySequence(condition, screening)
        }
    }

    private fun isSatisfiedByPeriod(condition: DiscountCondition, screening: Screening): Boolean {
        return condition.dayOfWeek == screening.whenScreened.dayOfWeek &&
               condition.startTime <= screening.whenScreened.toLocalTime() &&
               condition.endTime >= screening.whenScreened.toLocalTime()
    }

    private fun isSatisfiedBySequence(condition: DiscountCondition, screening: Screening): Boolean {
        return condition.sequence == screening.sequence
    }
}
```

- `isDiscountable` 메소드는 **`DiscountCondition` 의 `type` 으로 할인 조건**을 알아낸다.
- `isSatisfiedByPeriod`, `isSatisfiedBySequence` 의 내부 구현 역시 **`DiscountCondition` 을 통해 할인 여부를 판단**한다.
    
    **⇒ 두 메소드를, 데이터가 존재하는 `DiscountCondition` 으로 이동시킨다.**
    

```kotlin
class DiscountCondition(
    private val type: DiscountConditionType,
    private val sequence: Int,
    private val dayOfWeek: DayOfWeek,
    private val startTime: LocalTime,
    private val endTime: LocalTime
) {

    fun isDiscountable(screening: Screening): Boolean {
        return if (type == DiscountConditionType.PERIOD) {
            isSatisfiedByPeriod(screening)
        } else {
            isSatisfiedBySequence(screening)
        }
    }

    private fun isSatisfiedByPeriod(screening: Screening): Boolean {
        val screeningTime = screening.whenScreened.toLocalTime()
        return screening.whenScreened.dayOfWeek == dayOfWeek &&
               startTime <= screeningTime &&
               endTime >= screeningTime
    }

    private fun isSatisfiedBySequence(screening: Screening): Boolean {
        return sequence == screening.sequence
    }
}
```

- `isDiscountable` 메소드는 외부에서 호출 가능해야 하므로 **`public` 으로 가시성을 변경**한다.
    - `isDiscountable` 메소드가 `ReservationAgency` 에 속할 땐 **구현의 일부**였다.
    - 하지만, 이동한 후엔 **퍼블릭 인터페이스의 일부**가 된 것이다.

- `isDiscountable` 는 기존에 `DiscountCondition` 를 인자로 받았다.
    - 하지만, 이제 `DiscountCondition` 의일부가 되었기에 인자로 전달받을 필요가 없어졌다.
    - 이처럼 **메소드를 다른 클래스로 이동시킬 땐, 인자에 정의된 클래스 중 하나로 이동하는 경우가 일반적**이다.

- 이제 `DiscountCondition` 내부에서만 `DiscountCondition` 의 인스턴스 변수에 접근한다.
    - 따라서, `DiscountCondition` 에서 모든 접근자 메소드를 제거할 수 있다.
    - 결과적으로 `DiscountCondition` 의 내부 구현을 **캡슐화**할 수 있다.
- 또한 할인 조건을 계산하는 데 필요한 모든 로직이 `DiscountCondition` 에 모여있기 때문에, **응집도도 높아졌다.**

- `ReservationAgency` 는 내부 구현을 노출하는 접근자 메소드를 사용하지 않는다.
    - 오로지 **메시지를 통해서만 `DiscountCondition` 와 협력**한다.

- 이처럼 **데이터를 사용하는 메소드를 데이터를 가진 클래스로 이동시키면, 캡슐화, 높은 응집도, 낮은 결합도를 가지는 설계**를 얻게 된다.

```kotlin
class ReservationAgency {
    private fun checkDiscountable(screening: Screening): Boolean {
        return screening.movie.discountConditions.any { condition -> 
            condition.isDiscountable(screening) 
        }
    }
}
```

- 이전과 달리 이제는, 할인 여부를 판단하기 위해 `DiscountCondition` 의 **`isDiscountable` 메소드를 호출**한다.

- 변경 후 코드는 책임 주도 설계를 적용하여 구현했던 `DiscountCondition` 클래스의 초기 모습과 유사해졌다.
    - 여기에 다형성 패턴, 변경 보호 패턴을 차례대로 적용하면, 이전의 최종 설계와 유사한 모습의 코드를 얻게될 것이다.

- 처음부터 책임 주도 설계를 따르는 것보다, **우선 동작하는 코드를 작성한 후 리팩터링 하는 것이 더 훌륭한 결과물**을 얻을 수 있다.
    - 캡슐화, 결합도, 응집도를 이해하고 훌륭한 객체지향 원칙의 적용을 위해 노력한다면, 책임 주도 설계 방법을 단계적으로 따르지 않더라도 유연하고 깔끔한 코드를 얻을 수 있다.
