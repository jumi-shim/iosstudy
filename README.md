# swift

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
    

[참고1](https://corykim0829.github.io/swift/Understanding-Swift-Performance/#) [참고2](https://onemoonstudio.tistory.com/7) [참고3](https://babbab2.tistory.com/145)

### struct 사용 권장 경우
- 연관된 간단한 값의 집합을 캡슐화하는 것만이 목적일 때
- 캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할 때
- 구조체에 저장된 프로퍼티가 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때
- 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 때

## enum
열거형. 연관된 항목들을 묶어서 표현할 수 있는 타입. 타입 분류!

항목별로 값을 가질수도, 가지지 않을 수도 있음.

extension 이용 가능



## Copy On Write (COW)

실제 원본이나 복사본이 수정되기 전까지는 복사를 하지 않고 원본 리소스를 공유하고 원본이나 복사본에서 수정이 일어나면 그 때 복사하는 작업을 함. - Collection Type(Array Dictionary, Set)을 사용할 때

- 그런데 ? collection type은 구조체이고, 구조체는 위에서 말했듯 value type이다. 그래서 만약 array A를 array B에 대입했다면 값이 복사가 되어야 한다. 하지만 COW 때문에 A와 B는 동일 메모리를 가르키고 있고 A와 B중에 수정이 일어나면 그 변수에 메모리를 새로 할당해 준다. -> 성능 상승. 



## convenience init

init(Designated init)은 클래스의 모든 프로퍼티가 초기화가 될 수 있도록 해야함.

Convenience init은 Designated init을 자신 내부에서 실행하여 매개변수가 많아 외부에서 일일이 전달 인자를 전달하기 어렵거나 특정 목적으로 항상 같은 초기값으로 초기화 하고 싶을 경우에 씀.

1. 자식클래스의 Designated init은 부모 클래스의 Designated init을 반드시 호출해야 함.
2. Convenience init은 자신을 정의한 클래스의 다른 init을 반드시 호출해야 함.
3. Convenience init은 궁극적으로 Designated init을 반드시 호출해야 함.





## Any, Any Object

타입을 지정하지 않고 여러 타입의 값을 할당

### Any

함수 타입을 포함한 스위프트의 모든 데이터 타입 사용 가능.

### Any Object

클래스의 인스턴스만 할당할 수 있음.



- 타입 캐스팅을 수행할 때 일반적으로 상속 관계에 있는 클래스끼리만 캐스팅이 가능하지만 Any, AnyObject는 상속 관계에 있지 않아도 타입 캐스팅을 할 수 있다.

- Any, Any Object의 타입은 런타임 시점에 타입이 결정됨. -> 컴팡리 시점에 해당 타입에 대해 알 수 없기 때문에 해당 타입의 멤버에도 접근 불가. (ex: Any 타입에 String 타입을 할당해도 String 함수 쓰지 못함.) -> 다운캐스팅 사용





## Optionals

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





# iOS

## Frame vs Bounds

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



## UIViewController

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





## App thinning

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



## Cocoa Touch

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
