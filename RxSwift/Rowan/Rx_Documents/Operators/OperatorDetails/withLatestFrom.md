# withLatestFrom

연산자를 적용하는 옵저버블 시퀀스의 각 요소를 두 번째 소스의 최신 요소와 결합하여 두 개의 옵저버블 시퀀스를 하나의 시퀀스로 병합한다. (가능한 경우에만)

<img src="https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/images/withLatestFrom_complete.jpg" width="450">

- 참고: 두 번째 소스가 값을 방출하기 전에 첫 번째 소스에서 방출되는 요소는 모두 생략된다.
- parameter `second` : 두 번째 옵저버블 소스
- parameter `resultSelector` : 가능하다면 첫 번째 시퀀스에서 방출된 항목과 두 번째 소스의 최신 방출 항목의 결합을 일으키는 함수
- `returns` : 지정된 resultSelector를 사용하여 첫 번째 시퀀스의 각 항목을 두 번째 소스의 최신 항목과 결합한 결과를 포함하는 옵저버블 시퀀스
