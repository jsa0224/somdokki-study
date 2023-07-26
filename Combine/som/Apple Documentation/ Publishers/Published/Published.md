# ****Published****

특성으로 표시된 프로퍼티를 게시하는 형식이다.
![스크린샷 2023-07-26 오후 9 47 51](https://github.com/jsa0224/somdokki-study/assets/94514250/8cebc5dd-6346-4e61-ac41-bb125fce39d4)

```swift
@propertyWrapper
struct Published<Value>
```

# Overview

`@Published` 특성을 사용하여 프로퍼티를 게시하면 이 유형의 Publisher가 생성된다. 다음과 같이 $ 연산자를 사용하여 Publisher에 접근한다.

```swift
class Weather {
    @Published var temperature: Double
    init(temperature: Double) {
        self.temperature = temperature
    }
}

let weather = Weather(temperature: 20)
cancellable = weather.$temperature
    .sink() {
        print ("Temperature now: \($0)")
}
weather.temperature = 25

// Prints:
// Temperature now: 20.0
// Temperature now: 25.0
```

프로퍼티가 변경되면 속성의 `willSet` 블록에서 게시가 발생한다. 즉, Subscriber는 실제로 프로퍼티에 설정되기 전에 새 값을 받는다. 위의 예에서 싱크가 두 번째로 폐쇄를 실행하면 매개변수 값 25를 수신한다. 그러나 클로저가 `weather.temperature`를 평가했다면 반환된 값은 20이 될 것이다. 

💡 `@Published` 프로퍼티는 제한된 클래스이다. 구조와 같은 클래스가 아닌 타입이 아닌 클래스의 프로퍼티와 함께 사용하라.

# 참고 문서
-[Apple Documentation - Published](https://developer.apple.com/documentation/combine/published)
