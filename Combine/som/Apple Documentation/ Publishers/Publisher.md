# ****Publisher****

형식이 시간이 지남에 따라 일련의 값을 전송할 수 있음을 선언한다.
![스크린샷 2023-07-25 오후 4 00 09](https://github.com/jsa0224/somdokki-study/assets/94514250/9b39cddb-6718-4786-9e93-e8ba18dec4dc)

```swift
protocol Publisher<Output, Failure>
```

# Overview

Publisher는 하나 이상의 [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber) 인스턴스에 요소를 전달한다.
Subscriber의 [`Input`](https://developer.apple.com/documentation/combine/subscriber/input) 및 [`Failure`](https://developer.apple.com/documentation/combine/subscriber/failure) 관련 타입은 Publisher가 선언한 [`Output`](https://developer.apple.com/documentation/combine/publisher/output) 및 [`Failure`](https://developer.apple.com/documentation/combine/subscriber/failure) 타입과 일치해야 한다.
Publisher는 [`receive(subscriber:)`](https://developer.apple.com/documentation/combine/publisher/receive(subscriber:)) 메서드를 구현하여 Subscriber를 수락한다. 

그런 다음 Publisher는 Subscriber에서 다음 메서드를 호출할 수 있다.

- [`receive(subscription:)`](https://developer.apple.com/documentation/combine/subscriber/receive(subscription:)) : 구독 요청을 확인하고 [`Subscription`](https://developer.apple.com/documentation/combine/subscription) 인스턴스를 반환한다. Subscriber는 구독을 사용하여 Publisher에게 요소를 요청하고 Publisher를 취소하는 데 사용할 수 있다.
- [`receive(_:)`](https://developer.apple.com/documentation/combine/subscriber/receive(_:)) : Publisher에서 Subscriber에게 하나의 요소를 전달한다.
- [`receive(completion:)`](https://developer.apple.com/documentation/combine/subscriber/receive(completion:)) : Publishing이 정상적으로 또는 오류와 함께 종료되었음을 Subscriber에게 알린다.

다운스트림 Subscriber가 올바르게 작동하려면 모든 Publisher가 이 계약을 준수해야 한다. 

Publisher의 확장 기능은 정교한 이벤트 처리 체인을 만들기 위해 구성하는 다양한 연산자를 정의한다. 각 연산자는 Publisher 프로토콜을 구현하는 형식을 반환합니다. 이러한 형식의 대부분은 [`Publishers`](https://developer.apple.com/documentation/combine/publishers) 열거형의 확장으로 존재한다. 예를 들어 [`map(_:)`](https://developer.apple.com/documentation/combine/publisher/map(_:)-99evh) 연산자는 [`Publishers.Map`](https://developer.apple.com/documentation/combine/publishers/map)의 인스턴스를 반환한다. 

💡 Combine 게시자는 Swift 표준 라이브러리의 [`AsyncSequence`](https://developer.apple.com/documentation/Swift/AsyncSequence)와 유사하지만 별개의 역할을 수행한다. Publisher와 AsyncSequence는 모두 시간이 지남에 따라 요소를 생성한다. 그러나 Combine의 풀 모델은 [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber)를 사용하여 Publisher로부터 요소를 요청하는 반면 Swift 동시성은 for-await-in 구문을 사용하여 AsyncSequence가 게시한 요소를 반복한다. 두 API 모두 요소를 매핑하거나 필터링하여 시퀀스를 만드는 반면 Combine만이 [`debounce(for:scheduler:options:)`](https://developer.apple.com/documentation/combine/publisher/debounce(for:scheduler:options:)) 및 [`throttle(for:scheduler:latest:)`](https://developer.apple.com/documentation/combine/publisher/throttle(for:scheduler:latest:))과 같은 시간 기반 작업과 [`merge(with:)`](https://developer.apple.com/documentation/combine/publisher/merge(with:)-7fk3a) 및 [`combineLatest(_:_:)`](https://developer.apple.com/documentation/combine/publisher/combinelatest(_:_:)-1n30g)와 같은 결합 작업을 제공합니다.
  두 접근 방식을 연결하기 위해 프로퍼티 [`values`](https://developer.apple.com/documentation/combine/publisher/values-1dm9r)은 Publisher의 요소를 AsyncSequence로 노출하여 [`Subscriber`](https://developer.apple.com/documentation/combine/subscriber)를 연결하는 대신 for-await-in을 사용하여 반복할 수 있도록 한다.

# ****Creating Your Own Publishers****

Publisher 프로토콜을 직접 구현하는 대신 Combine 프레임워크에서 제공하는 여러 유형 중 하나를 사용하여 고유한 게시자를 만들 수 있다.

- [`PassthroughSubject`](https://developer.apple.com/documentation/combine/passthroughsubject)와 같은 [`Subject`](https://developer.apple.com/documentation/combine/subject)의 구체적인 하위 클래스를 사용하여 [`send(_:)`](https://developer.apple.com/documentation/combine/subject/send(_:)) 메서드를 호출하여 필요에 따라 값을 게시한다.
- 주체의 기본 값을 업데이트할 때마다 [`CurrentValueSubject`](https://developer.apple.com/documentation/combine/currentvaluesubject)를 사용하여 게시한다.
- 자체 유형 중 하나의 프로퍼티에 @Published 주석을 추가한다. 이렇게 하면 프로퍼티 값이 변경될 때마다 이벤트를 내보내는 Publisher가 프로퍼티에 생긴다. 이 접근 방식의 예는 [`Published`](https://developer.apple.com/documentation/combine/published) 타입을 참조하라.

# 참고 문서
- [Apple Documentation - Publisher](https://developer.apple.com/documentation/combine/publisher)
