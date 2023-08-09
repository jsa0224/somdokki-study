# Subject

## Subject 란
>  옵저버블(Observable)과 옵저버(Observer)를 결합하는 특별한 형태의 객체. 


1. Observable과 Observer 역할 모두 수행:

Subject는 Observable과 Observer의 역할을 모두 수행할 수 있는 객체입니다. 즉, 데이터를 발행하는 Observable 역할을 하면서 동시에 데이터를 구독하는 Observer 역할도 수행할 수 있습니다.

2. Observable과 Subject의 차이: 

Observable은 데이터 스트림을 생성하고, 이에 구독자들이 데이터를 받을 수 있도록 합니다. 반면에 Subject는 Observable의 일종으로, Observable을 확장하여 데이터를 발행하면서 여러 구독자들에게 동시에 전달할 수 있는 능력을 제공합니다.

3. Multicast vs Unicast: 

Subject의 중요한 특징 중 하나는 Multicast 방식입니다. Subject를 여러 구독자에게 공유하여 하나의 데이터 스트림을 여러 구독자들에게 동시에 전달할 수 있습니다. 이에 비해 일반적인 Observable은 Unicast 방식으로, 각각의 구독자에게 독립적인 데이터 스트림이 생성되어 전달됩니다.

![image](https://github.com/juun97/Today-I-Learned/assets/59365211/7ed5d218-11ab-464e-a612-1ecfc77cf348)

</br>

### Observable vs Subject

`Observable`과 `Subject`는 하나의 매우 중요한 차이를 가집니다. Observable은 단지 하나의 함수이기 때문에 어떤 상태도 가지지 않으므로 모든 새로운 Observer에 대해 관찰 가능한 create 코드를 반복해서 실행합니다. 코드는 각 관찰자에 대해 실행되므로 HTTP 호출인 경우 각 관찰자에 대해 호출됩니다. 이로 인해 주요 버그와 비효율이 발생합니다.

반면 Subject는 관찰자 세부 정보를 저장하고 코드를 한 번만 실행하고 모든 관찰자에게 결과를 제공합니다.

#### Observable - unicast
```swift
// --- Observable ---
    let randomNumGenerator1 = Observable<Int>.create{ observer in
        observer.onNext(Int.random(in: 0 ..< 100))
        return Disposables.create()
    }
    
    randomNumGenerator1.subscribe(onNext: { (element) in
        print("observer 1 : \(element)")
    })
    randomNumGenerator1.subscribe(onNext: { (element) in
        print("observer 2 : \(element)")
    })
    
    --------------------print------------------
    
observer 1 : 54
observer 2 : 69
```
>subscribe를 할 때 마다 create 가 진행이 되어서 두 옵저버가 다른 결과를 띄고 있습니다.



#### Subject - multicast
```swift
  // ------ BehaviorSubject/ Subject
    let randomNumGenerator2 = BehaviorSubject(value: 0)
    randomNumGenerator2.onNext(Int.random(in: 0..<100))
    
    randomNumGenerator2.subscribe(onNext: { (element) in
        print("observer subject 1 : \(element)")
    })
    randomNumGenerator2.subscribe(onNext: { (element) in
        print("observer subject 2 : \(element)")
    })

    --------------------print------------------

observer subject 1 : 92
observer subject 2 : 92

```
> subscribe 을 여러 번 해도 create 는 단 한번만 진행되어 두 옵저바가 같은 결과를 띄고 있습니다.

## Subject 종류

### AsyncSubject

<img src= "https://reactivex.io/documentation/operators/images/S.AsyncSubject.png" width = 500>


`AsyncSubject`는 소스 Observable로부터 배출된 마지막 값(만)을 배출하고 소스 Observalbe의 동작이 완료된 후에야 동작합니다. (만약, 소스 Observable이 아무 값도 배출하지 않으면 `AsyncSubject` 역시 아무 값도 배출하지 않습니다.)

<img src= "https://reactivex.io/documentation/operators/images/S.AsyncSubject.e.png" width = 500>

또한 `AsyncSubject`는 맨 마지막 값을 뒤 이어 오는 옵저버에 전달하는데, 만약 소스 Observable이 오류로 인해 종료될 경우 `AsyncSubject`는 아무 항목도 배출하지 않고 발생한 오류를 그대로 전달합니다.

</br>

### BehaviorSubject

<img src= "https://reactivex.io/documentation/operators/images/S.BehaviorSubject.png" width = 500>

옵저버가 `BehaviorSubject`를 구독하기 시작하면, 옵저버는 소스 Observable이 가장 최근에 발행한 항목(또는 아직 아무 값도 발행되지 않았다면 맨 처음 값이나 기본 값)의 발행을 시작하며 그 이후 소스 Observable(들)에 의해 발행된 항목들을 계속 발행합니다.

<img src= "https://reactivex.io/documentation/operators/images/S.BehaviorSubject.e.png" width = 500>

만약, 소스 Observable이 오류 때문에 종료되면 `BehaviorSubject`는 아무런 항목도 배출하지 않고 소스 Observable에서 발생한 오류를 그대로 전달합니다.

</br>

### PublishSubject

<img src= "https://reactivex.io/documentation/operators/images/S.PublishSubject.png" width = 500>

`PublishSubject`는 구독 이후에 소스 Observable(들)이 배출한 항목들만 옵저버에게 배출합니다.

**주의할 점**

1. `PublishSubject`는 생성 시점에서 즉시 항목들을 배출하기 시작합니다. 이로 인해 주제(Subject)가 생성되는 시점과 옵저버가 주제를 구독하기 시작하는 사이에 배출되는 항목들을 놓칠 수 있습니다.

2. 이러한 특성으로 인해 옵저버가 주제를 구독하기 전에 발생한 항목들은 옵저버에게 전달되지 않습니다. 따라서 소스 Observable이 배출하는 모든 항목들의 배출을 보장해야 한다면, "차가운" Observable을 생성하거나 `PublishSubject` 대신 `ReplaySubjec`를 사용하는 것이 좋습니다.


<img src= "https://reactivex.io/documentation/operators/images/S.PublishSubject.e.png" width = 500>

만약, 소스 Observable이 오류 때문에 종료되면 `BehaviorSubject`는 아무런 항목도 배출하지 않고 소스 Observable에서 발생한 오류를 그대로 전달합니다.

</br>

### ReplaySubject

<img src= "https://reactivex.io/documentation/operators/images/S.ReplaySubject.png" width = 500>

- `ReplaySubject`는 옵저버가 구독을 시작한 시점과 관계 없이 소스 Observable(들)이 배출한 모든 항목들을 모든 옵저버에게 배출합니다.
- 생성자 오버로드를 통해 재생 버퍼의 크기를 설정할 수 있으며, 버퍼의 크기에 따라 오래된 항목들을 제거할 수 있습니다.
- 또한 처음 배출 이후 지정한 시간이 경과할 경우 오래된 항목들을 제거하는 기능도 제공합니다.

**주의할 점**
- `ReplaySubject`를 옵저버로 사용할 경우, 멀티 스레드 환경에서는 문제가 발생할 수 있습니다. 동시에 호출되는 `onNext` 또는 기타 알림 메서드들은 순서가 보장되지 않을 수 있습니다. 이는 Observable의 계약을 위반하거나, 어느 항목이 먼저 재생되어야 하는지 모호함을 초래할 수 있습니다.



# Reference
- [ReactiveX - Subject](https://reactivex.io/documentation/ko/subject.html)
- [[RxSwift] Subject - naljin](https://sujinnaljin.medium.com/rxswift-subject-99b401e5d2e5)
- [Understanding rxjs Subjects](https://luukgruijs.medium.com/understanding-rxjs-subjects-339428a1815b)
