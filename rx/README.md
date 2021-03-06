# πRx

## π Reactive Programming

λΉλκΈ°μ μΈ λ°μ΄ν° νλ¦(stream)μ μ²λ¦¬νλ κ². 

λ°μ΄ν°λ₯Ό κ΄μ°°νκ³  μλ€κ° λ³νκ° μκΈ°λ©΄ κ·Έμ λ°λ₯Έ λ°μ΄ν° νΉμ UI μλ°μ΄νΈ κ°μ μΌμ μννλ κ². -> λ°μ Reactive!

iOS SDKμμ Closure, Notification, Delegate λ±μ μ΄μ©νλ©΄ λΉλκΈ° μ²λ¦¬κ° κ°λ₯νμ§λ§ λ°μ΄ν° λ³νκ° μκΈ°λ©΄ UI μλ°μ΄νΈλ₯Ό λͺμμ μΌλ‘ νΈμΆν΄μΌ νκ±°λ μ½λ°± μ§μ₯μ΄ μκΈ°λ λ±μ λ¬Έμ  μμλ€μ΄ μμ. μ΄λ Reactive Extension, Rxλ₯Ό μ¬μ©νλ©΄ μΌκ΄μ± μλ λΉλκΈ° μ½λ μμ±μ΄ κ°λ₯νκ³  μ€λ λ μ²λ¦¬κ° μ¬μμ§λ μ΄μ μ΄ μμ. 

Reactive Programmingμ νλμ ν¨λ¬λ€μμΌ λΏμ΄λ―λ‘ Rxλ₯Ό μ¬μ©νμ§ μμλ reactiveνκ² νλ‘κ·Έλ¨μ κ΅¬νν  μ μμ. Rxλ νΈνκ³  κ°λ¨νκ² κ΅¬νν  μ μλλ‘ λλ λκ΅¬μ!

### Rxμ 3μμ

- Observable
- Operator
- Scheduler



## π  Observable

μ΄λ²€νΈλ₯Ό λΉλκΈ°μ μΌλ‘ μμ±ν  μ μλ λμ. κ³μν΄μ μ΄λ²€νΈλ₯Ό μμ±νλλ° μ΄λ¬ν κ³Όμ μ Emitμ΄λΌ ν¨.

μ΄λ²€νΈλ€μ μ«μλ μ»€μ€νν μΈμ€ν΄μ€ λ±κ³Ό κ°μ κ°μ κ°μ§ μ μκ³ , ν­κ³Ό κ°μ μ μ€μ²μΌ μλ μμ.

- **next**

  μμλ€μ κ³μν΄μ λ°©μΆ. Observable κ΅¬λμμκ² λ°μ΄ν° μ λ¬.

- **Completed**

  μμλ€μ΄ λ€ λ°©μΆλλ©΄ μ΄λ²€νΈ μ’λ£. Observable κ΅¬λμμκ² μλ£λμμμ μλ¦Ό.

- **error**

  λ°©μΆν μμμ μλ¬κ° μμμ μκ³  μ€κ°μ μ’λ£. Observable κ΅¬λμμκ² μ€λ₯ μλ¦Ό.

### Observable μμ±

- **just** : μ€μ§ νλμ Elementλ₯Ό ν¬ν¨νλ Observable Sequence μμ±

  ```swift
  let observable: Observable<Int> = Observable<Int>.just(1)
  ```

- **of** : κ°λ³μ μΈ elementλ₯Ό ν¬ν¨νλ Observable Sequence μμ±

  ```swift
  let observable1 = Observable.of(1,2,3,4,5)	//λ°°μ΄ μλ. Int νμ.
  let observable2 = Observable.of([1,2,3])	//μΈμκ° λ°°μ΄μΌ λλ λ¨μΌμμ. <[Int]>
  ```

- **from** : λ°°μ΄ μμλ€λ‘ Observable Sequence μμ±

  ```swift
  let observable = Observable.from([1,2,3,4,5])	//Int νμ. 
  ```

- **empty** : μμλ₯Ό κ°μ§μ§ μλ Observable. `.completed` μ΄λ²€νΈλ§ λ°©μΆ.

  ```swift
  let observable = Observable<Void>.empty() //Observableμ λ°λμ νΉμ  νμμ΄ μ μλμ΄μΌ νλλ° νμ μΆλ‘ μ΄ λΆκ°λ₯νλ―λ‘ Void.
  ```

