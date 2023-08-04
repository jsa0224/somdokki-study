# ****Controlling Publishing with Connectable Publishers****

Publisher가 Subscriber에게 요소를 보내기 시작하는 시기를 조정한다.

# Overview

경우에 따라 Publisher가 해당 동작에 영향을 미치는 프로퍼티가 있는 경우와 같이 요소 생성을 시작하기 전에 Publisher를 구성하려고 한다.
그러나 [`sink(receiveValue:)`](https://developer.apple.com/documentation/combine/publisher/sink(receivevalue:))와 같이 일반적으로 사용되는 Subscriber는 무제한 요소를 즉시 요구하므로 Publisher를 원하는 방식으로 설정하지 못할 수 있다. 
준비가 되기 전에 값을 생성하는 Publisher는 Publisher에 두 명 이상의 Subscriber가 있는 경우에는 문제가 될 수 있다. 
이 다중 Multi-Subscriber 시나리오는 race condition을 생성한다. 
Publisher는 두 번째 Subscriber 존재하기 전에 첫 번째 Subscriber에게 요소를 보낼 수 있다. 

다음 그림의 시나리오를 고려하라.
[`URLSession.DataTaskPublisher`](https://developer.apple.com/documentation/foundation/urlsession/datataskpublisher)를 생성하고 여기에 Sink Subscriber(Subscriber 1)를 연결하면 데이터 작업이 URL의 데이터를 가져오기 시작한다. 
나중에 두 번째 Subscriber(Subscriber 2)를 연결한다. 
두 번째 Subscriber가 연결하기 전에 데이터 작업이 다운로드를 완료하면 두 번째 Subscriber는 데이터를 놓치고 완료만 볼 수 있다.

![image](https://github.com/jsa0224/somdokki-study/assets/94514250/e15e84da-5ce3-4e65-9612-6e53dd4c42a0)

## ****Hold Publishing by Using a Connectable Publisher****

준비가 되기 전에 Publisher가 요소를 보내는 것을 방지하기 위해 Combine은 [ConnectablePublisher](https://www.notion.so/ConnectablePublisher-9642d76429cc463c81e2e86ed28c543f?pvs=21) 프로토콜을 제공한다. 
Connectable Publisher는 해당 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21) 메서드를 호출할 때까지 요소를 생성하지 않는다. 
요소를 생성할 준비가 되어 있고 수요가 충족되지 않은 경우에도 Connectable Publisher는 명시적으로 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21)를 호출할 때까지 Subscriber에게 요소를 전달하지 않는다. 

다음 그림은 위의 [`URLSession.DataTaskPublisher`](https://developer.apple.com/documentation/foundation/urlsession/datataskpublisher) 시나리오를 보여주지만 앞에 [ConnectablePublisher](https://www.notion.so/ConnectablePublisher-9642d76429cc463c81e2e86ed28c543f?pvs=21) 가 있다. 
두 Subscriber가 연결될 때까지 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21) 호출을 기다리면 데이터 작업은 그때까지 다운로드를 시작하지 않는다. 
이렇게 하면 race condition이 제거되고 두 Subscriber가 모두 데이터를 수신할 수 있다.

![image](https://github.com/jsa0224/somdokki-study/assets/94514250/d8d1586d-c4e9-4f6d-8119-93ec5e153080)

자체 Combine 코드에서 [ConnectablePublisher](https://www.notion.so/ConnectablePublisher-9642d76429cc463c81e2e86ed28c543f?pvs=21) 를 사용하려면 [`makeConnectable()`](https://developer.apple.com/documentation/combine/publisher/makeconnectable()) 연산자를 사용하여 기존 게시자를 인스턴스로 래핑한다. 
다음 코드는 이 위에서 설명한 데이터 작업 Publisher race condition을 수정하는 방법을 보여준다. 
일반적으로 Sink(여기서 반환하는 [AnyCancellable](https://www.notion.so/AnyCancellable-0f01ee8e300c43a7ae3dc3ca1ff94058?pvs=21)로 식별되는 `cancellable1`)를 연결하면 데이터 작업이 즉시 시작된다. 
이 시나리오에서 `cancelable2`로 식별된 두 번째 Sink는 1초 후까지 연결되지 않으며 데이터 작업 Publisher는 두 번째 Sink가 연결되기 전에 완료될 수 있다. 
대신 명시적으로 [ConnectablePublisher](https://www.notion.so/ConnectablePublisher-9642d76429cc463c81e2e86ed28c543f?pvs=21) 를 사용하면 앱이 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21)를 호출한 후에만 데이터 작업이 시작된다. 
이 작업 2초 지연 후에 수행된다. 

```swift
let url = URL(string: "https://example.com/")!
let connectable = URLSession.shared
    .dataTaskPublisher(for: url)
    .map() { $0.data }
    .catch() { _ in Just(Data() )}
    .share()
    .makeConnectable()

cancellable1 = connectable
    .sink(receiveCompletion: { print("Received completion 1: \($0).") },
          receiveValue: { print("Received data 1: \($0.count) bytes.") })

DispatchQueue.main.asyncAfter(deadline: .now() + 1) {
    self.cancellable2 = connectable
        .sink(receiveCompletion: { print("Received completion 2: \($0).") },
              receiveValue: { print("Received data 2: \($0.count) bytes.") })
}

DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
    self.connection = connectable.connect()
}
```

💡 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21)는 유지해야 하는 [Cancellable](https://www.notion.so/Cancellable-44842f71a8104896b71fe8ab809dbbd5?pvs=21) 인스턴스를 반환한다.
이 인스턴스를 사용하여 명시적으로 [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel())를 호출하거나 초기화 해제를 허용하여 게시를 취소할 수 있다.

## ****Use the Autoconnect Operator If You Don’t Need to Explicitly Connect****

일부 Combine Publisher는 [`Publishers.Multicast`](https://developer.apple.com/documentation/combine/publishers/multicast) 및 [`Timer.TimerPublisher`](https://developer.apple.com/documentation/foundation/timer/timerpublisher)와 같은 [ConnectablePublisher](https://www.notion.so/ConnectablePublisher-9642d76429cc463c81e2e86ed28c543f?pvs=21)를 이미 구현하고 있다. 
이러한 Publisher를 사용하면 정반대의 문제가 발생할 수 있다. 
Publisher를 구성하거나 여러 Subsciber를 연결할 필요가 없는 경우 명시적으로 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21)해야 하는 것이 부담스러울 수 있다. 

이와 같은 경우 [ConnectablePublisher](https://www.notion.so/ConnectablePublisher-9642d76429cc463c81e2e86ed28c543f?pvs=21)는 [autoconnect()](https://www.notion.so/autoconnect-94da3622b22b42508f903a311d0d554f?pvs=21) 연산자를 제공한다. 
이 연산자는 Subscriber가 [`subscribe(_:)`](https://developer.apple.com/documentation/combine/publisher/subscribe(_:)-3fk20) 메서드를 사용하여 Publisher에 연결할 때 즉시 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21)를 호출한다.

다음 예제에서는 [autoconnect()](https://www.notion.so/autoconnect-94da3622b22b42508f903a311d0d554f?pvs=21)를 사용하므로 Subscriber는 1초에 한 번씩 [`Timer.TimerPublisher`](https://developer.apple.com/documentation/foundation/timer/timerpublisher)에서 요소를 즉시 수신한다. 
[autoconnect()](https://www.notion.so/autoconnect-94da3622b22b42508f903a311d0d554f?pvs=21)가 없으면 예제는 특정 지점에서 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21)를 호출하여 타이머 Publisher를 명시적으로 시작해야 한다. 

```swift
let cancellable = Timer.publish(every: 1, on: .main, in: .default)
    .autoconnect()
    .sink() { date in
        print ("Date now: \(date)")
     }
```

# 참고 문서
- [Apple Documentation - Controlling Publishing with Connectable Publishers](https://developer.apple.com/documentation/combine/controlling-publishing-with-connectable-publishers#Use-the-Autoconnect-Operator-If-You-Dont-Need-to-Explicitly-Connect)
