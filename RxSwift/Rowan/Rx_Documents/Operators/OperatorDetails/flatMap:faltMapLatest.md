# flatMap/flatMapLatest

# flatMap

옵저버블이 방출하는 항목을 옵저버블로 변환한 다음, 변환된 옵저버블의 방출을 단일 옵저버블로 평탄화한다.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/flatMap_complete.jpg" width="450">

*FlatMap* 연산자는 소스 옵저버블이 방출한 각 항목에 지정한 함수를 적용하여 옵저버블로 변환한다. 여기서 해당 함수는 자체적으로 항목을 방출하는 별도의 옵저버블을 반환한다. 그 다음 *flatMap*은 결과 옵저버블의 방출을 병합하여 병합된 결과를 자체 시퀀스로 방출한다.

예를 들어, 이 메서드는 옵저버블 멤버를 직접 가지고 있는 일련의 항목을 방출하는 옵저버블이나 다른 방법을 통해 옵저버블로 변환 가능한 옵저버블을 갖고 있을 때 유용하다. 이러한 경우 *flatMap*을 사용하면 내부 옵저버블이 방출하는 항목의 전체 컬렉션을 방출하는 새로운 옵저버블을 생성할 수 있다.

*FlatMap*은 이러한 옵저버블의 방출을 병합하여 기존의 스트림에 삽입(interleave)할 수 있다는 것을 알아두자.

여러 언어별 구현에는 반환된 옵저버블의 방출을 기존 스트림에 삽입하지 않는 대신 엄격한 순서로 이러한 방출을 방출하는 연산자(종종 *ConcatMap* 또는 이와 유사한 것으로 불림)도 있다.

### 🛑 주의사항

- flatMap 내부에서 만들어지는 새로운 옵저버블 스트림은 기존의 스트림과는 별도의 스트림이라고 생각해야 한다.
- 예를 들어 flatMap을 통해 새로운 옵저버블의 방출 항목을 기존의 스트림에 삽입하는 경우, 새로운 옵저버블에서 onError 또는 onCompleted 이벤트가 발생하면 기존 스트림에서 catch, materialize 연산자를 사용하더라도 스트림이 dispose 되는 것을 방지할 수 없다.

```swift
// 잘못된 onError 방지
let kakaoLogIn = input.kakaoLogInButtonTapped
    .withUnretained(self)
    .flatMap { owner, _ in
        owner.kakaoSocialUserDataUseCase.logIn()   // 새로운 스트림
    }
        .materialize()
    .do(onNext: { event in    // 기존 스트림에서 발생하는 error 이벤트만 받아진다.
        if let error = event.error {
            owner.errorRelay.accept(error)
        }
    })
    .filter { $0.error == nil }

// 정상적으로 onError 방지하기
let kakaoLogIn = input.kakaoLogInButtonTapped
    .withUnretained(self)
    .flatMap { owner, _ in
        owner.kakaoSocialUserDataUseCase.logIn()
            .materialize()
            .do(onNext: { event in
                if let error = event.error {
                    owner.errorRelay.accept(error)
                }
            })
            .filter { $0.error == nil }
    }
```

- 또한, flatMap의 기능은 스트림을 병합하는 것과 같기 때문에 RxSwift - merge.swift 파일에 병합 연산자들과 같이 정의되어 있다.

# flatMapLatest

기존 스트림의 각 항목을 새로운 옵저버블 시퀀스로 투영한 다음, 스트림 내부의 옵저버블 시퀀스 중 가장 최근 값을 방출한 시퀀스에서만 값을 생성하는 스트림으로 변환한다.

 `map` 연산자 + `switchLatest` 연산자의 조합으로 구성되어 있다.

- parameter `selector`: 방출되는 항목에 적용할 변환 함수
- `returns`: 각 소스 항목에서 변환 함수를 호출하여 생성된 옵저버블 시퀀스로부터 임의의 시점에서 가장 최근에 수신된 내부 옵저버블 시퀀스의 항목을 방출하는 옵저버블 시퀀스

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/flatMapLatest_complete.jpg" width="450">

- 간단히 생각하면, flatMapLatest 내부에서 새로운 옵저버블이 생성되면 그 전의 옵저버블은 버리고 새로운 것만을 관찰하게 되는 것이다.