- **never** : μ΄λ²€νΈ λ°©μΆ μ‘°μ°¨ νμ§ μμ.

  ```swift
  let observable = Observable<Any>.never()
  ```

- **range** : startλΆν° count ν¬κΈ° λ§νΌμ κ°μ κ°λ Observable μμ±.

  ```swift
  let observable = Observable<Int>.range(start: 2, count: 3) //2, 3, 4
  ```

- **repeatElement** : μ§μ λ element κ³μ λ°©μΆ.

  ```swift
  let observable = Observable<Int>.repeatElement(2) //2, 2, 2 ..
  ```

- **interval** : μ§μ λ μκ°μ νλ²μ© μ΄λ²€νΈ λ°©μΆ

  ```swift
  Observable<Int>.interval(2, scheduler: MainScheduler.instance) // 0, 1, 2, .. (2μ΄λ§λ€ 0λΆν° μ¦κ°)
  ```

- **create** : Observerμ μ§μ  μ΄λ²€νΈ λ°©μΆ

  ``` swift
  Observable<Int>.create({ (observer) -> Disposable in 
      observer.onNext(2)
      observer.onCompleted()
      observer.onNext(3)
      return Disposables.create()
  }) // next(2), completed
  ```



### Observable κ΅¬λ - subscribe

Observableμ΄ κ΅¬λλμ΄μΌμ§ μ΄λ²€νΈλ₯Ό λ³΄λ. subscribe μ¬μ©.

```swift
let observable = Observable.of(1, 2, 3)
observable.subscribe { event in 
    print(event) // next(1), next(2), next(3), completed
}
observable.subscribe(onNext: { element in 
    print(element) // 1, 2, 3
})
```



### Observable κ΅¬λ - drive

Traits. UIμ ν¨κ» μ¬μ©λλλ‘ λμ μ μΌλ‘ μμ±λ observable ν­λͺ©μ νΉμν κ΅¬νμ μ κ³΅. λ©μΈ μ€μΌμ€λ¬μμ κ΄μ°° κ΅¬λ. Driver, ControlProperty λ± ν¬ν¨.

### Observable κ΅¬λ μ·¨μ - Disposing

- **dispose**

  ```swift
  let subscription = observable.subscribe({ (event) in 
      print(event)
  }) // λ§μ½ μ΄λ²€νΈκ° λ¬΄νλκ° μλ€λ©΄ μΌμ  μμ€μμ dispose -> completed
  subscription.dispose()
  ```

