### 스위프트는,

---

- 애플 플랫폼에서 앱을 개발할 때 사용하고, C나 Objective-C와 유사하다. Int, String, Collection등을 잘 지원해준다.
- 상수를 타언어보다 강력하게 사용할 수 있어서, 안전하고 깨끗한 맥락으로 코드를 작성할 수 있다
- 튜플이란 타입도 지원한다. 간략한 문법으로 복잡한 값을 쉽게 전달할 수 있다 (말은 할 수 있지 ㅎ..)
- 값의 부재를 표현하는 옵셔널 타입도 지원한다
- 강타입 언어다. 타입이 잘못 전달되는 불상사를 미리 확인하고 막아낼 수 있다

### 상수와 변수

---

- 상수는 미래영겁 불변하고 변수는 변할 수 있다
    - 상수는 `let`, 변수는 `var`로 선언한다.
    - 웬만하면 `let`으로 작성하고 변할 때 `var`를 써라

### 타입 어노테이션

---

- 수를 선언할 때, 이 수에 어떤 값이 저장되는지를 명시할 수 있다. 예를 들어,
    
    ```swift
    let myName: String
    ```
    
    - 근데 굳이 안써도 된다
    
    ```swift
    let myName = "안알려줌"
    ```
    
    - 이것도 에러없이 동작한다. 언어가 추론을 하기 때문이다
    - 다만 복잡한 맥락이나, 나중에 값을 저장하는 경우 등에는 타입을 명시하는 게 도움이 된다
    - 그리고 같은 타입을 가지는 값들은 아래처럼 연속해서 선언할 수도 있다.
    
    ```swift
    let myName, myDogName, myCatName: String
    ```
    

### 수에 이름 붙이기

---

- 수들은 아래처럼 거의 모든 일반적인 기호로 이름을 붙일 수 있다
    
    ```swift
    let π = 3.14159
    let 你好 = "你好世界"
    let 🐶🐮 = "dogcow"
    ```
    
    - 하지만 몇몇 예외가 있다: 공백, 수의 연산기호, 화살표 등. 그러니까 아래와 같은 건 안된다
        
        ```swift
        let your name = "에러"
        let your+name = "에러"
        let let ="에러"
        ```
        
        - let과 같은 키워드 경우는 백틱으로 처리하면, 맥락에 따라 어거지로 쓸 수 있는 상황이 있다
        
        ```swift
        struct Value {
        	let `let` = "상수"
        }
        Value.let == "상수"
        
        ```
        

### 수를 출력하기

---

- `print(_:, separator:, terminator:)`라는 함수를 보통 사용한다
- 사용법은 사실 약간 복잡한데 보통 아래처럼 쓴다
    
    ```swift
    print(myName)
    ```
    
- 특정 값만 바뀌는, 탬플릿이 있는 문자열은 문자열 보간법이라고 하는 문법을 사용할 수 있다
    
    ```swift
    print("it is \(stuffName)")
    ```
    
    - 문자열 내에 `\(stuffName)` 과 같은 양식으로 변수를 넘기면 언어가 알아서 해준다

### 주석

---

- 말그대로 주석, 기능이 아니라 개발자에게 전달하는 메시지

```swift
//나는 한줄 주석
/*
나
는
여
러
줄
주
석
*/
```

### 세미콜론 (;)

---

- 다른(옛날) 언어들은 보통 이렇게 해야 프로그램을 작성할 수 있다
    
    ```swift
    let myName = "안알려줌";
    print(myName);
    ```
    
- 스위프트는 보통 안써도 되지만, 타언어처럼 쓸 수도 있다
    - 정말 의미에 맞는 문장기호처럼 사용할 수도 있다(한줄에 여러 맥락을 붙여서 쓸 때)
    
    ```swift
    let myName = "안알려줌"; let comment = "제 이름은요"; print(comment + myName)
    ```
    

### Integer정수

---

- 정수는 수학에서의 정수와 같다
    - 여기서 짚고 넘어가야하는 건 결국 프로그램이란, 컴퓨터라는 기계부품을 조작하기 위한 도구다
    - 무슨 말이냐면, 컴퓨터는 한정적인 자원을 가지고 있고, 이 자원을 효과적으로 쓰기 위해(특히 메모리) 스위프트는 정수에 다양한 종류를 두었다
    - Int8, Int32, Int64등이 그러하다. 뒤에 숫자는 bit다 → Int8 = 8bit, Int64 = 64bit
    - 음수를 사용하지 않는 UInt와 같은 경우도 있다

