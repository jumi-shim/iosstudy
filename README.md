# swiftstudy

## struct VS class
property, method로 데이터를 용도에 맞게 묶어 표현.
- 서브스크립트 문법을 통해 구조체 또느 클래스가 갖는 프로퍼티에 접근하도록 서브스크립트 정의 가능
- initializer 정의 가능
- extension을 이용해 확장 가능
- 특정 프로토콜 준수 가능

### struct
- 값 타입의 인스턴스 : 인스턴스를 let으로 선언했을 때 인스턴스 내부의 property 값 변경 불가
- 상속 불가

### class
- 참조 타입의 인스턴스 : 인스턴스를 let으로 선언했을 때 내부의 property 값 변경 가능
- Reference Counting 존재
- 클래스의 인스턴스는 타입캐스팅 허용
- deinitializer 활용 가능


### Value and Reference Semantics
메모리 할당과 해제는 stack이나 heap에서 처리됨. 둘 중 어디에서 메모리 관리를 하는지는? value semantics와 reference semantics로 결정!
- **Value Semantics** : struct, enum, tuple
  - 생성한 인스턴스 A를 다른 인스턴스 B에 할당하면 A의 값이 복사가 됨! 이 말은 곧 A와 B는 구분되어져 stack에 저장되고 B값을 변경해도 A값이 변경되지 않음. heap을 사용하지 않기 때문에 Reference Counting도 사용하지 않음.
  - 단, reference semantics를 따르는 타입(ex: String, UIFont 등)을 프로퍼티로 가지게 되면 reference counting 발생. 여기서 reference counting 개수는 구조체에 있는 레퍼런스 개수에 비례하게 되는데 따라서 구조체에 2개 이상의 레퍼런스를 가지게 되면 reference counting 오버헤드(간접적인 메모리 등)가 클래스보다 더 많이 발생하게 됨..! 
- **Reference Semantics** : class, function
  - stack에는 reference인 주소값을 할당하고, 실질적인 데이터는 heap에 할당. 인스턴스 A를 생성하고 B에 할당하면 값이 복사되는 것이 아니라 stack에 있는 주소값이 저장됨. 따라서 B를 수정하면 A도 값이 변함.  

**-> class가 struct보다 더 많은 비용 요구!!**

### Method Dispatch
프로그램이 어떤 메소드를 호출할 것인지 결정하여 그 메소드를 호출하는 과정. 어떤 메소드인지 결정되는 시점에 따라 **static method dispatch**와 **dynamic method dispatch**로 나뉘게 됨.
- **static method dispatch**
  - 컴파일 시점에 메소드의 실제 코드 위치 파악 가능. 런타임에 찾는 과정 없이 바로 해당 코드 실행.
  -  메소드 인라이닝(메소드 호출 시 해당 메소드로 이동하지 않고 메소드의 결과값을 바로 반환)과 같은 코드 최적화 적극 시행.
- **Dynamic Method Dispatch**
  - 컴파일 타임에 어떤 메소드를 호출하는지 판단할 수 없어 런타임에 vTable 구현을 참조하여 해당 메소드에 대한 정보를 가져와서 실행. -> indirect call!
  - 레퍼런스 카운팅, 힙 할당과 같은 쓰레드 동기 오버헤드가 없어 static dispatch보다 그렇게 많은 비용을 필요로 하지는 않지만 최적화 작업이 가능한 static dispatch와 달리 dynamic dispatch에서는 컴파일러가 추론 불가. 
  - class는 Reference Type이며 Struct와 다르게 상속이 가능. 즉, 다른 곳에서도 class의 함수를 호출할 가능성이 존재한다는 것. -> 컴파일러는 직접 접근 말고 참조 형식으로 해야겠다~~  

- **Static Dispatch를 이용한 성능 최적화 방법**
  - **final**
    - 상속, 오버라이딩 될 필요가 없는 클래스, 메소드, 프로퍼티에 final 키워드를 붙여 이용 시 stored property에 직접 접근, direct function call 사용!
  - **private**
    - 접근이 현재 파일로 제한되는 경우 private 키워드를 붙이기.
    - private 키워드가 참조될 수 있는 곳에서 잠재적으로 override가 될 수 있는지 파악. 없다면 스스로 final 키워드를 추론하고 indirect call & access 제거.
  - **WMO(Whole Module Optimization)**
    - Xcode build Setting에서 확인 가능.
    - 모듈 전체를 하나의 덩어리로 컴파일 하여, internal level에 대해서 오버라이딩 되는지 안 되는지를 추론할 수 있게 되고 오버라이딩이 되지 않을 경우 내부적으로 final를 붙힘.
    
[참고1](https://corykim0829.github.io/swift/Understanding-Swift-Performance/#)
[참고2](https://onemoonstudio.tistory.com/7)
[참고3](https://babbab2.tistory.com/145)

### struct 사용 권장 경우
- 연관된 간단한 값의 집합을 캡슐화하는 것만이 목적일 때
- 캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할 때
- 구조체에 저장된 프로퍼티가 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때
- 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 때

## enum
열거형. 연관된 항목들을 묶어서 표현할 수 있는 타입. 타입 분류!

항목별로 값을 가질수도, 가지지 않을 수도 있음.

extension 이용 가능
