# ****Deferred****

새 Subscriber를 위한 Publisher를 만들기 위해 제공된 클로저를 실행하기 전에 구독을 기다리는 Publisher.
![스크린샷 2023-08-04 오전 12 58 00](https://github.com/jsa0224/somdokki-study/assets/94514250/9d9366a4-bce9-47df-aff3-7ef9d3439a1e)


```swift
struct Deferred<DeferredPublisher> where DeferredPublisher : Publisher
```

# 참고 문서
- [Apple Documentation - Deferred](https://developer.apple.com/documentation/combine/just-publisher-operators)
