# Failure
이 Publisher가 게시할 수 있는 오류의 종류이다. 
![스크린샷 2023-07-27 오후 9 07 00](https://github.com/jsa0224/somdokki-study/assets/94514250/1ce70e0f-aa60-4c3a-9b0a-f5a4b008bf36)

```swift
associatedtype Failure : Error
```

# Discussion

이 `Publisher`가 오류를 게시하지 않는 경우 `Never`을 사용하라. 

# 참고 문서
- [Apple Documentation - Failure](https://developer.apple.com/documentation/combine/publisher/failure)
