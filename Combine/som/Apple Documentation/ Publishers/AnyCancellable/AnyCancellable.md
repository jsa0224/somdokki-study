# ****AnyCancellable****

취소 시 제공된 클로저를 실행하는 타입 제거 취소 가능 객체이다.
![스크린샷 2023-07-26 오후 10 12 31](https://github.com/jsa0224/somdokki-study/assets/94514250/b6242239-cc55-489f-8fb0-c79151493730)

```swift
final class AnyCancellable
```

# Overview

Subscriber 구현은 이 타입을 사용하여 호출자가 Publisher를 취소할 수 있지만 [`Subscription`](https://developer.apple.com/documentation/combine/subscription) 개체를 사용하여 항목을 요청할 수 없도록 하는 “취소 토큰”을 제공할 수 있다. 

[`AnyCancellable`](https://developer.apple.com/documentation/combine/anycancellable) 인스턴스는 초기화 해제 시 자동으로 [`cancel()`](https://developer.apple.com/documentation/combine/cancellable/cancel())을 호출한다. 

# 참고 문서
- [Apple Documentation - AnyCancellable](https://developer.apple.com/documentation/combine/anycancellable)
