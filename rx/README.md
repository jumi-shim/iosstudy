# 📍Rx

## 🖌 Reactive Programming

비동기적인 데이터 흐름(stream)을 처리하는 것. 

데이터를 관찰하고 있다가 변화가 생기면 그에 따른 데이터 혹은 UI 업데이트 같은 일을 수행하는 것. -> 반응 Reactive!

iOS SDK에서 Closure, Notification, Delegate 등을 이용하면 비동기 처리가 가능하지만 데이터 변화가 생기면 UI 업데이트를 명시적으로 호출해야 하거나 콜백 지옥이 생기는 등의 문제 요소들이 있음. 이때 Reactive Extension, Rx를 사용하면 일관성 있는 비동기 코드 작성이 가능하고 스레드 처리가 쉬워지는 이점이 있음. 

Reactive Programming은 하나의 패러다임일 뿐이므로 Rx를 사용하지 않아도 reactive하게 프로그램을 구현할 수 있음. Rx는 편하고 간단하게 구현할 수 있도록 돕는 도구임!

### Rx의 3요소

- Observable
- Operator
- Scheduler



## 🖌  Observable

이벤트를 비동기적으로 생성할 수 있는 대상. 계속해서 이벤트를 생성하는데 이러한 과정을 Emit이라 함.

이벤트들은 숫자나 커스텀한 인스턴스 등과 같은 값을 가질 수 있고, 탭과 같은 제스처일 수도 있음.

- **next**

  요소들을 계속해서 방출. Observable 구독자에게 데이터 전달.

- **Completed**

  요소들이 다 방출되면 이벤트 종료. Observable 구독자에게 완료되었음을 알림.

- **error**

  방출한 요소에 에러가 있음을 알고 중간에 종료. Observable 구독자에게 오류 알림.

### Observable 생성

- **just** : 오직 하나의 Element를 포함하는 Observable Sequence 생성

  ```swift
  let observable: Observable<Int> = Observable<Int>.just(1)
  ```

- **of** : 가변적인 element를 포함하는 Observable Sequence 생성

  ```swift
  let observable1 = Observable.of(1,2,3,4,5)	//배열 아님. Int 타입.
  let observable2 = Observable.of([1,2,3])	//인자가 배열일 때는 단일요소. <[Int]>
  ```

- **from** : 배열 요소들로 Observable Sequence 생성

  ```swift
  let observable = Observable.from([1,2,3,4,5])	//Int 타입. 
  ```

- **empty** : 요소를 가지지 않는 Observable. `.completed` 이벤트만 방출.

  ```swift
  let observable = Observable<Void>.empty() //Observable은 반드시 특정 타입이 정의되어야 하는데 타입 추론이 불가능하므로 Void.
  ```

- **never** : 이벤트 방출 조차 하지 않음.

  ```swift
  let observable = Observable<Any>.never()
  ```

- **range** : start부터 count 크기 만큼의 값을 갖는 Observable 생성.

  ```swift
  let observable = Observable<Int>.range(start: 2, count: 3) //2, 3, 4
  ```

- **repeatElement** : 지정된 element 계속 방출.

  ```swift
  let observable = Observable<Int>.repeatElement(2) //2, 2, 2 ..
  ```

- **interval** : 지정된 시간에 한번씩 이벤트 방출

  ```swift
  Observable<Int>.interval(2, scheduler: MainScheduler.instance) // 0, 1, 2, .. (2초마다 0부터 증가)
  ```

- **create** : Observer에 직접 이벤트 방출

  ``` swift
  Observable<Int>.create({ (observer) -> Disposable in 
      observer.onNext(2)
      observer.onCompleted()
      observer.onNext(3)
      return Disposables.create()
  }) // next(2), completed
  ```



### Observable 구독 - subscribe

Observable이 구독되어야지 이벤트를 보냄. subscribe 사용.

```swift
let observable = Observable.of(1, 2, 3)
observable.subscribe { event in 
    print(event) // next(1), next(2), next(3), completed
}
observable.subscribe(onNext: { element in 
    print(element) // 1, 2, 3
})
```



### Observable 구독 - drive

Traits. UI와 함께 사용되도록 독점적으로 생성된 observable 항목의 특수한 구현을 제공. 메인 스케줄러에서 관찰 구독. Driver, ControlProperty 등 포함.

