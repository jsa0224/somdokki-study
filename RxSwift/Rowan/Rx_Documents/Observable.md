# Observable

ReactiveX에서 **옵저버(Observer)**는 **옵저버블(Observable)**을 구독한다. 이후 옵저버는 옵저버블이 방출하는 하나 또는 연속된 항목에 반응한다. 이 패턴은 동시성 연산을 가능하게 한다. 왜냐하면 이 패턴은 옵저버블이 객체를 방출하는 것을 기다리면서 연산을 block하지 않는 대신에 옵저버블이 미래에 값을 방출했을 때 적절하게 반응할 준비를 하는 보초인 옵저버를 만들기 때문이다.

이 문서에서는 리액티브 패턴, 옵저버블, 옵저버 및 옵저버의 옵저버블을 구독하는 방법을 설명한다. [다른 페이지](https://www.notion.so/Operators-fcbf6e948b6549d3b6cbe01a576cbf3b?pvs=21)([Operators](https://reactivex.io/documentation/operators.html))에서는 다양한 옵저버블 연산자를 사용해 옵저버블을 함께 연결하고 동작을 변경하는 방법을 보여준다.

문서에서 “마블 다이어그램”을 활용한 내용 설명이 자주 보인다. 아래 그림을 통해 마블 다이어그램이 어떻게 옵저버블과 옵저버블의 전환을 표현하는지 알 수 있다.

![MableDiagram](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8f4c706c-bac7-4584-83ce-c3d37c128d33/Untitled.png)

- 가로 줄은 옵저버블의 타임라인이다.
- 도형은 옵저버블에 의해 방출되는 항목을 나타낸다.
- 타임라인 안의 세로 줄은 옵저버블이 성공적으로 완료되었음을 나타낸다.
- 점선과 중앙 박스는 옵저버블에 변환이 적용됨을 나타낸다. 박스 내부 텍스트는 변환의 성질을 보여준다.
- 아래 쪽 옵저버블은 변환의 결과이다.
- 어떤 이유로 옵저버블이 에러와 함께 비정상적으로 종료되면 X로 표현한다.

---

# Background

코드를 살펴보다 보면 개발자들은 자신의 코드가 작성된 순서에 따라 실행되고 완료 되기를 기대한다는 사실을 쉽게 알 수 있다. 그러나 ReactiveX에서는 많은 명령이 동시에 병렬적으로 실행될 수 있고 그 결과는 나중에 옵저버에 의해 임의의 순서로 캡처된다. *메서드를 호출*하기보다는 “옵저버블” 형식으로 데이터를 조회하고 변환하는 메커니즘을 정의한다. 이후 옵저버블에 옵저버를 구독시키면 이전에 정의된 메커니즘이 옵저버블의 방출이 준비될 때마다 방출을 포착하고 대응하기 위해 감시하는 옵저버와 함께 작동한다.

이 접근 방식의 장점은 서로 의존하지 않는 많은 작업이 있을 때 다음 작업을 시작하기 전에 각 작업이 완료될 때까지 기다리지 않고 동시에 모두 시작할 수 있다는 것이다. 이를 통해 전체 작업 묶음을 수행하는데에는 묶음 안의 가장 긴 작업의 시간 만큼이 소요되게 된다.

이 비동기 프로그래밍 및 디자인 모델을 설명하는 데 사용되는 많은 용어가 있다. 이 문서에서는 다음 용어를 사용할 것이다. 

`**옵저버**는 **옵저버블**을 **구독**한다`

옵저버블은 옵저버의 메서드를 호출하여 항목을 방출하거나 옵저버에게 알림을 보낸다.

다른 문서 및 기타 컨텍스트에서 우리가 “옵저버(observer)”라고 부르는 것은 때때로 “subscriber”, “watcher” 및 “reactor”라고 한다. 일반적으로 이 모델을 “리액터 패턴”이라고 한다.

---

# Establishing Observers

이 페이지에는 예제에 Groovy와 유사한 pseudocode(의사 코드)를 사용하지만 여러 언어로 된 ReactiveX 구현이 있다.

일반적인 메서드 호출(즉, ReactiveX에서 일반적인 일종의 비동기식 병렬 호출이 아님)에서 흐름은 다음과 같다.

1. 메서드를 호출한다.
2. 해당 메서드의 반환 값을 변수에 저장한다.
3. 해당 변수와 새 값을 사용하여 유용한 작업을 수행한다.

```swift
// `returnVal`에 메서드의 반환값을 할당하기 위해 메서드 호출
var returnValue = someMethod(itsParameters)
// ...반환 값을 통해 유용한 작업 수행
```

비동기 모델에서 흐름은 다음과 같이 진행된다.

1. 비동기 호출의 반환 값으로 유용한 작업을 수행하는 메서드를 정의한다. 해당 메서드는 옵저버 내부에 정의된다.
2. 옵저버블에서 비동기 호출 자체를 정의한다.
3. 옵저버블을 구독하여 옵저버를 해당 옵저버블에 연결한다(이는 또한 옵저버블의 행동을 시작한다).
4. 다른 작업을 수행한다; 비동기 호출이 반환될 때마다 옵저버의 메서드는 반환 값 또는 옵저버블이 방출하는 항목에서 작동하기 시작한다.

```swift
// 구독자(subscriber == observer)의 onNext handler를 정의하고 호출하지 않는다
// (이 예제에서는 옵저버는 매우 간단하고 onNext handler를 오직 하나만 갖는다)
func myOnNext() { 
    //...do something useful with it 
}

// 옵저버블을 정의하고 호출하지 않는다
let myObservable = someObservable(itsParameters)

// 구독자를 옵저버블에 구독하고 옵저버블을 호출한다
myObservable.subscribe(myOnNext)

//... 다른 작업을 수행한다
```

## onNext, onCompleted, and onError

Subscribe 메서드는 옵저버를 옵저버블에 연결하는 메서드이다. 우리의 옵저버는 다음 메서드 중 일부를 구현한다.

### `onNext`

옵저버블은 옵저버블이 항목을 방출할 때마다 이 메서드를 호출한다. 이 메서드는 옵저버블이 방출한 항목을 매개변수로 사용한다.

### `onError`

옵저버블은 이 메서드를 호출하여 예상 데이터 생성에 실패했거나 다른 오류가 발생했음을 나타낸다. 이후에는 onNext 나 onCompleted 메서드를 더 이상 호출하지 않는다.  onError 메서드는 오류의 원인을 나타내는 파라미터를 갖는다. (비정상적인 구독의 종료를 의미.)

### `onCompleted`

오류가 발생하지 않은 경우 옵저버블은 마지막으로 onNext를 호출한 후 이 메서드를 호출한다. (정상적인 구독의 종료를 의미.)

옵저버블의 계약 조건에 따라 onNext를 0번 이상 호출할 수 있으며, 이후 onCompleted 또는 onError를 호출하여 해당 호출을 따를 수 있지만 둘 다 호출할 수는 없다. 즉, onCompleted와 onError가 구독 중 마지막 메서드 호출이 된다. 관례적으로 이 문서에서 onNext에 대한 호출은 일반적으로 항목의 “방출(emissions)”이라고 하는 반면 onCompleted 또는 onError에 대한 호출은 “알림(Notifications)”이라고 한다.

보다 완전한 구독 메서드(`subscribe`) 호출 예제는 다음과 같다.

```swift
let myOnNext = { item in 
    // ...방출된 항목으로 유용한 작업하기
}
let myError = { throwable in
    // ...호출 실패에 현명하게 반응하기
}
let myComplete  = {
     // ...최종 응답 이후 정리하기
}
let myObservable = someMethod(itsParameters)
myObservable.subscribe(myOnNext, myError, myComplete)

// ...다른 작업 수행
```

## Unsubscribing

일부 ReactiveX 구현에는 `unsubscribe` 메서드를 구현하는 특화된 옵저버 인터페이스인 `Subscriber`가 있다. 이 메서드를 호출하여 구독자(옵저버)가 현재 구독 중인 옵저버블에 더 이상 관심이 없음을 나타낼 수 있다(구독의 취소가 가능하다).  그 다음 옵저버블은 (구독 중인 다른 옵저버가 없는 경우) 방출할 새 항목 생성을 중지하도록 선택할 수 있다.

이 구독 취소의 결과는 옵저버가 구독한 옵저버블에 적용되는 연산자 체인(chain of operators)을 통해 다시 흐르며(cascade → 스트림에 단계적으로 취소가 적용된다는 의미로 이해됨) 이로 인해 체인의 각 링크가 항목 방출을 중지하게 된다. 그러나 구독 취소가 즉시 발생한다고 보장할 수는 없으며 옵저버블의 방출을 관찰할 옵저버가 남아 있지 않은 후에도 잠시 동안 항목을 생성하고 방출하려고 시도할 수 있다.

## Some Notes on Naming Conventions

ReactiveX의 각 언어별 구현에는 고유한 naming 지정 문제가 있다. 구현 사이에는 많은 공통점이 있지만 정식 명명 표준은 정해진 것이 없다.

그에 더해, 이러한 이름 중 일부는 다른 컨텍스트에서 다른 의미를 갖거나 특정 구현 언어의 관용구에서 어색해 보일 수 있다. (Combine과 RxSwift의 타입 / 클래스 이름 차이에서 느낄 수 있음)

예를 들어, `onEvent` naming pattern(예: `onNext`, `onCompleted`, `onError`)이 있다. 일부 컨텍스트에서 이러한 이름은 이벤트 핸들러가 등록되는 방법을 나타낸다. 그러나 ReactiveX에서는 이벤트 핸들러 자체의 이름을 지정한다.

---

# “Hot” and “Cold” Observables

옵저버블은 언제 항목 시퀀스를 방출하기 시작할까? 방출 시점은 옵저버블에 따라 다르다. “Hot” 옵저버블은 생성되자마자 항목 방출을 시작할 수 있으므로 나중에 해당 옵저버블을 구독하는 모든 옵저버는 중간 어딘가에서 시퀀스를 관찰하기 시작할 수 있다. 반면 “Cold” 옵저버블은 항목을 방출하기 전에 옵저버가 구독할 때까지 기다리므로 이러한 옵저버는 처음부터 전체 시퀀스를 관찰하는 것이 보장된다.

ReactiveX의 일부 구현에는 “연결가능한” 옵저버블(Connectable Observable)이라는 것이 있다. 이러한 옵저버블은 옵저버가 구독했는지 여부에 관계없이 `Connect` 메서드가 호출될 때까지 항목 방출을 시작하지 않는다.

---

# Composition via Observable Operators

옵저버블과 옵저버는 ReactiveX의 시작일 뿐이다. 그 자체로는 표준 옵저버 패턴(단일 콜백보다는 이벤트 시퀀스를 처리하는 데 더 적합한 패턴)의 약간의 확장에 지나지 않는다.

진정한 힘은 “reactive extensions”(ReactiveX)와 함께 제공된다. - 연산자(operators)가 옵저버블에서 방출한 항목 시퀀스를 변환, 결합, 조작 및 작업할 수 있게 해준다.

이러한 Rx 연산자를 사용하면 일반적으로 비동기 시스템과 관련된 콜백 핸들러를 중첩하는 단점 없이 콜백의 모든 효율성 이점과 함께 선언적인 방식으로 비동기 시퀀스를 구성할 수 있다.

이 문서는 [다양한 연산자](https://www.notion.so/Operators-fcbf6e948b6549d3b6cbe01a576cbf3b?pvs=21)([원문](https://reactivex.io/documentation/operators.html))에 대한 정보와 사용 예제를 다음 페이지에 그룹화한다.

- **[Creating Observables](https://reactivex.io/documentation/operators.html#creating)**
    
    `Create`, `Defer`, `Empty`/`Never`/`Throw`, `From`, `Interval`, `Just`, `Range`, `Repeat`, `Start`, and `Timer`
    
- **[Transforming Observable Items](https://reactivex.io/documentation/operators.html#transforming)**
    
    `Buffer`, `FlatMap`, `GroupBy`, `Map`, `Scan`, and `Window`
    
- **[Filtering Observables](https://reactivex.io/documentation/operators.html#filtering)**
    
    `Debounce`, `Distinct`, `ElementAt`, `Filter`, `First`, `IgnoreElements`, `Last`, `Sample`, `Skip`, `SkipLast`, `Take`, and `TakeLast`
    
- **[Combining Observables](https://reactivex.io/documentation/operators.html#combining)**
    
    `And`/`Then`/`When`, `CombineLatest`, `Join`, `Merge`, `StartWith`, `Switch`, and `Zip`
    
- **[Error Handling Operators](https://reactivex.io/documentation/operators.html#error)**
    
    `Catch` and `Retry`
    
- **[Utility Operators](https://reactivex.io/documentation/operators.html#utility)**
    
    `Delay`, `Do`, `Materialize`/`Dematerialize`, `ObserveOn`, `Serialize`, `Subscribe`, `SubscribeOn`, `TimeInterval`, `Timeout`, `Timestamp`, and `Using`
    
- **[Conditional and Boolean Operators](https://reactivex.io/documentation/operators.html#conditional)**
    
    `All`, `Amb`, `Contains`, `DefaultIfEmpty`, `SequenceEqual`, `SkipUntil`, `SkipWhile`, `TakeUntil`, and `TakeWhile`
    
- **[Mathematical and Aggregate Operators](https://reactivex.io/documentation/operators.html#mathematical)**
    
    `Average`, `Concat`, `Count`, `Max`, `Min`, `Reduce`, and `Sum`
    
- **[Converting Observables](https://reactivex.io/documentation/operators.html#conversion)**
    
    `To`
    
- **[Connectable Observable Operators](https://reactivex.io/documentation/operators.html#connectable)**
    
    `Connect`, `Publish`, `RefCount`, and `Replay`
    
- **[Backpressure Operators](https://reactivex.io/documentation/operators/backpressure.html)**
    
    특정 흐름 제어 정책을 시행하는 다양한 연산자
    

이 페이지들은 ReactiveX의 핵심 부분은 아니지만 하나 이상의 언어별 구현 또는 선택적 모듈에서 구현되는 일부 연산자에 대한 정보가 포함되어 있다.

## Chaining Operators

대부분의 연산자는 옵저버블에 작용하고 옵저버블을 반환한다. 이를 통해 연산자를 체이닝하여 차례로 적용할 수 있다. 체인의 각 연산자는 이전 연산자의 작업 결과인 옵저버블을 수정한다.

빌더 패턴과 같이, 특정 클래스의 다양한 메서드가 메서드 작업을 통해 해당 객체를 수정하여 동일한 클래스의 항목에서 작동하는 다른 패턴이 있다. 이러한 패턴을 사용하면 유사한 방식으로 메서드를 체이닝(연결)할 수도 있다. 그러나 빌더 패턴에서는 메서드가 체인에 나타나는 순서는 일반적으로 중요하지 않지만(단지 객체의 프로퍼티만 수정하고 같은 타입으로 반환되기 때문) 옵저버블 연산자 순서는 **중요하다**.

옵저버블 연산자 체인은 체인을 생성한 원래의 옵저버블에서 독립적으로 작동하지 않지만 체인에서 바로 앞의 연산자가 생성한 옵저버블에 차례로 작동한다.(반환되는 옵저버블은 항상 같은 타입이 아님에 주의해서 연산자를 체이닝 해야한다)