- **DisposeBag** : dispose λ°ν κ° λ΄λ κ°μ²΄

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(1,2,3)
      .subscribe {
          print($0)
      }
      .disposed(by: disposeBag)
  ```






## π Hot & Cold Observable

- Hot Observable : μμ± μ¦μ λ°©μΆ. λμ€μ κ΅¬λν observerλ μ€κ°λΆν° μνμ€ κ΄μ°°. 

  - λ€λ₯Έ κ΅¬λμλ€μκ² μ νμ  κ³ λ € κ°λ₯ .. ?

  - λ§μ°μ€, ν€λ³΄λ, μμ€ν μ΄λ²€νΈμ μ£Όλ‘ μ¬μ©

  - λ©ν°νμ€νΈ ν¬ν¨

    - Cold Observableμ Hot Observableλ‘ λ°κΎΈκΈ° μν΄μλ publish, share μ€νΌλ μ΄ν° μ¬μ©. or Subject μ¬μ©.

      Subjectλ Observableμ΄λ©΄μ Observer. -> Observableμ²λΌ κ΅¬λν  μλ μκ³ , Observerμ²λΌ next, complete λ©μλ μ§μ  νΈμΆ κ°λ₯.

  - κ΅¬λ μ΄μ μλ μ°μ° μμμ μλͺ¨νμ§ μμ.

  - μ€νΈλ¦Όμ λΆκΈ°μν€λ μ±μ§

- Cold Observable : observerκ° κ΅¬λν  λκΉμ§ λκΈ°. μ²μλΆν° μ μ²΄ μνμ€λ₯Ό λ³Ό μ μμ.

  - κ΅¬λνλ μμ λΆν° μ΄λ²€νΈ μμ±νμ¬ λ°©μΆ
  - μΉ μμ²­, λ°μ΄ν°λ² μ΄μ€ μΏΌλ¦¬ λ±μ΄ μ¬μ©.
  - μ€νΈλ¦Όμ λΆκΈ°μν€λ μ±μ§μ κ°μ§κ³  μμ§ μμ. -> Cold Observableμ **μ¬λ¬λ² κ΅¬λνλ κ²½μ° κ°κ° λ³λμ μ€νΈλ¦Όμ΄ μμ±**λκ³  ν λΉλ¨.





## π Subject

observableκ³Ό observerμ μ­ν μ λͺ¨λ ν  μ μλ bridge/proxy Observable. -> emit, subscribe κ°λ₯.

- Subject : multicast. μ¬λ¬κ°μ observerλ₯Ό subscribe κ°λ₯.

  stateλ₯Ό κ°μ§λ©° dataλ₯Ό λ©λͺ¨λ¦¬μ μ μ₯. κ΄μ°°μ μΈλΆ μ λ³΄λ₯Ό μ μ₯νκ³  μ½λλ₯Ό **ν λ²λ§ **μ€ννμ¬ λͺ¨λ  κ΄μ°°μμκ² κ²°κ³Ό μ κ³΅.

  observableμ μμ±νκ³  κ΄μ°°ν  μ μμ -> data producer, consumer.

  μμ£Ό λ°μ΄ν°λ₯Ό μ μ₯νκ³  μμ ν  λ, μ¬λ¬κ°μ observerκ° λ°μ΄ν°λ₯Ό κ΄μ°°ν΄μΌ ν  λ, observerμ observable μ¬μ΄ proxy μ­ν .

- observer : unicast. νλμ observerλ§ subscribe. 

  νλμ ν¨μλ‘ μ΄λ ν μνλ κ°μ§μ§ μμ. λͺ¨λ  μλ‘μ΄ Observerμ λν΄ κ΄μ°° κ°λ₯ν create μ½λλ₯Ό λ°λ³΅ μ€ν. μ½λλ κ° κ΄μ°°μμ λν΄ μ€νλλ―λ‘ HTTP νΈμΆμΈ κ²½μ° κ° κ΄μ°°μμ λν΄ νΈμΆ -> λ²κ·Έ, λΉν¨μ¨

  νλμ μ΅μ λ²μ λν κ°λ¨ν observableμ΄ νμν  λ.

[μ°Έκ³ ](https://sujinnaljin.medium.com/rxswift-subject-99b401e5d2e5)

- **Publish Subject**

  Element μμ΄ λΉ μνλ‘ μμ±. subscriberλ subscribe ν μμ  μ΄νμ λ°μλλ μ΄λ²€νΈλ§ μ λ¬ λ°μ.

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

  subscribeκ° λ°μνλ©΄ λ°μ μμ  μ΄μ μ λ°μν μ΄λ²€νΈ μ€ κ°μ₯ μ΅μ  μ΄λ²€νΈλ₯Ό μ λ¬ λ°μ. -> μ΄κΈ°κ° νμ

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

  BufferSizeμ ν¨κ» μμ±. Buffer Size λ§νΌμ μ΅μ  μ΄λ²€νΈλ₯Ό μ λ¬ λ°μ.

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

- **Variable** -> Deprecate. Behavior Relay μ¬μ©.

  BehaviorSubjectμ Wrapper.

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



## π Relay

RxCocoa.

- **Behavior Relay**

  Behavior Subjectμ Wrapper ν΄λμ€.

  ```swift
  let relay = BehaviorRelay(value: ["1"])
  var value = relay.value
  value.append("2")
  relay.accept(value)
  
  relay.asObservable()
      .subscribe {print($0)}
  //next(["1","2"])
  ```

  
## π Filtering Operators

- **IgnoreElements** : source observableμμ λ°©μΆλλ μμλ λ¬΄μνκ³  onError, onCompletedλ§ νμ©.

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

- **ElementAt** : source observableμμ λ°©μΆλλ μμ μ€ nλ²μ¬ μμλ§ λ°μ.

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
  // out (2λ²μ§Έ μ΄λ²€νΈ)
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

- **Skip** : μ²μ nκ° μμλ skip.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of("A","B","C","D")
      .skip(2)
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  // C, D
  ```

