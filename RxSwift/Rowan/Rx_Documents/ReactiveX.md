# ReactiveX

ReactiveX는 관찰 가능한 시퀀스를 사용하여 비동기 및 이벤트 기반 프로그램을 구성하기 위한 라이브러리이다.

이 라이브러리는 옵저버 패턴을 확장하여 데이터 및 이벤트 시퀀스를 지원하고 낮은 수준의 스레드화, 동기화, thread safety, 동시성 데이터 구조 및 비차단 I/O와 같은 문제를 추상화하면서 선언적으로 시퀀스를 함께 구성할 수 있는 연산자를 추가한다.

**옵저버블은 여러 항목의 비동기 시퀀스에 액세스하는 이상적인 방법이 되어 격차를 메움**

|  | single items | multiple items |
| --- | --- | --- |
| synchronous | T getData() | Iterable<T> getData() |
| asynchronous | Future<T> getData() | Observable<T> getData() |

ReactiveX는 때로 “functional reactive programming(기능적 반응형 프로그래밍)”이라고 불리지만 이것은 잘못된 이름이다. ReactiveX는 functional(기능적) 할 수도 있고 reactive(반응적) 할 수도 있지만 “functional reactive programming”은 전혀 다른 것이다. 주요한 차이점 중 하나는 functional reactive programming이 시간이 지남에 따라 지속적으로 변경되는 값에서 작동하는 반면 ReactiveX는 시간이 지남에 따라 방출되는 불연속적인 값에서 작동한다는 것이다. (Functional reactive programming에 대한 보다 정확한 정보는 [Conal Elliott의 작업](https://github.com/conal/essence-and-origins-of-frp)을 참조.)

# Why Use Observables?

ReactiveX 옵저버블 모델을 사용하면 Array와 같은 데이터 컬렉션에 사용하는 것과 동일한 종류의 단순하고 구성 가능한 작업으로 비동기 이벤트 스트림을 처리할 수 있다. 이를 통해 우리를 복잡한 콜백 구조로부터 자유롭게 해줌으로서 우리의 코드를 더 읽기 쉽고 버그가 덜 발생하도록 만들어준다.

## Observables Are Composable

Java Futures와 같은 기술은 단일 수준의 비동기 실행에 사용하기 쉽지만 중첩될 때 사소하지 않은 복잡성을 추가하기 시작한다.

조건부 비동기 실행 흐름을 최적으로 구성하기 위해 Futures를 사용하는 것은 어렵다(또는 각 request의 대기 시간이 런타임에 다르기 때문에 불가능함). 물론 이 작업을 수행할 수 있지만 빠르게 복잡해지거나(이에 따라 오류가 발생하기 쉬움) `Future.get()`을 조기에 차단하여 비동기 실행의 이점을 제거한다.

반면, ReactiveX 옵저버블은 비동기 데이터의 흐름과 시퀀스를 구성하기 위한 것이다.

## Observables Are Flexible

ReactiveX 옵저버블은 단일 스칼라 값(Future 처럼)의 방출뿐 아니라 값의 시퀀스 또는 무한 스트림도 지원한다. `옵저버블`은 이러한 사용 사례에 사용할 수 있는 단일 추상화이다. 옵저버블은 Iterable과 유사하며 이와 관련된 모든 유연성과 우아함을 가지고 있다.

**옵저버블은 synchronous/pull Iterable에 대한 asynchronous/push “dual”이다**

| event | Iterable (pull) | Observable (push) |
| --- | --- | --- |
| retrieve data | T next() | onNext(T) |
| discover error | throws Exception | onError(Exception) |
| complete | !hasNext() | onCompleted() |

## Observables Are Less Opinionated

ReactiveX는 특정 동시성 또는 비동기성 소스에 치우치지 않는다. 옵저버블은 스레드 풀, 이벤트 루프, 비차단 I/O, 액터(예: Akka) 또는 우리의 수요 또는 전문 지식에 맞는 구현을 사용하여 구현될 수 있다. 클라이언트 코드는 기본 구현이 blocking이든 non-blocking이든 상관없이 옵저버블과의 모든 상호 작용을 비동기식으로 처리하며  사용자가 구현을 선택하는 방식으로 처리한다. 

**옵저버블은 어떻게 구현되어있을까?**

---

`public Observable<data> getData();`

---

- Caller와 동일한 스레드에서 동기적으로 작동하는가?
- 별개의 스레드에서 비동기식으로 작동하는가?
- 어떤 순서로든 caller에게 데이터를 반환할 수 있는 여러 스레드로 작업을 분할하는가?
- 스레드 풀 대신 액터(또는 여러 액터)를 사용하는가?
- 비동기 네트워크 액세스를 수행하기 위해 이벤트 루프와 함께 NIO(non-blocking Input/Output)를 사용하는가?
- 콜백 스레드에서 작업 스레드를 분리하기 위해 이벤트 루프를 사용하는가?

---

**옵저버의 입장에서는 전혀 상관이 없다!**

그리고 중요한 것은 ReactiveX를 사용하면 나중에 마음을 바꿀 수 있고, 옵저버블의 consumer를 손상시키지 않고도 옵저버블 구현의 기본 특성을 근본적으로 변경할 수 있다는 점이다.

## Callbacks Have Their Own Problems

콜백은 아무것도 차단하지 않도록 하여 `Future.get()`에 대한 조기 차단 문제를 해결한다. Response가 준비되면 실행되기 때문에 자연스럽게 효율적이다.

그러나 Futures와 마찬가지로 콜백은 단일 수준의 비동기 실행으로 사용하기 쉬운 반면 중첩된 구성으로 사용하기가 어려워진다.

## ReactiveX Is a Polyglot Implementation

ReactiveX는 현재 해당 언어의 관용구를 존중하는 방식으로 다양한 언어로 구현되며 더 많은 언어가 빠른 속도로 추가되고 있다. 

---

# Reactive Programming

ReactiveX는 옵저버블을 filter, select, transform, combine 및 compose 할 수 있는 연산자들을 제공한다. 이를 통해 효율적인 실행 및 구성이 가능하다.

우리는 옵저버블 클래스를 “pull”인 Iterable과 동등한 “push”로 생각할 수 있다. Iterable을 사용하면 consumer는 해당 값이 도착할 때까지 producer와 스레드 블록에서 값을 가져온다. 대조적으로 옵저버블을 사용하면 producer는 값을 사용할 수 있을 때마다 consumer에게 값을 푸시한다. 이 접근 방식은 값이 동기식 또는 비동기식으로 도착할 수 있기 때문에 더 유연하다.

**유사한 고차함수를 Iterable 및 Observable에 적용할 수 있는 방법을 보여주는 예제**

`Iterable`

```swift
getDataFromLocalMemory()
  .skip(10)
  .take(5)
  .map({ s -> return s + " transformed" })
  .forEach({ println "next => " + it })
```

`Observable`

```swift
getDataFromNetwork()
  .skip(10)
  .take(5)
  .map({ s -> return s + " transformed" })
  .subscribe({ println "onNext => " + it })
```

Observable 타입은 Iterable 타입에서 사용할 수 있는 것과 일치하도록 Gang of Four의 옵저버 패턴에 두 가지 누락된 의미 체계를 추가한다.

1. 더 이상 사용할 수 있는 데이터가 없다는 것을 producer가 consumer에게 알릴 수 있는 능력 (Iterable의 foreach 루프는 이러한 경우 정상적으로 완료되고 반환된다. Observable은 Observer의 `onCompleted` 메서드를 호출한다.)
2. Producer가 consumer에게 오류가 발생했음을 알릴 수 있는 능력 (Iterable은 반복 중에 오류가 발생하면 예외를 발생시킨다. Observable은 Observer의 `onError` 메서드를 호출한다.)

이러한 추가로 ReactiveX는 Iterable 및 Observable 타입을 조화시킨다. 그들 사이의 유일한 차이점은 데이터가 흐르는 방향이다. 이제 Iterable에서 수행할 수 있는 모든 작업을 Observable에서도 수행할 수 있기 때문에 이것은 매우 중요하다.
