# 🛑 Observable 복수 구독 시 주의점

RxSwift를 활용해 View - ViewModel을 바인딩하는 과정에서 View의 다양한 Input들을 조합 / 변환하여 사용하게 된다. 여기서 Observable을 여러번 구독시키는 경우가 있다. 이러한 경우, 코드 작성 시 직접 겪었던 문제를 통해 발생할 수 있는 **문제점**에 대해 살펴보고 주의사항을 기록하고자 한다.

문제가 발생했던 코드

```swift
// ViewModel

struct Input {
    let title: Driver<String>
    let date: Driver<Date>
    let body: Driver<String>
    let rightBarButtonTapped: Driver<Void>
    let leftBarButtonTapped: Driver<Void>
}
    
struct Output {
    let canSave: Driver<Bool>
    let save: Observable<Void>
    let canEdit: Driver<Bool>
    let dismiss: Driver<Void>
}

func transform(_ input: Input) -> Output {
    let contents = Driver.combineLatest(input.title, 
                                                                                             input.date, 
                                                                                             input.body)
        
    let canSave = projectContents
        .map { title, _, body in
            return title != "" || body != ""
        }
        .asDriver(onErrorJustReturn: false)
    
// 문제가 발생한 Observable
    let save = input.rightBarButtonTapped
        .asObservable()
        .withLatestFrom(projectContents)
        .map { title, date, body in
            return Contents(title: title,
                            date: date,
                            body: body,
                            id: UUID())
        }
        .map { [unowned self] project in
            try useCase.create(project: project)
            delegate?.projectCreated()
        }
    
    let canEdit = Driver.just(true)
    
    let dismiss = Driver.merge(input.leftBarButtonTapped,
                               projectSave.asDriver(onErrorJustReturn: ()))
        .do(onNext: navigator.toMain)
    
    return Output(canSave: canSave,
                  save: projectSave,
                  canEdit: canEdit,
                  dismiss: dismiss)
}
```

ViewModel의 Input은 차례로 설명하면 아래와 같다.

- title: UITextField.text
- date: UIDatePicker.date
- body: UITextView.text
- rightBarButtonTapped: UIBarButtonItem tap event
- leftBarButtonTapped: UIBarButtonItem tap event

위 코드에서 문제가 발생했던 부분은 `save` 이다.

`save`는 오른쪽 네비게이션 바 버튼의 tap 이벤트를 트리거로 하여 view에서 받아온 Input을 useCase를 통해 DB에 저장하는 스트림이다.

`dismiss` 의 경우 왼쪽, 오른쪽 네비게이션 바 버튼의 tap 이벤트를 merge하여 화면을 dismiss하는 스트림이다.

두 스트림은 모두 Output으로 반환되어 ViewController에서 구독이 일어난다.

---

**🔎 문제점**

- rightBarButton의 tap event가 발생했을 때, `save` 스트림이 2회 실행된다.
- 이에 따라 DB에 동일한 Entity가 2회 저장된다.

 

❗️ 문제가 발생한 이유(주의사항): **Observable은 Subscribe(구독) 횟수만큼 execute(실행)된다!!**

`save` 는 `dismiss` 에서도 값을 방출하고 있기 때문에 view controller에서 `save`와 `dismiss`를 모두 구독하게 되면 2회 구독하는 것과 같았다. 이러한 이유로 DB에 데이터가 2회 저장되었던 것이다.

**🛠️ 해결방안**

- Operator - `[share](https://www.notion.so/share-b818b49e65bf40609debaf601bf15bd1?pvs=21)` 활용하기

```swift
    let save = input.rightBarButtonTapped
        .asObservable()
        .withLatestFrom(projectContents)
        .map { title, date, body in
            return Contents(title: title,
                            date: date,
                            body: body,
                            id: UUID())
        }
        .map { [unowned self] project in
            try useCase.create(project: project)
            delegate?.projectCreated()
        }
                .share()
```

`share` 연산자는 ReactiveX document의 `Replay` 연산자를 참고하여 이해하면 좋은 것 같다.

Replay는 observable이 항목을 방출하기 시작한 후에 구독하더라도 모든 observer가 동일한 emitted item sequence를 볼 수 있도록 보장하는 operator이다.

`share`는 이를 활용하여 **단일 구독**을 공유할 수 있도록 만들어주는 operator라고 이해했다.

save 스트림 최하단에 share operator를 붙여 단일 구독을 공유할 수 있도록 하여 여러번 구독하더라도 1회만 실행되도록 하여 문제를 해결할 수 있었다. 👍

즉, Observable을 복수 구독할 때 해당 공유되는 스트림이 구독 횟수만큼 실행되지 않고 1회만 실행될 것을 원한다면 `share` operator를 적용해야 한다.
