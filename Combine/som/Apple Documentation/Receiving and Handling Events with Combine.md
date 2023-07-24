# ****Receiving and Handling Events with Combine****

비동기 소스에서 이벤트를 사용자 지정하고 수신한다.

# ****Overview****

Combine 프레임워크는 앱이 이벤트를 처리하는 방법에 대한 선언적 접근 방식을 제공한다. 잠재적으로 여러 delegate callbacks 또는 handler closures를 구현하는 대신 지정된 이벤트 소스에 대한 단일 처리 체인을 만들 수 있다. 체인의 각 부분은 이전 단계에서 받은 요소에 대해 고유한 작업을 수행하는 Combine 연산자이다.

텍스트 필드의 내용을 기반으로 테이블 또는 컬렉션 뷰를 필터링해야 하는 앱을 고려해보자. AppKit에서 텍스트 필드의 각 키 입력은 Combine으로 구독할 수 있는 [`Notification`](https://developer.apple.com/documentation/foundation/notification)을 생성한다. 알림을 받은 후 연산자를 사용하여 이벤트 전달의 내용과 타이밍을 변경하고 최종 결과를 사용하여 앱의 사용자 인터페이스를 업데이트할 수 있다. 

## ****Connect a Publisher to a Subscriber****

Combine으로 텍스트 필드의 알림을 받으려면 [`NotificationCenter`](https://developer.apple.com/documentation/foundation/notificationcenter)의 기본 인스턴스에 접근하고 [`publisher(for:object:)`](https://developer.apple.com/documentation/foundation/notificationcenter/3329353-publisher) 메서드를 호출한다. 이 호출은 알림을 원하는 알림 이름과 소스 개체를 가져오고 알림 요소를 생성하는 Publisher를 반환한다. 

```swift
let pub = NotificationCenter.default
    .publisher(for: NSControl.textDidChangeNotification, object: filterField)
```

[`Subscriber`](https://developer.apple.com/documentation/combine/subscriber)를 사용하여 Publisher로부터 요소를 수신한다. Subscriber는 연결된 타입은 [`Input`](https://developer.apple.com/documentation/combine/subscriber/input)을 정의하여 수신하는 타입을 선언한다. Publisher는 또한 생성하는 항목을 선언하기 위해 `[Output](https://developer.apple.com/documentation/combine/publisher/output)` 타입을 정의한다. Publisher와 Subscriber는 모두 `[Failure](https://developer.apple.com/documentation/combine/publisher/failure)` 타입을 정의하여 생성하거나 수신하는 오류의 종류를 나타낸다. Subscriber를 생산자에 연결하려면 `[Output](https://developer.apple.com/documentation/combine/publisher/output)`이 `[Input](https://developer.apple.com/documentation/combine/subscriber/input)`과 일치해야 하며 `[Failure](https://developer.apple.com/documentation/combine/publisher/failure)` 타입도 일치해야 한다. 

Combine은 연결된 게시자의 출력 및 실패 유형을 자동으로 일치시키는 두 개의 기본 제공 구독자를 제공한다.

- [`sink(receiveCompletion:receiveValue:)`](https://developer.apple.com/documentation/combine/publisher/sink(receivecompletion:receivevalue:))는 2개의 클로저를 취한다. 첫번째 클로저는 [`Subscribers.Completion`](https://developer.apple.com/documentation/combine/subscribers/completion)을 수신할 때 실행되며 이는 게시자가 정상적으로 완료되었는지 또는 오류로 인해 실패했는지를 나타내는 열거형이다. 두 번째 클로저는 Publisher로부터 요소를 수신할 때 실행된다.
- [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:))은 프로퍼티를 나타내는 키 경로를 사용하여 주어진 객체의 프로퍼티에 수신하는 모든 요소를 즉시 할당한다.

예를 들어 Sink Subscriber를 사용하여 Publisher가 완료할 때와 요소를 받을 때마다 기록할 수 있다. 

```swift
let sub = NotificationCenter.default
    .publisher(for: NSControl.textDidChangeNotification, object: filterField)
    .sink(receiveCompletion: { print ($0) },
          receiveValue: { print ($0) })
```

`sink(receiveCompletion:receiveValue:)` 및 [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:)) Subscriber 모두 Publisher에게 무제한의 요소를 요청한다. 요소를 수신하는 속도를 제어하려면 [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) 프로토콜을 구현하여 고유한 Subscriber를 생성하라. 

## ****Change the Output Type with Operators****

이전 섹션의 Sink Subscriber는 receiveValue 클로저에서 모든 작업을 수행한다. 수신된 요소로 많은 사용자 지정 작업을 수행하거나 호출 간에 상태를 유지해야 하는 경우 부담이 될 수 있다. Combine의 이점은 연산자를 결합하여 이벤트 전달을 사용자 정의하는 데 있다. 

예를 들어, [`NotificationCenter.Publisher.Output`](https://developer.apple.com/documentation/foundation/notificationcenter/publisher/output)은 텍스트 필드의 문자열 값만 필요한 경우 콜백에서 수신하기에 편리한 타입이 아니다. Publisher의 출력은 본질적으로 시간이 지남에 따라 요소의 시퀀스이므로 Combine은 [`map(_:)`](https://developer.apple.com/documentation/combine/publisher/map(_:)-99evh), [`flatMap(maxPublishers:_:)`](https://developer.apple.com/documentation/combine/publisher/flatmap(maxpublishers:_:)-3k7z5) 및 [`reduce(_:_:)`](https://developer.apple.com/documentation/combine/publisher/reduce(_:_:))와 같은 시퀀스 수정 연산자를 제공한다. 이러한 연산자의 동작은 Swift 표준 라이브러리의 해당 항목과 유사하다.

Publisher의 출력 타입을 변경하려면 클로저가 다른 타입을 반환하는 [`map(_:)`](https://developer.apple.com/documentation/combine/publisher/map(_:)-99evh) 연산자를 추가한다. 이 경우 알림 객체를 [`NSTextField`](https://developer.apple.com/documentation/appkit/nstextfield)로 가져온 다음 필드의 [`stringValue`](https://developer.apple.com/documentation/appkit/nscontrol/1428950-stringvalue)를 가져올 수 있다. 

```swift
let sub = NotificationCenter.default
    .publisher(for: NSControl.textDidChangeNotification, object: filterField)
    .map( { ($0.object as! NSTextField).stringValue } )
    .sink(receiveCompletion: { print ($0) },
          receiveValue: { print ($0) })
```

Publisher 체인이 원하는 타입을 생성한 후 `sink(receiveCompletion:receiveValue:)`를 [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:))으로 바꾼다. 다음 예제는 Publisher 체인에서 받은 문자열을 가져와 사용자 지정 뷰 모델 개체의 `filterString`에 할당한다.

```swift
let sub = NotificationCenter.default
    .publisher(for: NSControl.textDidChangeNotification, object: filterField)
    .map( { ($0.object as! NSTextField).stringValue } )
    .assign(to: \MyViewModel.filterString, on: myViewModel)
```

## ****Customize Publishers with Operators****

수동으로 코딩해야 하는 작업을 수행하는 연산자를 사용하여 [`Publisher`](https://developer.apple.com/documentation/combine/publisher) 인스턴스를 확장할 수 있다. 다음은 연산자를 사용하여 이 이벤트 처리 체인을 개선할 수 있는 세 가지 방법이다.

- 텍스트 필드에 입력된 문자열로 뷰 모델을 업데이트하는 대신 [`filter(_:)`](https://developer.apple.com/documentation/combine/publisher/filter(_:)) 연산자를 사용하여 특정 길이 미만의 입력을 무시하거나 영숫자가 아닌 문자를 거부할 수 있다.
- 필터링 작업에 비용이 많이 드는 경우(예: 대규모 데이터 베이스를 쿼리하는 경우) 사용자가 입력을 중지할 때까지 기다려야 할 수 있다. 이를 위해 [`debounce(for:scheduler:options:)`](https://developer.apple.com/documentation/combine/publisher/debounce(for:scheduler:options:)) 연산자를 사용하면 가 이벤트를 생성하기 전에 경과해야 하는 최소 시간을 설정할 수 있다. [`RunLoop`](https://developer.apple.com/documentation/foundation/runloop) 클래스는 시간 지연을 초 또는 밀리초 단위로 지정하기 위한 편의를 제공한다.
- 결과가 UI를 업데이트하면 [`receive(on:options:)`](https://developer.apple.com/documentation/combine/publisher/receive(on:options:)) 메서드를 호출하여 기본 스레드에 콜백을 전달할 수 있다. [`RunLoop`](https://developer.apple.com/documentation/foundation/runloop) 클래스에서 제공하는 스케줄러 인스턴스를 첫 번째 매개변수로 지정하여 Combine에게 메인 런 루프에서 호출하도록 지시한다.

결과 Publisher 선언은 다음과 같다.

```swift
let sub = NotificationCenter.default
    .publisher(for: NSControl.textDidChangeNotification, object: filterField)
    .map( { ($0.object as! NSTextField).stringValue } )
    .filter( { $0.unicodeScalars.allSatisfy({CharacterSet.alphanumerics.contains($0)}) } )
    .debounce(for: .milliseconds(500), scheduler: RunLoop.main)
    .receive(on: RunLoop.main)
    .assign(to:\MyViewModel.filterString, on: myViewModel)
```

## ****Cancel Publishing when Desired****

Publisher는 정상적으로 완료되거나 실패할 때까지 요소를 계속 내보낸다. Publisher를 더 이상 구독하지 않으려면 구독을 취소할 수 있다. `sink(receiveCompletion:receiveValue:)` 및 [`assign(to:on:)`](https://developer.apple.com/documentation/combine/publisher/assign(to:on:))에 의해 생성된 타입은 모두 [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) 메서드를 제공하는 [`Cancellable`](https://developer.apple.com/documentation/combine/cancellable) 프로토콜을 구현한다. 

```swift
sub?.cancel()
```

사용자 지정 [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber)를 생성하는 경우 Publisher는 [`Subscription`](https://developer.apple.com/documentation/combine/subscription) 개체를 처음 구독할 때 구독 개체를 보낸다. 이 구독을 저장한 다음 게시를 취소하려면 해당 [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) 메서드를 호출하라. 사용자 지정 를 만들 때 [`Cancellable`](https://developer.apple.com/documentation/combine/cancellable) 프로토콜을 구현하고, [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel()) 구현이 저장된 구독에 대한 호출을 전달하도록 해야 한다. 

# 참고 자료
- [Apple Documentation - Receiving and Handling Events with Combine](https://developer.apple.com/documentation/combine/receiving-and-handling-events-with-combine)
