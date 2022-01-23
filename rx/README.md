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
