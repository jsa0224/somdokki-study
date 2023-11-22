# materialize/dematerialize

- materialize - 방출된 항목과 방출된 항목으로 전송된 알림을 모두 나타낸다.
- dematerialize - 위 프로세스를 반대로 나타낸다.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/materialize_complete.jpg" width="450">

잘 구성된 유한한(finite) 옵저버블은 옵저버의 `onNext` 메서드를 0번 이상 호출한 다음 `onCompleted` 또는 `onError` 메서드를 정확히 한 번 호출한다. Materialize 연산자는 일련의 호출(원본 onNext 알림과 터미널 onCompleted 또는 onError 알림 모두)을 옵저버블이 방출하는 항목으로 변환한다.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/dematerialize_complete.jpg" width="450">

Dematerialize 연산자는 위 프로세스를 반대로 수행한다. 이는 이전에 materialize에 의해 변환된 옵저버블에서 작동하고 방출되는 항목들을 원래 형식으로 반환한다.

### ▪️ 활용

- materialize → onError, onComplete 이벤트 filter → dematerialize 과정을 통해 스트림이 dispose 되는 경우를 방지할 수 있다.
- 마치 driver / relay를 활용하는 것과 같은 효과를 얻을 수 있음.
