# ****Empty****

값을 게시하지 않고 선택적으로 즉시 완료하는 Publisher이다.
![image](https://github.com/jsa0224/somdokki-study/assets/94514250/0dd688a6-d955-4143-8a9b-128f24b9a43e)

```swift
struct Empty<Output, Failure> where Failure : Error
```

# Overview

이니셜라이저 `Empty(completeImmediately: false)`를 사용하여 값을 보내지 않고 완료하거나 실패하지 않은 “Never” Publisher를 만들 수 있다. 

# 참고 문서
- [Apple Documentation - Empty](https://developer.apple.com/documentation/combine/empty)
