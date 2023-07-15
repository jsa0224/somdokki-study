#Functional Reactive Programming Paradigm

## 용어

### Programming
- Input 을 넣어 Output 을 만들어 내는 것
- 요즘은, 마우스나 터치이벤트도 하나의 Input으로 본다
![image](https://github.com/juun97/Today-I-Learned/assets/59365211/6d85e894-f310-451f-ad87-973af4a95065)

### Paradigm
- 어떤 한 시대 사람들의 견해나 사고를 근본적으로 규정하고 있는 테두리로서의 인식의 체계, 또는 사물에 대한 이론적인 틀이나 체계를 의미하는 개념 -Wiki백과-

### Functional
> Side-Effect 가 없도록 프로그래밍 하는 패러다임

#### 종류

![image](https://github.com/juun97/Today-I-Learned/assets/59365211/3d8ce2fc-4287-4ea3-b20c-019ac6bd2bc1)

| 번호 | 이름 | Input | Output |
| --- | --- | --- | --- |
| 1️⃣| Generator | X | O |
| 2️⃣ | Function  | O | O |
| 3️⃣ | Consumer | O | X |

#### 예시

```swift
// 1️⃣ Generator
func getTen() -> Int {
    return 10
}

// 2️⃣ Function
func addTwo(n1: Int, n2: Int) -> Int {
    return n1 + n2
}

// 3️⃣ Consumer
func output(value: Int) {
    print("\(value)")
}

// Function들의 Composition
output(value: addTwo(n1: getTent() , n2: getTemt()))
// 20
```

#### 정리

- 데이터는 immutable 하게 취급하자
- 데이터 변경이 필요할 때는 새로 만들자
- Side-Effect 을 없애기 위해서 Pure Function 을 사용하자
- Function 들의 Composition 과 High-Order Function 으로 프로그램을 만들자
- Data가 아닌 Process에 집중해서 프로그램을 만들자



### Reactive
> Async 한 작업을 Functional 하게 처리하는 아이디어
- 반응형 프로그래밍은 데이터 스트림과 변화의 전달과 관련된 비동기 프로그래밍 패러다임
- Stream 으로 연결하여 데이터를 흘러보내자는 아이디어


#### rxSwift
> reactive 아이디어를 구현한 Swift 라이브러리

![image](https://github.com/juun97/Today-I-Learned/assets/59365211/d716c0d2-3457-4d12-b031-830a255748b9)


- 데이터를 만들어내는 시작점 -> Observable
- 데이터를 사용하는 끝점 -> Subscriber
- Observable과 Subscriber 사이를 stream 으로 연결한다.
- Stream 으로 흘러가는 과정에서 Operator 라는것을 활용해 데이터의 변형 및 정제가 가능하다.


#### 정리
- Async한 처리를 Functional 하게 처리하자
- 리턴값은 Stream인 Observable을 반환하자
- Stream에 흐르는 Data/Event 를 Operator 로 처리하자
- Stream 과 Stream 을 연결하자
- Data가 아닌 Process에 집중해서 프로그램을 만들자


### Functional & Reactive

두가지 모두 `Data가 아닌 Process에 집중해서 프로그램을 만들자` 는 관점을 가지고 있다.

그래서 Functional 과 Reactive 가 같이 다뤄지고 있다!

### Reference

- [let us: Go! 2017 Functional Reactive Programming 패러다임](https://www.youtube.com/watch?v=cXi_CmZuBgg)
