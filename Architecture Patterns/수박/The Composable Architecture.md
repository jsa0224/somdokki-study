# TCA

## 구성요소

---

### A. 핵심 구성요소

---

1. State
    - 값 → Feature를 수행하거나 UI를 그릴 때 사용
2. Action
    - 이벤트 → Reducer의 트리거(user action, notification, etc)
3. Reducer
    - (PreviousState, Action) → Effetct: NextState
4. Store
    - Container
    - typealias StoreOf = Store<R.State, R.Action> where R: Reducer
        
        ⇒ StoreOf<Feature>
        
    - Action → Reduce, Effect → Store → State →  Action…
    - 처음 상태와 Reducer를 주입받는데, 새걸 만들 수도, 전달할 수도 있음

<br>

### B. 맵핑을 위한 요소

---

- ViewStore
    - TCA의 구성요소와 프레젠터를 맵핑시켜주는 요소
    - 예를 들면 touch 이벤트를 onTouch라고 하는 Reducer.Action으로 바꿔서 Reducer에게 넘겨줌
    - 맵핑을 위해 store를 주입받는다
    - send, yield등 지원
- WithViewStore
    - 구조체
    - 스유에서 ViewStore를 직접 사용하는 건 드물고 보통 얘를 사용
- ForEachStore, IfLetStore, SwitchStroe, NavigationStackStore
    - 상동. 인터페이스는 조금 다른데 딱 설탕문법 정도
- 모델을 뷰로 연길시키기 위한 것들로 인식하면 편함,  viewStore의 기능을 호출하기 위해 쓰는 껍데기들

---

- Binding
    - 나이브하게는 viewStore.binding(get:, send:)을 씀
    - 뷰의 값들을 다시 스토어에게 보내는, 트리거를 위한 용도
- BindingState, BindingAction, BindingReducer
    - State와 Action이 너무 많다면 위 propertyWrapper들을 이용해서 압축할 수도 있음
    - 결국 reduce는 해야하지만… Reducer를 쪼개면 안써도 될 것 같은데, 일단 기억은 해두면 좋을듯

<br>
<br>

## 지향점

---

1. 상태 관리(Redux)
2. 합성
3. 독립성
4. 테스트
5. 인간친화적인 인터페이스

…

라고 하는데 결국 함수형을 쓰기 위한 용도인듯 ⇒ 스유랑 매우 잘 어울림.

+ 컨텍스트가 잘 구분이 되고 직관적이어서 좋음. 설탕도 잘쳐져있음.