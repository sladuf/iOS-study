## 🍏 UIKit 프레임워크

## 1. Frame/Bounds

> ### 💁🏻‍♂️ 1-1 : frame과 bounds의 차이점에 대해서 설명해 주세요
1. 프레임과 바운스 모두 view의 위치와 크기를 나타냅니다.
2. frame은 super view를 기준으로 view의 위치를 나타냅니다. 만약, `frame.origin = CGPoint(x:10, y:10)`이라면 super view의 (0,0) 기준으로 (10,10)이동한 곳에 왼쪽 위 모서리 값을 찍게 됩니다.
3. bounds는 자기 자신을 기준으로 view의 위치를 나타냅니다. 그래서 처음은 (0,0)으로 초기화 됩니다. bounds의 위치는 **하위뷰의 시점을 이동**하기 위해 사용합니다.
4. frame은 view 영역을 모두 감싸는 사각형으로 크기를 나타냅니다. 만약 view가 회전하여 모서리의 위치가 바뀌는 경우에는 frame.size도 함께 변화합니다.
5. bounds는 view 자체의 크기를 나타냅니다. 그래서 view가 회전을 하더라도 bounds.size의 크기는 변하지 않습니다.

![image](https://user-images.githubusercontent.com/63739061/222399630-9c222608-d7fd-4c8f-84df-377d19cdcccc.png)


> **💁🏻‍♂️💁🏻‍♂️ 하위뷰의 시점을 이동한다는게 무슨 말이죠?**
1. bounds의 값이 (10,10)으로 옮겨지더라도 view는 이동하지 않습니다.
2. A뷰의 `bounds.origin = CGPoint(x:10,y:10)`으로 주고, B뷰의 super view를 A로 설정했다고 가정하겠습니다.
3. bounds의 위치는 하위뷰의 시점을 이동시킨다고 했으므로 A의 (10,10)위치에 있던 B를 A의 시작점 위치로 가져오게 됩니다.
4. 즉, 하위뷰는 super view의 (10,10)위치를 시작점으로 생각하게 되는 것입니다.

![image](https://user-images.githubusercontent.com/63739061/222399965-4bcd958a-a53f-460e-b6b8-cd678edb2342.png)

> ### 💁🏻‍♂️ 1-2 : frame과 bounds은 각각 언제 사용하나요?

1. frame은 view의 위치와 크기를 지정할 때 사용합니다.
2. frame은 super view를 기준으로 위치를 정하고, view 전체 영역의 크기를 지정하므로 시각적으로 설계하기 쉽습니다.
3. bounds는 view를 회전했지만 실제 크기를 알고 싶을 때 사용할 수 있습니다.
4. 또, bounds는 시점을 이동하므로 scrollView에서 스크롤 할 때 사용합니다.

> **💁🏻‍♂️💁🏻‍♂️ 스크롤뷰에서 bounds에 대해서 자세히 설명해 주세요**

1. scrollView에 하위뷰들을 넣어주면 화면을 넘어가는 UI를 스크롤을 통해 볼 수 있습니다.
2. 이때, 하위뷰들이 위치를 이동하는 것이 아니라, scrollView가 하위뷰의 시점을 이동시키는 것입니다.
3. 예를들어, 아래에 있는 데이터를 보고 싶을 때는 scrollView의 bound.origin.y값을 수정하여 **superView가 이동**하는 것입니다.


- [https://babbab2.tistory.com/44](https://babbab2.tistory.com/44)
- [https://babbab2.tistory.com/45](https://babbab2.tistory.com/45)
- [https://babbab2.tistory.com/46](https://babbab2.tistory.com/46)

***

## 2. Frame과 AutoLayout 속도 차이

1. autolayout은 결과적으로 내부에서 frame으로 계산되기 때문에 **autolayout → frame으로 변환**하는 과정을 거칩니다.
2. 그래서 계산 속도는 frame이 더 빠를 수 밖에 없습니다. UI 속도(성능)에 민감한 서비스에 대해서는 frame을 사용할 수 있습니다.
3. 단, frame의 origin x,y값을 그냥 주는 것 보다는 다양한 화면 크기에 대응하기 위해 화면의 크기를 불러와서 비율로 계산하는 방법을 지향해야 합니다.
4. 그런 면에서 개발 속도는 view의 배치가 상대적으로 간단한 autolayout이 더 빠를 수 있습니다.

***

## 3. CGFloat과 Float의 차이

1. Float는 실수타입을 나타내며 32비트를 사용합니다.
2. CGFloat는 CPU 아키텍처에 따라 자동적으로 32비트가 될 수도, 64비트가 될 수도 있습니다.
3. 멀티 플랫폼을 개발하는 경우 CGFloat를 사용하면, 실수타입에 대해 코드 수정을 할 필요가 없습니다.

***

## 4. Controller

> ### 💁🏻‍♂️ 4-1 : 모든 View Controller 객체의 상위 클래스는 무엇인가요?

1. 모든 ViewController의 상위클래스는 UIViewController입니다.
2. UIViewController는 UIKit에서 **뷰의 계층을 관리해주는 객체** 입니다.
3. 데이터가 변화함에 따라 뷰 컨텐츠를 업데이트 하고, 뷰의 크기와 레이아웃을 관리합니다.
4. UIViewController는 **UIResponder**을 상속하기 때문에 이벤트를 처리할 수도 있습니다.

> ### 💁🏻‍♂️ 4-2: UIResponder가 무엇인가요?

1. UIKit에서 이벤트 처리에 대한 부분을 담당합니다.
2. UIKit에서는 이벤트가 발생하면 이벤트를 **Responder** 객체에게 전달해서 처리합니다.
3. Responder는 UIResponder의 인스턴스입니다. UIViewConroller도 Responder입니다.
4. Responder는 UIResponder에 있는 이벤트 처리 메서드중에 사용할 메서드를 오버라이드 해서 구현해야 합니다.

> **💁🏻‍♂️💁🏻‍♂️ 그럼 first responder에 대해서 말해보실래요?**

1. UIKit에서는 Responder들을 연결해서 관리합니다. 이를 Responder Chain이라고 합니다. Responder Chain은 링크드 리스트 구조를 가지고 있습니다.
2. 이벤트를 받은 Responder는 자신이 이벤트를 처리하거나 Responder Chain에 있는 다음 Responder에게 이벤트를 전달할 수 있습니다.
3. First Responder는 이벤트를 가장 먼저 전달받는 Responder라는 의미입니다. 즉, 이벤트가 발생하면 가장 먼저 처리할 Responder라는 것입니다.

- [https://yjhong.tistory.com/34](https://yjhong.tistory.com/34)

> ### 💁🏻‍♂️ 4-3: UINavigationController에 대해서 설명해 주세요

1. 스택 계층구조를 기반으로 한 컨테이너 뷰컨트롤러 입니다.
2. 뷰 컨트롤러들을 스택에 담아서 관리합니다. (자료구조 Stack의 FILO 특성을 그대로 반영합니다.)
3. 가장 아래에 위치한 뷰컨트롤러는 rootViewController로 컨테이너를 빠져나올 수 없는 뷰컨트롤러입니다.
4. UINavigationController의 메서드를 사용해서 뷰컨트롤러를 스택에 push, pop할 수 있습니다.
- [https://etst.tistory.com/84](https://etst.tistory.com/84)

> **💁🏻‍♂️💁🏻‍♂️ present와 pushViewController는 어떤 차이점이 있죠?**

1. present는 현재 화면 위에 새로운 view controller을 모달창으로 띄웁니다.
2. 네비게이션의 pushViewController 메서드는 네비게이션 스택에 view controller를 추가하고 최상단에 띄워주며, navigation bar를 자동 생성하여 보여줍니다.

> ### 💁🏻‍♂️ 4-4: ViewController의 생명주기를 설명해 주세요

1. **`init(nibName:bundle:)`** or **`init(coder:)`**: 뷰컨트롤러 객체가 생성됩니다.
2. **`loadView()`**: 뷰컨트롤러의 뷰가 **메모리에 로드**됩니다.
3. **`viewDidLoad()`**: 뷰컨트롤러의 뷰가 **메모리에 로드된 직후** 호출됩니다.
4. **`viewWillAppear(_:)`**: 뷰컨트롤러의 **뷰가 화면에 나타나기 직전**에 호출됩니다.
5. **`viewWillLayoutSubviews()`**: 뷰컨트롤러의 뷰가 **서브뷰들을 레이아웃하기 직전**에 호출됩니다.
6. **`viewDidLayoutSubviews()`**: 뷰컨트롤러의 **뷰가 서브뷰들을 레이아웃한 직후** 호출됩니다.
7. **`viewDidAppear(_:)`**: 뷰컨트롤러의 **뷰가 화면에 나타난 직후** 호출됩니다.
8. **`viewWillDisappear(_:)`**: 뷰컨트롤러의 **뷰가 화면에서 사라지기 직전**에 호출됩니다.
9. **`viewDidDisappear(_:)`**: 뷰컨트롤러의 **뷰가 화면에서 사라진 직후** 호출됩니다.
10. **`deinit`**: 뷰컨트롤러 객체가 **메모리에서 해제**될 때 호출됩니다.

> **💁🏻‍♂️💁🏻‍♂️ 각각의 메서드에서 어떤 작업을 하면 좋을까요?**

1. **`viewDidLoad()`**: 뷰컨트롤러가 로드될 때 처음에 한 번만 호출됩니다. 예를 들어 뷰컨트롤러에서 사용할 데이터를 로드하거나, 뷰를 초기화하는 작업
2. **`viewWillAppear(_:)`**: 뷰가 나타날 때 마다 수행해야 하는작업. 예를 들어 새로운 데이터 가져오기, 화면 갱신
3. **`viewDidAppear(_:)`**: 뷰가 나타나자 마자 시작해야 하는 작업. 예를 들어 애니메이션
4. **`viewWillDisappear(_:)`**: 뷰가 사라지기 전에 시작해야 하는 작업. 예를 들어 애니메이션, 입력된 데이터저장
5. **`viewDidDisappear(_:)`**: 뷰가 사라질 때 마다 수행해야 하는 작업. 예를 들어 메모리에서 뷰컨트롤러 데이터를 해제하는 작업
6. **`deinit`**: 뷰컨트롤러가 메모리에서 해제될 때 수행해야 하는 작업. 사용했던 리소스를 해제 (객체 참조 해제)

***

## 5. View

> ### 💁🏻‍♂️ 5-1 : UIView에 대해서 설명해 주세요

1. UIView는 직사각형 모양의 화면을 관리하는 객체입니다.
2. UIKit Core Graphics를 사용하여 직사각형 영역에 콘텐츠를 그립니다.
3. Layout을 사용하여 뷰 계층 구조의변경에 따라 뷰의 크기, 위치를 조정하는 규칙을 만들 수 있습니다.
4. 이벤트에 응답할 수 있습니다. 

- [https://icksw.tistory.com/139](https://icksw.tistory.com/139)

> **💁🏻‍♂️💁🏻‍♂️ UIView 에서 Layer 객체는 무엇이고 어떤 역할을 담당하나요?**

1. 그래픽을 그리기 위해서는 GPU에 직접 접근해서 그리면 렌더링 속도가 매우 빠르지만 코드의 양이 많은 단점이 있습니다.
2. 이를 보완하기 위해 고수준 프레임워크인 Core Animation과 UIKit 프레임워크가 생기게 되었습니다.
3. UIKit은 Core Animation보다 한 단계 높은 수준의 API 입니다.
4. 즉, Core Animation보다 코드를 작성하기 쉽다는 말이기도 합니다. 
5. UIView.layer는 CALayer의 객체 입니다. CALayer는 Core Animation에서 제공하고, UIView는 UIKit에서 제공합니다.
6. layer를 사용하면 조금더 복잡한 애니메이션과 퍼포먼스를 보여줄 수 있습니다.
- [https://ios-development.tistory.com/977](https://ios-development.tistory.com/977)

> ### 💁🏻‍♂️ 5-2 : UIWindow에 대해서 설명해 주세요

1. 뷰들을 담을 수 있는 비어있는 컨테이너 입니다. 이벤트를 전달해주는 매개체 역할을 합니다.
2. iOS 앱은 최소 하나 이상의 윈도우를 가지고 있습니다.
3. 스토리보드로 개발할 때는 윈도우가 자동 생성되지만, 그렇지 않은 경우에는 `SceneDelegate` 에 UIWindow를 만들어 rootViewController를 설정해주어야 합니다.

***

## 6. tableView & collectionView

> ### 💁🏻‍♂️ 6-1 : TableView를 동작 방식을 설명해 주세요

1. 테이블뷰는 재사용큐를 통해 메모리를 효율적으로 사용할 수 있도록 합니다.
2. 화면에 보이는 cell과 앞 뒤의 cell 몇 개만 만들어두고, 나머지는 스크롤에 의해 생성됩니다.
3. `dequeueReusableCell()`을 사용해서 화면에 보여주고 싶은 cell이 재사용큐에 있는지 확인하고, 있으면 재사용큐에서 꺼내서 cell을 재활용합니다. (identifier를 통해 확인)
4. cell이 화면에 보여지기 직전에 `WillDisplayCell` 함수가 호출됩니다.
5. 스크롤을 통해 cell이 화면 밖으로 벗어나게되면 `didEndDisplayingCell` 함수가 호출되고, cell은 재사용큐에 들어가서 대기합니다.
6. iOS 10+ 부터는 `UITableViewDataSourcePrefetching` 프로토콜을 채택함으로써 화면에 보이지 않는 cell의 정보를 미리 호출하여 테이블뷰가 스크롤될 때 cell을 더 빠르게 로드할 수 있습니다.

- https://i-colours-u.tistory.com/15

> ### 💁🏻‍♂️ 6-2 : TableView 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명해 주세요

- 섹션마다 표시할 셀의 개수를 반환하는 메서드
    - `func tableView(UITableView, numberOfRowsInSection: Int)`
- 인덱스마다 어떤 셀을 사용할지 반환하는 메서드로, cell에 데이터를 바인딩하거나 조작하여 return 합니다.
    - `func tableView(UITableView, cellForRowAt: IndexPath)`

> ### 💁🏻‍♂️ 6-3 : TableView와 CollectionView의 차이점을 설명해 주세요

1. tableView는 세로 스크롤만 지원하며, 단일 컬럼, 단일 섹션 레이아웃만 지원합니다.
2. collectionView는 상하좌우 스크롤이 가능하며, 다중 컬럼을 가질 수 있고, 다양한 레이아웃을 옵션을 지원합니다.

> ### 💁🏻‍♂️ 6-4 : prepareForReuse에 대해서 설명해 주세요

1. 테이블뷰는 같은 identifier를 가진 cell을 재사용하여 메모리를 효율적으로 사용합니다.
2. 화면에서 사라진 셀은 재사용큐에 저장되고 같은 identifier를 가진 cell을 호출할 때 재사용됩니다.
3. `prepareForReuse`는 테이블 뷰 cell이 재사용될 때 호출되는 메서드입니다.
4. prepareForReuse는 `cellForItemAt` 이 호출되기 전에 호출되어서 셀의 설정들을 초기화할 수 있도록 도와줍니다.

```
🎯 설명에는 편의상 테이블뷰만 얘기했지만, 컬렉션뷰 cell도 가지는 메서드 입니다!!
```

## 7. URLSession

> ### 💁🏻‍♂️ 7-1 : URLSession에 대해서 설명해 주세요.

1. URLSession은 네트워크 통신을 제공하는 기본 프레임워크의 클래스입니다. Foundation 프레임워크에 포함되어 있습니다.
2. SessionTask를 만들고 여기에 통신에 대한 설정과 콜백을 정의해서 넘기면 네트워크 통신이 완료되었을 때 클로저가 실행됩니다.
3. URLSession은 내부적으로 GCD를 시용하여 네크워크 작업을 비동기적으로 처리합니다.

> **💁🏻‍♂️💁🏻‍♂️ GCD로 어떻게요? 자세히 설명해주세요**

1. URLSession에서 데이터 요청을 시작하면, **GCD**는 백그라운드에서 작업을 수행합니다.
2. **FIFO**방식을 사용하여 요청이 들어온 순서대로 처리합니다.
3. 백그라운드에서 작업을 수행하기 때문에 메인 스레드를 차단하지 않습니다. 즉, 작업하는 동안 UI를 멈추지 않고 사용자와 계속해서 상호작용 할 수 있습니다.

> ### 💁🏻‍♂️ 7-2 : URLDownloadTask와 URLSessionDataTask를 비교해서 설명해 주세요.

1. 둘 다 URLSession에서 제공하는 네트워크 데이터 다운로드 작업을 수행하는 데 사용되는 클래스입니다. 비동기 방식으로 작업을 수행합니다.
2. **downloadTask**는 파일 다운로드에 특화된 작업 클래스 입니다.
3. **로컬 저장소에 저장**하기 때문에 특정 경로에 저장한다면 파일을 영구적으로 보관할 수 있습니다.
4. 저장경로를 지정하지 않으면 기본적으로 앱의 **캐시 디렉터리**에 저장됩니다. 이는 앱이 종료될 때 자동으로 삭제됩니다.
5. **dataTask**는 일반적인 데이터 다운로드를 할 때 사용됩니다.
6. NSData 타입으로 데이터를 내려받기 때문에 로컬 저장소에는 저장하지 않고 **메모리에 저장**합니다.
7. 메모리에 데이터를 저장하기 때문에 dataTask를 통해 용량이 큰 데이터를 받으면 메모리 부족 현상이 발생할 수 있습니다.

|  | DownloadTask | DataTask |
| --- | --- | --- |
| 데이터의 종류 | 영상, 음악, 이미지, PDF | JSOM, XML |
| 데이터의 크기 | 큰거 | 작은거 |
| 데이터의 저장 | 로컬(영구 저장 가능) | 메모리 |

## n. 추가 개념

> ### 💁🏻‍♂ n-1 : UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?

- mainThread : UIKit은 UI를 다루는 프레임워크 입니다. UI와 관련된 코드는 반드시 mainThread에서 동작해야 합니다.


> ### 💁🏻‍♂️ n-2 : 자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.

1. custom view를 만들기 위해서는 UIView를 반드시 상속해야 합니다.
2. 이니셜라이저를 만들고 싶은 경우에는 `super.init(frame: CGRect)`를 반드시 호출해야 하며, `required init?(coder: NSCoder)`도 함께 작성해야 합니다.

> ### 💁🏻‍♂ n-3 : 뷰의 위치나 크기를 재조정하려면 어떻게 해야하나요?

1. **frame**을 사용합니다. frame.origin을 통해 x,y 좌표값으로 view를 이동하고, frame.size를 통해 width, height값을 통해 크기를 조절할 수 있습니다.
    - frame을 사용하면 정확한 위치와 크기를 지정할 수 있습니다.
2. **autolayout**을 사용합니다. 뷰의 위치와 크기를 constraints로 지정하여 상위 뷰의 크기에 따라 뷰의 위치와 크기를 자동으로 조절합니다.
    - autolayout을 사용하면 다양한 화면 크기에 대응하여 뷰를 유연하게 배치할 수 있습니다.
3. **CGAffineTransform**을 사용합니다. 뷰의 변환을 나타내는 구조체로, 뷰의 회전, 확대, 축소 등의 변형을 줄 수 있습니다.
    - 복잡한 뷰의 이동, 애니메이션을 구현해야 하는 경우에 사용하는 것이 적합합니다.

> ### 💁🏻‍♂ n-4 : Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명해 주세요

1. Cocoa프레임워크에서 제공되는 기본 프레임워크 입니다.
2. objc와 swift에서 모두 사용할 수 있습니다.
3. 다양한 플랫폼 (macOS, iOS, watchOS, tvOS)에서 공통으로 사용할 수 있습니다.
4. 데이터 관리, 파일 및 디렉터리 관리, 네크워킹, 문자열 처리, 날짜 및 시간 처리 등의 기능을 제공합니다.

- 대표적인 클래스들

```
- NSString / NSMutableString : 문자열 처리 클래스
- NSArray / NSMutableArray : 배열 처리 클래스
- NSDictionary / NSMutableDictionary : 키-값 쌍 데이터 처리 클래스
- NSUserDefaults : 사용자 설정 데이터 처리 클래스
- NSURL / NSURLRequest / NSURLSession : 네트워킹 처리 클래스
- NSFileManager : 파일 및 디렉터리 관리 클래스
- NSDate / NSDateComponents / NSDateFormatter : 날짜 및 시간 처리 클래스
- NSTimer : 타이머 클래스
- NSNotificationCenter : 알림 처리 클래스
- NSRegularExpression : 정규 표현식 처리 클래스
- NSJSONSerialization : JSON 데이터 처리 클래스
```

> ### 💁🏻‍♂ n-5 : NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명해 주세요.

1.  `key-value` 쌍으로 데이터를 저장하는 자료구조라는 공통점이 있습니다.
2. NSCache는 메모리 공간이 부족하면 자동으로 캐시된 객체를 제거함으로 메모리 관리 측면에서 유용합니다.
3. NSCache는 동시에 여러 스레드에서 접근하여 데이터를 수정할 수 있습니다.
4. 딕셔너리는 thread-safe하지 않아서 여러 스레드에서 동시 접근하면 오류가 발생할 수 있습니다.
5. 메모리 관리와 성능 측면에서 NSCache를 쓰는 것을 권장하지만 **오버헤드**가 발생하므로 단일 스레드 환경이거나 작은 데이터를 캐싱할 때는 딕셔너리를 사용할 수 있습니다.

> **💁🏻‍♂️💁🏻‍♂️ NSCache는 왜 thread-safe한가요?**

- 딕셔너리와 달리 NSCache는 내부적으로 뮤텍스와 같은 동시성 제어 메커니즘을 사용합니다.

> **💁🏻‍♂️💁🏻‍♂️ NSCache는 왜 오버헤드가 발생하나요?**

1. 캐시된 객체가 많아지면 메모리 부족 현상이 발생할 수 있습니다.
2. NSCache는 이를 방지하기 위해 캐시된 객체의 총 크기를 제한하고, 제한에 도달하면 **LRU알고리즘**을 사용하여 오래 전에 사용된 객체를 제거합니다.
3. 이런 추가적인 기능 때문에 딕셔너리보다 많은 오버헤드가 발생합니다.



### 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.

### setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오.

### stackView의 장점과 단점에 대해서 설명하시오.




