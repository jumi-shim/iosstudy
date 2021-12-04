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

## enum
열거형. 연관된 항목들을 묶어서 표현할 수 있는 타입. 타입 분류!

항목별로 값을 가질수도, 가지지 않을 수도 있음.

extension 이용 가능
