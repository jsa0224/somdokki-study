# ****Future****

결국 단일 값을 생성한 다음 완료하거나 실패하는 Publisher
![스크린샷 2023-07-31 오후 10 03 50](https://github.com/jsa0224/somdokki-study/assets/94514250/328320e6-177c-4d86-bd95-6bce1ff57488)

```swift
final class Future<Output, Failure> where Failure : Error
```

# Overview

Future를 사용하여 일부 작업을 수행한 다음 단일 요소를 비동기적으로 게시한다. [`Future.Promise`](https://developer.apple.com/documentation/combine/future/promise)를 취하는 클로저로 Future를 초기화한다. 클로저는 성공 또는 실패를 나타내는 [`Result`](https://developer.apple.com/documentation/Swift/Result)로 약속을 호출한다. 성공 사례에서 Future의 다운스트림 Subscriber는 게시 스트림이 정상적으로 완료되기 전에 요소를 받는다. 결과가 오류인 경우 해당 오류와 함께 게시가 종료된다. 결과가 오류인 경우 해당 오류와 함께 게시가 종료된다.

다음 예제에서는 Future를 사용하여 잠시 지연된 후 비동기식으로 난수를 게시하는 방법을 보여준다.

```swift
func generateAsyncRandomNumberFromFuture() -> Future <Int, Never> {
    return Future() { promise in
        DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
            let number = Int.random(in: 1...10)
            promise(Result.success(number))
        }
    }
}
```

게시된 값을 받으려면 다음과 같이 [`Subscribers.Sink`](https://developer.apple.com/documentation/combine/subscribers/sink)와 같은 Combine Subscriber를 사용한다. 

```swift
cancellable = generateAsyncRandomNumberFromFuture()
    .sink { number in print("Got random number \(number).") }
```

## ****Integrating with Swift Concurrency****

Swift 5.5의 async-await 구문과 통합하기 위해 `Future`는 대기 중인 호출자에게 값을 제공할 수 있다. [`Publisher`](https://developer.apple.com/documentation/combine/publisher)를 준수하고 잠재적으로 많은 요소를 게시하는 다른 타입과 달리 `Future`는 하나의 요소만 게시(또는 실패)하기 때문에 이는 특히 유용하다. [`value`](https://developer.apple.com/documentation/combine/future/value-9iwjz) 프로퍼티를 사용하여 위의 호출 지점은 다음과 같다. 

```swift
let number = await generateAsyncRandomNumberFromFuture().value
print("Got random number \(number).")
```

## ****Alternatives to Futures****

비동기 작업이 완료된 후 일부 작업을 수행하려는 경우 Swift의 `async-await` 구문은 future 사용을 완전히 대체할 수도 있다. 

다음 예제는 위의 Future 예제와 동일한 비동기식 난수 생성을 비동기 메서드로 수행한다.

```swift
func generateAsyncRandomNumberFromContinuation() async -> Int {
    return await withCheckedContinuation { continuation in
        DispatchQueue.main.asyncAfter(deadline: .now() + 2) {
            let number = Int.random(in: 1...10)
            continuation.resume(returning: number)
        }
    }
}
```

이 메서드의 호출 지점은 Future의 Sink’s Subscriber가 하는 것처럼 클로저를 사용하지 않는다. 단순히 대기하고 결과를 할당한다.

```swift
let asyncRandom = await generateAsyncRandomNumberFromContinuation()
```

Continuations에 대한 자세한 내용은 Swift 표준 라이브러리의 [Concurrency](https://developer.apple.com/documentation/swift/concurrency) 주제를 참조하라. 

# 참고 문서
- [Apple Documentation - Future](https://developer.apple.com/documentation/combine/future)
