# ****autoconnect()****

이 연결 가능한 Publisher와의 연결 또는 연결 해제 프로세스를 자동화한다.
![스크린샷 2023-08-04 오전 12 59 47](https://github.com/jsa0224/somdokki-study/assets/94514250/42fc4e1e-cb5d-462d-a7b6-5136f9366453)

```swift
func autoconnect() -> Publishers.Autoconnect<Self>
```

# Return Value
연결 가능한 업스트림 Publisher에 자동으로 연결되는 Publisher

# Discussion
`autoconnect()`를 사용하여 Foundation 프레임워크의 [`Timer.TimerPublisher`](https://developer.apple.com/documentation/foundation/timer/timerpublisher)와 같은 [ConnectablePublisher](https://www.notion.so/ConnectablePublisher-9642d76429cc463c81e2e86ed28c543f?pvs=21) 인스턴스 작업을 단순화한다. 

다음 예제에서 [`publish(every:tolerance:on:in:options:)`](https://developer.apple.com/documentation/foundation/timer/3329589-publish) 연산자는 [`ConnectablePublisher`](https://developer.apple.com/documentation/combine/connectablepublisher)인 [`Timer.TimerPublisher`](https://developer.apple.com/documentation/foundation/timer/timerpublisher)를 생성한다. 
결과적으로 가입자는 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21)를 호출할 때까지 어떤 값도 받지 못 한다. 
단일 Subscriber와 작업할 때 편의를 위해 `autoconnect()` 연산자는 Subscriber가 연결할 때 [connect()](https://www.notion.so/connect-ba2f035cd34140a1a3a085d1c18b396f?pvs=21) 호출을 수행한다. 

```swift
cancellable = Timer.publish(every: 1, on: .main, in: .default)
    .autoconnect()
    .sink { date in
        print ("Date now: \(date)")
    }
```

# 참고 문서
- [Apple Documentation - autoconnect()](https://developer.apple.com/documentation/combine/connectablepublisher/autoconnect())
