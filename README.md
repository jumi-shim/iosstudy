# 📕 swift

## 🖌 struct VS class
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
    

[참고1](https://corykim0829.github.io/swift/Understanding-Swift-Performance/#) [참고2](https://onemoonstudio.tistory.com/7) [참고3](https://babbab2.tistory.com/145)

### struct 사용 권장 경우
- 연관된 간단한 값의 집합을 캡슐화하는 것만이 목적일 때
- 캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할 때
- 구조체에 저장된 프로퍼티가 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때
- 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 때

## 🖌 enum
열거형. 연관된 항목들을 묶어서 표현할 수 있는 타입. 타입 분류!

항목별로 값을 가질수도, 가지지 않을 수도 있음.

extension 이용 가능



## 🖌 Copy On Write (COW)

실제 원본이나 복사본이 수정되기 전까지는 복사를 하지 않고 원본 리소스를 공유하고 원본이나 복사본에서 수정이 일어나면 그 때 복사하는 작업을 함. - Collection Type(Array Dictionary, Set)을 사용할 때

- 그런데 ? collection type은 구조체이고, 구조체는 위에서 말했듯 value type이다. 그래서 만약 array A를 array B에 대입했다면 값이 복사가 되어야 한다. 하지만 COW 때문에 A와 B는 동일 메모리를 가르키고 있고 A와 B중에 수정이 일어나면 그 변수에 메모리를 새로 할당해 준다. -> 성능 상승. 



## 🖌 convenience init

init(Designated init)은 클래스의 모든 프로퍼티가 초기화가 될 수 있도록 해야함.

Convenience init은 Designated init을 자신 내부에서 실행하여 매개변수가 많아 외부에서 일일이 전달 인자를 전달하기 어렵거나 특정 목적으로 항상 같은 초기값으로 초기화 하고 싶을 경우에 씀.

1. 자식클래스의 Designated init은 부모 클래스의 Designated init을 반드시 호출해야 함.
2. Convenience init은 자신을 정의한 클래스의 다른 init을 반드시 호출해야 함.
3. Convenience init은 궁극적으로 Designated init을 반드시 호출해야 함.



## 🖌 Any, Any Object

타입을 지정하지 않고 여러 타입의 값을 할당

### Any

함수 타입을 포함한 스위프트의 모든 데이터 타입 사용 가능.

### Any Object

클래스의 인스턴스만 할당할 수 있음.



- 타입 캐스팅을 수행할 때 일반적으로 상속 관계에 있는 클래스끼리만 캐스팅이 가능하지만 Any, AnyObject는 상속 관계에 있지 않아도 타입 캐스팅을 할 수 있다.

- Any, Any Object의 타입은 런타임 시점에 타입이 결정됨. -> 컴팡리 시점에 해당 타입에 대해 알 수 없기 때문에 해당 타입의 멤버에도 접근 불가. (ex: Any 타입에 String 타입을 할당해도 String 함수 쓰지 못함.) -> 다운캐스팅 사용



## 🖌 Optionals

값이 있을 수도, 없을 수도(nil) 있음을 나타내는 표현. 데이터 타입 뒤에 물음표(?)를 붙여 표현.

enum. 제네릭이 적용된 열거형.

swift의 안정성 제공. 프로그래머 간의 원활한 커뮤니케이션에 도움.

- __옵셔널 바인딩__ : 옵셔널에 값이 있는지 확인할 때 사용

  if, while 구문과 결합하여 사용. 옵셔널에서 추출한 값을 일정 블록 안에서 사용할 수 있는 상수나 변수로 할당해서 옵셔널이 아닌 형태로 사용.

- __암시적 추출 옵셔널__ : nil을 할당해줄 수 있는 옵셔널이 아닌 변수나 상수

  타입 뒤에 느낌표(!)

  옵셔널 바인딩으로 매번 값을 추출하기 귀찮거나 로직상 nil 때문에 런타임 오류가 발생하지 않을 것 같다는 확신이 들 때 사용.

- __옵셔널 체이닝__

  옵셔널에 속해 있는 nil일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정.

  중첩된 옵셔널 중 하나라도 값이 존재하지 않으면 nil 반환.



## 🖌 Escaping Closure

함수의 인자로 전달된closure를  함수가 종료 후 실행할 수 있음.

함수의 인자가 함수의 scope를 탈출하여 함수 밖에서 사용하기 위해서는 @escaping을 이용하면 됨. 함수가 마무리 된 후 클로져를 실행한다는 점에서 유용.  -> 비동기에서 실행되는 HTTP Request CompletionHandler.

- escaping, non-escaping 클로저를 나눠서 사용하는 이유?

  컴파일러의 퍼포먼스와 최적화 때문. 

  non-escaping 클로저는 컴파일러가 클로저의 실행이 언제 종료될지 알기 때문에 때에 따라 클로저에서 사용하는 특정 객체에 대한 retain, release 등의 처리를 생략해 객체의 life-cycle을 효율적으로 관리할 수 있음.

  escaping 클로저는 함수 밖에서 실행되기 때문에 클로저가 함수 밖에서도 적절히 실행되는 것을 보장하기 위해, 클로저에서 사용하는 객체에 대한 추가적인 reference cycles 관리 등을 해줘야 함. -> 컴파일러의 퍼포먼스와 최적화에 영향.



## 🖌 ARC

Automatic Reference Counting. 자동 메모리 관리 방식.

더이상 필요하지 않은 클래스(참조 타입)의 인스턴스를 메모리에서 해제하는 방식으로 동작. 

- 참조 카운팅 시점 : 컴파일 시
- 장점 : 컴파일 당시 이미 인스턴스의 해제 시점이 정해져 있어 인스턴스가 언제 메모리에서 해제될지 예측할 수 있고 메모리 관리를 위한 시스템 자원을 추가할 필요가 없음.
- 단점 : ARC의 자동 규칙을 모르고 사용하면 인스턴스가 메모리에서 영원히 해제되지 않을 가능성 있음.

### 🖊 강한참조

인스턴스를 다른 인스턴스의 프로퍼티나 변수, 상수 등에 할당할 때 강한참조를 사용하면 참조 횟수가 1 증가하고 nil을 할당해주면 원래 자신에게 할당되어 있던 인스턴스의 참조 횟수가 1 감소함.

- 지역변수(상수)가 인스턴스를 참조하고있고 지역변수가 사용된 범위의 코드 실행이 종료되면 참조하고 있던 인스턴스의 참조 횟수가 1 감소함.

- #### 강한참조 순환 문제

  ```swift
  class A {
    var b: B ?
    deinit {
      print("a deinit")
    }
  }
  class B {
    var a: A?
    deinit {
      print("b deinit")
    }
  }
  
  var a1: A? = A()	//A 인스턴스의 참조 횟수 : 1
  var b1: B? = B()	//B 인스턴스의 참조 횟수 : 1
  a1?.b = b1 //B 인스턴스의 참조 횟수 : 2
  b1?.a = a1 //A 인스턴스의 참조 횟수 : 2
  a1 = nil // A 인스턴스의 참조 횟수 : 1
  b2 = nil // B 인스턴스의 참조 횟수 : 1
  // A 메모리를 해지하기 위해 A의 프로퍼티 b를 보니까 b는 B를 참조 중이고 B를 해지하려 보니까 B의 프로퍼티 a는 A를 참조중이고..~~~
  //A와 B 인스턴스를 참조할 방법이 없음. 메모리에 잔존.
  ```



### 🖊 약한참조

자신이 참조하는 인스턴스의 참조 횟수를 증가시키지 않음. 참조 타입의 프로퍼티나 변수의 선언 앞에 weak 키워드 써주면 됨. 

자신이 참조하는 인스턴스가 메모리에서 해제될 수도 있다는 것을 예상해볼 수 있어야 함. 옵셔널 변수만 약한 참조 가능!! -> 참조 횟수를 증가시키지 않았기 때문에 그 인스턴스를 강한 참조하던 프로퍼티나 변수에서 참조 횟수를 감소시켜 0으로 만들면 메모리 해제되기 때문.

```swift
class B {
  weak var a: A?	//B는 A에게 의존적이어서 A가 없으면 없어도 될 때..!!
  deinit {
    print("b deinit")
  }
}
var a1: A? = A()	//A 인스턴스의 참조 횟수 : 1
var b1: B? = B()	//B 인스턴스의 참조 횟수 : 1
a1?.b = b1 //B 인스턴스의 참조 횟수 : 2
b1?.a = a1 //A 인스턴스의 참조 횟수 : 1
a1 = nil // A 인스턴스의 참조 횟수 : 0, B 인스턴스의 참조 횟수 : 1, a deinit
print(b1?.a)	//nil
b1 = nil //B 인스턴스의 참조 횟수 : 0, b deinit
```







# 📗 iOS 

## 🖌 Frame vs Bounds

- view의 위치와 크기 나타냄.
- super view : 최상의 View인 루트 View가 아니라 __나의 한칸 윗 계층 View__

### Frame

__super view 좌표계__에서 view의 위치와 크기를 나타냄

- origin(x,y) : super view의 원점을 (0,0)으로 놓고 원점으로부터 얼마나 떨어져 있는지. super view 좌표계 (원점 : view의 가장 왼쪽, 가장 윗 부분) 
- size(width,height) : view 영역을 모두 __감싸는__ 사각형으로 나타낸 것. view 자체의 크기가 아니라 view가 차지하는 영역을 감싸서 만든 사각형.

### Bounds 

__자신의 좌표계__에서 view의 위치와 크기를 나타냄

- origin(x,y) : 자신의 원점을 (0,0)으로 놓음. 즉, (0,0)으로 초기화. bounds를 바꿔줘야 하는 경우(ex: ScrollView의 ContentOffset)에 사용.

  만약 origin을 변경했다면 변경한 위치로 view를 이동시키는 것이 아니라 __viewport(핸드폰 화면에 보여지는 창)를 view 자체 좌표계에서 이동__한다는 것.

- size(width, height) : view 자체의 영역. 감싼 X

- view를 회전한 후 view의 실제 크기를 알고 싶을 때, view 내부에 그림을 그릴 때(drawRect), ScrollView에서 스크롤링 할 때 등에 사용.

[참고](https://babbab2.tistory.com/45?category=831129)



## 🖌 UIViewController

UIKit앱의 View hierarchy 관리.

View controller는 하나의 root view를 관리하고 이 root views는 subviews를 포함할 수 있음.

__Content ViewController__와 __Container ViewController__가 있음.

- View를 통해 사용자 상호 작용에 응답

- 전체 인터페이스의 레이아웃을 관리

- 앱에서 다른 ViewController를 포함한 다른 객체들과 조정

- 데이터가 변경되면 뷰의 콘텐츠를 업데이트

### Container ViewController

하나 이상의 ViewController를 child ViewController로 관리하는 컨트롤러.

Child ViewController에 속한 view들을 관리하지 않음.

-> 왜 필요할까? Navigation 로직(라우팅)을 분리하여 단일 책임원칙을 지키기 위해. 

ex : UITabBarController, UINavigationController, UIPageViewController 등.

### Content ViewController

화면을 구성하는 뷰를 직접 구현하고 관련된 이벤트를 처리하는 뷰 컨트롤러





## 🖌 App thinning

애플리케이션이 디바이스에 설치될 때, 앱 스토어와 운영체제가 디바이스의 특성에 맞게 설치되도록 하는 설치 최적화 기술. 최소한의 디스크 사용과 빠른 다운로드 제공 가능.

슬라이싱(slicing), 비트코드(bitcode), 주문형 리소스(on-demand resource)

### 슬라이싱(slicing)

앱이 지원하는 여러 디바이스에 대해 애플리케이션 번들의 variants를 생성하고, 해당 디바이스에 가장 적합한 variant를 전달하는 기술. variants는 실행가능한 아키텍쳐, 리소스만 포함.

개발자가 App Store connect에 업로드하면, 앱 스토어에서 디바이스 특성에 따라 다양한 버전의 조각들을 생성하면 사용자가 그 조각 중에서 가장 알맞은 app variant를 다운로드.

Xcode는 개발 중 슬라이싱을 시뮬레이션하므로 앱의 variants를 만들고 테스트 가능.

### 비트코드(bitcode)

기계언어로 번역되기 이전 단계의 중간표현(Intermediate Representation). 현재 iOS에서는 옵션이나 기본 설정으로 되어 있으며, 개발자가 프로젝트 옵션에서 선택가능. 

비트코드를 사용하여 업로드를 하면 애플이 애플리케이션을 재컴파일하여 앱 바이너리(app binary)를 생성. 비트코드를 사용하지 않으면, 모든 경우의 디바이스 경우에 따라 바이너리로 생성하여 합쳐져서 fat binary라는 것이 업로드되지만, 비트코드를 사용하면 필요 경우에 따라 재컴파일하게 되므로 여기에서 최적화 가능.

### 주문형 리소스(on-demand resource)

이미지나 사운드같은 리소스를 키워드로 태그할 수 있고 태그별로 그룹을 요청할 수 있음. 앱스토어는 apple 서버의 리소스를 호스팅하고 다운로드를 관리. 주문형 리소스를 분할시켜 앱의 변형을 최적화.

필요할 때 다운받음. 예를 들어 사용자가 게임을 할 때 현재 레벨보다 상위레벨은 필요하지 않으므로 갖고 있을 필요가 없음. 사용자의 레벨이 필요할 때 다운로드 받는 것.

[참고](https://zeddios.tistory.com/655)



## 🖌 Cocoa Touch

iOS 개발에 필요한 여러 프레임워크를 포함하고 있는 최상위 프레임워크

Cocoa는 Objective-C 런타임을 기반으로하고, NSObject를 상속받는 모든 클래스 또는 객체를 가리킬 때 사용.

Cocoa 또는 Cocoa Touch는 iOS 또는 macOS의 전반적인 기능을 활용해 애플리케이션을 제작할 때 사용하는 프레임워크

UIKit, Foundation, CoreData, MapKit, CoreAnimation 등이 포함.

### UIKit

iOS 애플리케이션 사용자 인터페이스를 구현하고 이벤트를 관리하는 프레임워크

제스처 처리, 애니메이션, 그림 그리기, 이미지 처리, 텍스트 처리 등 사용자 이벤트 처리를 위한 클래스를 포함.

테이블뷰, 슬라이더, 버튼, 텍스트 필드 등 애플리케이션의 화면을 구성하는 요소 포함.

UIKit 클래스 중 UIReponder에서 파생된 클래스나 사용자 인터페이스에 관련된 클래스는 애플리케이션의 메인 스레드 혹은 메인 디스패치 큐에서만 사용 가능.

### Foundation

원시 데이터 타입(String, Int, Double), 컬렉션 타입(Array, Dictionary, Set), 운영체제 서비스를 사용해 애플리케이션의 기본적인 기능을 관리하는 프레임워크.

### Core Data

Persistence를 포함한 다양한 기능을 제공하는 Framework.

In-memory 형태 -> 메모리에 로드된 객체에 대해서만 수정 가능.

single thread 환경으로 thread-safe 하지 않음.

- __Object-Oriented__

  SQL을 쓸 일 없이 Object-Oriented 방식으로만 데이터를 다룰 수 있음. 데이터는 Object로 표현되며 NSManagedObjectModel의 인스턴스로 구현됨. 이러한 __Object가 관계를 형성하여 Object Graphs를 이루고 이를 관리하는 프레임워크가 Core Data.__

  객체들이 할당되고 연결되었을 경우 한 객체에만 접근 가능하다면, 추가적인 Fetch 작업 없이 해당 객체로부터 나머지 연결된 객체들을 타고 넘어가며 접근 가능. -> 일단 데이터들이 메모리에 로딩되면 연결을 따라 이동하는 것은 검색없이 가능하기 때문에 searchless 특성.

- __Map__

  관계형 데이터베이스에서 처럼 객체나 변수를 mapping함. Core Data도 내부적으로는 SQL을 이용하여 데이터를 저장하지만, 개발자는 Xcode에 내장된 데이터 모델 에디터를 통해 데이터의 타입, 관계를 지정하고 코드로 관련 클래스를 수정할 수 있음. 

[공식문서](https://developer.apple.com/documentation/coredata/)

##### SQLite VS Core Data

|                         |        SQLite        |      Core Data       |
| ----------------------- | :------------------: | :------------------: |
| 형태                    |    데이터 베이스     |     프레임 워크      |
| 속도                    |         느림         |      속도 빠름       |
| 메모리 및 저장공간 사용 |         적음         |         많음         |
|                         | Lightweight solution | Complex object graph |



## 🖌 Delegate

클래스 또는 구조체가 책임의 일부를 다른 유형의 인스턴스에 넘겨주거나 위임을 할 수 있도록 하는 디자인 패턴.

위임된 기능을 제공하기 위해 준수 형식(대리자)이 보장되도록 위임된 책임을 캡슐화하는 프로토콜을 정의하여 구현됨.

- 클래스에서 프로토콜을 채택할 때에는 Retain Cycle이 발생할 수 있기 때문에 weak 이용.

  ```swift
  protocol AClassDelegate:class{
    func doSomething()
  }
  class AViewController:UIViewController {
    weak var delegate:AClassDelegate?
    func action(){
      delegate?.doSomething()
    }
  }
  class BViewController:UIViewController, AClassDelegate {
    var aVC:AViewController?
    override func viewDidLoad() {
      super.viewDidLoad()
      aVC = AViewController()
      aVC?.delegate = self
    }
    func doSomething() {
    }
  }



## 🖌 URLSession

iOS 앱에서 서버와 통신하기 위한 API.

- Request

  서버로 요청을 보낼 때 어떻게 데이터를 캐싱할 것인지, 어떤 http 메소드를 사용할 것인지, 어떤 내용을 전송할 것인지 등을 설정.

  - URL 객체를 통해 직접 통신하는 형태
  - URLRequest 객체를 만들어서 옵션을 설정하여 통신하는 형태

- Response

  - 설정된 Task의 Completion Handler 형태로 response를 받는 형태.

  - URLSessionDelegate를 통해 지정된 메소드를 호출하는 형태로 response를 받는 형태.

    앱이 background 상태로 들어갈 때에도 파일 다운로드를 지원하도록 설정하는 경우나 인증과 캐싱을 default 옵션으로 사용하지 않는 상황인 경우 등에서 delegate 패턴 사용.

- Session

  - Default Session : 기본적인 Session으로 디스크 기반 캐싱 지원
  - Ephemeral Session : 어떠한 데이터도 저장하지 않는 형태의 세션.
  - Background Session : 앱이 종료된 이후에도 통신이 이루어지는 것을 지원하는 세션

- Task

  Session 객체가 서버로 요청을 보낸 후, 응답을 받을 때 URL 기반의 내용들을 받는(retrieve) 역할. 

  - Data Task : Data 객체를 통해 데이터를 주고 받는 Task. GET.
  - Download Task : data를 파일의 형태로 전환 후 다운 받는 Task. 백그라운드 다운로드 지원.
  - Upload Task : data를 파일의 형태로 전환 후 업로드 하는 Task. POST, PUT.

- Life Cycle
  1. Session configuration을 결정하고 Session을 생성한다.
  2. 통신할 URL과 Request 객체를 설정한다.
  3. 사용할 Task를 결정하고, 그에 맞는 Completion Handler나 Delegate 메소드들을 작성한다.
  4. 해당 Task를 실행한다.
  5. Task 완료 후 Completion Handler가 실행된다.



## 🖌 Notification Center

Observer pattern에서 observer를 등록하고, notification을 주는 역할만 빼서 추상화 레벨을 올린 구현체.

기존의 Observer pattern에서는 Subject가 observer list를 관리하고 알림을 줄 일이 발생하면 직접 notification을 dispatch 했다면, 이제는 notification dispatch도 외주를 맡기는 셈.

subject와 observer 둘 다 Notification Center에만 등록되어 있다면 그에 맞는 notification 발생시 Notification Center에서 그에 맞는 일을 알아서 해줌. 보통 백그라운드 작업의 결과, 비동기 작업의 결과 등 현재 작업의 흐름과 다른 흐르의 작업으로부터 이벤트를 받을 때 사용.

- iOS's MVC pattern

  Model: subject

  View Controller: observer

  model에 변화 -> view를 변화 일 때, view controller가 model의 변화 notification에 observer 등록하고, model에서 update가 생기면 post notification 하면 됨..!

Single 프로그램 내에서만 notification 전송할 수 있으며 다른 프로세스로부터 notification 받으려면 DistributedNotificationCenter 이용.

[참고1](https://developer.apple.com/documentation/foundation/notificationcenter), [참고2](https://daheenallwhite.github.io/ios/2019/10/13/Notification-Center/), [참고3](https://silver-g-0114.tistory.com/106)



## 🖌 Key Chain

Key Chain은 디바이스 안에 암호된 데이터 저장 공간 의미. 사용자는 암호화된 공간에 데이터를 안전하게 보관 가능.

로그인 및 암호(해시), 결제 데이터, 알고리즘 암호화를 위한 키 등 저장.

- 사용자가 직접 제거하지 않는 이상 앱을 제거하고 설치해도 데이터는 남아있음.
  - KeyChain의 위치는 Sandbox. Sandbox는 외부에서 받은 파일을 그 자리에서 실행하지 않고 보호된 영역에서 실행시켜 잘못된 파일과 프로그램이 내부 시스템에 악영향 주는 것 방지.
- 디바이스를 lock 하면 KeyChain도 잠기고, 디바이스를 Unlock하면 KeyChain 풀림.
  - KeyChain 잠긴 상태, Item 들에 접근할 수도 복호화 할 수도 없음.
  - KeyChain이 풀린 상태, 해당 Item을 생성하고 저장한 어플리케이션에서만 접근 가능.
- app은 자신의 KeyChain에만 접근할 수 있으며 같은 개발자가 개발한 여러 앱에서 키체인 정보 공유할 수 있음.

- __KeyChain Service API__

  민감한 데이터를 암호화, 복호화 하며 재사용 하는 행위를 쉽고 안전하게 사용할 수 있게 함.

- __KeyChain Item__

  데이터를 Attributes와 함께 Item으로 KeyChain에 저장.

- __KeyChain Item Class__

  - kSecClassInternetPassword : 인터넷 암호 항목
  - kSecClassCertificate : 인증서 항목
  - kSecClassGenericPassword : 일반 암호 항목
    - kSecAttrService : 공개키
    - kSecAttrAccount : 개인키
  - kSecClassIdentitiy : ID 항목
  - KSecClassKey : 암호화 키 항목

User Defaults에도 데이터를 쉽게 저장할 수 있지만, 단순히 .info 파일에 키-값 쌍을 텍스트 형태로 저장하기 때문에 OS를 탈옥하면 내용물을 볼 수 있다는 문제.





# 📘 Rx

## 🖌 Reactive Programming

비동기적인 데이터 흐름(stream)을 처리하는 것. 

데이터를 관찰하고 있다가 변화가 생기면 그에 따른 데이터 혹은 UI 업데이트 같은 일을 수행하는 것. -> 반응 Reactive!

iOS SDK에서 Closure, Notification, Delegate 등을 이용하면 비동기 처리가 가능하지만 데이터 변화가 생기면 UI 업데이트를 명시적으로 호출해야 하거나 콜백 지옥이 생기는 등의 문제 요소들이 있음. 이때 Reactive Extension, Rx를 사용하면 일관성 있는 비동기 코드 작성이 가능하고 스레드 처리가 쉬워지는 이점이 있음. 

Reactive Programming은 하나의 패러다임일 뿐이므로 Rx를 사용하지 않아도 reactive하게 프로그램을 구현할 수 있음. Rx는 편하고 간단하게 구현할 수 있도록 돕는 도구임!

### Rx의 3요소

- Observable
- Operator
- Scheduler





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

