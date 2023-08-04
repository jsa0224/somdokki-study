# Operators
거의 모든 연산자들은 Observable 상에서 동작하고 Observable을 리턴한다. 이 접근 방법은 연산자들을 연달아 호출 할 수 있는 연산자 체인을 제공한다. 연산자 체인에서 각각의 연산자는 이전 연산자가 리턴한 Observable을 기반으로 동작하며 동작에 따라 Observable을 변경한다.

특정 클래스가 제공하는 여러 메서드들을 호출하여 클래스의 항목들을 변경하는 빌더 패턴과 같은 것도 존재한다. 빌더 패턴 역시 연산자 체인과 유사하게 메서드 체인을 제공한다. 하지만, 빌더 패턴의 메서드 체인과는 달리 Observable의 연산자 체인은 호출 순서에 따라 실행 결과가 달라지는 차이점이 있다.

따라서, Observable의 연산자 체인은 원본 Observable과 독립적으로 실행될 수 없고 순서대로 실행되어야 한다. 왜냐하면, 이미 이해하고 있듯이 연산자 체인에서 먼저 실행된 연산자가 리턴한 Observable을 기반으로 다음 연산자가 동작하기 때문이다.


## Operator's Category

### Observable 생성
> 새로운 Observable 을 만드는 연산자들

- `Create` — 직접적인 코드 구현을 통해 옵저버 메서드를 호출하여 Observable을 생성한다
- Defer — 옵저버가 구독하기 전까지는 Observable 생성을 지연하고 구독이 시작되면 옵저버 별로 새로운 Observable을 생성한다
- Empty/Never/Throw — 아주 정확하고 제한된 행동을 하는 Observable을 생성한다
- From — 다른 객체나 자료 구조를 Observable로 변환한다
- Interval — 특정 시간별로 연속된 정수형을 배출하는 Observable을 생성한다
- Just — 객체 하나 또는 객채집합을 Observable로 변환한다. 변환된 Observable은 원본 객체들을 발행한다
- Range — 연속된 범위(Range)의 정수를 발행하는 Observable을 생성한다
- Repeat — 특정 항목이나 연속된 항목들을 반복적으로 배출하는 Observable을 생성한다
- Start — 함수의 실행 결과를 배출하는 Observable을 생성한다
- Timer — 지정된 시간이 지나고 난 후 항목을 하나 배출하는 Observable을 생성한다

### Observable 변환
> Observable이 배출한 항목들을 변환하는 연산자들

- Buffer — Observable로부터 정기적으로 항목들을 수집하고 묶음으로 만든 후에 묶음 안에 있는 항목들을 한번에 하나씩 배출하지 않고 수집된 묶음 단위로 배출한다
- `FlatMap` — 하나의 Observable이 발행하는 항목들을 여러개의 Observable로 변환하고, 항목들의 배출을 차례차례 줄 세워 하나의 Observable로 전달한다
- GroupBy — 원본 Observable이 배출하는 항목들을 키(Key) 별로 묶은 후 Observable에 담는다. 이렇게 키 별로 만들어진 Observable들은 자기가 담고 있는 묶음의 항목들을 배출한다
- `Map` — Observable이 배출한 항목에 함수를 적용한다
- Scan — Observable이 배출한 항목에 연속적으로 함수를 적용하고 실행한 후 성공적으로 실행된 함수의 리턴 값을 발행한다
- Window — 정기적으로 Observable의 항목들을 더 작은 단위의 Observable 윈도우로 나눈 후에, 한번에 하나씩 항목들을 발행하는 대신 작게 나눠진 윈도우 단위로 항목들을 배출한다

### Observable 필터링
> Observable에서 선택적으로 항목을 배출하는 연산자들

- Debounce — Observable의 시간 흐름이 지속되는 상태에서 다른 항목들은 배출하지 않고 특정 시간 마다 그 시점에 존재하는 항목 하나를 Observable로부터 배출한다
- Distinct — Observable이 배출하는 항목들 중 중복을 제거한 항목들을 배출한다
- ElementAt — Obserable에서 n번째 항목만 배출한다
- Filter — 테스트 조건을 만족하는 항목들만 배출한다
- First — 맨 첫 번째 항목 또는 조건을 만족하는 첫 번째 항목만 배출한다
- IgnoreElements — 항목들을 배출하지는 않고 종료 알림은 보낸다
- Last — Observable의 마지막 항목만 배출한다
- Sample — 특정 시간 간격으로 최근에 Observable이 배출한 항목들을 배출한다
- Skip — Observable이 배출한 처음 n개의 항목들을 숨긴다
- SkipLast — Observable이 배출한 마지막 n개의 항목들을 숨긴다
- Take — Observable이 배츨한 처음 n개의 항목들만 배출한다
- TakeLast — Observable이 배출한 마지막 n개의 항목들만 배출한다

### Obserables 결합
> 여러 개의 소스 Observable들을 하나의 Observable로 만드는 연산자들

