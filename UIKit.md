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

***

## 5. View

> ### 💁🏻‍♂️💁🏻‍♂️ 5-1 : UIView에 대해서 설명해 주세요

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

> ### 💁🏻‍♂️💁🏻‍♂️ 5-2 : UIWindow에 대해서 설명해 주세요

1. 뷰들을 담을 수 있는 비어있는 컨테이너 입니다. 이벤트를 전달해주는 매개체 역할을 합니다.
2. iOS 앱은 최소 하나 이상의 윈도우를 가지고 있습니다.
3. 스토리보드로 개발할 때는 윈도우가 자동 생성되지만, 그렇지 않은 경우에는 `SceneDelegate` 에 UIWindow를 만들어 rootViewController를 설정해주어야 합니다.

***


### 자신만의 Custom View를 만들려면 어떻게 해야하는지 설명하시오.

### TableView를 동작 방식과 화면에 Cell을 출력하기 위해 최소한 구현해야 하는 DataSource 메서드를 설명하시오.

### 하나의 View Controller 코드에서 여러 TableView Controller 역할을 해야 할 경우 어떻게 구분해서 구현해야 하는지 설명하시오.

### setNeedsLayout와 setNeedsDisplay의 차이에 대해 설명하시오.

### stackView의 장점과 단점에 대해서 설명하시오.

### NSCache와 딕셔너리로 캐시를 구성했을때의 차이를 설명하시오.

### URLSession에 대해서 설명하시오.

### prepareForReuse에 대해서 설명하시오.



### ViewController의 생명주기를 설명하시오.

### TableView와 CollectionView의 차이점을 설명하시오.

### UIKit 클래스들을 다룰 때 꼭 처리해야하는 애플리케이션 쓰레드 이름은 무엇인가?

### 뷰의 위치나 크기를 재조정하려면 어떻게 해야하나요?

### Foundation Kit은 무엇이고 포함되어 있는 클래스들은 어떤 것이 있는지 설명하시오.
