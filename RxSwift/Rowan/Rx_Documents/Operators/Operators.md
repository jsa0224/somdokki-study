# Operators

# Introduction

ReactiveX의 각 언어별 구현에는 Operator(연산자)가 포함된다. 언어별 구현 간에 겹치는 부분이 많지만 특정 언어에서만 구현되는 일부 Operator도 있다. 또한 각 언어별 구현에는 해당 언어의 유사한 메서드의 연산자와 유사하게 Operator의 이름을 지정하는 경향이 있다.

## Chaining Operators

대부분의 연산자는 observable에 연산하고 옵저버블을 반환한다. 이는 연산자를 chaining하여 차례로 적용할 수 있게 해준다. 연산자 체인의 각 연산자는 이전 연산자의 작업 결과인 옵저버블을 수정한다.

특정 클래스의 다양한 메서드가 메서드 연산을 통해 해당 객체를 수정하여 동일한 클래스의 항목에 작동하는 빌더 패턴과 같이 다른 패턴이 있다. 이러한 패턴을 사용하면 유사한 방식으로 메서드를 연결할 수도 있다. 그러나 빌더 패턴에서는 체인에서 연결하는 메서드의 순서는 일반적으로 중요하지 않지만 옵저버블에서 연산자 순서는 중요하다.

옵저버블의 연산자 체인은 처음 체인을 만들어낸 원래 옵저버블에서 독립적으로 연산하지 않는다. 연산자 체인에서 연산자는 바로 앞의 연산자가 생성한 옵저버블에 차례로 연산하게 된다.

## The Operators of ReactiveX

이 페이지에서는 먼저 ReactiveX의 “핵심” 연산자로 간주할 수 있는 항목을 기재하고 이러한 연산자의 작동 방식과 특정 언어별 ReactiveX 버전에서 이러한 연산자를 구현한 방법에 대한 자세한 정보가 있는 페이지로 연결한다.

다음은 활용하기에 가장 적합한 연산자를 선택하는 데 도움이 되는 “decision tree”이다.

마지막으로, ReactiveX의 여러 언어별 구현에서 사용할 수 있는 대부분의 연산자가 알파벳 순으로 나열되어 있다. 해당 리스트는 언어별 연산자와 가장 유사한 핵심 연산자를 문서화하는 페이지로 연결되는 링크를 포함하고 있다. (예를 들어, Rx.NET “SelectMany” 연산자는 FlatMap ReactiveX 연산자의 문서에 연결되며 “SelectMany”는 Rx.NET의 구현 내용이다.)