- **SkipWhile** : μ‘°κ±΄μ΄ μ²μμΌλ‘ falseμΌ λλΆν° μμ λ°μ.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(2,2,3,4,4)
      .skipWhile{ $0 % 2 == 0 }
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  //3, 4, 4
  ```

- **SkipUntil** : triggerμμ μ΄λ²€νΈ λ°μκΉμ§ subjectμ μ΄λ²€νΈ skip

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

- **Take** : μ²μ λ°μνλ nκ°μ μ΄λ²€νΈλ§ λ°μ.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(1,2,3,4)
      .take(3)
      .subscribe(onNext: {
          print("$0")
      }).disposed(by: disposeBag)
  //1,2,3
  ```

- **TakeWhile** : μ‘°κ±΄μ΄ μ²μμΌλ‘ falseμΌ λ μ κΉμ§λ§ μ΄λ²€νΈ λ°μ.

  ```swift
  let disposeBag = DisposeBag()
  Observable.of(2,4,5,6,8)
      .takeWhile{ $0 % 2 == 0}
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  // 2, 4
  ```

- **TakeUntil** : triggerμμ μ΄λ²€νΈ λ°μ μ κΉμ§ subject μ΄λ²€νΈ λ°μ.

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

  

## π Transforming Operators

- **ToArray** : νλμ μμλ₯Ό λ°©μΆνλ Observableλ‘ λ³ν. λμ΄μ μμλ₯Ό λ°©μΆνμ§ μλ μμ μ λ°°μ΄μ λ΄μ μ λ¬.

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

  

- **Flat Map** : μ΄λ²€νΈλ₯Ό λ€λ₯Έ observableλ‘ λ§λ€κ³  λ§λ€μ΄μ§ observableμμ μμ λ°©μΆ. νλμ sequenceλ‘ μ λ¬.

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

  [μ°Έκ³ ](https://eunjin3786.tistory.com/41)

- **Flat Map Lastest** : λ§μ§λ§ observableλ§ κ΄μ°°.

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

  

## π Combining Operator

- **StartWith** : μμλ€μ λ°©μΆνκΈ° μ  νΉμ  μμλ₯Ό λ¨Όμ  λ°©μΆ.

  ```swift
  let numbers = Observable.of(2, 3)
  let observable = numbers.startWith(1)
  observable.subscribe(onNext: {
      print($0)
  }).disposed(by: disposeBag)
  //1, 2, 3
  ```

- **Concat** : λκ°μ observable μ°κ²°. νλμ Observable λ°©μΆμ΄ Completedλλ©΄ μ΄μ΄μ§λ Observable λ°©μΆ.

  ```swift
  let first = Observable.of(1, 2)
  let second = Observable.of(3, 4)
  let observable = Observable.concat([first, second])
  observable.subscribe(onNext: {
      print($0)
  }).disposed(by: disposeBag)
  //1, 2, 3, 4
  ```

- **Merge** : λ κ° μ΄μμ Observableμ λ³ν©νκ³  λͺ¨λ  Observableμμ λ°©μΆνλ μμλ€μ μμλλ‘ λ°©μΆ.

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

- **CombineLatest** : λ κ°μ sequenceλ₯Ό νλλ‘ ν©μΉ¨. subsequenceμμ μμκ° λ°©μΆλ  λλ§λ€ λ°©μΆ. ν©μ³μ§ λ sequenceλ μμλ₯Ό μ‘°ν©νμ¬ μλ‘μ΄ μμ λ°©μΆ.

  subsequenceκ° κ°κ° μ΅μ΄ μμλ₯Ό λ°©μΆν΄μΌ ν©μ³μ§ sequenceμμ μμ λ°©μΆ.

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

- **WithLastestFrom** : triggerκ° λ°©μΆμ νμ λ νΉμ  μνμ μ΅μ  κ°μ μ»κ³  μΆμ λ μ¬μ©

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

  

- **Scan** : μ΄μ μ λ°©μΆλ μμμ κ°κ³Ό λ°©μΆν  μμμ κ°μ ν©μ³ λ°©μΆ.

  ```swift
  let source = Observable.of(1,2,3)
  source.scan(0, accumulator: +)
      .subscribe(onNext: {
          print($0)
      }).disposed(by: disposeBag)
  //1, 3, 6
  ```

  
