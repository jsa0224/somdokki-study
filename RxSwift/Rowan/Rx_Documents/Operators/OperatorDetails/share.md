# share

단일 구독(subscription)을 기본 시퀀스에 공유하고 구독 즉시 버퍼의 요소를 replay하는 observable sequence를 반환하는 operator.

본 Operator는 아래 operator와 같다.

- `whileConnected`

```swift
// 각 connection에는 replay event를 저장하는 고유한 subject 인스턴스가 있다.
// Connection은 서로 분리될 것이다.

source.multicast(makeSubject: { Replay.create(bufferSize: replay) }).refCount()
```

- `forever`

```swift
// 한 subject는 source의 모든 connection에 대한 replay event를 저장한다.
// Connection은 서로 분리되지 않을 것이다.

source.multicast(Replay.create(bufferSize: replay)).refCount()
```

본 operator는 대부분의 일반적인 작업에 최적화된 연산자 버전을 사용한다.

- parameter **replay**: ****재생 버퍼의 최대 element 수
- parameter scope: subject 공유의 lifetime scope. 자세한 내용은 `SubjectLifetimeScope` 열거형을 참조.
- 참고: [shareReplay operator on reactivex.io](http://reactivex.io/documentation/operators/replay.html)
- returns: Source sequence를 멀티캐스트하여 생성된 시퀀스의 element를 포함하는 observable sequence
