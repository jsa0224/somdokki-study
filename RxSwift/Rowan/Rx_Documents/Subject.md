# Subject

Subject는 옵저버와 옵저버블의 역할을 하는 ReactiveX의 일부 구현에서 사용할 수 있는 일종의 bridge 또는 proxy이다. Subject는 옵저버이기 때문에 하나 이상의 옵저버블을 구독할 수 있다. 또한 옵저버블이기 때문에 관찰한 항목을 다시 방출해 넘겨줄 수 있으며, 새 항목을 방출할 수도 있다.

Subject가 옵저버블을 구독하기 때문에 옵저버블이 항목의 방출을 시작하도록 트리거한다(옵저버블이 “Cold”인 경우. 즉, 옵저버블이 항목의 방출을 시작하기 전에 구독을 기다리는 경우). 이 방법으로 생성된 Subject가 원래 “Cold” 옵저버블을 “Hot” 옵저버블로 변형하는 효과를 가질 수 있다.

</br>

## Varieties of Subjects

특정 사용 사례를 위해 설계된 subject에는 네 가지 종류가 있다. 모든 구현에서 이들 모두를 사용할 수 있는 것은 아니며 일부 구현에서는 다른 명명법을 사용한다. (예: RxScala 에서는 이 문서에서 “PublishSubject”라고 하는 것을 간단히 “Subject”라고 함.)

</br>

### 1️⃣ AsyncSubject

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/S.AsyncSubject_Marble_Complete.jpg" width="450">

AsyncSubject는 소스 옵저버블이 방출한 최근 값(오직 최종 값만)을 해당 소스 옵저버블이 완료된 후에 방출한다. (소스 옵저버블이 값을 방출하지 않으면 AsyncSubject도 값을 방출하지 않고 완료된다.)

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/S.AsyncSubject_Marble_Error.jpg" width="450">

또한 동일한 최종 값을 후속 옵저버에게 방출한다. 그러나 소스 옵저버블이 오류와 함께 종료되면 AsyncSubject는 항목을 방출하지 않고 단순히 소스 옵저버블의 오류 알림을 전달한다.

</br>

### 2️⃣ BehaviorSubject

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/S.BehaviorSubject_Marble_complete.jpg" width="450">

옵저버가 BehaviorSubject에 구독하면 소스 옵저버블에서 가장 최근에 방출된 항목(또는 아직 방출되지 않은 경우 시드 / 기본값)을 방출하는 것으로 시작한 다음 소스 옵저버블에서 나중에 방출된 다른 항목을 계속 내보낸다.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/S.BehaviorSubject_Marble_Error.jpg" width="450">

그러나 소스 옵저버블이 오류와 함께 종료되면 BehaviorSubject는 후속 관찰자에게 항목을 내보내지 않고 단순히 소스 옵저버블의 오류 알림을 전달한다.

</br>

### 3️⃣ PublishSubject

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/S.PublishSubject_Marble_Complete.jpg" width="450">

PublishSubject는 구독 이후 소스 옵저버블에 의해 방출된 항목만 옵저버에게 방출한다.

PublishSubject는 생성 즉시 항목을 방출하기 시작할 수 있으며(사용자가 이러한 문제를 방지하기 위한 조치를 취하지 않은 경우), 따라서 subject가 생성된 후 옵저버가 항목에 구독할 때까지 하나 이상의 항목이 손실될 위험이 있다. 소스 옵저버블에서 방출된 모든 항목의 전송을 보장해야 하는 경우 `Create`를 사용하여 해당 옵저버블을 구성해 “cold” 옵저버블 동작을 manual 하게 재도입하거나(이 경우, 항목 방출을 시작하기 전에 모든 옵저버가 구독했는지 확인해야 함) ReplaySubject를 사용하도록 전환해야 한다.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/S.PublishSubject_Marble_Error.jpg" width="450">

소스 옵저버블이 오류와 함께 종료되면 PublishSubject는 후속 옵저버에게 항목을 방출하지 않고 단순히 소스 옵저버블의 오류 알림을 전달한다.

</br>

### 4️⃣ ReplaySubject

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/S.ReplaySubject_Marble_Complete.jpg" width="450">

ReplaySubject는 옵저버가 구독하는 시점에 관계없이 소스 옵저버블에 의해 방출된 모든 항목을 모든 옵저버에게 방출한다.

또한 replay buffer가 특정 크기 이상으로 커지거나 항목이 원래 방출된 이후 지정된 시간이 지나면 이전 항목을 버리는 ReplaySubject 버전도 있다.

만약 ReplaySubject를 옵저버로 사용하는 경우, 여러 스레드에서 `onNext` 메서드(또는 다른 메서드)를 호출하지 않도록 주의해야 한다. 이로 인해 동시에(순차적이지 않게) 호출이 발생하여 옵저버블 계약을 위반하고 생성된 subject에 어떤 항목이나 알림을 먼저 반복해야 하는지에 대한 모호함이 발생할 수 있다. (thread-unsafe 하다는 뜻으로 생각됨)
