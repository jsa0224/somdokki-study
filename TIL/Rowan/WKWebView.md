# WKWebView

인앱 브라우저와 같은 상호작용 가능한 웹 콘텐츠를 표시하는 객체.

## Overview

WKWebView 객체는 웹 컨텐츠를 앱의 UI에 원활하게 통합하는 데 사용하는 platform-native 뷰이다. Web view는 완전한 웹 탐색 경험을 지원하고 앱의 기본 뷰와 함께 HTML, CSS 및 JavaScript 컨텐츠를 제공한다. 웹 기술이 native view보다 앱의 레이아웃 및 스타일 요구사항을 더 쉽게 충족할 경우 사용할 것. 예를 들어, 앱의 컨텐츠가 자주 변경될 때 사용할 수 있다.

Web view는 delegate 객체를 통해 navigation 및 UX를 제어할 수 있다. 사용자가 웹 컨텐츠의 링크를 클릭하거나 navigation에 영향을 미치는 방식으로 컨텐츠와 상호 작용할 때 반응하려면 navigation delegate를 사용할 것. 예를 들어, 특정 조건이 충족되지 않으면 사용자가 새 컨텐츠로 이동하지 못하도록 할 수 있다. UI delegate를 사용하여 웹 컨텐츠와의 상호 작용에 대한 응답으로 경고 또는 상황별 메뉴와 같은 기본 UI 요소를 표시한다.

> Note
WKWebView는 iOS 8 이상의 UIWebView 클래스를 대체하고, macOS 10.10 이상의 WebView 클래스를 대체함
> 

프로그래밍 방식으로 WKWebView 객체를 뷰 계층 구조에 포함하거나 Interface Builder를 사용하여 추가할 것. IB는 데이터 탐지기 구성, 미디어 재생 및 상호 작용 동작과 같은 다양한 Customization을 지원한다. 보다 광범위한 customization을 위해서는 WKWebViewConfiguration 객체를 사용하여 프로그래밍 방식으로 web view를 생성하자. 예를 들어, web view configuration 객체를 사용하여 custom URL schemes에 대한 handler를 지정하고, 쿠키를 관리하고, 웹 컨텐츠에 대한 기본 설정을 customize 한다.

Web view가 화면에 나타나기 전에 URLRequest 구조체를 사용하여 웹 서버에서 컨텐츠를 로드하거나 로컬 파일이나 HTML string에서 직접 컨텐츠를 로드할 것. Web view는 초기 load 요청의 일부로 이미지 또는 비디오와 같은 포함된 리소스를 자동으로 로드한다. 그 다음 컨텐츠를 렌더링하고 뷰의 bounds 사각형 내부에 결과를 표시한다. 다음 예제 코드는 default view를 custom WKWebView 객체로 바꾸는 뷰 컨트롤러를 보여준다.

```swift
import UIKit
import WebKit

class ViewController: UIViewController, WKUIDelegate {
    
    var webView: WKWebView!
    
    override func loadView() {
        let webConfiguration = WKWebViewConfiguration()
        webView = WKWebView(frame: .zero, configuration: webConfiguration)
        webView.uiDelegate = self
        view = webView
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        
        let myURL = URL(string:"https://www.apple.com")
        let myRequest = URLRequest(url: myURL!)
        webView.load(myRequest)
    }
}
```

Web view는 웹 컨텐츠에 나타나는 전화번호를 전화 링크로 자동 변환한다. 사용자가 전화 링크를 탭하면 전화 앱이 실행되고 해당 번호로 전화를 건다. 기본 데이터 탐지기 동작을 변경하려면 WKWebViewConfiguration 객체를 사용하자.

또한 `setMagnification(_:centeredAt:)` 을 사용하여 web view에 처음 나타날 때 웹 컨텐츠의 크기를 프로그래밍 방식으로 설정할 수 있다. 이후 사용자는 제스처를 사용해 배율을 변경할 수 있다.

## Manage the navigation through your web content