- 위에서 말했듯 정수는 여러 종류가 있으나, 모든 수를 표현할 수 없다. 본질적으로 컴퓨터가 한정적인 자원을 가지고 있기 때문이다.
    - 즉 정수는 종류마다 상한과 하한이 있다
        
        ```swift
        Int8.max; Int8.min; Int64.max; Int64.min; UInt64.max; UInt64.min;
        ```
        

- 그냥 Int라고 하면, 컴퓨터의 한계에 따라 다른 Int를 사용한다
    - 컴퓨터가 32bit 플랫폼이면 Int32
    - 컴퓨터가 64bit 플랫폼이면 Int64
    - UInt도 마찬가지다

- 스위프트는 숫자를 보통 일단 Int로 추론한다

### Floating-point numbers부동소수점

---

- Float은 32bit, Double은 64bit 내에서 부동소수점을 표현하여 Int보다 다양한 값을 표현할 수 있다
    - Decimal은 왜 얘기 안해주는지 모르겠음

### 타입 안정성, 타입 추론

---

- 스위프트는 수의 타입을 분명히 함으로써, 코드가 동작하기 전부터, 동작하는 맥락을 분명히 한다
    - `숫자 + 문자열`이 불가능할 때, 이 맥락이 보이면 개발자에게 이 맥락에 실수가 있다고 알려준다
- 이 말이 곧 모든 타입에 타입 어노테이션을 붙여야한다는 것은 아니다. 스위프트는 타입을 추론한다
    
    ```swift
    let leftSide = 0, rightSide = 1;
    print(leftSide + rightSide)
    ```
    
    - 스위프트는 보통 정수를 Int로 추론하기 때문에, Int + Int = Int가 된다(부동소수점은 Double로 추론)
    
    ```swift
    let result = 123 + 0.456
    print(result)
    ```
    
    - 위와 같은 맥락에서는 123도 Double로 추론해준다

### 진법을 나타내는 기호들

---

- 정수는 보통은 십진법으로 읽고 쓴다
- 2진법은 `0b`를 접두사로 사용하고 8진법은 `0o`, 16진법은 `0x`를 사용한다
    
    ```swift
    let 십진법17 = 17
    let 이진법17 = 0b10001
    let 팔진법17 = 0o21
    let 십육진법17 = 0x11
    ```
    
- 부동소수점도 마찬가지로 보통 십진법으로 읽고 쓰고 16진법은 `0x`를 접두사로 쓴다
- 부동소수점은 지수를 쓸 수 있는데 십진법의 밑은 기본적으로 10이고, 십육진법의 밑은 기본적으로 2다
    - 1.25e2 → 1.25 * 100 → 125
    - 0xFp-2 → 15 * 1/4 → 3.75
- 숫자는 언더바를 붙여서 쉽게 읽게 만들 수 있다. 스위프트가 문제삼지 않는다
    
    ```swift
    let 백만 = 1_000_000
    ```
    

### 정수 변환

---

- Int8과 Int16을 더하면 Int16이 된다
    - 같은 맥락으로 피연산자들 사이에서 작은 범위가 큰 범위에 포함되면 그 결과는 큰 범위가 된다

### 정수와 소수의 변환

---

- 얘네는 명시적으로 연산해야 한다. 아래는 에러가 나고
    
    ```swift
    let int = 0, double = 1.1
    print(int + double) // occur error
    ```
    
    - 아래는 동작한다
    
    ```swift
    print(Double(int) + double)
    ```
    
- 부동소수점을 정수로 변환하면 뒤에 소수점을 버린다
    
    ```swift
    print(int + Int(double)) //1
    ```
    

### 타입별칭

---

- 이미 존재하는 타입에 새 별칭을 붙여줄 수도 있다. 별칭일 뿐, 기능은 똑같다
    
    ```swift
    typealias Age = UInt8
    ```
    

### 불리언(참거짓)

---