- And/Then/When — 두 개 이상의 Observable들이 배출한 항목들을 'Pattern'과 'Plan' 중계자를 이용해서 결합한다
- CombineLatest — 두 개의 Observable 중 하나가 항목을 배출할 때 배출된 마지막 항목과 다른 한 Observable이 배출한 항목을 결합한 후 함수를 적용하여 실행 후 실행된 결과를 배출한다
- Join — A Observable과 B Observable이 배출한 항목들을 결합하는데, 이때 B Observable은 배출한 항목이 타임 윈도우를 가지고 있고 이 타임 윈도우가 열린 동안 A Observable은 항목의 배출을 계속한다. Join 연산자는 B Observable의 항목을 배출하고 배출된 항목은 타임 윈도우를 시작시킨다. 타임 윈도우가 열려 있는 동안 A Observable은 자신의 항목들을 계속 배출하여 이 두 항목들을 결합한다
- Merge — 복수 개의 Observable들이 배출하는 항목들을 머지시켜 하나의 Observable로 만든다
- StartWith — 소스 Observable이 항목을 배출하기 전에 다른 항목들을 앞에 추가한 후 배출한다
- Switch — Observable들을 배출하는 Observable을 싱글 Observable로 변환하다. 변환된 싱글 Observable은 변환 전 소스 Observable들이 배출한 항목들을 배출한다
- Zip — 명시한 함수를 통해 여러 Observable들이 배출한 항목들을 결합하고 함수의 실행 결과를 배출한다


### 오류 처리 연산자
> Observable이 던진 오류를 복구할 수 있도록 도와주는 연산자들

- Catch — 오류를 무시하고 배출되는 항목들을 계속 진행시켜 'onError'로부터 전달된 오류를 복구한다
- Retry — 만약 소스 Observable이 'onError' 알림을 보낼 경우, 오류 없이 실행이 완료되기를 기대하며 재구독을 시도한다


### Observable 유틸리티 연산자
> Obserable과 함께 동작하는 유용한 도우미 연산자들

- Delay — Observable의 배출을 특정 시간동안 미룬다
- Do — Observable의 생명주기 동안 발생하는 여러 이벤트에서 실행 될 액션을 등록한다
- Materialize/Dematerialize — 배출된 항목이 어떤 알림을 통해 옵저버에게 전달 됐는지를 표현하며, 그 반대 과정을 수행할 수 있다
- ObserveOn — 옵저버가 어느 스케줄러 상에서 Observable을 관찰할지 명시한다
- Serialize — Observable이 직렬화된 호출을 생성하고 제대로 동작하도록 강제한다
- Subscribe — Observable이 배출하는 항목과 알림을 기반으로 동작한다
- SubscribeOn — Observable을 구독할 때 사용할 스케줄러를 명시한다
- TimeInterval — 항목들을 배출하는 Observable을, 항목을 배출하는데 걸린 시간이 얼마인지를 가리키는 Observable로 변환한다
- Timeout — 소스 Obvservable을 그대로 전달하지만, 대신 특정 시간 동안 배출된 항목이 없으면 오류 알림을 보낸다
- Timestamp — Observable이 배출한 항목에 타임 스탬프를 추가한다
- Using — 소스 Observable과 동일한 생명주기를 갖는 Observable을 생성하는데, 이 Observable은 생명주기가 완료되면 리소스를 종료하고 반환한다

### 조건과 불린 연산자(Boolean)
> 하나 이상의 Observable 또는 Observable이 배출한 항목을 평가하는 연산자들

- All — Observable이 배출한 전체 항목들이 어떤 조건을 만족시키는지 판단한다
- Amb — 두 개 이상의 소스 Observable이 주어 질때, 그 중 첫 번째로 항목을 배출한 Observable이 배출하는 항목들을 전달한다
- Contains — Observable이 특정 항목을 배출하는지 아닌지를 판단한다
- DefaultIfEmpty — 소스 Observable이 배출하는 항목을 전달한다. 만약 배출되는 항목이 없으면 기본 항목을 배출한다
- SequenceEqual — 두 개의 Observable이 항목을 같은 순서로 배출하는지 판단한다
- SkipUntil — 두 번째 Observable이 항목을 배출하기 전까지 배출된 항목들을 버린다
- SkipWhile — 특정 조건이 false를 리턴하기 전까지 Observable이 배출한 항목들을 버린다
- TakeUntil — 두 번째 Observable이 항목을 발행하기 시작햤거나 두 번째 Observable이 종료되면 그 때부터 발행되는 항목들은 버린다
- TakeWhile — 특정 조건이 false를 리턴하기 시작하면 그 이후에 배출되는 항목들을 버린다

### 수학과 집계 연산자
> Observable이 배출하는 항목 전체를 대상으로 동작하는 연산자들

- Average — Observable이 발행한 항목의 평균 값을 발행한다
- Concat — 두 개 이상의 Observable들이 항목을 발행할 때 Observable 순서대로 배출하는 항목들을 하나의 Observable 배출로 연이어 배출한다
- Count — 소스 Observable이 발행한 항목의 개수를 배출한다
- Max — Observable이 발행한 항목 중 값이 가장 큰 항목을 배출한다
- Min — Observable이 발행한 항목 중 값이 가장 작은 항목을 배출한다
- Reduce — Observable이 배출한 항목에 함수를 순서대로 적용하고 함수를 연산한 후 최종 결과를 발행한다
- Sum — Observable이 배출한 항목의 합계를 배출한다


### 연결 가능한 Observable 연산자
> 좀 더 정확히 제어되는 구독 역학을 가진 전문 Observable들

- Connect — 구독자가 항목 배출을 시작할 수 있도록 연결 가능한 Observable에게 명령을 내린다
- Publish — 일반 Observable을 연결 가능한 Observable로 변환한다
- RefCount — 일반 Observable처럼 동작하는 연결 가능한 Observable을 만든다
- Replay — 비록 옵저버가 Observable이 항목 배출을 시작한 후에 구독을 했다 하더라도 배출된 모든 항목들을 볼 수 있도록 한다

## Reference

- [Reactive X - Operators](https://reactivex.io/documentation/ko/operators.html)
