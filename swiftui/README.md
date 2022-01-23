# 📍SwiftUI

## 🖌 ObservableObject
- **Protocol ObservableObject**

  ObservableObject 프로토콜을 채택한 클래스의 인스턴스를 관찰하고 있다가 값이 변경되면 뷰를 업데이트 함.

- **@Published**

  ObservableObject에서 속성을 선언할 때 사용하는 PropertyWrapper.

  @Published로 선언된 속성이 ObservableObject에 포함되어 있다면 해당 속성이 업데이트 될 때마다 뷰 업데이트.

- **@ObservedObject**

  ObservableObject를 구독하고 값이 업데이트 될 때마다 뷰를 갱신하는 PropertyWrapper.



## 🖌 @state, @binding

- **@state**

  struct 안에서는 변수를 바꾸는 것이 안 됨. -> @state 이용

  state 변수는 해당 view 안에서만 사용해야 함 -> private 이용.

  해당 view 안에서 property를 전달하고 싶으면 prefix operator $를 이용하여 바인딩.

- **@binding**

  데이터를 저장하는 property와 데이터를 표시하고 변경하는 뷰 사이에 양방향 연결 만듦.

  직접 데이터를 저장하는 대신에 property를 source of truth stored elsewhere에 연결.