만약 직접 연산자를 구현하고 싶다면 [Implementing Your Own Operators](https://reactivex.io/documentation/implement-operator.html)를 참고할 것.

### Contents

1. [Operators By Category](#operators-by-category)
2. [A Decision Tree of Observable Operators](#a-decision-tree-of-observable-operators)
3. [List of Observable Operators](#list-of-observable-operators)

---

# Operators By Category

## Creating Observables

새로운 옵저버블을 생성하는 연산자

- [**Create**](https://reactivex.io/documentation/operators/create.html) — 프로그래밍 방식으로 옵저버 메서드를 호출하여 옵저버블을 처음부터 생성
- [**Defer**](https://reactivex.io/documentation/operators/defer.html) — 옵저버가 구독할 때까지 옵저버블을 생성하지 않고 각 옵저버에 대해 새로운 옵저버블을 생성
- [**Empty / Never / Throw**](https://reactivex.io/documentation/operators/empty-never-throw.html) — 매우 정확하고 제한된 동작을 가진 옵저버블 생성
- [**From**](https://reactivex.io/documentation/operators/from.html) — 다른 객체 또는 데이터 구조를 옵저버블로 변환
- [**Interval**](https://reactivex.io/documentation/operators/interval.html) — 특정 시간 간격으로 정수 시퀀스를 방출하는 옵저버블 생성
- [**Just**](https://reactivex.io/documentation/operators/just.html) — 객체 또는 객체 집합을 해당 객체 또는 객체 집합을 방출하는 옵저버블로 변환
- [**Range**](https://reactivex.io/documentation/operators/range.html) — 순차적 정수 범위를 방출하는 옵저버블 생성
- [**Repeat**](https://reactivex.io/documentation/operators/repeat.html) — 특정 항목 또는 항목의 시퀀스를 반복적으로 방출하는 옵저버블 생성
- [**Start**](https://reactivex.io/documentation/operators/start.html) — 함수의 반환 값을 방출하는 옵저버블 생성
- [**Timer**](https://reactivex.io/documentation/operators/timer.html) — 주어진 지연시간 후에 단일 항목을 방출하는 옵저버블 생성

## Transforming Observables

옵저버블이 방출하는 항목을 변환하는 연산자

- [**Buffer**](https://reactivex.io/documentation/operators/buffer.html) — 옵저버블에서 주기적으로 항목을 묶음으로 수집하고 한 번에 하나씩 항목을 방출하는 대신 수집한 묶음을 방출
- [**FlatMap**](https://reactivex.io/documentation/operators/flatmap.html) — 옵저버블이 내보낸 항목을 옵저버블로 변환한 다음 해당 항목의 방출 항목을 단일 옵저버블로 평탄화
- [**GroupBy**](https://reactivex.io/documentation/operators/groupby.html) — 옵저버블을 원래 옵저버블에서 다른 항목 그룹을 방출하는 옵저버블 집합으로 나눔
- [**Map**](https://reactivex.io/documentation/operators/map.html) — 각 항목에 함수를 적용하여 옵저버블이 방출하는 항목을 변환
- [**Scan**](https://reactivex.io/documentation/operators/scan.html) — 옵저버블이 내보낸 각 항목에 함수를 순차적으로 적용하고 각 연속 값을 방출
- [**Window**](https://reactivex.io/documentation/operators/window.html) — 주기적으로 옵저버블의 항목을 옵저버블 윈도우에 세분화하고 항목을 한 번에 하나씩 방출하는 대신 세분화한 윈도우를 방출

## Filtering Observables

소스 옵저버블로부터 항목을 선택적으로 방출하는 연산자

- [**Debounce**](https://reactivex.io/documentation/operators/debounce.html) — 다른 항목을 방출하지 않고 특정 시간 범위가 경과한 경우에만 옵저버블에서 항목을 방출
- [**Distinct**](https://reactivex.io/documentation/operators/distinct.html) — 옵저버블이 방출하는 중복되는 항목을 억제
- [**ElementAt**](https://reactivex.io/documentation/operators/elementat.html) — 옵저버블이 방출하는 항목 중 n만을 방출 (n: index, 0부터 시작)
- [**Filter**](https://reactivex.io/documentation/operators/filter.html) — 주어진 predicate 테스트를 통과하는 옵저버블의 항목만 방출
- [**First**](https://reactivex.io/documentation/operators/first.html) — 옵저버블에서 첫 번째 항목 또는 조건을 만족하는 첫 번째 항목만 방출
- [**IgnoreElements**](https://reactivex.io/documentation/operators/ignoreelements.html) — 옵저버블에서 항목을 방출하지 않고 종료 알림(onError, onCompleted)을 미러링
- [**Last**](https://reactivex.io/documentation/operators/last.html) — 옵저버블이 방출한 마지막 항목을 방출
- [**Sample**](https://reactivex.io/documentation/operators/sample.html) — 주기적인 시간 간격 내 옵저버블이 방출한 가장 최근 항목을 방출
- [**Skip**](https://reactivex.io/documentation/operators/skip.html) — 옵저버블이 방출한 처음 n개의 항목을 억제
- [**SkipLast**](https://reactivex.io/documentation/operators/skiplast.html) — 옵저버블이 방출한 마지막 n개의 항목을 억제
- [**Take**](https://reactivex.io/documentation/operators/take.html) — 옵저버블이 방출한 처음 n개의 항목만 방출
- [**TakeLast**](https://reactivex.io/documentation/operators/takelast.html) — 옵저버블이 방출한 마지막 n개의 항목만 방출

## Combining Observables

단일 옵저버블을 생성하기 위해 여러 소스 옵저버블과 함께 작동하는 연산자

- [**And / Then / When**](https://reactivex.io/documentation/operators/and-then-when.html) — `Pattern` 및 `Plan` 중개자를 통해 둘 이상의 옵저버블에서 방출된 항목 집합을 결합
- [**CombineLatest**](https://reactivex.io/documentation/operators/combinelatest.html) — 항목이 두 옵저버블 중 하나에 의해 방출되면 지정된 함수를 통해 각 옵저버블에서 방출된 최신 항목을 결합하고 이 함수의 결과에 따라 항목을 방출
- [**Join**](https://reactivex.io/documentation/operators/join.html) — 다른 옵저버블에 의해 방출된 항목에 따라 정의된 시간 윈도우 동안 하나의 옵저버블에서 항목이 방출될 때마다 두 옵저버블에서 방출된 항목을 결합
- [**Merge**](https://reactivex.io/documentation/operators/merge.html) — 방출을 병합하여 여러 옵저버블을 하나로 결합
- [**StartWith**](https://reactivex.io/documentation/operators/startwith.html) — 소스 옵저버블에서 항목을 방출하기 전에 지정된 항목 시퀀스를 방출
- [**Switch**](https://reactivex.io/documentation/operators/switch.html) — 옵저버블을 방출하는 옵저버블을 해당 옵저버블 중 가장 최근에 방출된 항목을 방출하는 단일 옵저버블로 변환
- [**Zip**](https://reactivex.io/documentation/operators/zip.html) — 지정된 함수를 통해 여러 옵저버블의 방출을 함께 결합하고 이 함수의 결과를 기반으로 각 조합에 대해 단일 항목을 방출

## Error Handling Operators

옵저버블의 오류 알림에서 복구하는 데 도움이 되는 연산자

- [**Catch**](https://reactivex.io/documentation/operators/catch.html) — 오류 없이 시퀀스를 계속하여 `onError` 알림에서 복구
- [**Retry**](https://reactivex.io/documentation/operators/retry.html) — 소스 옵저버블이 `onError` 알림을 보내면 오류 없이 완료되기를 바라며 재구독

## Observable Utility Operators

옵저버블 작업을 위한 유용한 연산자

- [**Delay**](https://reactivex.io/documentation/operators/delay.html) — 특정 양만큼 옵저버블의 방출 시간을 이동
- [**Do**](https://reactivex.io/documentation/operators/do.html) — 다양한 옵저버블 수명 주기 이벤트에 대해 수행할 작업을 등록
- [**Materialize / Dematerialize**](https://reactivex.io/documentation/operators/materialize-dematerialize.html) — 방출한 항목과 방출한 항목으로 전송된 알림을 모두 나타내거나 이 과정을 반대로 진행
- [**ObserveOn**](https://reactivex.io/documentation/operators/observeon.html) — 옵저버가 이 옵저버블을 관찰할 스케줄러를 지정
- [**Serialize**](https://reactivex.io/documentation/operators/serialize.html) — 옵저버블이 직렬화된 호출을 수행하고 올바르게 작동하도록 강제
- [**Subscribe**](https://reactivex.io/documentation/operators/subscribe.html) — 옵저버블의 방출 및 알림에 따라 연산
- [**SubscribeOn**](https://reactivex.io/documentation/operators/subscribeon.html) — 옵저버블이 구독할 때 사용해야 하는 스케줄러 지정
- [**TimeInterval**](https://reactivex.io/documentation/operators/timeinterval.html) — 항목을 방출하는 옵저버블을 해당 방출 사이에 경과된 시간의 표시를 방출하는 것으로 변환
- [**Timeout**](https://reactivex.io/documentation/operators/timeout.html) — 원본 옵저버블을 미러링하지만 방출된 항목 없이 특정 기간이 경과하면 오류 알림을 발행
- [**Timestamp**](https://reactivex.io/documentation/operators/timestamp.html) — 옵저버블이 방출한 각 항목에 타임 스탬프를 첨부
- [**Using**](https://reactivex.io/documentation/operators/using.html) — 옵저버블과 동일한 수명을 가진 일회용 리소스를 생성

## Conditional and Boolean Operators

하나 이상의 옵저버블 또는 옵저버블이 방출한 항목을 평가하는 연산자

- [**All**](https://reactivex.io/documentation/operators/all.html) — 옵저버블이 방출한 모든 항목이 몇몇 기준을 충족하는지 확인
- [**Amb**](https://reactivex.io/documentation/operators/amb.html) — 두 개 이상의 소스 옵저버블이 주어지면 첫 번째 옵저버블에서만 모든 항목을 방출하여 항목을 방출
- [**Contains**](https://reactivex.io/documentation/operators/contains.html) — 옵저버블이 특정 항목을 방출하는지 여부를 결정
- [**DefaultIfEmpty**](https://reactivex.io/documentation/operators/defaultifempty.html) — 소스 옵저버블에서 항목을 방출하거나 소스 옵저버블이 아무 것도 방출하지 않으면 기본 항목을 방출
- [**SequenceEqual**](https://reactivex.io/documentation/operators/sequenceequal.html) — 두 옵저버블이 동일한 항목 시퀀스를 방출하는지 확인
- [**SkipUntil**](https://reactivex.io/documentation/operators/skipuntil.html) — 두 번째 옵저버블이 항목을 내보낼 때까지 옵저버블이 내보낸 항목을 버림
- [**SkipWhile**](https://reactivex.io/documentation/operators/skipwhile.html) — 지정된 조건이 거짓이 될 때까지 옵저버블이 방출하는 항목을 버림
- [**TakeUntil**](https://reactivex.io/documentation/operators/takeuntil.html) — 두 번째 옵저버블이 항목을 방출하거나 종료된 후 옵저버블이 방출한 항목을 버림
- [**TakeWhile**](https://reactivex.io/documentation/operators/takewhile.html) — 지정된 조건이 거짓이 된 후 옵저버블이 방출하는 항목 폐기

## Mathematical and Aggregate Operators

옵저버블이 방출한 항목의 전체 시퀀스에서 작동하는 연산자

- [**Average**](https://reactivex.io/documentation/operators/average.html) — 옵저버블이 내보낸 숫자의 평균을 계산하고 해당 평균을 방출
- [**Concat**](https://reactivex.io/documentation/operators/concat.html) — 두 개 이상의 옵저버블이 방출하는 값을 스트림 사이에 끼워넣지 않고 방출
- [**Count**](https://reactivex.io/documentation/operators/count.html) — 소스 옵저버블이 내보낸 항목 수를 세고 방출된 항목의 수만을 방출
- [**Max**](https://reactivex.io/documentation/operators/max.html) — 옵저버블이 방출한 최대값 항목을 결정하고 방출
- [**Min**](https://reactivex.io/documentation/operators/min.html) — 옵저버블이 방출한 최소값 항목을 결정하고 방출
- [**Reduce**](https://reactivex.io/documentation/operators/reduce.html) — 옵저버블이 방출한 각 항목에 함수를 순차적으로 적용하고 최종 값을 방출
- [**Sum**](https://reactivex.io/documentation/operators/sum.html) — 옵저버블이 방출한 숫자의 합을 계산하고 해당 합계를 방출

## Backpressure Operators

- **[backpressure operators](https://reactivex.io/documentation/operators/backpressure.html)** — 옵저버가 소비하는 것보다 빠르게 항목을 생성하는 옵저버블에 대처하기 위한 전략

## Connectable Observable Operators

보다 정밀하게 제어되는 구독 역학을 가진 특별한 옵저버블

- [**Connect**](https://reactivex.io/documentation/operators/connect.html) — 연결 가능한(Connectable) 옵저버블에게 구독자로의 항목 방출을 시작하도록 지시
- [**Publish**](https://reactivex.io/documentation/operators/publish.html) — 일반 옵저버블을 연결 가능한 옵저버블로 변환
- [**RefCount**](https://reactivex.io/documentation/operators/refcount.html) — 연결 가능한 옵저버블이 일반 옵저버블처럼 동작하도록 만듦
- [**Replay**](https://reactivex.io/documentation/operators/replay.html) — 옵저버블이 항목 방출을 시작한 후에 구독하더라도 모든 옵저버가 방출된 항목의 동일한 시퀀스를 볼 수 있도록 보장

## Operators to Convert Observables

- [**To**](https://reactivex.io/documentation/operators/to.html) — 옵저버블을 다른 객체 또는 데이터 구조로 변환

---

# A Decision Tree of Observable Operators

ReactiveX Observable 연산자를 찾는 데 도움이 되는 tree

### 💡 새로운 옵저버블을 생성하고 싶을 때

- 특정 항목을 방출하는 옵저버블: `Just`
    - 구독 시간에 호출된 함수에서 항목이 반환되는 경우: `Start`
    - `Action`, `Callable`, `Runnable` 또는 구독 시간에 호출된 것과 유사한 것으로부터 항목이 반환되는 경우: `From`
    - 지정된 시간 지연 후 항목을 방출하고 싶은 경우: `Timer`
- 특정 Array, Iterable 또는 이와 유사한 것으로부터 항목을 방출하는 옵저버블: `From`
- Future로부터 항목을 검색하는 옵저버블: `Start`
- Future로부터 항목의 시퀀스를 얻는 옵저버블: `From`
- 항목의 시퀀스를 반복적으로 방출하는 옵저버블: `Repeat`
- 커스텀 로직을 포함한 옵저버블: `Create`
- 구독하는 각 옵저버에 대한 옵저버블: `Defer`
- 정수의 시퀀스를 방출하는 옵저버블: `Range`
    - 특정 시간 간격으로 항목을 방출하고 싶은 경우: `Interval`
        - 지정된 시간 지연 후 항목을 방출하고 싶은 경우: `Timer`
- 항목을 방출하지 않고 완료하는 옵저버블: `Empty`
- 아무것도 하지 않는 옵저버블: `Never`

### 💡 다른 옵저버블과 결합해 옵저버블을 만들고 싶을 때

- 수신된 순서에 관계없이 모든 옵저버블의 항목을 방출하는 옵저버블: `Merge`
- 모든 옵저버블의 항목을 방출하며, 한 번에 하나의 옵저버블에서 방출되는 모든 항목을 방출하는 옵저버블: `Concat`
- 두 개 이상의 옵저버블에서 항목을 순차적으로 결합해 방출할 새 항목을 생성하는 옵저버블
    - 각 옵저버블이 모두 새 항목을 방출할 때마다 해당 항목을 결합해 방출하는 경우: `Zip`
    - 어떤 옵저버블이 새 항목을 방출할 때마다 결합해 방출하는 경우: `CombineLatest`
    - 다른 옵저버블이 방출한 항목에 의해 정의된 *window*에서 한 옵저버블이 항목을 방출할 때마다 해당 항목들을 결합해 방출하는 경우: `Join`
    - *Pattern* 및 *Plan* 중개자를 통해 결합하여 방출하는 경우: `And/Then/When`
- 옵저버블들의 가장 최근에 방출된 항목들만 결합해 방출하는 경우: `Switch`

### 💡 항목을 변환한 후 옵저버블로부터 방출하고 싶을 때

- 한 번에 하나의 함수를 통해 변환하는 옵저버블: `Map`
- 옵저버블에서 방출된 모든 항목을 변환해 방출하는 옵저버블: `FlatMap`
    - 항목이 방출되는 순서대로 한 번에 하나씩 방출하고 싶은 경우: `ConcatMap`
- 이전의 모든 항목을 기반으로 변환해 방출하는 옵저버블: `Scan`
- 타임 스탬프를 첨부해 변환하여 방출하는 옵저버블: `Timestamp`
- 항목이 방출되기 전에 경과된 시간을 나타내는 indicator로 변환하여 방출하는 옵저버블: `TimeInterval`

### 💡 옵저버블의 항목 방출 시간을 이동시키고 싶을 때

- `Delay`

### 💡 옵저버블의 항목과 알림을 함께 방출하고 싶을 때

- Notification 객체에 wrapping한 연산자: `Materialize`
    - 이후 다시 unwrap 하는 연산자: `Dematerialize`

### 💡 옵저버블이 방출한 모든 항목을 무시하고 완료/오류 알림만 전달하고 싶을 때

- `IgnoreElements`

### 💡옵저버블을 미러링하고 싶지만 시퀀스에 접두사 항목을 추가하고 싶을 때

- `StartWith`
    - 시퀀스가 비어 있는 경우: `DefaultIfEmpty`

### 💡 옵저버블에서 항목을 수집하고 항목의 버퍼로 재방출하고 싶을 때

- `Buffer`
    - 방출된 마지막 항목만을 포함하는 경우: `TakeLastBuffer`

### 💡 하나의 옵저버블을 여러 옵저버블로 나누고 싶을 때

- `Window`
    - 유사한 항목이 동일한 옵저버블에서 끝나도록 할 경우: `GroupBy`

### 💡옵저버블이 내보낸 특정 항목을 검색(retrieve)하고 싶을 때

- 완료되기 전에 방출된 마지막 항목을 찾는 연산자: `Last`
- 오직 하나의 항목을 방출하는 옵저버블: `Single`
- 첫 번째 항목을 방출하는 연산자: `First`

### 💡 옵저버블에서 특정 항목만 다시 방출하고 싶을 때

- 일부 predicate와 일치하지 않는 항목을 필터링하는 연산자: `Filter`
- 오직 첫 번째 항목만 방출하는 연산자: `First`
- 처음으로부터 n 번째 항목까지만 방출하는 연산자: `Take`
- 오직 마지막 항목만 방출하는 연산자: `Last`
- n 번째 항목만 방출하는 연산자: `ElementAt`
- 첫 번째 항목 이후의 항목들을 다시 방출하는 경우
    - 첫 번째 항목에서 n개의 항목 이후에 방출하는 연산자: `Skip`
    - 주어진 조건이 거짓이 될 때까지 방출된 항목을 버리는 연산자 : `SkipWhile`
    - 처음부터 일정 시간동안 방출된 항목을 버리는 연산자: `Skip`
    - 두 번째 옵저버블이 방출하기 전 방출된 항목을 버리는 연산자: `SkipUntil`
- 마지막 항목을 제외하고 방출하는 경우
    - 마지막으로부터 n개의 항목을 제외하는 연산자: `SkipLast`
    - 주어진 조건이 거짓이 될 때까지 옵저버블이 방출하는 항목을 미러링하는 연산자: `TakeWhile`
    - 소스가 완료되기 전에 일정 기간 동안 방출된 항목을 제외하는 연산자: `SkipLast`
    - 두 번째 옵저버블이 항목을 방출한 후에 방출된 항목을 제외하는 연산자: `TakeUntil`
- 옵저버블을 주기적으로 샘플링하는 연산자: `Sample`
- 일정 기간 내에 다른 항목이 방출되지 않는 항목만 방출하는 연산자: `Debounce`
- 이미 방출된 항목과 중복되는 항목을 억제하는 연산자: `Distinct`
    - 뒤따라 방출되는 항목이 이전 항목과 다를 때만 방출하는 연산자: `DistinctUntilChanged`
- 항목을 방출하기 시작한 후 일정 시간 동안 구독을 지연하는 연산자: `DelaySubscription`

### 💡 옵저버블 Collection 중 처음으로 항목을 방출한 옵저버블에서만 항목을 방출하고 싶을 때

- `Amb`

### 💡옵저버블이 내보낸 항목의 전체 시퀀스를 평가하고 싶을 때

- 모든 항목이 주어진 테스트를 통과했는지 나타내는 단일 bool 값을 방출하는 연산자: `All`
- 옵저버블이 주어진 테스트를 통과하는 항목을 배출했는지 표시하는 단일 bool 값을 방출하는 연산자: `Contains`
- 옵저버블이 항목을 방출했는지 안했는지 여부를 나타내는 단일 bool 값을 방출하는 연산자: `IsEmpty`
- 시퀀스가 두 번째 옵저버블이 방출한 것과 동일한지를 나타내는 단일 bool 값을 방출하는 연산자: `SequenceEqual`
- 방출된 항목의 평균을 방출하는 연산자: `Average`
- 방출된 항목의 합계를 방출하는 연산자: `Sum`
- 시퀀스에서 방출한 항목의 개수를 방출하는 연산자: `Count`
- 방출된 항목 중 최대값을 갖는 항목을 방출하는 연산자: `Max`
- 방출된 항목 중 최소값을 갖는 항목을 방출하는 연산자: `Min`
- 각 항목에 집계 함수(aggregation function)을 차례로 적용하고 결과를 방출하는 연산자: `Scan`
- 

### 💡 옵저버블이 방출한 항목의 전체 시퀀스를 다른 데이터 구조로 변환하고 싶을 때

- `To`

### 💡 연산자가 특정 스케줄러에서 작동하게 하고 싶을 때

- `SubscribeOn`
    - 옵저버에게 이를 알리고 싶은 경우: `ObserveOn`

### 💡 특정 이벤트가 발생하면 옵저버블이 특정 작업을 호출하기를 원할 때

- `Do`

### 💡 옵저버에게 오류를 알리는 옵저버블을 원할 때

- `Throw`
    - 항목을 방출하지 않고 지정된 시간이 경과하면 이를 알리는 경우: `Timeout`

### 💡 옵저버블이 정상적으로 복구되기를 원할 때

- 시간이 초과된 이후 백업 옵저버블로 전환하는 경우: `Timeout`
- 업스트림 오류 알림으로부터 복구되는 경우: `Catch`
    - 업스트림 옵저버블에 다시 구독을 시도하여 복구하는 경우: `Retry`

### 💡 옵저버블과 수명이 같은 리소스를 만들고 싶을 때

- `Using`

### 💡 옵저버블을 구독하고 옵저버블이 완료될 때까지 차단하는 `Future`를 받고 싶을 때

- `Start`

### 💡 요청을 받을 때까지 구독자에게 항목을 방출하지 않는 옵저버블을 원할 때

- `Publish`
    - 항목을 방출하지 않고 있다가 시퀀스의 마지막 항목만 방출하는 경우: `PublishLast`
    - 항목을 방출하지 않고 있다가 시퀀스가 시작된 이후 새로 구독하는 구독자에게도 전체 시퀀스를 방출하는 경우: `Replay`
    - 항목을 방출하지 않지만 모든 구독자가 구독을 취소하면 사라지는 옵저버블을 원하는 경우: `RefCount`
    - 항목을 방출하지 않고 있다가 방출 시작을 요청하는 경우: `Connect`

---

# List of Observable Operators
🔸 [ObserveOn](https://github.com/jsa0224/somdokki-study/blob/main/RxSwift/Rowan/Rx_Documents/Operators/OperatorDetails/ObserveOn.md)
