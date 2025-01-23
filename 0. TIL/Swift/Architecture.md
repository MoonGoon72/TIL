## 클린 아키텍쳐

![](https://i.imgur.com/u90xCKm.png)

> [!NOTE]
> ### 특징
>
> 1. Independent of Frameworks : 아키텍쳐는 소프트웨어 라이브러리의 존재에 의존하지 않는다.
> 2. Testable : 비즈니스 로직은 UI, Database 등 없이 독립적으로 테스트 가능하다.
> 3. Independent UI : UI는 system과 비즈니스 로직에 관계없이 쉽게 변경 가능해야 한다.
> 4. Independent of Database : 어떤 DB를 사용하더라도 (변경하더라도) 비즈니스 로직은 DB에 바인딩 되지 않으니 동일하게 동작한다.
> 5. Independent of any external agency : 비즈니스 로직은 외부 세계에 대해 알지 못한다.

#### The Dependency Rule

> 내부 레이어는 외부의 레이어에 의존성을 띄면 안된다.

### Layers

#### Entity

> [!NOTE]
> ### 역할
> 비즈니스 도메인의 핵심 규칙을 표현하는 객체

- 흔히 말하는 Model 데이터구조에 해당한다.
- 모든 레이어가 Entity레이어에 의존적이기 때문에 변화가 가장 적어야한다.
- Entity는 외부 Layer를 몰라야한다.

#### UseCase

> [!NOTE]
> ### 역할
> 애플리케이션의 특정 기능을 수행하는 비즈니스 로직을 구현
> 엔티티의 상태를 변경하거나 정보를 제공하는 역할

> [!NOTE]
> ### Repository
> - 데이터 접근의 추상화된 인터페이스(프로토콜) 제공
> - DIP를 적용
> - UseCase는 Entity의 정보를 어떻게 가져오는지 알 필요 없이 Repository를 통해서 데이터를 가져올 수 있게된다. (데이터를 가져오는 방식을 숨길 수 있다.)

#### Presenter

> [!NOTE]
> ### 역할
> 엔티티 데이터를 그대로 **표현**하는데 필요한 계층
>
> - Coordinator
> - View
> - ViewModel
> - Behaviors: 특정 View의 event에 관해 적용되는 UI

#### External Interfaces

> [!NOTE]
> ### 역할
> 안쪽 원과 통신할 연결 코드 이외에는 별다른 코드를 작성하지 않음
>
> - Network
> - Database
