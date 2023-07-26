# ****AnyPublisher****

다른 Publisher를 래핑하여 타입 삭제를 수행하는 Publisher.
![스크린샷 2023-07-26 오후 9 25 25](https://github.com/jsa0224/somdokki-study/assets/94514250/f0d0a59f-a20f-41b9-bfc4-48bde5110404)

```swift
@frozen
struct AnyPublisher<Output, Failure> where Failure : Error
```

# Overview

[`AnyPublisher`](https://developer.apple.com/documentation/combine/anypublisher)는 고유한 중요한 프로퍼티가 없고 업스트림 Publisher의 요소 및 완료 값을 전달하는 [`Publisher`](https://developer.apple.com/documentation/combine/publisher)의 구체적인 구현이다. 

[`AnyPublisher`](https://developer.apple.com/documentation/combine/anypublisher)를 사용하여 다른 모듈과 같이 API 경계를 넘어 노출하고 싶지 않은 세부 정보가 있는 유형의 Publisher를 래핑한다. [`AnyPublisher`](https://developer.apple.com/documentation/combine/anypublisher)로 제목을 래핑하면 호출자가 [`send(_:)`](https://developer.apple.com/documentation/combine/subject/send(_:)) 메서드에 접근하는 것도 방지된다.
이러한 방식으로 타입 삭제를 사용하면 시간이 지남에 따라 기존 클라이언트에 영향을 주지 않고 기본 게시자 구현을 변경할 수 있다. 

Combine의 [`eraseToAnyPublisher()`](https://developer.apple.com/documentation/combine/publisher/erasetoanypublisher()) 연산자를 사용하여 Publisher를 AnyPublisher로 래핑할 수 있다.

# 참고 링크
- [Apple Documentation - AnyPublisher](https://developer.apple.com/documentation/combine/anypublisher)
