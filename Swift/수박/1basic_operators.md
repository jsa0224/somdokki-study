### 기초 연산자들

---

- `연산자`란 특정 기호나 구절을 의미한다. 할당, 산술, 비교처럼.
	- 수학처럼(혹은 C언어에서 사용하던 것처럼) +, -, *, / 등이 있고 %를 통해 나머지도 구할 수 있고...
	- &&처럼 좌우항이 모두 참일 때 참이 되는 연산 같은 것도 존재한다
	- =는 일반적 의미의 비교는 아니고 ==를 사용한다. =는 값을 반환하거나 비교하지 않으며, 할당을 한다. 휴먼에러를 방지하기 위함이다
	- 스위프트는 타언어와는 다르게 범위를 만드는 연산자도 존재한다 [0,5] -> 0...5 , [0,5) -> 0..<5

- 연산자에는 크게 3가지 종류가 있다.
	- 단항연산 -> 접두사나 접미사를 붙이는 연산이다, -1에서 -, isUnary!에서 !같은 것들
	- 이항연산 -> 좌항과 우항에 대한 관계를 정의하는 연산이다. 더하기 같은 거
	- 삼항연산 -> 스위프트에는 딱하나 있다. 즉 삼항연산자는 단 하나의 연산을 가리킨다
		- `condition ? whenTrue : whenFalse`
		- 문서는 그런데 만들면 만들 수 있지 않을까 싶기도 하고... 안만들어봐서 모르겠음


<br><br>
	
	
### 할당 연산

---

- 할당이란 값을 초기화(init)하거나 업데이트를 하는 것을 말한다. 대충 아래와 같다
	```swift
		let number = 0
		var myName = "soobak"
		myName = "melon"
		var (year, month, day) = (1994, 12, 26)
		year = 9999, month = 13, day = 00
	```
	- 좌우항이 서로 같은지는 ==를 통해 처리한다. 


<br><br>


### 산술 연산

---

- 더하기는 +, 빼기는 -, 곱하기는 *, 나누기는 /
- 상기해야할 게 있다. 앞서 basic에서 이야기한 것처럼 숫자는 상하한이 있다.
	- 산술연산을 통해 상하한을 넘어서면 스위프트는 이걸 막는다.
- String문자열도 +를 통해 합칠 수 있다 -> "hello" + " " + "world" -> "hello world"
- 나머지 연산(%)도 있다
	- 예를 들면 5 = 2*2 +1에서 1과 같은 나머지를 구하는 연산(5%2 == 1)


<br><br>


### 연산자 축약

---

- a = a + 1 -> a += 1
- a = a - 1 -> a -= 1
- 단순 산술연산과 할당을 동시에 할 수 있다


<br><br>


### 비교 연산과 논리 연산, nil 병합 연산

---

- 같은지==, 같지 않은지!=, 좌항이 큰지>, 좌항이 작은지<, 좌항이 크거나 같은지 >=, 좌항이 작거나 같은지 <=
	- 이런 비교연산은 Bool값을 리턴한다
- 참이 아닌지!condition, 좌항과 우항이 모두 참인지 condition1 && condition2, 둘중 하나가 참인지 condition1 || condition2
	- 이런 논리연산도 Bool값을 리턴한다
	- 많이 쓸 수 있는데 좌측부터 순서대로 처리하고 거짓이 나오면 뒤의 연산은 생략한다.
	- 소괄호를 통해 논리표현을 좀 더 명료하게 전달할 수도 있다
- 좌항이 비어있지 않으면 쓰고, 비어있다면 우항을 기본값으로 사용하는 optionalValue ?? defaultValue
	- 닐 병합 연산자는 Bool값을 리턴하진 않는다.
	- Optional로 랩핑됐던 값에 반드시 값을 보장함으로, 언랩핑이 된다

<br><br>


#### 범위 연산자

---

- 닫힌구간 [a,b] -> a...b
	- [1,5] -> 1...5 -> 1,2,3,4,5

- 반열린구간 [a,b) -> a..<b
	- [1,5) -> 1..<5 -> 1,2,3,4

- 단면구간..? 부분구간..? One-Sided ranges
	- ->  [a, infinity] -> a...
	- ->  [infinity, a] -> ...a
	- 맥락에 따라 가능한 상한과 하한을 사용해서 좀 더 유려한 처리를 할 수 있도록 도와준다