### Observable 구독 취소 - Disposing

- **dispose**

  ```swift
  let subscription = observable.subscribe({ (event) in 
      print(event)
  }) // 만약 이벤트가 무한대가 있다면 일정 수준에서 dispose -> completed
  subscription.dispose()
  ```

- **DisposeBag** : dispose 반환 값 담는 객체

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(1,2,3)
      .subscribe {
          print($0)
      }
      .disposed(by: disposeBag)
  ```






## 🖌 Hot & Cold Observable

- Hot Observable : 생성 즉시 방출. 나중에 구독한 observer는 중간부터 시퀀스 관찰. 

  - 다른 구독자들에게 선택적 고려 가능 .. ?

  - 마우스, 키보드, 시스템 이벤트에 주로 사용

  - 멀티태스트 포함

    - Cold Observable을 Hot Observable로 바꾸기 위해서는 publish, share 오퍼레이터 사용. or Subject 사용.

      Subject는 Observable이면서 Observer. -> Observable처럼 구독할 수도 있고, Observer처럼 next, complete 메소드 직접 호출 가능.

  - 구독 이전에는 연산 자원을 소모하지 않음.

  - 스트림을 분기시키는 성질

- Cold Observable : observer가 구독할 때까지 대기. 처음부터 전체 시퀀스를 볼 수 있음.

  - 구독하는 시점부터 이벤트 생성하여 방출
  - 웹 요청, 데이터베이스 쿼리 등이 사용.
  - 스트림을 분기시키는 성질을 가지고 있지 않음. -> Cold Observable을 **여러번 구독하는 경우 각각 별도의 스트림이 생성**되고 할당됨.





## 🖌 Subject

observable과 observer의 역할을 모두 할 수 있는 bridge/proxy Observable. -> emit, subscribe 가능.

- Subject : multicast. 여러개의 observer를 subscribe 가능.

  state를 가지며 data를 메모리에 저장. 관찰자 세부 정보를 저장하고 코드를 **한 번만 **실행하여 모든 관찰자에게 결과 제공.

  observable을 생성하고 관찰할 수 있음 -> data producer, consumer.

  자주 데이터를 저장하고 수정할 때, 여러개의 observer가 데이터를 관찰해야 할 때, observer와 observable 사이 proxy 역할.

- observer : unicast. 하나의 observer만 subscribe. 

  하나의 함수로 어떠한 상태도 가지지 않음. 모든 새로운 Observer에 대해 관찰 가능한 create 코드를 반복 실행. 코드는 각 관찰자에 대해 실행되므로 HTTP 호출인 경우 각 관찰자에 대해 호출 -> 버그, 비효율

  하나의 옵저버에 대한 간단한 observable이 필요할 때.

[참고](https://sujinnaljin.medium.com/rxswift-subject-99b401e5d2e5)

- **Publish Subject**

  Element 없이 빈 상태로 생성. subscriber는 subscribe 한 시점 이후에 발생되는 이벤트만 전달 받음.

  ```swift
  let subject = PublishSubject<Int>()
  subject.onNext(1)
  subject.subscribe { event in 
      print(event)
  }
  subject.onNext(2)
  subject.onNext(3)
  subject.onCompleted()
  subject.onNext(4)
  // onNext(2), onNext(3), completed
  ```

- **Behavior Subject**

  subscribe가 발생하면 발생 시점 이전에 발생한 이벤트 중 가장 최신 이벤트를 전달 받음. -> 초기값 필요

  ```swift
  let subject = BehaviorSubject(value: "Inital Value")
  subject.onNext("Last Issue")
  subject.subscribe{ event in 
      print(event)
  }
  subject.onNext("1")
  //next(Last Issue), next(1)
  ```

- **Replay Subject**

  BufferSize와 함께 생성. Buffer Size 만큼의 최신 이벤트를 전달 받음.

  ```swift
  let subject = ReplaySubject<String>.create(bufferSize: 2)
  subject.onNext("1")
  subject.onNext("2")
  subject.onNext("3")
  subject.subscribe{ print($0) }	//next(2), next(3), next(4), next(5), next(6)
  subject.onNext("4")
  subject.onNext("5")
  subject.onNext("6")
  subject.subscribe{ print($0) } // next(5), next(6)
  ```

- **Variable** -> Deprecate. Behavior Relay 사용.

  BehaviorSubject의 Wrapper.

  ```swift
  let variable = Variable([String]())
  variable.value.append("1")
  variable.asObservable()
      .subscribe {
        print($0)
      }
  variable.value.append("2")
  //next(["1"]), next(["1","2"])
  ```



## 🖌 Relay

RxCocoa.

- **Behavior Relay**

  Behavior Subject의 Wrapper 클래스.

  ```swift
  let relay = BehaviorRelay(value: ["1"])
  var value = relay.value
  value.append("2")
  relay.accept(value)
  
  relay.asObservable()
      .subscribe {print($0)}
  //next(["1","2"])
  ```

  
## 🖌 Filtering Operators

- **IgnoreElements** : source observable에서 방출되는 요소는 무시하고 onError, onCompleted만 허용.

  ```swift
  let strikes = PublishSubject<String>()
  let disposeBag = DisposeBag()
  strikes
      .ignoreElements()
      .subscribe({ _ in
          print("subscription")
      })
      .disposed(by: disposeBag)
  strikes.onNext("A")
  strikes.onNext("B")
  strikes.onCompleted()
  //subscription
  ```

- **ElementAt** : source observable에서 방출되는 요소 중 n번재 요소만 받음.

  ```swift
  let strikes = PublishSubject<String>()
  let disposeBag = DisposeBag()
  strikes
      .elementAt(2)
      .subscribe(onNext: { _ in 
          print("out")
      })
      .disposed(by: disposeBag)
  strikes.onNext("X")
  strikes.onNext("X")
  strikes.onNext("X")
  // out (2번째 이벤트)
  ```

- **Filter**

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(1,2,3,4)
      .filter{ $0 % 2 == 0}
      .subscribe(onNext: {
        print($0)
      }).disposed(by: disposeBag)
  // 2, 4
  ```

