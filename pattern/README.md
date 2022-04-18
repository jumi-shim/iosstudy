# 📙 Pattern

__🧐 Design Pattern VS Architectural Pattern?__

- __Design Pattern__

  소프트웨어 구성에서 발생하는 문제 해결.

  코드 레벨에서의 공통점. 프로그램의 언어 영향 받음.

- __Architectural Pattern__

  소프트웨어의 기본 구조. 메커니즘.

  Design Pattern에서 보다 더 높은 레벨에서 공통점.

- __re-use, classification, abstraction to distill the commonality__

# 📍Design Pattern

## 🖌 Observer Pattern

관찰 중인 객체에서 발생하는 이벤트를 여러 다른 객체에 알리는 메커니즘을 정의할 수 있는 패턴.

다른 객체의 상태가 변경될 때마다 어떤 행동을 하고 싶을 때 사용.

- __Subject (Publisher)__

  - Observer들을 가지고 있으며 개수 제한 없음. Observer 추가, 제거 인터페이스 제공.

- __Concrete Subject (Publisher)__

  - Concrete Observer 객체의 상태 저장. 상태 변경되면 Observer (Subscriber)에게 알림.

- __Observer (Subscriber)__

  - 객체의 변경 사항을 알려야 하는 객체에 대한 Update 인터페이스 제공. 

- __Concrete Observer (Subscriber)__

  - Concrete Subject (Publisher) 객체에 대한 참조 유지.

  - Subject (Publisher)의 상태와 일관성 유지.

  - 객체의 상태와 일관성을 유지하기 위해 update 인터페이스 구현.

- 장점

  - Open / Close 원칙을 지킬 수 있음. Subject(Publisher)의 코드를 수정하지 않고 새로운 Observer(Subscriber) 클래수 추가 가능.

  - 런타임에서 각체간 관계 설정 가능.

- 단점

  - Observer(Subscriber)에게 알림이 가는 순서 보장하지 않음.

  - Observer, Subject의 관계가 잘 정의되지 않으면 원하지 않는 동작이 발생할 수도 있음.

