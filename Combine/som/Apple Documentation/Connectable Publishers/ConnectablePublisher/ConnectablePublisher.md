# ****ConnectablePublisher****

게시 연결 및 취소의 명시적 수단을 제공하는 Publisher이다.

![스크린샷 2023-08-04 오후 4 22 25](https://github.com/jsa0224/somdokki-study/assets/94514250/699cd9eb-d9f9-4cc6-9099-79c7112a7715)

```swift
protocol ConnectablePublisher<Output, Failure> : Publisher
```

# Overview

요소를 생성하기 전에 추가 구성 또는 설정을 수행해야 하는 경우 ConnectablePublisher를 사용한다. 

이 Publisher는 [`connect()`](https://developer.apple.com/documentation/combine/connectablepublisher/connect()) 메서드를 호출할 때까지 요소를 생성하지 않는다. 

[`makeConnectable()`](https://developer.apple.com/documentation/combine/publisher/makeconnectable())을 사용하여 실패 타입이 [`Never`](https://developer.apple.com/documentation/Swift/Never)인 Publisher로부터 ConnectablePublisher를 생성한다. 

# 참고 문서
- [Apple Documentation - ConnectablePublisher](https://developer.apple.com/documentation/combine/connectablepublisher)
