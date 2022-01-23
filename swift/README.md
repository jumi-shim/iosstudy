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



## 🖌 Generic 제네릭

제네릭을 이용하면 타입에 유연하게 대응 가능. 제네릭으로 구현한 기능과 타입은 재사용이 쉽고 코드의 중복을 줄일 수 있음. 실제 타입 이름(Int, Stirng)을 쓰는 것 대신 플레이스홀더 사용.

- 제네릭 함수

  ```swift
  func swapTwoValues<T>(_ a: inout T, _ b: inout T) { // a, b는 같은 타입. T의 타입은 함수가 호출되는 순간 결정.
    let temporaryA: T = a
    a = b
    b = temporaryA
  }
  ```

- 제네릭 타입

  ```swift
  struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element){
      items.append(item)
    }
    mutating func pop() -> Element {
      items.removeLast()
    }
  }
  var intStack: Stack<Int> = Stack<Int>() //정해준 타입에서만 동작 -> 안전
  ```

  - extension으로 제네릭 타입에 기능 추가

    ```swift
    extension Stack {
      var topElement: Element? {	//기존 제네릭 타입에 정의된 Element 이용.
        return self.items.last
      }
    }
    ```

- 타입 제약

  타입이 특정 프로토콜이나 클래스를 따르는 타입만 사용할 수 있도록 제약. 

  열거형, 구조체 등의 타입은 타입 제약의 타입으로 사용 불가.

  ```swift
  public struct Dictionary<Key: Hashable, Value> : Collection, ExpressibleByDictionaryLiteral { }
  //Hashable 프로토콜을 준수하는 Key. 
  ```

  여러 제약을 주고싶으면 where 절 사용.

- 프로토콜 Associated 연관 타입

  프로토콜에서 사용할 수 있는 플레이스 홀더이름. associatedtype 이용.

  
