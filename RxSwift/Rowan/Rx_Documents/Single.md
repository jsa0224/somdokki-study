# Single

RxJava(및 RxGroovy 및 RxScala와 같은 파생 제품)는 “Single”이라는 옵저버블의 변형을 개발했다.

Single은 옵저버블과 유사하지만 일련의 값(아무것도 없는 것부터 무한한 수까지)을 방출하는 대신 항상 하나의 값을 방출하거나 오류 알림을 방출한다.

이러한 이유로 옵저버블의 알림에 응답하는 데 사용하는 세 가지 메서드(onNext, onError 및 onCompleted)로 Single을 구독하는 대신 두 가지 메서드를 사용하여 구독한다.

- **onSuccess**
    
    Single은 Single이 방출하는 유일한 항목을 `onSuccess` 메서드에 전달한다.
    
- **onError**
    
    Single은 Single이 항목을 방출할 수 없게 만든 throwable을 `onError` 메서드에 전달한다.
    

Single은 이러한 메서드 중 하나만 호출하며, 오직 한 번만 호출한다. 두 메서드 중 하나를 호출하면 Single이 종료되고 이에 대한 구독도 종료된다.

</br>

# Composition via Single Operators

옵저버블과 마찬가지로 Single도 다양한 연산자를 사용하여 조작할 수 있다. 일부 연산자는 옵저버블 컨텍스트와 싱글 컨텍스트 사이의 인터페이스를 허용해 두 종류를 혼합할 수도 있다.

| operator | returns | description |
| --- | --- | --- |
| *compose* | Single | 사용자 정의 연산자 만들기 |
| *concat* and *concatWith* | Observable | 여러 Single이 방출한 항목을 옵저버블 방출로 연결 |
| *create* | Single | 구독자 메서드를 명시적으로 호출하여 처음부터 Single 만들기 |
| *delay* | Single | Single에서 항목의 방출 시간을 이동 |
| *doOnError* | Single | onError 를 호출할 때 지정한 메서드도 호출하는 Single을 반환 |
| *doOnSuccess* | Single | onSuccess 를 호출할 때 지정한 메서드도 호출하는 Single을 반환 |
| *error* | Single | 구독자에게 오류를 즉시 알리는 Single을 반환 |
| *flatMap* | Single | Single이 방출한 항목에 함수를 적용한 결과인 Single을 반환 |
| *flatMapObservable* | Observable | Single이 방출한 항목에 함수를 적용한 결과인 Observable을 반환 |
| *from* | Single | Future를 Single로 변환 |
| *just* | Single | 지정된 항목을 방출하는 Single을 반환 |
| *map* | Single | Single 소스에서 방출된 항목에 함수를 적용한 결과를 방출하는 Single을 반환 |
| *merge* | Single | Single을 방출하는 Single을 하나의 싱글로 변환 |
| *merge* and *mergeWith* | Observable | 여러 Single이 방출한 항목을 옵저버블 방출로 병합 |
| *observeOn* | Single | Single에게 특정 스케줄러의 구독자 메서드를 호출하도록 지시 |
| *onErrorReturn* | Single | 오류 알림을 보내는 Single을 지정된 항목을 방출하는 Single로 변환 |
| *subscribeOn* | Single | Single에게 특정 스케줄러에서 작동하도록 지시 |
| *timeout* | Single | 소스 Single이 지정된 시간 동안 값을 내보내지 않는 경우 오류 알림을 보내는 Single을 반환 |
| *toSingle* | Single | 단일 항목을 방출하는 옵저버블을 해당 항목을 방출하는 Single로 변환 |
| *toObservable* | Observable | Single을 Single이 방출한 항목을 방출하고 완료하는 옵저버블로 변환 |
| *zip* and *zipWith* | Single | 둘 이상의 다른 Single이 방출한 항목에 함수를 적용한 결과를 방출하는 Single을 반환 |

각 연산자의 Marble Diagram은 [ReactiveX 페이지](https://reactivex.io/documentation/single.html) 참고.
