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