- 스위프트에서는 불Bool이라고 한다. 논리적 참과 거짓을 표현한다
    
    ```swift
    let iAmHuman = true
    let iAmOld = false
    ```
    
    - 이를 통해 조건문이라고 하는 문법을 사용할 수도 있다
    
    ```swift
    if iAmHuman {
    	eat()
    }
    
    if iAmOld { 
      doNotLie()
    } else {
      cool()
    }
    ```
    
    - 조건(컨디션)은 Bool만 사용될 수 있다. 다른 수는 연산자를 통해 Bool로 변환해야 한다
    
    ```swift
    let number = 1234
    
    if number == 1 {
    	print("number is 1")
    } else {
    	print("it is \(number)")
    }
    ```
    

### 튜플

---

- 소괄호 안에 여러 값을 묶어서 마치 하나의 값인 것처럼 표현할 수 있다
    
    ```swift
    let myProfile = ("안알려줌", 1994, 12, 26, "man", "chicken")
    myProfile.0 == "안알려줌"
    myProfile.5 == "chicken"
    ```
    
    - 각 값에 이름을 붙여줄 수도 있다
    
    ```swift
    let myProfile = (이름: "안알려줌", 생년: 1994, 생월: 12, 생일: 26, 성별: "man", 좋아하는음식: "chicken")
    myProfile.이름 == "안알려줌"
    myProfile.좋아하는음식 == "chicken"
    ```
    

### 옵셔널, nil닐

---

- 값의 부재를 표현할 수 있다
    
    ```swift
    var myWatch: Watch?
    ```
    
    - 값의 부재를 nil이라고 하는 상수로 표현한다
    
    ```swift
    myWatch == nil
    ```
    
    - 값이 있을 수도 있다
    
    ```swift
    var myWatch: Watch? = AppleWatch()
    myWatch != nil
    ```
    

### 옵셔널 바인딩

---

- 옵셔널의 맥락에서 값이 존재할 때에만, 값을 꺼내서 사용할 수 있다
    
    ```swift
    if let myWatch = myBag.watch {
    	print("가방에 \(myWatch)가 있어요")
    } else {
    	print("가방에 시계가 없어요")
    }
    ```
    
    - 표현법이 다양하다
    
    ```swift
    let myWatch = myBag.watch // Optional
    if let myWatch { 
    	// exist
    } else { 
    	// nil
    }
    
    ```
    
    - 값이 없을 땐 아래처럼 기본값을 줄 수도 있다
    
    ```swift
    let myWatch = myBag.watch ?? NewWatch()
    ```
    
    - 이런 과정을 거치지 않고 느낌표를 통해 강제로 꺼낼 수도 있다. 단 없으면 에러가 난다
    
    ```swift
    let myWatch = myBag.watch!
    ```
    
    - Optional로 표현했지만, 반드시 있는 경우가 있을 수도 있다. 이때는 강제언랩핑을 통해 명시적으로 옵셔널을 없앨 수 있다

### 에러핸들링

---

- 예외가 발생했을 때, 이를 처리하는 방법들이 있다
    
    ```swift
    func canThrowAnError() throws { } 
    ```
    
    - `throws` 키워드를 통해 예외가 발생하는 함수를 작성할 수 있다. 이러한 함수는,
    
    ```swift
    do {
    	try canThrowAnError()
    } catch {
    	handle(error)
    }
    ```
    
    - 이처럼 `do-catch`라고 하는 구절을 통해 사용하고 에러를 처리할 수 있다
    
    ```swift
    func makeASandwich() throws {
        // ...
    }
    
    do {
        try makeASandwich()
        eatASandwich()
    } catch SandwichError.outOfCleanDishes {
        washDishes()
    } catch SandwichError.missingIngredients(let ingredients) {
        buyGroceries(ingredients)
    }
    ```
    

### Assertion와 Prediction

---

- 프로그램이 실행중일 때 개발자가 만든 조건이 잘 맞아떨어지는지 확인하는 코드들
    - 안맞으면 앱을 종료시켜버린다. assert는 개발 중에, prediction은 실제 운영 중에
    
    ```swift
    assert(myBag.watch != nil)
    
    precondition(myBag.watch != nil)
    ```
    
    - precondition는 fatalError와 맥락이 비슷할 수 있는데, precondition은 상용코드에서 최적화가 된다. 하지만 fatalError는 그러지 않는다.
