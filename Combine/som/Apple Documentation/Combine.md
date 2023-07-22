# ****Combine****

이벤트 처리 연산자를 결합하여 비동기 이벤트 처리를 사용자 지정한다.
![스크린샷 2023-07-22 오후 7 18 14](https://github.com/jsa0224/somdokki-study/assets/94514250/f690fdc2-0e28-4b1d-906f-43ac0f1c2c89)

# ****Overview****

Combine 프레임워크는 시간 경과에 따라 값을 처리하기 위한 선언적 Swift API를 제공한다. 이러한 값은 다양한 종류의 비동기 이벤트를 나타낼 수 있다. Combine은 게시자가 시간이 지남에 따라 변경될 수 있는 값을 노출하도록 선언하고 Subscriber는 Publisher로부터 해당 값을 수신하도록 선언한다.

- [`Publisher`](https://developer.apple.com/documentation/combine/publisher) 프로토콜은 시간이 지남에 따라 일련의 값을 전달할 수 있는 타입을 선언한다. 게시자는 업스트림 Publisher로부터 받은 값에 따라 조치를 취하고 이를 다시 게시하는 *operators*가 있다.
- Publisher 체인의 끝에서 Subscriber는 요소에 대해 작동한다. Publisher는 Subscriber가 명시적으로 요청한 경우에만 값을 내보낸다. 이렇게 하면 Subscriber 코드가 연결된 Publisher로부터 이벤트를 받는 속도를 제어할 수 있다.

여러 Foundation 타입은 [`Timer`](https://developer.apple.com/documentation/foundation/timer), [`NotificationCenter`](https://developer.apple.com/documentation/foundation/notificationcenter) 및 [`URLSession`](https://www.notion.so/URLSession-04628793ca4641eb91789013406b4609?pvs=21)을 포함하여 Publisher를 통해 기능을 노출한다. Combine은 또한 Key-Value Observing을 준수하는 모든 프로퍼티에 대한 built-in publisher를 제공한다. 

여러 Publisher의 출력을 결합하고 상호 작용을 조정할 수 있다. 예를 들어 텍스트 필드 Publisher의 업데이트를 구독하고 텍스트를 사용하여 URL 요청을 수행할 수 있다. 그런 다음 다른 Publisher를 통해 응답을 처리하고 이를 사용하여 앱을 업데이트할 수 있다. 

Combine을 채택하면 이벤트 처리 코드를 중앙 집중화하고 중첩 클로저 및 규칙 기반 콜백과 같은 번거로운 기술을 제거하여 코드를 더 쉽게 읽고 유지 관리할 수 있다.
