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



## 🖌 AppDelegate, Scene Delegate

![img](https://i.imgur.com/D4VfgIv.png)

![img](https://i.imgur.com/CDwg5Ny.png) 

iOS12까지는 대부분 하나의 앱에 하나의 window 였지만 iOS13부터는 window의 개념이 scene으로 대체되고 하나의 앱에서 여러개의 scene을 가질 수 있음.

AppDelegate의 역할 중 UI의 상태를 알 수 있는 UILifeCycle에 대한 부분은 SceneDelegate가 하게 되고 AppDelegate에 Session Lifecycle에 대한 역할이 추가 됨. -> Scene Session이 생성되거나 삭제될 때 AppDelegate에 알리는 두 메소드 추가. Scene Session은 앱에서 생성한 모든 scene의 정보를 관리함.

- Scene?

  UIKit은 UIWindowScene 객체를 사용하는 앱 UI의 각 인스턴스를 관리. Scene에는 UI의 하나의 인스턴스를 타나내는 windows와 view controllers가 들어있음.

  각 scene에 해당하는 UIWindowSceneDelegate 객체를 가지고 있고, 이 객체는 UIKit과 앱 간의 상호작용을 조정하는 데 사용. Scene들은 같은 메모리와 앱 프로세스 공간을 공유하면서 서로 동시에 실행. 결과적으로 하나의 앱은 여러 scene과 scene delegate 객체를 동시에 활성화 할 수 있음.

- Scene Session?

  scene 고유의 런타임 인스턴스를 관리. 사용자가 앱에 새로운 scene을 추가하거나 프로그래밍적으로 scene을 요청하면 시스템은 그 scene을 추적하는 session 객체를 생성. session에는 고유한 식별자와 scene의 configuration details이 들어있음.

  UIKit은 session 정보를 그 scene 자체의 생애동안 유지하고 app switcher에서 사용자가 그 scene을 클로징하는 것에 대응하여 session을 파괴. session 객체는 직접 생성하지 않고 UIKit이 앱의 사용자 인터페이스에 대응하여 생성함. 

- AppDelegate가 하는 일?

  앱의 데이터 구조 초기화. 앱의 scene 환경설정(configuration). 앱 밖에서 발생한 알림에 대응. 특정 scenes, views, view controllers에 한정되지 않고 앱 자체를 타겟하는 이벤트에 대응하는 것. 애플 푸쉬 알림 서비스와 같이 실행시 요구되는 모든 서비스를 등록하는 것.

  iOS12까지는 앱의 생명주기 이벤트를 관리했지만 더이상 하지 않음

### Scene 기반의 생명주기 이벤트

앱이 Scene을 지원한다면 UIKit은 다양한 생명주기 이벤트를 Scene에게 제공함. 각 Scene은 그들만의 생명주기를 갖고 있기 때문에 유저는 각각의 앱마다 여러개의 Scene을 만들 수 있고 각 Scene마다 보여지거나 숨길 수 있음. 

[참고1](https://velog.io/@zeke/앱-생명주기) [참고2](https://velog.io/@dev-lena/iOS-AppDelegate와-SceneDelegate)



## 🖌 앱의 생명 주기 App's Life Cycle

App의 실행/종료 및 App이 foreground/background 상태에 있을 때 시스템이 발생시키는 event에 의해 App의 상태가 전환되는 일련의 과정.

앱은 main 함수에서 시작. UIKit framework가 main 함수를 관리하기 때문에 개발자가 직접 main 함수에 코드를 작성하지는 않음. UIKit이 main 함수를 다루는 과정에서 UIApplicationMain 함수를 실행하고 이 함수를 통해 UIApplication 객체가 생성됨. 이 객체를 통해 개발자는 앱 실행에 부분적으로 관여할 수 있게 됨.

- 앱이 실행되면 ?

  UIApplication 객체 생성 -> @Main 클래스를 찾아 AppDelegate 객체 생성. -> Main Event Loop 실행

- Main Run Loop

  유저가 일으키는 이벤트들 처리하는 프로세스. UIApplication 객체는 앱이 실행될 때, Main Run Loop를 실행하고 이 Main Run Loop를 View와 관련된 이벤트나 View의 업데이트에 활용. -> Main Thread에서 실행.

### 앱 상태 App State

1. Not Running

   앱이 실행되지 않았거나 완전히 종료되어 동작하지 않는 상태

2. Inactive (Foreground)

   앱이 실행되면서 foreground에 진입하지만, 어떠한 이벤트도 받지 않는 상태. 앱의 상태 전환 과정에서 잠깐 머무는 단계

3. Active (Foreground)

   앱이 실행 중이며 foreground에 있고 이벤트를 받고 있는 상태.

4. Background

   앱이 백그라운드에 있으며 다른 앱으로 전환되었거나 홈버튼을 눌러 밖으로 나갔을 때의 상태.

5. Suspended

   앱이 Background 상태에 있지만 아무 코드도 실행하지 않은 상태. 백그라운드에서 특별한 작업이 없을 경우 Suspended 상태가 됨. 이 상태에서 앱은 메모리 상에 올라가있지만 아무 일도 하지 않기 때문에 배터리 사용하지 않음. 또한 OS에 의해 메모리 부족현상이 발생하면 이 상태의 앱은 메모리에서 없어질 수 있고 따로 알림을 하진 않음.

[참고](https://blog.naver.com/PostView.naver?blogId=soojin_2604&logNo=222423840595&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView)

## 🖌 View Controller의 생명주기 Life Cycle

![img](https://t1.daumcdn.net/cfile/tistory/2613D13C58C64DE32C)

- **loadView**

  컨트롤러가 관리하는 뷰를 만드는 역할.

- **viewDidLoad**

  뷰의 로딩이 완료 되었을 때 시스템에 의해 자동 호출.

  **화면이 처음 만들어질 때 한 번만 실행**. -> 다른 뷰 갔다가 돌아오면 실행 x

  초기 화면 구성, 리소스 초기화할 때 사용.

- **viewWillAppear**

  view가 나타날 거라는 신호를 컨트롤러에게 알리는 역할. 뷰가 나타나기 직전에 호출.

- **viewDidAppear**

  view가 나타났다는 것을 컨트롤러에게 알리는 역할. view가 화면에 나타난 직후에 실행. 

  화면에 적용될 애니메이션 그려줌.

- **viewWillDisappear**

  view가 사라지기 직전에 호출되는 함수. 뷰가 삭제 되려고 하고 있는 것을 컨트롤러에게 알림.

- **viewDidDisappear**

  view가 사라졌음을 컨트롤러에게 알림.

- **viewDidUnload**

  view가 메모리에서 해제된 뒤 호출.



- 네비게이션 컨트롤러(스택구조)에서 첫 번째 뷰에서 두 번째 뷰로 넘어갔다가 다시 첫 번째 뷰로 돌아올 때 ?!

  1st viewWillDisappear -> 2nd viewDidLoad -> **2nd viewWillAppear -> 1st view Did Disappear -> 2nd viewDidAppear** -> 2nd viewWillDisppear -> **1st viewWillAppear**(viewDidLoad 호출 x. 네비게이션 컨트롤러의 root view.) -> 2nd viewDidDisappear -> 1st viewDidAppear



## 🖌 UIKit operations이 main thread에서만 동작해야 하는 이유

1. UI와 관련된 모든 이벤트 처리를 main thread에서 함.

   Cocoa Touch에서 UIApplication인 application의 인스턴스가 UIApplicationMain()(Cocoa Touch의 진입점 함수!!!)에 의해 만들어진 mian thread에 attach됨.

   UIApplicationMain() 함수는 application object와 delegate를 만들고 이벤트 사이클을 설정함.

   앱의 이벤트는 일반적으로 UIApplication -> UIWindow -> UIViewController -> UIView -> subviews(UILabel, UIButton 등)와 같이 chain을 따라 UIResponder에 전달됨.

   Main Thread에서 Responders는 버튼 누리기, 탭 하기 등과 같은 이벤트를 처리하고 이것이 UI 변경으로 변환.  

2. Main Thread가  View Drawing Cycle을 통해 View를 동시에 업데이트. 🧐

   View Drawing Cycle로 뷰의 변경 사항은 즉시 변경되지 않고 현재 run loop 마지막에 다시 그려짐. 모든 변경 사항들을 동시에 활성화 시킬 수 있음.

   만약에 BackGround Thread에서 run loop로 업데이트를 하게 되면 view가 제멋대로 동작.

3. 성능 보호

   UIKit은 모든 종류의 컴포넌트를 포함하며 사용자의 이벤트를 관리하고 **렌더링 코드 포함 하지 않음**

   Core Animation이 모든 뷰의 그리기, 보여주기, 애니메이션을 담당.

   Rendering Process는 CoreAnimation -> Render Server -> GPU -> Present로 이루어짐.

   여러 Thread에서 UI를 업데이트하면 다른 뷰 계층 구조를 인코딩하여  Render Server로 전송하고 GPU는 많은 렌더링 요청으로 성능 저하.

   -> 그래도 background에서 작업하고 싶다면 `Texture`,  `ComponentKit`과 같은 프레임워크 있음.



## 🖌 App Bundle

앱이 정상적으로 작동하기 위해 필요한 모든 것들

- Info.plist

  앱에 대한 구성 configuration 정보(bundle ID, version number 등)를 포함한 파일

- 실행파일 Excutable

  모든 앱은 실행 가능 파일이 있어야 하고 앱의 메인 진입점과 정적으로 앱 타겟에 연결된 코드 포함.

- Resources files

  앱의 실행 가능한 파일 외부에 데이터 파일로 존재. 이미지, 아이콘, 사운드, nib 파일, 문자열 파일 등으로 구성.

  Localized 된 파일은 lproj 확장자 파일.

- 기타 서포트 파일

  커스텀 데이터 리소스 등.



## 🖌 UIViewController

모든 view controller 객체의 상위 클래스. 

직접 UIViewController의 객체를 만드는 경우는 없는 대신 뷰 계층을 관리하는데에 필요한 메서드와 속성 이용 가능.

- View Controller의 역할
  - 뷰와 사용자 상호 작용에 응답. 
  - 기본 데이터 변경에 대한 응답으로 뷰의 콘텐츠 업데이트.
  - 앱에서 다른 뷰 컨트롤러를 포함한 다른 객체와 조정.
  - 뷰 크기 조정 및 전체 인터페이스의 레이아웃 관리.

View controller는 UIResponder 객체이며 다른 view controller에 속하는 view controller의 루트 뷰와 해당 뷰의 super view 사이의 responder chain에 삽입 됨.

### View Managemnet

각 view controller는 view의 계층 구조를 관리.

루트 뷰는 해당 클래스의 view property에 저장됨. 루트 뷰는 나머지 보기 계층 구조에 대한 컨테이너 역할을 함. 루트 뷰의 크기와 위치는 view controller나 앱의  window인 루트 뷰를 소유한 객체에 의해 결정됨.

⭐️ view controller는 뷰와 vc이 생성하는 모든 하위 뷰의 유일한 소유자. 때문에 view controller가 해제될 때와 같이 적절한 시점에 뷰를 생성하고 소유권을 포기하는 책임이 있음. 그래서 뷰 객체를 저장하게 되면  vc이 요청하면 각 vc 객체는 자동으로 그들의 뷰의 복사본을 갖게됨. 그러나 만약 뷰를 수동으로 생성하는 경우에는 각 vc에 고유한 뷰 set이 있어야 하고 vc 간에 공유할 수 없음.

### Handling View-Related Notification

뷰가 변화되면 vc는 자동으로 자신의 메소드를 호출(viewWillAppear() ..)하여 하위 클래스가 변경사항에 응답할 수 있음. -> 생명주기 확인

### Handling View Rotations

뷰의 회전은 vc의 뷰 크기 변경으로 처리됨. viewWillTransition() 이용. 인터페이스 방향이 변경되면 UIKit은 window의 루트 vc에서 이 메소드를 호출함. 그 vc은 자식 vc에게 알리고 vc 계층 구조 전체에 메시지 전파.

### Implementing a Container View Controller

custom UIViewController 하위 클래스는 컨테이너 vc 역할 가능.

컨테이너 vc는 자식 vc의 콘텐츠 표시를 관리함.
