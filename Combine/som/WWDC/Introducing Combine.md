# Introducing Combine

## Combine이 등장하게 된 계기

![스크린샷 2023-08-08 오후 3 23 24](https://github.com/jsa0224/somdokki-study/assets/94514250/9a79d666-01dd-4a6e-a724-fa4ef473faae)

위의 예제에서는 다양한 비동기 작업이 일어나고 있다.

1. 사용자 이름을 입력할 때 Target/Action을 사용해서 사용자가 타이핑하고 있음에 대해 알림을 받는다. 추가로 사용자가 타이핑을 잠시 멈춘 후 타이머를 사용해 잠시 기다려서 서버에 네트워킹 요청을 과도하게 보내지 않도록 한다.
2. KVO를 사용해서 이런 asynchronous operation 후에 업데이트 된 내용을 감지한다.
3. URL session request로 요청한 것에 응답값을 받아 UI를 업데이트 한다.

![스크린샷 2023-08-08 오후 3 24 24](https://github.com/jsa0224/somdokki-study/assets/94514250/febf5c1a-aaca-4f37-9eb0-38091ea5c41d)

하지만 이런 비동기 작업을 Cocoa SDK의 위 요소들로 하게 되면 closure나 completion block을 가지게 되어 꽤 복잡한 코드가 탄생한다. 이러한 문제로 Combine이 등장했는데, Combine은 위의 인터페이스들을 완전히 대체하기 보다는 인터페이스들의 공통점을 기반으로 둔다.

## Combine의 특징

![스크린샷 2023-08-08 오후 3 24 40](https://github.com/jsa0224/somdokki-study/assets/94514250/c2e644bf-2b0e-45da-8034-40b0e83c6843)

- **Generic** : Generic을 사용해 다양한 타입에 비동기 인터페이스를 적용 가능
- **Type Safe** : Combine은 type safe하기에 컴파일 시간에 에러를 확인 가능
- ****Composition First**** : 간단하고 이해하기 쉽지만, 결합하여 더 많은 것을 만들기 가능 → 요소들을 조합하여 더 복잡한 무언가를 쉽게 만들 수 있게 디자인함!
- ****Request Driven**** : Request 기반이라 앱의 메모리 사용과 성능을 더 잘 관리

## Combine의 핵심 개념

### Publisher

![스크린샷 2023-08-08 오후 3 25 30](https://github.com/jsa0224/somdokki-study/assets/94514250/4b3ee7c7-1e9e-4b96-b978-181f55325a69)

Publisher는 Combine API의 선언적 부분이다. 값과 에러가 **어떻게 생성되는 지를 정의**한다. (값과 에러를 직접 생성하는 것 ❌)

즉 설명하는 역할이며, 값 타입으로 Swift에서는 struct를 사용한다.

Publisher는 Subscriber 등록도 허용한다. 이를 통해 시간에 따라 값들을 받게 된다.

#### Publisher 프로토콜

![스크린샷 2023-08-08 오후 3 25 45](https://github.com/jsa0224/somdokki-study/assets/94514250/8739de47-1132-4835-909a-ae3c09c81443)

- `Output`: 생성되는 값의 종류
- `Failure`: 오류의 종류(Publisher가 오류를 생성할 수 없는 경우 associatedtype에 대해 Never 사용 가능)
- `subscribe()`: Subscriber의 Input이 Publisher의 Output과 일치하고, Subscriber의 Failure이 Publisher의 Failure와 일치해야한다.

#### Publisher의 사용 예시

![스크린샷 2023-08-08 오후 3 26 00](https://github.com/jsa0224/somdokki-study/assets/94514250/22097c01-2dfd-4e72-8cae-ae7bdfcf40e5)

이 예시는 NotificationCenter의 새로운 Publisher이다. Struct 타입이고 Output 타입은 Notification이고, Failure 타입은 Never이다. NotificationCenter와 Notification.Name, object가 초기화된다. 

이 예시에서 보는 것처럼 기존 NotificationCenter를 대체하지 않고 Combine을 NotificationCenter에 적용하는 것이다. 

### Subscriber

![스크린샷 2023-08-08 오후 3 26 15](https://github.com/jsa0224/somdokki-study/assets/94514250/55535793-f47c-49e5-9178-01048bc3068b)

Publisher의 반대편으로, 서로 짝이라고 볼 수 있다.

Publisher가 제한적인 경우 completion을 포함하여 값을 받는다. 

Subscriber는 일반적으로 값을 수신하면 상태를 변경하고 작동하기 때문에 Swift에서는 참조 타입을 사용한다. → 클래스

### Subscriber 프로토콜

![스크린샷 2023-08-08 오후 3 26 57](https://github.com/jsa0224/somdokki-study/assets/94514250/c160af92-9827-4ed7-b265-342b6250db60)

- `Input`
- `Failure`: 오류의 종류(Subscriber가 오류를 받을 수 없는 경우 Never 사용 가능)
- `receive(subscription:)`: `subscription`은 Subscriber가 Publisher에서 Subscriber로 이동하는 데이터 플로우를 어떻게 제어하는지를 나타낸다.
- `receive()`: Input 받기
- `receive(completion:)`: 연결된 Publisher가 제한적이라면 `completion(Finished/Failure)`를 받을 수 있다.

#### Subscriber의 사용 예시
![스크린샷 2023-08-08 오후 3 27 12](https://github.com/jsa0224/somdokki-study/assets/94514250/d3e81560-adb2-4df4-8ac0-6349466eb9e0)

Assign은 클래스이며, 객체와 type safe한 keyPath로 초기화된다. Assign은 Input이 들어오면 그 객체의 프로퍼티에 작성한다. Swift에서는 단순히 프로퍼티에 값을 쓸 때 에러를 핸들링할 방법이 없기 때문에 Assign의 Failure 타입을 Never로 설정했다.

### Publisher와 Subscriber 어떻게 같이 동작함?

![스크린샷 2023-08-08 오후 3 27 33](https://github.com/jsa0224/somdokki-study/assets/94514250/28ff31e2-9bfe-4eac-826e-8403bbff45b2)

1. 먼저 Subscriber를 가지고 있는 controller 객체나 다른 무언가가 있을 것이고, 그 객체는 subscribe를 호출해서 Subscriber를 Publisher에 붙인다.
2. 그러면 Publisher는 Subscriber에게 subscription을 보내는데, 이 subscription으로 Subscriber는 Publisher에게 특정 수나 제한되지 않은 수의 값을 받겠다고 Publisher에 요청을 보낼 것이다.
3. 이제 Publisher는 Subscriber에게 값을 자유롭게 보낼 수 있다.
4. 마지막으로 만약 Publisher가 제한되어 있다면 Completion이나 Error를 보낸다.

✅ Publisher는 유튜버고 나는 subscriber다. 처음에 나는 유튜버의 채널을 구독(subscribe 호출)해서 구독자가 된다. 그러면 유튜버는 나한테 실시간 채팅 권한(subscription)을 주고, 나는 이 실시간 채팅 권한으로 유튜버한테 실시간 채팅(request 호출)을 남길 수 있다. 그래서 내가 원하는 게임을 해달라고 유튜버에게 요청을 하면 유튜버는 내가 원하는 게임 방송을 보여주는 것이다.(receive)

#### Publisher와 Subscriber의 사용 예시

![스크린샷 2023-08-08 오후 3 27 48](https://github.com/jsa0224/somdokki-study/assets/94514250/47bd7ba8-a1a2-4452-af4d-841b5a634e0e)

- 이 코드에서 하고자 하는 것: 학생들이 졸업할 때 알림을 받아 모델 객체의 값을 업데이트

`NotificationCenter.Publisher` 타입과 `Subscribers.Assign` 타입은 동일하지 않기 때문에 `subscribe()`를 호출하게 되면 컴파일 에러가 발생한다. 

![스크린샷 2023-08-08 오후 3 28 49](https://github.com/jsa0224/somdokki-study/assets/94514250/cd5c9bab-f075-4f6c-aec0-bedce3d58b6d) ![스크린샷 2023-08-08 오후 3 29 19](https://github.com/jsa0224/somdokki-study/assets/94514250/219b623a-fc5a-4bf2-b67e-92718ff0b0d8)

NotificationCenter는 notification을 만드는데, Assign는 정수 값을 받기를 예상하고 있기 때문이다. 그래서 위와 같이 중간에서 notification과 integer를 변환할 무언가가 필요한데, 이것이 **Operator**다.

### ****Operator****

Operator는 Publisher 프로토콜을 채택하고 있는 Publisher다. 그래서 Operator도 선언적이며 값 타입이다. Operator가 하는 역할은 값을 변환하거나, 값을 추가하거나 삭제하거나 다른 연산들에 대한 행위를 정의한다.

#### ****Operator의 사용 예시****

![스크린샷 2023-08-08 오후 3 30 26](https://github.com/jsa0224/somdokki-study/assets/94514250/273616c4-04f8-455c-a1d9-0cb29515d7fe)

Map은 연결할 upstream과 어떻게 upstream의 결과를 자신의 output으로 변환하는지에 대한 정보로 초기화 된다. Map은 스스로 Failure를 생성하지 않기 때문에 단순히 upstream의 Failure 타입을 그대로 전달한다.

### ****Publisher-Operator-Subscriber****
![스크린샷 2023-08-08 오후 3 30 39](https://github.com/jsa0224/somdokki-study/assets/94514250/522ec7e1-d2d2-4685-b6c4-75192a2f0411)

위에서 봤던 코드에서 publisher와 subscriber는 그대로 사용하고, `converter`가 추가됐다. 코드를 보면 `converter`는 `graduationPublisher`와 연결되어 있고 클로저를 가지고 있다. 클로저는 notification을 받는데, userInfo에서 “NewGrade”라는 키를 가진 값을 받는다.

그리고 이 키에 값이 있다면 정수로 변환하고, 없거나 정수가 아니라면 default 값 0를 리턴한다. 그래서 중요한 것은 이 `converter`가 notification을 받아 integer로 변환시켜준다는 것이고, 이를 또 subscriber에 연결하는 모든게 연결이 다 잘 된 것이다.

그런데 위 코드의 문법을 보면 꽤 길고 쓸데없는 단어가 있다고 생각할 수 있는데, 더 **유연한 문법**으로도 작성할 수 있다.

### ****New Syntax****
![스크린샷 2023-08-08 오후 3 30 55](https://github.com/jsa0224/somdokki-study/assets/94514250/a7698b38-679d-4b2b-8d3c-2fa2a87b3b3e)

먼저 NotificationCenter Publisher를 생성한다. 그리고 notification을 받으면, 클로저를 사용해서 map을 하고, Merlin의 grade property에 assign한다.

위 코드의 문법을 보면 선형적이고, 단계마다 어떻게 진행될 수 있는 지 알기 쉽기 때문에 이해하기도 쉽다. WWDC에서는 이런 단계별 문법이 Combine을 사용하는데 핵심이라고 했다. 각 단계는 다음 명령어를 연속적으로 설명하고, 값들은 처음에 Publisher에서 생성되고, 여러 개의 Operator를 거쳐 마지막에는 Subscriber로 들어온다.

그리고 이런 Operator가 많은데, 이를 **Declarative Operator API**라고 부른다고 한다.

### **Declarative Operator API**

![스크린샷 2023-08-08 오후 3 31 35](https://github.com/jsa0224/somdokki-study/assets/94514250/f7b44537-8329-4f80-a94c-509980326b88)

- Functional transformations: Map, Filter, Reduce 등등
- List operations: Publisher의 몇 번째 요소 이런 것들을 알 수 있음 등등
- Error handling: 에러가 발생하면 이를 default나 다른 값으로 값을 설정
- Thread or queue movement: 무거운 작업을 백그라운드 쓰레드로 옮기거나 UI작업을 메인 쓰레드로 옮김
- Scheduling and time: loop, dispatch queue, timer, timeouts 등등을 포함

이러한 연산자가 너무 많기 때문에 이러한 연산자 사이를 탐색하는 방법에 대해 생각하는 것이 약간 어려울 수 있다. 그래서 WWDC에서는 Combine에 대한 핵심 디자인 원칙인 Composition으로 돌아가는 것을 권장한다. 

### Composition

![스크린샷 2023-08-08 오후 3 31 50](https://github.com/jsa0224/somdokki-study/assets/94514250/e510f7d1-a206-413e-a7f8-c74caa5f8029)

많은 작업을 수행하는 몇 가지 연산자를 제공하는 대신 각각 조금씩만 수행하는 많은 연산자를 제공하므로 이해하기 쉽다. 따라서 이러한 모든 연산자 사이를 탐색할 수 있도록 기존 Swift Collection API에서 해당 이름에 대한 영감을 얻었다. 

![스크린샷 2023-08-08 오후 3 32 09](https://github.com/jsa0224/somdokki-study/assets/94514250/0b51cd38-594a-480b-8061-72d576bc7a79)

위의 사분면 그래프는 한쪽에 동기식 API가 있고 다른 쪽에 비동기식 API가 있다. 상단에는 단일 값이 있고 하단에는 많은 값이 있다. 

||Swift|Combine|
|:---:|:---:|:---:|
|단일 값|Int|Future|
|많은 값|Array|Publisher|

#### compactMap 예시

클로저가 nil을 반환하도록 허용한 다음 nil 값을 필터링

![스크린샷 2023-08-08 오후 3 34 55](https://github.com/jsa0224/somdokki-study/assets/94514250/706054fb-817c-45e2-bbf9-09fbe68fd73e)
                                                                                                                                
![스크린샷 2023-08-08 오후 3 35 22](https://github.com/jsa0224/somdokki-study/assets/94514250/1bd61c1c-4824-4c59-a1b3-6009baa75aa0)

클로저에서 nil을 반환하면 compactMap이 이를 필터링하여 스트림 아래로 더 이상 진행되지 않도록 함

#### filter 예시
![스크린샷 2023-08-08 오후 3 36 08](https://github.com/jsa0224/somdokki-study/assets/94514250/a3c8ded5-88b0-46c4-becc-9cdea9a8ec46)

Filter를 사용해서 특정 값 이상인 값들만 거를 수 있고, prefix를 사용해서 앞의 몇 개의 값만 받을 수 있도록 설정할 수 있다. 이런 연산들은 array에서도 같은 기능을 한다. 위 코드에서는 `prefix(3)`를 통해 3개의 값을 받으면, upstream을 취소하고 downstream에 completion을 보내게 된다.

### Combining Publisher

![스크린샷 2023-08-08 오후 3 36 23](https://github.com/jsa0224/somdokki-study/assets/94514250/fb972a85-6467-4684-b3ac-504885b9439e)

Map과 filter는 굉장히 좋은 API인데, 주로 synchronous항 연산들에 사용된다. Combine은 이 때 asynchronous한 영역에서 빛을 발한다.

이런 상황에서 유용한 operator가 있는데, zip과 combineLatest다.

#### zip

예를 들어 지팡이를 발급받기 위해 3가지 비동기 작업이 완료될 때까지 기다려야 하는 앱이 있다고 해보자. 세 비동기 작업이 완료된 후에 활성화 되는 버튼이 있어서 이 버튼을 눌러야 다음 단계로 넘어갈 수 있다. 이런 작업을 zip으로 할 수 있다.

Zip은 여러 upstream input들을 하나의 tuple로 변환한다.

![스크린샷 2023-08-08 오후 3 37 05](https://github.com/jsa0224/somdokki-study/assets/94514250/20bdd27b-1dff-4a48-a336-e4550b21507b)

예를 들어 첫 번째 publisher가 A를 생성하고, 그리고 두 번째 publisher가 1을 생성하면, 이제 tuple을 생성해서 이 값을 downstream subscriber로 전송할 수 있는 것이다. 그림을 보면 (A, 1)이라는 tuple이 생성되어 downstream에 전송되고 있다.
![스크린샷 2023-08-08 오후 3 37 27](https://github.com/jsa0224/somdokki-study/assets/94514250/52755220-da34-4b16-81df-67a1a04e0347)

위에서 얘기한 지팡이를 발급받기 위해 세 비동기 작업을 기다려야 하는 앱이다. 코드를 보면 세 개의 upstream을 가진 zip을 사용해서 세 개의 비동기 작업이 완료되기를 기다리고 있다. 그리고 각 연산은 Boolean 값을 리턴한다. 그래서 이 세 Boolean 값을 tuple로 map을 하고, `isEnabled` 프로퍼티에 할당하는 것이다.

#### ****CombineLatest****

![스크린샷 2023-08-08 오후 3 37 46](https://github.com/jsa0224/somdokki-study/assets/94514250/cb4a5268-c693-4524-9162-d901d6b0a15f)

이제 지팡이를 발급받았다! 

그런데 발급 받고 나서 지팡이를 사용하기 전에 동의해야 하는 이용 약관들이 존재한다. 

세 가지 약관을 다 동의해야 넘어갈 수 있는데, 가령 세 약관을 다 동의했다가 아래와 같이 한 약관을 다시 동의하지 않음으로 변경했다고 해보자.

이러면 하단의 play 버튼은 활성화 되어 있다가 다시 비활성화되어야 할 것이다. 이런 작업을 combineLatest로 할 수 있다.

![스크린샷 2023-08-08 오후 3 38 06](https://github.com/jsa0224/somdokki-study/assets/94514250/50b6c4ac-f576-4458-8d54-1e3945f635b6)

Zip과 똑같이, 여러 upstream input을 받아 하나의 값으로 만든다. 하지만 Zip과는 다르게, upstream들 중 어떤 것에서만 값이 들어와도 진행할 수 있다는 것이다. Zip은 모든 upstream에서 값이 들어와야지 진행되었는데, combineLatest는 한 upstream에서 값이 내려오든 여러 upstream에서 값이 내려오든 상관하지 않는 것이다.

이를 위해 각 upstream에서 받았던 가장 최신의 값을 저장하고 있고, 이를 하나의 downstream 값으로 변환할 closure로 구성되어 있다.

![스크린샷 2023-08-08 오후 3 38 18](https://github.com/jsa0224/somdokki-study/assets/94514250/7b221c5b-6f90-4225-b37b-e464f2b7fb1f) ![스크린샷 2023-08-08 오후 3 38 28](https://github.com/jsa0224/somdokki-study/assets/94514250/685d37ac-ba6a-486b-950c-bbabeff5d7b1)

마찬가지로 예시를 들어서 두 개의 upstream이 있고 각 publisher로부터 A, 1을 받았다고 해보자. 그러면 (A, 1)이라는 tuple을 생성해 downstream으로 보냈을 것이다. 그리고 이후 두 번째 publisher가 2를 내려보내면, 기존에 두 번째 publisher한테 받았던 1을 2로 대체하고, 새로운 (A, 2) 튜플을 만들어 downstream에 보내게 된다. 그래서 upstream 중 하나라도 값이 변하게 되면 새로운 이벤트가 발생하게 된다.

![스크린샷 2023-08-08 오후 3 38 49](https://github.com/jsa0224/somdokki-study/assets/94514250/5273b1f6-61fc-448b-bb01-911ae0b8a5dd)

다시 돌아가서 이용 약관 동의하는 부분을 보자. 세 개의 upstream을 갖는 CombineLatest를 사용했고, 변할 수 있는 세 개의 boolean 값을 하나의 Boolean 값으로 만들어 play 버튼의 isEnabled 프로퍼티에 할당하고 있다.

## Try it

당장 비동기적인 모든 작업들을 combine으로 대체할 수는 없다. 따라서 지금부터 적용할 수 있는 부분을 찾아 Combine을 사용하면 된다.

예를 들어서 NotificationCenter를 사용하고 있고, notification을 받는데 무언가를 할지 말지를 결정해야 한다면 Filter를 사용할 수있다. 그리고 여러 asynchronous operation의 결과가 중요하다면 zip을 사용할 수 있다. 그리고 URL Session을 사용해서 데이터를 받고 이를 JSON Decoder를 사용해서 나만의 객체로 만들어야 한다면 Decode를 사용할 수도 있다.

# 참고 링크
- [WWDC19 - Introducing Combine](https://developer.apple.com/videos/play/wwdc2019/722/?time=1098)