WKWebView는 링크, 앞으로가기 및 뒤로가기 버튼 등을 사용하여 다양한 웹페이지 간을 탐색하는 기능을 포함하여 완벽한 탐색 경험을 제공한다. 사용자가 컨텐츠의 링크를 클릭하면 web view는 브라우저처럼 작동하여 해당 링크의 컨텐츠를 표시한다. 탐색을 허용하지 않거나 web view의 탐색 동작을 customizing 하려면 `WKNavigationDelegate` 프로토콜을 준수하는 객체인 navigation delegate를 web view에 제공하자. Web view의 탐색 동작을 수정하거나 새 컨텐츠의 로드 진행률을 추적하려면 navigation delegate를 사용하자. 또한 WKWebView의 메서드를 사용하여 컨텐츠를 프로그래밍 방식으로 탐색하거나 앱 인터페이스의 다른 부분에서 탐색을 트리거할 수 있다. 예를 들어, UI에 앞으로가기 및 뒤로가기 버튼이 포함된 경우 해당 버튼을 web view의 `goBack(_:)` 및 `goForward(_:)` 메서드에 연결하여 해당 웹 탐색을 트리거한다. `canGoBack` 및 `canGoForward` 프로퍼티를 사용하여 버튼을 활성화하거나 비활성화할 시기를 결정한다.

## Provide sharing options

사람들은 web view의 컨텐츠를 앱이나 다른 사람들과 공유하기를 원할 수 있다. `UIActivityViewController`를 사용하여 사람들이 웹 컨텐츠를 공유할 수 있는 모든 방법을 제공하는 share sheet를 보여주자.

앱에 `com.apple.developer.web-browser` 권한이 있는 경우 iOS share sheet는 http 또는 https 웹페이지에 대한 홈 화면에 추가를 제공하여 웹 앱이나 북마크에 대한 편리한 링크를 생성할 수 있다. 누군가 현재 웹페이지를 홈 화면에 추가할 수 있도록 하려면 `init(activityItems:applicationActivities:)` 를 호출하여 UIActivityViewController를 생성할 때 ActivityItems 배열에 WKWebView 인스턴스를 포함시킬 것. 브라우저 앱 빌드에 대한 자세한 내용은 `[Preparing your app to be the default web browser](https://developer.apple.com/documentation/xcode/preparing-your-app-to-be-the-default-browser)` 를 참조.

---

# WKNavigationDelegate

탐색 변경 사항을 수락 또는 거부하고 탐색 요청 진행 상황을 추적하는 methods.

## Overview

Web view의 메인 프레임에서 변경 사항을 조정하는 데 사용하는 객체에 WKNavigationDelegate 프로토콜의 메서드를 구현한다. 사용자가 웹 컨텐츠 탐색을 시도하면 web view는 navigation delegate와 조정하여 모든 전환을 관리한다. 예를 들어, 이러한 방법을 사용하여 컨텐츠 내 특정 링크의 탐색을 제한할 수 있다. 또한 이를 사용하여 요청 진행 상황을 추적하고 오류 및 인증 문제에 응답할 수도 있다.

---

# ****WKNavigationAction****

탐색을 발생시키는 작업에 대한 정보가 포함된 객체.

## Overview

WKNavigationAction 객체를 사용하여 앱의 web view 내에서 탐색을 허용할지 여부에 대한 정책 결정을 내림. WKNavigationAction 객체를 직접 생성하지 말 것. 대신 web view는 이를 생성하여 적절한 delegate 객체에 전달한다. Delegate 메서드를 사용하여 작업을 분석하고 결과 탐색이 발생하도록 허용할지 여부를 결정한다.

## Topics

### 네비게이션 타입

- `[navigationType](https://developer.apple.com/documentation/webkit/wknavigationaction/1401914-navigationtype)` : 탐색을 트리거한 작업 유형
    - case linkActivated
    - case formSubmitted
    - case backForward
    - case reload
    - case formResubmitted
    - case other

### 네비게이션 정보 확인

- `[request](https://developer.apple.com/documentation/webkit/wknavigationaction/1401910-request)` : 탐색 작업과 관련된 URLRequest 객체
