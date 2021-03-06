# π Pattern

__π§ Design Pattern VS Architectural Pattern?__

- __Design Pattern__

  μννΈμ¨μ΄ κ΅¬μ±μμ λ°μνλ λ¬Έμ  ν΄κ²°.

  μ½λ λ λ²¨μμμ κ³΅ν΅μ . νλ‘κ·Έλ¨μ μΈμ΄ μν₯ λ°μ.

- __Architectural Pattern__

  μννΈμ¨μ΄μ κΈ°λ³Έ κ΅¬μ‘°. λ©μ»€λμ¦.

  Design Patternμμ λ³΄λ€ λ λμ λ λ²¨μμ κ³΅ν΅μ .

- __re-use, classification, abstraction to distill the commonality__

# πDesign Pattern

## π Observer Pattern

κ΄μ°° μ€μΈ κ°μ²΄μμ λ°μνλ μ΄λ²€νΈλ₯Ό μ¬λ¬ λ€λ₯Έ κ°μ²΄μ μλ¦¬λ λ©μ»€λμ¦μ μ μν  μ μλ ν¨ν΄.

λ€λ₯Έ κ°μ²΄μ μνκ° λ³κ²½λ  λλ§λ€ μ΄λ€ νλμ νκ³  μΆμ λ μ¬μ©.

- __Subject (Publisher)__

  - Observerλ€μ κ°μ§κ³  μμΌλ©° κ°μ μ ν μμ. Observer μΆκ°, μ κ±° μΈν°νμ΄μ€ μ κ³΅.

- __Concrete Subject (Publisher)__

  - Concrete Observer κ°μ²΄μ μν μ μ₯. μν λ³κ²½λλ©΄ Observer (Subscriber)μκ² μλ¦Ό.

- __Observer (Subscriber)__

  - κ°μ²΄μ λ³κ²½ μ¬ν­μ μλ €μΌ νλ κ°μ²΄μ λν Update μΈν°νμ΄μ€ μ κ³΅. 

- __Concrete Observer (Subscriber)__

  - Concrete Subject (Publisher) κ°μ²΄μ λν μ°Έμ‘° μ μ§.

  - Subject (Publisher)μ μνμ μΌκ΄μ± μ μ§.

  - κ°μ²΄μ μνμ μΌκ΄μ±μ μ μ§νκΈ° μν΄ update μΈν°νμ΄μ€ κ΅¬ν.

- μ₯μ 

  - Open / Close μμΉμ μ§ν¬ μ μμ. Subject(Publisher)μ μ½λλ₯Ό μμ νμ§ μκ³  μλ‘μ΄ Observer(Subscriber) ν΄λμ μΆκ° κ°λ₯.

  - λ°νμμμ κ°μ²΄κ° κ΄κ³ μ€μ  κ°λ₯.

- λ¨μ 

  - Observer(Subscriber)μκ² μλ¦Όμ΄ κ°λ μμ λ³΄μ₯νμ§ μμ.

  - Observer, Subjectμ κ΄κ³κ° μ μ μλμ§ μμΌλ©΄ μνμ§ μλ λμμ΄ λ°μν  μλ μμ.

