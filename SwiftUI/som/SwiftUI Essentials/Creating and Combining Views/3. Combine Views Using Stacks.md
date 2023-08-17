# ****Combine Views Using Stacks****
![image](https://github.com/jsa0224/somdokki-study/assets/94514250/fac78da9-e4d0-448c-96cb-fd5567474ef1)

이전 섹션에서 생성한 제목 뷰 외에 공원 이름 및 상태와 같은 랜드마크에 대한 세부 정보를 포함하는 텍스트 뷰를 추가한다.

SwiftUI 뷰를 생성할 때 뷰의 본문 속성에서 콘텐츠, 레이아웃 및 동작을 설명한다. 그러나 body 프로퍼티는 단일 보기만 반환한다. 가로, 세로 또는 뒤에서 앞으로 함께 뷰를 그룹화하는 스택에 여러 보기를 결합하고 포함할 수 있다. 

이 섹션에서는 수직 스택을 사용하여 공원에 대한 세부 정보가 포함된 수평 스택 위에 제목을 배치한다.

## Step 1
![image](https://github.com/jsa0224/somdokki-study/assets/94514250/b3af947d-e524-49a5-8ebe-c847ec0e13bb)

Xcode의 구조적 편집 지원을 사용하여 컨테이너 뷰에 뷰를 포함하거나 인스펙터를 열거나 기타 유용한 변경에 도움을 줄 수 있다.

구조화된 편집 팝오버를 표시하려면 텍스트 뷰의 초기화 프로그램을 Command-클릭한 다음 "VStack에 포함"을 선택한다.

## Step 2
![image](https://github.com/jsa0224/somdokki-study/assets/94514250/54f95f6a-1906-4f81-9673-0a6262df9908)

다음으로 라이브러리에서 텍스트 보기를 드래그하여 스택에 텍스트 보기를 추가한다.

Xcode 창의 오른쪽 상단에 있는 더하기 버튼(+)을 클릭하여 라이브러리를 연 다음 Text 보기를 "Turtle Rock" 텍스트 보기 바로 아래의 코드 위치로 드래그한다.

## Step 3

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Text("Turtle Rock")
                .font(.title)
            Text("Joshua Tree National Park")
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

텍스트 보기의 placeholder 텍스트를 "Joshua Tree National Park"로 바꾼다.

## Step 4

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Text("Turtle Rock")
                .font(.title)
            Text("Joshua Tree National Park")
                .font(.subheadline)
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

원하는 레이아웃과 일치하도록 위치를 사용자 지정한다.

위치의 글꼴을 부제목으로 설정한다.

## Step 5

```swift
struct ContentView: View {
    var body: some View {
        VStack(alignment: .leading) {
            Text("Turtle Rock")
                .font(.title)
            Text("Joshua Tree National Park")
                .font(.subheadline)
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

뷰를 리딩 에지로 정렬하도록 VStack 이니셜라이저를 편집한다. 기본적으로 스택은 축을 따라 콘텐츠를 중앙에 배치하고 상황에 맞는 간격을 제공한다.

## Step 6
![image](https://github.com/jsa0224/somdokki-study/assets/94514250/072a4882-5b80-4571-a31b-d1bd7c0ab7c0)

다음으로 위치 오른쪽에 또 다른 텍스트 뷰를 추가할 것이다. 이것은 공원의 상태에 대한 것이다. 캔버스에서 "Joshua Tree National Park"를 Command-클릭하고 "HStack에 포함"을 선택한다.

## Step 7

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack(alignment: .leading) {
            Text("Turtle Rock")
                .font(.title)
            HStack {
                Text("Joshua Tree National Park")
                    .font(.subheadline)
                Text("California")
                    .font(.subheadline)
            }
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

위치 뒤에 새 텍스트 뷰를 추가하고 자리 표시자 텍스트를 공원 상태로 변경한 다음 글꼴을 부제목으로 설정한다.

## Step 8

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack(alignment: .leading) {
            Text("Turtle Rock")
                .font(.title)
            HStack {
                Text("Joshua Tree National Park")
                    .font(.subheadline)
                Spacer()
                Text("California")
                    .font(.subheadline)
            }
        }
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

장치의 전체 너비를 사용하도록 레이아웃을 지시하려면 두 개의 텍스트 뷰를 보유하는 수평 스택에 Spacer를 추가하여 공원과 상태를 분리하라. Spacer는 콘텐츠에 의해서만 크기가 정의되는 대신 포함하는 뷰가 부모 뷰의 모든 공간을 사용하도록 확장된다.

## Step 9

```swift
import SwiftUI

struct ContentView: View {
    var body: some View {
        VStack(alignment: .leading) {
            Text("Turtle Rock")
                .font(.title)
            HStack {
                Text("Joshua Tree National Park")
                    .font(.subheadline)
                Spacer()
                Text("California")
                    .font(.subheadline)
            }
        }
        .padding()
    }
}

struct ContentView_Previews: PreviewProvider {
    static var previews: some View {
        ContentView()
    }
}
```

마지막으로 `padding()` 수정자 메서드를 사용하여 랜드마크의 이름과 세부 정보에 조금 더 많은 공간을 제공한다.
