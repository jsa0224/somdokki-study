# observe(on:)

**옵저버가 이 옵저버블을 관찰할 스케줄러를 지정하는 연산자**

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/observeOn_Complete_Marble.jpg" width="450">

ReactiveX의 많은 구현은 “스케줄러(`Scheduler`)”를 사용하여 다중 스레드 환경에서 스레드 간 옵저버블의 전환을 관리한다. ObserveOn 연산자를 통해 특정 스케줄러의 옵저버에게 알림을 보내도록 옵저버블에 지시할 수 있다.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/observeOn_Error_Marble.jpg" width="450">

ObserveOn은 `onError` 종료 알림을 받으면 즉시 전달하며, 느리게 소비하는 옵저버가 먼저 알 수 있는 아직 방출되지 않은 항목을 수신할 때까지 기다리지 않는다. 이는 위의 다이어그램에서와 같이 `onError` 알림이 소스 옵저버블에서 방출된 항목보다 우선적으로 전달된다는 것을 의미할 수 있다.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/observeOn+scheduler.jpg" width="450">

`SubscribeOn` 연산자는 `observeOn` 과 비슷하지만 옵저버블이 지정된 스케줄러에서 자체적으로 작동하고 해당 스케줄러에서 옵저버에서 알리도록 지시한다.

기본적으로 옵저버블과 옵저버블에 적용하는 연산자 체인이 작업을 수행하고 `subscribe` 메서드가 호출된 동일한 스레드에서 옵저버에게 알린다. SubscribeOn 연산자는 옵저버블이 작동(opearte)해야 하는 다른 스케줄러를 지정하여 이 동작을 변경한다. ObserveOn 연산자는 옵저버블이 옵저버에게 알림을 보내는 데 사용할 다른 스케줄러를 지정한다.

위 그림에서 볼 수 있듯이 SubscribeOn 연산자는 해당 연산자가 연산자 체인의 호출되는 지점에 상관없이 옵저버블이 작동을 시작할 스레드를 지정한다. 반면에 ObserveOn은 옵저버블이 해당 연산자가 나타난 부분 아래에서 사용할 스레드에 영향을 미친다. 이러한 이유로 특정 연산자가 작동하는 스레드를 변경하기 위해 옵저버블 연산자 체인 동안 다양한 지점에서 ObserveOn을 여러 번 호출할 수 있다.
