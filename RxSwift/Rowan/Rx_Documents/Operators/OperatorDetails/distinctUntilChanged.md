# distinctUntilChanged()

옵저버블이 방출한 중복 항목을 억제하는 연산자.

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/distinctUntilChanged_Marble.jpg" width="450">

`distinct` 연산자는 아직 방출되지 않은 항목만 허용하여 옵저버블을 필터링한다.

일부 구현에는 두 항목이 “고유”하다고 간주되는 기준을 조정할 수 있는 변형이 존재한다. 일부는 항목의 고유성을 직전 항목과만 비교하여 시퀀스에서 연속적인 중복 항목을 필터링하는 변형 연산자가 있다.