- **Skip** : 처음 n개 요소는 skip.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of("A","B","C","D")
      .skip(2)
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  // C, D
  ```

- **SkipWhile** : 조건이 처음으로 false일 때부터 요소 받음.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(2,2,3,4,4)
      .skipWhile{ $0 % 2 == 0 }
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  //3, 4, 4
  ```

- **SkipUntil** : trigger에서 이벤트 발생까지 subject의 이벤트 skip

  ```swift
  let disposeBag = DisposeBag()
  let subject = PublishSubject<String>()
  let trigger = PublishSubject<String>()
  
  subject.skipUntil(trigger)
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  subject.onNext("A")
  subject.onNext("B")
  trigger.onNext("X")
  subject.onNext("C")
  //C
  ```

- **Take** : 처음 발생하는 n개의 이벤트만 받음.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(1,2,3,4)
      .take(3)
      .subscribe(onNext: {
          print("$0")
      }).disposed(by: disposeBag)
  //1,2,3
  ```

- **TakeWhile** : 조건이 처음으로 false일 때 전까지만 이벤트 받음.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(2,4,5,6,8)
      .takeWhile{ $0 % 2 == 0}
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  // 2, 4
  ```

- **TakeUntil** : trigger에서 이벤트 발생 전까지 subject 이벤트 받음.

  ```swift
  let disposeBag = DisposeBag()
  let subject = PublishSubject<String>()
  let trigger = PublishSubject<String>()
  subject.takeUntil(trigger)
      .subscribe(onNext: {
          print($0)
      }).disposed(by:disposeBag)
  subject.onNext("A")
  subject.onNext("B")
  trigger.onNext("X")
  subject.onNext("C")
  //A, B
  ```

  

## 🖌 Transforming Operators