[참고](https://icksw.tistory.com/257)



## 🖌 Repository Pattern

비즈니스 로직과 데이터 레이어를 분리하고 중앙 집중 처리 방식을 통해 일관된 데이터와 로직을 제공해야 한다는 것. 데이터가 있는 여러 저장소(Local Data Source, Remote Data Source)를 추상화.

Repository는 데이터 소스 레이어와 비즈니스 레이어 사이를 중재함. Repository를 통해 비즈니스 레이어는 데이터의 출처를 몰라도 됨.

Repository는 데이터 소스에 쿼리를 날리거나 데이터를 다른 도메인에서 사용할 수 있도록 맵핑.

![img](https://blog.kakaocdn.net/dn/c1hNsQ/btqDfCHHvR5/KC5OAAZEkzoHK3K6LblEn1/img.jpg)

ViewModel이 여러 Repository를 공유하더라도 Interface를 통해 데이터의 일관성을 유지할 수 있음.

데이터 저장소의 데이터를 캡슐화할 수 있음 -> 객체지향적 프로그래밍에 적합 ??

단위 테스트를 통한 검증이 가능하고 객체 간의 결합도가 감소함.

[참고1](https://medium.com/tiendeo-tech/ios-repository-pattern-in-swift-85a8c62bf436)

[참고2](https://devming.medium.com/repository-pattern%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-255731577927)

[참고3](https://vagabond95.me/posts/android-repository-pattern/)

# 📍Architectural Pattern

## 🖌 MVC

Model - View - Controller

- **Model**

  데이터. 데이터 관리 로직.

  - 데이터로 사용하는 구조체
  - 네트워크 로직. 네트워크 통해 받아온 DTO 구조체. 네트워크에 요청, 결과 받아오는 로직.
  - 데이터 파싱 로직
  - Persistance 로직. 메모리에 저장되는 데이터 로드 및 세이브 로직.
  - Manager 객체(shared 객체). 구조체 만들어두고 필요한 경우 접근해서 사용하는 경우.
  - Util, Extension, Constant. Color 정의. String 추가 기능. 특정 사이즈 정보.

- **View**

  UI. 유저들에게 데이터 보여주고 어떻게 보여줄지 화면 구성하는 부분.

  직접 유저와 상호작용 하고 이것을 Controller 계층으로 전달하는 역할.

  기본적으로 View는 Controller로부터 정리된 데이터를 받아 화면에 보여주는 역할을 하지만 간단한 정보는 View에서도 정의 가능.

  재사용성 중요. ex 앱 전반에서 사용되는 버튼 .. 등 

  - UIView를 상속해 만들어진 subclass
  - Core Animation
  - Core Graphics

- **Controller**

  Model과 View의 중간 역할. 데이터를 Model에서 가져와 View에 데이터를 보여주기 위해 Controller가 보내주면서 View를 refresh. View로부터 유저 상호작용에 대한 정보 받고 해당 로직 실행. Model 정보 업데이트. 

- 특징

  Apple's MVC 에서 Controller가 View의 Life Cycle까지 관리하기 때문에 View와 Controller를 분리하기 어려움. -> 재사용성 떨어짐. 유닛 테스트 진행 어려움. Controller에 코드 밀집

  ![img](https://media.vlpt.us/images/zooneon/post/8c95699b-2cbe-4159-b105-ba4ea3d652bf/image.png)

[참고](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)

## 🖌 MVVM

Model - View - ViewModel  

![img](https://miro.medium.com/max/1400/1*fQ0LXptWrJIbGS9LZE8p6g.png)

- **Model**

  데이터 담는 구조체. 네트워크 로직. JSON 파싱 코드 등

- **View**

  UI에 대한 코드. View의 각 컴포넌트에 대한 정보(재사용성 강조). 어느 위치에 어떻게 배치 될 지 등.

  ViewModel로부터 데이터를 가져와 어떻게 배치할지. 상황에 따라 ViewModel의 어떤 메서드를 이용할지. 

- **ViewModel**

  앱의 핵심적인 비즈니스 로직. MVC 패턴의 Controller와 비슷한 역할. View로부터 전달받는 요청을 해결할 비즈니스 로직. UI 관련 코드로부터 완전히 분리. 
  
  **View는 ViewModel을 바라만 보고 있음! ViewModel은 View에 대해 모름!!!**

1. 사용자가 화면에서 Action을 취하면 **Command Pattern**으로 View -> ViewModel로 전달됨.
2. ViewModel이 Model에게 data 요청.
3. Model은 요청받은 data를 통해 update된 data를 ViewModel로 전달함.
4. ViewModel은 응답받은 데이터를 가공해서 저장.
5. View는 ViewModel과의 **Data Binding**을 통해서 자동으로 갱신.

- **Command Pattern?**

  실행될 기능을 추상화, 캡슐화하여 한 클래스에서 여러 기능을 실행할 수 있도록 하는 패턴.

- **Data Binding?**

  view와 로직이 분리되어 있어도 한 쪽이 바뀌면 다른 쪽도 업데이트가 이루어 지는 것.

  - KVO
  - Delegation
  - Functional Reactive Programming
  - Property Observer

- 특징

  유지보수에 좋음. 자동화된 테스팅에 적합한 모델 (View Model과 View 간의 의존성이 없기 때문)

[참고](https://medium.com/hcleedev/ios-swiftui의-mvvm-패턴과-mvc와의-비교-8662c96353cc)



## 🖌 Clean Architecture

![img](https://blog.kakaocdn.net/dn/rgFxI/btqFK2Yz8oi/dprufN5ctDe1WQgQn3BgE0/img.png)



- 가장 바깥쪽 원은 저수준의 구체적 상세 정보를 담고 안쪽으로 가면서 소프트웨어는 추상화 되고 고수준의 정책을 캡슐화 함. 
- **안쪽의 원은 바깥쪽의 원에 대해 전혀 알지 못함.** 특히 바깥쪽의 원에서 선언된 어떠한 이름을 안쪽 원에서 참조해서는 안 됨. -> 소스 코드는 안쪽을 향해서만 의존. 바깥쪽 원의 어떠한 것도 안쪽에 영향 주면 안 됨.

1. **Entities** Enterprise Business Rules

   비즈니스 모델(Actor가 필요로 하는 데이터 모델). 엔티티는 메서드를 갖는 객체일 수도 있고 데이터 구조와 함수의 집합일 수도 있음.

   특정 도메인에서 사용되는 struct 모델.

2. **Use cases** Application Business Rules

   Actor가 Entity를 원하는데, 이 값은 계산되거나 특정 로직에 의해 얻어지므로 Actor가 원하는 Entity를 얻어내고 있는 **로직** 의미.

   애플리케이션 고유 비즈니스 규칙을 포함하며 시스템의 모든 유스케이스를 캡슐화하고 구현함.

   엔티티로 부터의 혹은 엔티티에서의 데이터 흐름을 조합.

   이 계층의 변경이 엔티티에 영향을 주지 않을 것을 기대하며 데이터베이스, UI 또는 공통의 프레임워크의 변경으로부터 영향받지 않을 것도 기대.

3. **Controllers, Gateway, Prestenters** Interface Adapter

   유스케이스와 엔티티에 있어 용이한 형식으로부터 데이터베이스나 웹 등 외부의 기능에 용이한 형식으로 데이터를 변환함.

   Coordinator, ViewModel, ViewController, View, Behaviors(특정 View의 event에 관해 적용되는 UI).

4. **Devices, Web, UI DB External Interfaces** Frameworks & Drivers

   데이터베이스나 웹 프레임워크 등 일반적으로 프레임워크나 도구로 구성됨. 

   안쪽의 원과 통신할 연결 코드 이외에는 작성 안 함.

   Network, CoreData



## 🖌 Clean Architecture + MVVM

![img](https://blog.kakaocdn.net/dn/yoKKQ/btqFKl5Y86D/3ZD7kkO3Nkw7FIZK9g8cDk/img.png)

Domain > Interfaces > Repositories 에 있는 파일들은 모두 프로토콜.

**Data > Repositories 에 있는 파일들은 Domain > Interfaces > Repositories의 프로토콜들을 채택한 클래스들.**

![img](https://blog.kakaocdn.net/dn/bHEH6y/btqFKMhY1wf/Ted9svEN3OwQzwt1gij7ek/img.jpg)

Repository는 Domain Layer와 Data Layer 중간쯤에 있음.

![img](https://blog.kakaocdn.net/dn/bOX0P0/btqFK2q3TWe/Xi8TCdfPHkWd9XiZlZuDR0/img.png)



잘 변하는 것이 잘 변하지 않는 곳에 의존하는 것이 이상적.

잘 변하지 않는 Domain 계층으로 Presentation Layer와 Data Layer가 의존.



1. **Domain**

   Entities + Use Cases. 

   다른 Layer들에게 어떠한 영향도 받지 않음. 다른 프로젝트에 의하여 재사용 될 수 있음.

   Entities(비즈니스 모델), Use Cases, **Repository Interfaces**.

2. **Presentation Layer**

   Presenters + UI.

   UI(UIViewControllers, SwiftUI Views), ViewModels(Presenters).

   ViewModel은 하나 이상의 UseCases를 execute하기 때문에 Presentation Layer는 Domain Layer를 의존함.

3. **Data Layer**

   DB + API

   Repository 프로토콜에 대한 구현(**Repository Implementations**)과 Data Sources.

   Repository는 다른 Data Sources (로컬 DB, API)로부터 데이터를 처리하는 책임 있음.

   Data Layer는 API 응답으로 받은 JSON Data를 Domain Layer에 있는 모델로 변환하는 작업이 있음. -> Data Layer는 Domain Layer 의존.



![img](https://blog.kakaocdn.net/dn/MQ1R1/btqFKDrPxtp/pjv2GVcCJ7ubcfjq8ZxQkK/img.png)

##### Data Flow

1. View(UI)는 ViewModel(Presenter)의 메소드를 콜.
2. ViewModel은 UseCase를 실행함.
3. UseCase는 User와 Repository로부터 데이터 조합.
4. 각각의 Repository는 Remote Data(Network) 또는 Persistent DB storage Source 또는 In-memory Data (Remote or Cached)로부터 데이터를 가져옴.
5. 정보는 다시 View(UI)로 흘러 (Information flows back to the View(UI)) 새로운 화면을 보게 됨.

[참고1](https://eunjin3786.tistory.com/207) [참고2](https://ios-development.tistory.com/667?category=989887)



#### DTO

Data Transfer Object : JSON의 response를 Entity로 변환

API로 부터 받은 response는 그대로 받고, response를 completion handler에서 domain 모델로 변환하는 식으로 사용.

받은 response는 그대로 표출 > API의 response 모델을 알아보기 용이

사용하는 쪽에서 domain 모델을 사용하도록 반환

[참고](https://ios-development.tistory.com/668?category=989887)

- DTO를 만드는 이유?

  DTO를 두면 Model, Entity를 종속적이지 않게 만들 수 있음.

  Data Layer를 통해 받는 데이터 응답이 달라질 때마다 DTO만 수정하면 되고 model은 수정하지 않아도 됨.



#### 🤔 ViewModel VS UseCase

ViewModel에 비즈니스 로직을 넣는 경우는 잘못된 접근 :: 비즈니스 로직은 UseCase에 존재.

ViewModel의 역할은 UI Event들이 발생하면 무엇을 해야하는지 알고 있는 것. 무엇을 해야하는지 알고있기 때문에 UseCase를 실행시키고 UI에 업데이트를 알리는 역할.

비즈니스 로직은 앱에서 사용자와 상호작용이 아닌 업무 요구사항을 담고 있는 것. :: 개발팀 외부의 사업 부서 사람도 알고 있어야 하는 로직.