[μ°Έκ³ ](https://icksw.tistory.com/257)



## π Repository Pattern

λΉμ¦λμ€ λ‘μ§κ³Ό λ°μ΄ν° λ μ΄μ΄λ₯Ό λΆλ¦¬νκ³  μ€μ μ§μ€ μ²λ¦¬ λ°©μμ ν΅ν΄ μΌκ΄λ λ°μ΄ν°μ λ‘μ§μ μ κ³΅ν΄μΌ νλ€λ κ². λ°μ΄ν°κ° μλ μ¬λ¬ μ μ₯μ(Local Data Source, Remote Data Source)λ₯Ό μΆμν.

Repositoryλ λ°μ΄ν° μμ€ λ μ΄μ΄μ λΉμ¦λμ€ λ μ΄μ΄ μ¬μ΄λ₯Ό μ€μ¬ν¨. Repositoryλ₯Ό ν΅ν΄ λΉμ¦λμ€ λ μ΄μ΄λ λ°μ΄ν°μ μΆμ²λ₯Ό λͺ°λΌλ λ¨.

Repositoryλ λ°μ΄ν° μμ€μ μΏΌλ¦¬λ₯Ό λ λ¦¬κ±°λ λ°μ΄ν°λ₯Ό λ€λ₯Έ λλ©μΈμμ μ¬μ©ν  μ μλλ‘ λ§΅ν.

![img](https://blog.kakaocdn.net/dn/c1hNsQ/btqDfCHHvR5/KC5OAAZEkzoHK3K6LblEn1/img.jpg)

ViewModelμ΄ μ¬λ¬ Repositoryλ₯Ό κ³΅μ νλλΌλ Interfaceλ₯Ό ν΅ν΄ λ°μ΄ν°μ μΌκ΄μ±μ μ μ§ν  μ μμ.

λ°μ΄ν° μ μ₯μμ λ°μ΄ν°λ₯Ό μΊ‘μνν  μ μμ -> κ°μ²΄μ§ν₯μ  νλ‘κ·Έλλ°μ μ ν© ??

λ¨μ νμ€νΈλ₯Ό ν΅ν κ²μ¦μ΄ κ°λ₯νκ³  κ°μ²΄ κ°μ κ²°ν©λκ° κ°μν¨.

[μ°Έκ³ 1](https://medium.com/tiendeo-tech/ios-repository-pattern-in-swift-85a8c62bf436)

[μ°Έκ³ 2](https://devming.medium.com/repository-pattern%EC%97%90-%EB%8C%80%ED%95%B4%EC%84%9C-255731577927)

[μ°Έκ³ 3](https://vagabond95.me/posts/android-repository-pattern/)

# πArchitectural Pattern

## π MVC

Model - View - Controller

- **Model**

  λ°μ΄ν°. λ°μ΄ν° κ΄λ¦¬ λ‘μ§.

  - λ°μ΄ν°λ‘ μ¬μ©νλ κ΅¬μ‘°μ²΄
  - λ€νΈμν¬ λ‘μ§. λ€νΈμν¬ ν΅ν΄ λ°μμ¨ DTO κ΅¬μ‘°μ²΄. λ€νΈμν¬μ μμ²­, κ²°κ³Ό λ°μμ€λ λ‘μ§.
  - λ°μ΄ν° νμ± λ‘μ§
  - Persistance λ‘μ§. λ©λͺ¨λ¦¬μ μ μ₯λλ λ°μ΄ν° λ‘λ λ° μΈμ΄λΈ λ‘μ§.
  - Manager κ°μ²΄(shared κ°μ²΄). κ΅¬μ‘°μ²΄ λ§λ€μ΄λκ³  νμν κ²½μ° μ κ·Όν΄μ μ¬μ©νλ κ²½μ°.
  - Util, Extension, Constant. Color μ μ. String μΆκ° κΈ°λ₯. νΉμ  μ¬μ΄μ¦ μ λ³΄.

- **View**

  UI. μ μ λ€μκ² λ°μ΄ν° λ³΄μ¬μ£Όκ³  μ΄λ»κ² λ³΄μ¬μ€μ§ νλ©΄ κ΅¬μ±νλ λΆλΆ.

  μ§μ  μ μ μ μνΈμμ© νκ³  μ΄κ²μ Controller κ³μΈ΅μΌλ‘ μ λ¬νλ μ­ν .

  κΈ°λ³Έμ μΌλ‘ Viewλ Controllerλ‘λΆν° μ λ¦¬λ λ°μ΄ν°λ₯Ό λ°μ νλ©΄μ λ³΄μ¬μ£Όλ μ­ν μ νμ§λ§ κ°λ¨ν μ λ³΄λ Viewμμλ μ μ κ°λ₯.

  μ¬μ¬μ©μ± μ€μ. ex μ± μ λ°μμ μ¬μ©λλ λ²νΌ .. λ± 

  - UIViewλ₯Ό μμν΄ λ§λ€μ΄μ§ subclass
  - Core Animation
  - Core Graphics

- **Controller**

  Modelκ³Ό Viewμ μ€κ° μ­ν . λ°μ΄ν°λ₯Ό Modelμμ κ°μ Έμ Viewμ λ°μ΄ν°λ₯Ό λ³΄μ¬μ£ΌκΈ° μν΄ Controllerκ° λ³΄λ΄μ£Όλ©΄μ Viewλ₯Ό refresh. Viewλ‘λΆν° μ μ  μνΈμμ©μ λν μ λ³΄ λ°κ³  ν΄λΉ λ‘μ§ μ€ν. Model μ λ³΄ μλ°μ΄νΈ. 

- νΉμ§

  Apple's MVC μμ Controllerκ° Viewμ Life CycleκΉμ§ κ΄λ¦¬νκΈ° λλ¬Έμ Viewμ Controllerλ₯Ό λΆλ¦¬νκΈ° μ΄λ €μ. -> μ¬μ¬μ©μ± λ¨μ΄μ§. μ λ νμ€νΈ μ§ν μ΄λ €μ. Controllerμ μ½λ λ°μ§

  ![img](https://media.vlpt.us/images/zooneon/post/8c95699b-2cbe-4159-b105-ba4ea3d652bf/image.png)

[μ°Έκ³ ](https://developer.apple.com/library/archive/documentation/General/Conceptual/CocoaEncyclopedia/Model-View-Controller/Model-View-Controller.html)

## π MVVM

Model - View - ViewModel  

![img](https://miro.medium.com/max/1400/1*fQ0LXptWrJIbGS9LZE8p6g.png)

- **Model**

  λ°μ΄ν° λ΄λ κ΅¬μ‘°μ²΄. λ€νΈμν¬ λ‘μ§. JSON νμ± μ½λ λ±

- **View**

  UIμ λν μ½λ. Viewμ κ° μ»΄ν¬λνΈμ λν μ λ³΄(μ¬μ¬μ©μ± κ°μ‘°). μ΄λ μμΉμ μ΄λ»κ² λ°°μΉ λ  μ§ λ±.

  ViewModelλ‘λΆν° λ°μ΄ν°λ₯Ό κ°μ Έμ μ΄λ»κ² λ°°μΉν μ§. μν©μ λ°λΌ ViewModelμ μ΄λ€ λ©μλλ₯Ό μ΄μ©ν μ§. 

- **ViewModel**

  μ±μ ν΅μ¬μ μΈ λΉμ¦λμ€ λ‘μ§. MVC ν¨ν΄μ Controllerμ λΉμ·ν μ­ν . Viewλ‘λΆν° μ λ¬λ°λ μμ²­μ ν΄κ²°ν  λΉμ¦λμ€ λ‘μ§. UI κ΄λ ¨ μ½λλ‘λΆν° μμ ν λΆλ¦¬. 
  
  **Viewλ ViewModelμ λ°λΌλ§ λ³΄κ³  μμ! ViewModelμ Viewμ λν΄ λͺ¨λ¦!!!**

1. μ¬μ©μκ° νλ©΄μμ Actionμ μ·¨νλ©΄ **Command Pattern**μΌλ‘ View -> ViewModelλ‘ μ λ¬λ¨.
2. ViewModelμ΄ Modelμκ² data μμ²­.
3. Modelμ μμ²­λ°μ dataλ₯Ό ν΅ν΄ updateλ dataλ₯Ό ViewModelλ‘ μ λ¬ν¨.
4. ViewModelμ μλ΅λ°μ λ°μ΄ν°λ₯Ό κ°κ³΅ν΄μ μ μ₯.
5. Viewλ ViewModelκ³Όμ **Data Binding**μ ν΅ν΄μ μλμΌλ‘ κ°±μ .

- **Command Pattern?**

  μ€νλ  κΈ°λ₯μ μΆμν, μΊ‘μννμ¬ ν ν΄λμ€μμ μ¬λ¬ κΈ°λ₯μ μ€νν  μ μλλ‘ νλ ν¨ν΄.

- **Data Binding?**

  viewμ λ‘μ§μ΄ λΆλ¦¬λμ΄ μμ΄λ ν μͺ½μ΄ λ°λλ©΄ λ€λ₯Έ μͺ½λ μλ°μ΄νΈκ° μ΄λ£¨μ΄ μ§λ κ².

  - KVO
  - Delegation
  - Functional Reactive Programming
  - Property Observer

- νΉμ§

  μ μ§λ³΄μμ μ’μ. μλνλ νμ€νμ μ ν©ν λͺ¨λΈ (View Modelκ³Ό View κ°μ μμ‘΄μ±μ΄ μκΈ° λλ¬Έ)

[μ°Έκ³ ](https://medium.com/hcleedev/ios-swiftuiμ-mvvm-ν¨ν΄κ³Ό-mvcμμ-λΉκ΅-8662c96353cc)



## π Clean Architecture

![img](https://blog.kakaocdn.net/dn/rgFxI/btqFK2Yz8oi/dprufN5ctDe1WQgQn3BgE0/img.png)



- κ°μ₯ λ°κΉ₯μͺ½ μμ μ μμ€μ κ΅¬μ²΄μ  μμΈ μ λ³΄λ₯Ό λ΄κ³  μμͺ½μΌλ‘ κ°λ©΄μ μννΈμ¨μ΄λ μΆμν λκ³  κ³ μμ€μ μ μ±μ μΊ‘μν ν¨. 
- **μμͺ½μ μμ λ°κΉ₯μͺ½μ μμ λν΄ μ ν μμ§ λͺ»ν¨.** νΉν λ°κΉ₯μͺ½μ μμμ μ μΈλ μ΄λ ν μ΄λ¦μ μμͺ½ μμμ μ°Έμ‘°ν΄μλ μ λ¨. -> μμ€ μ½λλ μμͺ½μ ν₯ν΄μλ§ μμ‘΄. λ°κΉ₯μͺ½ μμ μ΄λ ν κ²λ μμͺ½μ μν₯ μ£Όλ©΄ μ λ¨.

1. **Entities** Enterprise Business Rules

   λΉμ¦λμ€ λͺ¨λΈ(Actorκ° νμλ‘ νλ λ°μ΄ν° λͺ¨λΈ). μν°ν°λ λ©μλλ₯Ό κ°λ κ°μ²΄μΌ μλ μκ³  λ°μ΄ν° κ΅¬μ‘°μ ν¨μμ μ§ν©μΌ μλ μμ.

   νΉμ  λλ©μΈμμ μ¬μ©λλ struct λͺ¨λΈ.

2. **Use cases** Application Business Rules

   Actorκ° Entityλ₯Ό μνλλ°, μ΄ κ°μ κ³μ°λκ±°λ νΉμ  λ‘μ§μ μν΄ μ»μ΄μ§λ―λ‘ Actorκ° μνλ Entityλ₯Ό μ»μ΄λ΄κ³  μλ **λ‘μ§** μλ―Έ.

   μ νλ¦¬μΌμ΄μ κ³ μ  λΉμ¦λμ€ κ·μΉμ ν¬ν¨νλ©° μμ€νμ λͺ¨λ  μ μ€μΌμ΄μ€λ₯Ό μΊ‘μννκ³  κ΅¬νν¨.

   μν°ν°λ‘ λΆν°μ νΉμ μν°ν°μμμ λ°μ΄ν° νλ¦μ μ‘°ν©.

   μ΄ κ³μΈ΅μ λ³κ²½μ΄ μν°ν°μ μν₯μ μ£Όμ§ μμ κ²μ κΈ°λνλ©° λ°μ΄ν°λ² μ΄μ€, UI λλ κ³΅ν΅μ νλ μμν¬μ λ³κ²½μΌλ‘λΆν° μν₯λ°μ§ μμ κ²λ κΈ°λ.

3. **Controllers, Gateway, Prestenters** Interface Adapter

   μ μ€μΌμ΄μ€μ μν°ν°μ μμ΄ μ©μ΄ν νμμΌλ‘λΆν° λ°μ΄ν°λ² μ΄μ€λ μΉ λ± μΈλΆμ κΈ°λ₯μ μ©μ΄ν νμμΌλ‘ λ°μ΄ν°λ₯Ό λ³νν¨.

   Coordinator, ViewModel, ViewController, View, Behaviors(νΉμ  Viewμ eventμ κ΄ν΄ μ μ©λλ UI).

4. **Devices, Web, UI DB External Interfaces** Frameworks & Drivers

   λ°μ΄ν°λ² μ΄μ€λ μΉ νλ μμν¬ λ± μΌλ°μ μΌλ‘ νλ μμν¬λ λκ΅¬λ‘ κ΅¬μ±λ¨. 

   μμͺ½μ μκ³Ό ν΅μ ν  μ°κ²° μ½λ μ΄μΈμλ μμ± μ ν¨.

   Network, CoreData



## π Clean Architecture + MVVM

![img](https://blog.kakaocdn.net/dn/yoKKQ/btqFKl5Y86D/3ZD7kkO3Nkw7FIZK9g8cDk/img.png)

Domain > Interfaces > Repositories μ μλ νμΌλ€μ λͺ¨λ νλ‘ν μ½.

**Data > Repositories μ μλ νμΌλ€μ Domain > Interfaces > Repositoriesμ νλ‘ν μ½λ€μ μ±νν ν΄λμ€λ€.**

![img](https://blog.kakaocdn.net/dn/bHEH6y/btqFKMhY1wf/Ted9svEN3OwQzwt1gij7ek/img.jpg)

Repositoryλ Domain Layerμ Data Layer μ€κ°μ―€μ μμ.

![img](https://blog.kakaocdn.net/dn/bOX0P0/btqFK2q3TWe/Xi8TCdfPHkWd9XiZlZuDR0/img.png)



μ λ³νλ κ²μ΄ μ λ³νμ§ μλ κ³³μ μμ‘΄νλ κ²μ΄ μ΄μμ .

μ λ³νμ§ μλ Domain κ³μΈ΅μΌλ‘ Presentation Layerμ Data Layerκ° μμ‘΄.



1. **Domain**

   Entities + Use Cases. 

   λ€λ₯Έ Layerλ€μκ² μ΄λ ν μν₯λ λ°μ§ μμ. λ€λ₯Έ νλ‘μ νΈμ μνμ¬ μ¬μ¬μ© λ  μ μμ.

   Entities(λΉμ¦λμ€ λͺ¨λΈ), Use Cases, **Repository Interfaces**.

2. **Presentation Layer**

   Presenters + UI.

   UI(UIViewControllers, SwiftUI Views), ViewModels(Presenters).

   ViewModelμ νλ μ΄μμ UseCasesλ₯Ό executeνκΈ° λλ¬Έμ Presentation Layerλ Domain Layerλ₯Ό μμ‘΄ν¨.

3. **Data Layer**

   DB + API

   Repository νλ‘ν μ½μ λν κ΅¬ν(**Repository Implementations**)κ³Ό Data Sources.

   Repositoryλ λ€λ₯Έ Data Sources (λ‘μ»¬ DB, API)λ‘λΆν° λ°μ΄ν°λ₯Ό μ²λ¦¬νλ μ±μ μμ.

   Data Layerλ API μλ΅μΌλ‘ λ°μ JSON Dataλ₯Ό Domain Layerμ μλ λͺ¨λΈλ‘ λ³ννλ μμμ΄ μμ. -> Data Layerλ Domain Layer μμ‘΄.



![img](https://blog.kakaocdn.net/dn/MQ1R1/btqFKDrPxtp/pjv2GVcCJ7ubcfjq8ZxQkK/img.png)

##### Data Flow

1. View(UI)λ ViewModel(Presenter)μ λ©μλλ₯Ό μ½.
2. ViewModelμ UseCaseλ₯Ό μ€νν¨.
3. UseCaseλ Userμ Repositoryλ‘λΆν° λ°μ΄ν° μ‘°ν©.
4. κ°κ°μ Repositoryλ Remote Data(Network) λλ Persistent DB storage Source λλ In-memory Data (Remote or Cached)λ‘λΆν° λ°μ΄ν°λ₯Ό κ°μ Έμ΄.
5. μ λ³΄λ λ€μ View(UI)λ‘ νλ¬ (Information flows back to the View(UI)) μλ‘μ΄ νλ©΄μ λ³΄κ² λ¨.

[μ°Έκ³ 1](https://eunjin3786.tistory.com/207) [μ°Έκ³ 2](https://ios-development.tistory.com/667?category=989887)



#### DTO

Data Transfer Object : JSONμ responseλ₯Ό Entityλ‘ λ³ν

APIλ‘ λΆν° λ°μ responseλ κ·Έλλ‘ λ°κ³ , responseλ₯Ό completion handlerμμ domain λͺ¨λΈλ‘ λ³ννλ μμΌλ‘ μ¬μ©.

λ°μ responseλ κ·Έλλ‘ νμΆ > APIμ response λͺ¨λΈμ μμλ³΄κΈ° μ©μ΄

μ¬μ©νλ μͺ½μμ domain λͺ¨λΈμ μ¬μ©νλλ‘ λ°ν

[μ°Έκ³ ](https://ios-development.tistory.com/668?category=989887)

- DTOλ₯Ό λ§λλ μ΄μ ?

  DTOλ₯Ό λλ©΄ Model, Entityλ₯Ό μ’μμ μ΄μ§ μκ² λ§λ€ μ μμ.

  Data Layerλ₯Ό ν΅ν΄ λ°λ λ°μ΄ν° μλ΅μ΄ λ¬λΌμ§ λλ§λ€ DTOλ§ μμ νλ©΄ λκ³  modelμ μμ νμ§ μμλ λ¨.



#### π€ ViewModel VS UseCase

ViewModelμ λΉμ¦λμ€ λ‘μ§μ λ£λ κ²½μ°λ μλͺ»λ μ κ·Ό :: λΉμ¦λμ€ λ‘μ§μ UseCaseμ μ‘΄μ¬.

ViewModelμ μ­ν μ UI Eventλ€μ΄ λ°μνλ©΄ λ¬΄μμ ν΄μΌνλμ§ μκ³  μλ κ². λ¬΄μμ ν΄μΌνλμ§ μκ³ μκΈ° λλ¬Έμ UseCaseλ₯Ό μ€νμν€κ³  UIμ μλ°μ΄νΈλ₯Ό μλ¦¬λ μ­ν .

λΉμ¦λμ€ λ‘μ§μ μ±μμ μ¬μ©μμ μνΈμμ©μ΄ μλ μλ¬΄ μκ΅¬μ¬ν­μ λ΄κ³  μλ κ². :: κ°λ°ν μΈλΆμ μ¬μ λΆμ μ¬λλ μκ³  μμ΄μΌ νλ λ‘μ§.