- **ToArray** : 하나의 요소를 방출하는 Observable로 변환. 더이상 요소를 방출하지 않는 시점에 배열에 담아 전달.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(1,2,3)
      .toArray()
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  //[1,2,3]
  ```

  ```swift
  let subject = PublishSubject<Int>()
  subject.toArray()
      .subscribe{ print($0) }
      .disposed(by: disposeBag)
  subject.onNext(1)
  subject.onNext(2)
  subject.onNext(3)
  subject.onCompleted()
  //[1,2,3]
  ```

- **Map**

  ```swift
  Observable.of(1,2,3)
      .map{
          return $0*2
      }.subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  //2, 4, 6
  ```

  

- **Flat Map** : 이벤트를 다른 observable로 만들고 만들어진 observable에서 요소 방출. 하나의 sequence로 전달.

  ```swift
  let Student {
      var score: BehaviorRelay<Int>
  }
  let a = Student(score: BehaviorRelay(value: 75))
  let b = Student(score: BehaviorRelay(value: 95))
  
  let student = PublishSubject<Student>()
  student.asObservable()
      .flatMap{ $0.score.asObservable }
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  
  student.onNext(a)
  a.score.accept(100)
  student.onNext(b)
  b.score.accept(80)
  a.score.accept(40)
  // 75, 100, 95, 80, 40
  ```

  [참고](https://eunjin3786.tistory.com/41)

- **Flat Map Lastest** : 마지막 observable만 관찰.

  ```swift
  student.asObservable()
      .flatMapLastest{ $0.score.asObservable() }
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  
  student.onNext(a)
  a.score.accept(100)
  student.onNext(b)
  b.score.accept(80)
  a.score.accept(40)	//x
  //75, 100, 95, 80
  ```

  

## 🖌 Combining Operator

- **StartWith** : 요소들을 방출하기 전 특정 요소를 먼저 방출.

  ```swift
  let numbers = Observable.of(2, 3)
  let observable = numbers.startWith(1)
  observable.subscribe(onNext: {
      print($0)
  }).disposed(by: disposeBag)
  //1, 2, 3
  ```

- **Concat** : 두개의 observable 연결. 하나의 Observable 방출이 Completed되면 이어지는 Observable 방출.

  ```swift
  let first = Observable.of(1, 2)
  let second = Observable.of(3, 4)
  let observable = Observable.concat([first, second])
  observable.subscribe(onNext: {
      print($0)
  }).disposed(by: disposeBag)
  //1, 2, 3, 4
  ```

- **Merge** : 두 개 이상의 Observable을 병합하고 모든 Observable에서 방출하는 요소들을 순서대로 방출.

  ```swift
  let left = PublishSubject<Int>()
  let right = PublishSubject<Int>()
  let source = Observable.of(left.asObservable(), right.asObserable())
  let observable = source.merge()
  observable.subscribe(onNext: {
      print($0)
  }).disposed(by: disposeBag)
  
  left.onNext(4)
  left.onNext(2)
  right.onNext(8)
  right.onNext(5)
  left.onNext(1)
  // 4, 2, 8, 5, 1
  ```

- **CombineLatest** : 두 개의 sequence를 하나로 합침. subsequence에서 요소가 방출될 때마다 방출. 합쳐진 두 sequence는 요소를 조합하여 새로운 요소 방출.

  subsequence가 각각 최초 요소를 방출해야 합쳐진 sequence에서 요소 방출.

  ```swift
  let left = PublishSubject<Int>()
  let right = PublishSubject<Int>()
  let source = Observable.combineLatest(left, right, resultSelector: { lastLeft, lastRight in
      "\(lastLeft) \(lastRight)"
  })
  let disposable = observable.subscribe(onNext: { value in
      print(value)
  })
  
  left.onNext(45)
  right.onNext(1)
  left.onNext(30)
  right.onNext(99)
  right.onNext(2)
  // 45 1, 30 1, 30 99, 30 2
  ```

- **WithLastestFrom** : trigger가 방출을 했을 때 특정 상태의 최신 값을 얻고 싶을 때 사용

  ```swift
  let button = PublishSubject<Void>()
  let textfield = PublishSubject<String>()
  let observable = button.withLastestFrom(textfield)
  let disposable = observable.subscribe(onNext: {
      print($0)
  })
  textField.onNext("Sw")
  textField.onNext("Swif")
  textField.onNext("Swift ")
  textField.onNext("Swift Rocks!")
  
  button.onNext(())
  button.onNext(())
  //Swift Rocks!, Swift Rocks!
  ```

- **Reduce** 

  ```swift
  let source = Observable.of(1,2,3)
  source.reduce(0, accumulator: +)
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  // 6
  ```

  

- **Scan** : 이전에 방출된 요소의 값과 방출할 요소의 값을 합쳐 방출.

  ```swift
  let source = Observable.of(1,2,3)
  source.scan(0, accumulator: +)
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  //1, 3, 6
  ```

  
