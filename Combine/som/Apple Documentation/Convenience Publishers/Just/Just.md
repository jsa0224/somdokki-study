# ****Just****

각 Subsciber에게 한 번만 출력을 내보낸 다음 완료되는 Publisher
![스크린샷 2023-07-31 오후 10 07 11](https://github.com/jsa0224/somdokki-study/assets/94514250/ffd0a69d-476f-46cc-a6b8-167ddd5f3274)

```swift
struct Just<Output>
```

# Overview

Just Publisher를 사용하여 체인을 시작할 수 있다. Just Publisher는 값을 [`Publishers.Catch`](https://developer.apple.com/documentation/combine/publishers/catch)로 바꿀 때도 유용하다. 

[****Publisher****](https://www.notion.so/Publisher-6fe5eb05495d4830b59d5c205f7231ad?pvs=21)와 대조적으로 Just Publisher는 오류로 인해 실패할 수 없다. 그리고  [****Publisher****](https://www.notion.so/Publisher-6fe5eb05495d4830b59d5c205f7231ad?pvs=21)와 달리 Just Publisher는 항상 value를 창출한다.

# 참조 문서
- [Apple Documentation - Just](https://developer.apple.com/documentation/combine/just)
