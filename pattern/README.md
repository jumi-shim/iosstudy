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

