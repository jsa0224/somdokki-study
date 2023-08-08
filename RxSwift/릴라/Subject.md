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



# Reference
- [ReactiveX - Subject](https://reactivex.io/documentation/ko/subject.html)
- [[RxSwift] Subject - naljin](https://sujinnaljin.medium.com/rxswift-subject-99b401e5d2e5)
- [Understanding rxjs Subjects](https://luukgruijs.medium.com/understanding-rxjs-subjects-339428a1815b)
