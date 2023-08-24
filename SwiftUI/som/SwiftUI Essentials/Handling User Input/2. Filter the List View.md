# ****Filter the List View****
![image](https://github.com/jsa0224/somdokki-study/assets/94514250/53e6a1ed-38e0-4687-b092-7676daee7a8d)

모든 랜드마크를 표시하거나 사용자가 즐겨찾는 항목만 표시하도록 List View를 사용자 정의할 수 있다. 이렇게 하려면 LandmarkList 타입에 약간의 상태를 추가해야 한다. 

상태는 시간이 지남에 따라 변경될 수 있고 뷰의 동작, 콘텐츠 또는 레이아웃에 영향을 미치는 값 또는 값 집합이다. @State 특성이 있는 프로퍼티를 사용하여 뷰에 상태를 추가한다. 

## Step 1

```swift
import SwiftUI

struct LandmarkList: View {
    var body: some View {
        NavigationView {
            List(landmarks) { landmark in
                NavigationLink {
                    LandmarkDetail(landmark: landmark)
                } label: {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

프로젝트 탐색기에서 LandmarkList.swift를 선택하고 목록의 단일 버전만 표시하도록 미리보기를 되돌린다. 

## Step 2

```swift
import SwiftUI

struct LandmarkList: View {
    @State private var showFavoritesOnly = false

    var body: some View {
        NavigationView {
            List(landmarks) { landmark in
                NavigationLink {
                    LandmarkDetail(landmark: landmark)
                } label: {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

초기 값이 false로 설정된 showFavoritesOnly라는 @State 프로퍼티를 추가한다. 상태 프로퍼티를 사용하여 뷰와 해당 하위 뷰에 특정한 정보를 보유하므로 항상 상태를 비공개로 생성한다. 

## Step 3
![image](https://github.com/jsa0224/somdokki-study/assets/94514250/4aca73ae-f935-45b7-9e3e-d08cabf6566d)

캔버스를 새로 고치려면 재개를 클릭한다. 프로퍼티 추가 또는 수정과 같이 뷰 구조를 변경하는 경우 캔버스를 수동으로 새로 고쳐야 한다. 

## Step 4

```swift
import SwiftUI

struct LandmarkList: View {
    @State private var showFavoritesOnly = false

    var filteredLandmarks: [Landmark] {
        landmarks.filter { landmark in
            (!showFavoritesOnly || landmark.isFavorite)
        }
    }

    var body: some View {
        NavigationView {
            List(landmarks) { landmark in
                NavigationLink {
                    LandmarkDetail(landmark: landmark)
                } label: {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

`showFavoritesOnly` 프로퍼티와 각 `Landmark.isFavorite` 값을 확인하여 랜드마크 목록의 필터링된 버전을 계산한다. 

## Step 5

```swift
import SwiftUI

struct LandmarkList: View {
    @State private var showFavoritesOnly = false

    var filteredLandmarks: [Landmark] {
        landmarks.filter { landmark in
            (!showFavoritesOnly || landmark.isFavorite)
        }
    }

    var body: some View {
        NavigationView {
            List(filteredLandmarks) { landmark in
                NavigationLink {
                    LandmarkDetail(landmark: landmark)
                } label: {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

목록에 있는 랜드마크 목록의 필터링된 버전을 사용하라. 

## Step 6

```swift
import SwiftUI

struct LandmarkList: View {
    @State private var showFavoritesOnly = true

    var filteredLandmarks: [Landmark] {
        landmarks.filter { landmark in
            (!showFavoritesOnly || landmark.isFavorite)
        }
    }

    var body: some View {
        NavigationView {
            List(filteredLandmarks) { landmark in
                NavigationLink {
                    LandmarkDetail(landmark: landmark)
                } label: {
                    LandmarkRow(landmark: landmark)
                }
            }
            .navigationTitle("Landmarks")
        }
    }
}

struct LandmarkList_Previews: PreviewProvider {
    static var previews: some View {
        LandmarkList()
    }
}
```

목록이 어떻게 반응하는지 확인하려면 `showFavoritesOnly`의 초기 값을 `true`로 변경한다.
