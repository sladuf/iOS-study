# 🍎 앱 디자인 패턴

## 1. MVVM

> ### 💁🏻‍♂️ : MVVM 패턴에 대해 설명해주세요

1. Model / View / View Model 로 이루어진 디자인 패턴입니다.

2. MVVM 패턴의 목표는 **뷰 로직과 비즈니스 로직을 분리**하는 것입니다.

3. View 에서는 오직 뷰 로직만 다룹니다.

4. ViewModel 에서는 View 를 위한 상태와 메서드를 정의하고 View 는 ViewModel 의 상태변화를 **옵저버 패턴**을 통해 관찰합니다. 이를 **데이터 바인딩**이라고 합니다.

5. ViewModel 은 View 가 쉽게 사용할 수 있도록 Model 의 데이터를 가공해서 View 에게 전달합니다.

- **MVVM 장점**

1. 뷰 로직과 비즈니스 로직의 책임을 분리했기 때문에 테스트 및 유지 보수가 유용합니다.

2. 개발자와 디자이너가 병렬적으로 작업이 가능하다. UI 디자인이 나오지 않았더라도 미리 정의된 뷰와 뷰 모델을 먼저 개발할 수 있습니다.

3. 개발을 할때는 수정하기 좋은 코드를 짜는 것이 좋은데, MVVM 패턴을 통해서 코드 변경을 최소화하며 수정할 수 있습니다.

***

## 2. 옵저버 패턴

> ### 💁🏻‍♂️ : 옵저버 패턴에 대해 설명해주세요

1. 옵저버 패턴이란, 어떤 객체의 상태가 변화할 때 그를 관찰하는 구독자들에게 이벤트를 발생시켜주는 디자인 패턴입니다. 이 때 이벤트를 발행하는 객체를 Publisher, 구독자를 Observer 라고 합니다.

2. 옵저버 패턴을 사용함으로써 **객체지향의 Open-Close 원칙** (개방 폐쇄 원칙)을 지킬 수 있습니다. (옵저버 패턴의 장점)

> **💁🏻‍♂️ → 왜죠?**


1. Publisher 와 Observer 가 인터페이스를 통해 느슨하게 결합되어 있기 때문입니다. 

2. Observer 는 Publisher 가 누구인지 정확하게 알고 있어야 하지만, Publisher 는 구체적으로 구독자를 알지 못해도, Observer 라는 인터페이스를 지키는 객체들이 있다는 사실 정도만 알고 있어도 됩니다.

3. Observer interface 를 사용하면, 새로운 타입의 구독자가 생겼을 때 Oberver interface 를 준수하는 형태로 구현하기만 하면 기존 코드가 돌아가는데 문제가 없습니다. (기능 확장에 열려있다)

4. Observer 들의 특성이 변경될일이 생겼다고 했을 때, 일일히 클래스를 수정할 필요가 없이 interface 통해 효율적으로 수정을 할 수 있습니다. (기존 코드의 변경에 닫혀있다)

***


## 3. 애플의 MVC / MVVM

<Blockquote>

** 💁🏻‍♂️ : 애플은 Swift 라는 언어를 설계할 때 MVC 를 추구하며 설계했습니다. UIKit 에서 ViewController 라는 노골적인 단어를 사용하는 것만 봐도 알 수 있죠. 왜 그렇게 했을까요? **

</Blockquote>

1. 여러가지 변형된 형태의 MVC 들이 있었지만 애플이 추구한 MVC 는 명확한 방향성이 있었습니다.

2. 그건 바로 **View 와 Model 의 완전한 분리**입니다. 다시말해, 뷰와 모델이 소통하기 위해서는 반드시 ViewController 를 거쳐야 한다는 특징입니다.

3. 이렇게 한 이유는 '**UI 재사용성을 극대화**'하기 위해서입니다. 뷰에 특정한 로직이 들어가거나, 모델에서 직접 데이터를 받아오거나 가공하게 한다면 뷰의 재사용성은 낮아질 것입니다.

4. 뷰는 그저 어떻게 화면에 그릴지만 담당하고, 셀이 눌리면 어떤 작업을 할지, 셀 안에 어떤 텍스트를 넣어야 할지와 같은 것들은 Delegate, DataSource 프로토콜을 활용해서 UIViewController 에게 위임합니다.

5. 덕분에 그래서 UITableView, UICollectionView 와 같은 UI컴포넌트들은 거의 모든 iOS 앱에서 재사용됩니다. 괜찮은 커스텀 뷰를 제작한다면 서로 다른 앱 에서도 재활용할수도 있습니다. 하지만 ViewController 의 재사용성은 매우 낮습니다.

<Blockquote>

**💁🏻‍♂️ +) 그럼에도 불구하고 개발자들은 왜 설계 역사를 거스르고 MVVM 패턴을 많이 사용하는 것일까요?**
</Blockquote>

1. 애플의 MVC 에는 문제점이 있었습니다. 그건 View 와 ViewController 가 너무 강하게 결합된다는 것입니다. UIKit 프레임워크를 사용하는 이상, ViewController 는 매우 중요한 역할을 담당할 수 밖에 없습니다.

2. ViewController 가 직접 View 를 생성하는데다가, View 의 대부분 로직을 ViewController 에게 위임하기 때문에, 결합도가 매우 높을 수 밖에 없습니다. 프레젠테이션 로직이 결합되어있으면 유닛 테스팅을 할 때도 불편함으로 다가옵니다.

3. 그래서 MVVM 패턴 에서는 View 와 ViewController 를 묶어 'View'로 간주합니다. MVVM 의 목표는 뷰 로직과 비즈니스 로직을 분리하는 것이고, 이는 테스팅 및 유지 보수에 더 유리한 패턴이 됩니다. 


- https://velog.io/@eddy_song/ios-mvc


***

## 4. MVP vs MVVM

> ### 💁🏻‍♂️ : MVP 와 MVVM 의 차이점은 무엇인가요?

1. 가장 큰 차이점은, MVVM 에서는 옵저버 패턴을 활용한 View Data Binding이 일어난다는 것입니다.

2. MVP 에서는 View 와 Presenter 사이에 의존성이 있었지만, MVVM 은 상대적으로 View, ViewModel, Model 모두 의존성이 낮습니다.

```
📌 애플의 MVC 와 애플의 MVP 는 비슷하게 동작하지만 이런 구조적 차이가 있다.
- MVC 의 V 는 (View) 이고 C 는 (ViewController)이다.
- MVP 의 V는 (View + ViewController)이고 P (Presenter) 가 따로 존재.
- ViewController 와 Presenter 는 같은 역할 -> View 와 Model 의 분리. 다리 역할.

🤔 그럼 MVC 랑 MVP 는 다를게 뭔데 ?
- 사실 애플의 MVC 와 애플의 MVP 는 구현상의 차이 말고는 큰 차이가 없다. 
- MVP 의 Presenter 에서 단지 UIViewController 의 부담을 덜어줄 수 있다는 정도.
- 전통적인 MVC (애플의 MVC 말고) 는 View 와 Model 사이에 결합성이 존재했다.
- MVP 는 그래서 전통적인 MVC 의 단점을 해결하기 위해서 등장했다.
- View 와 Model 사이의 결합성은 해결했지만, 이로 인해 View 와 Presenter 사이의 결합성이 생겼다.
```

- https://ykss.netlify.app/web/design_pattern/

***

## 5. 디자인 패턴 정의

> ### 💁🏻‍♂️ : 디자인 패턴이란 무엇이고, 왜 사용하나요?

- 디자인 패턴이란 일반적인 개발 과정에서 자주 발생하는 문제를 해결하기 위한 개발자들의 교과서입니다.

- 사용 이유

1. 이미 증명된 솔루션이기 때문에 개인적으로 생각해낸 해결방안보다 사용할 근거가 충분합니다.

2. 약속되어있기 때문에 아예 다른 사람이 처음 코드를 보게 되더라도, 디자인 패턴에 대한 이해만 있다면 수월하게 코드를 이해할 수 있습니다.

- https://velog.io/@k7120792/Model-View-ViewModel-Pattern#%EC%9E%A5%EC%A0%90

***

# 🍏 iOS 프로그래밍

## 6. 메모리 관리

> ### 💁🏻‍♂️ 6-1 : 메모리 릭에 대해 설명해주세요

- 메모리 릭 (= 메모리 누수) 이란 **사용되고 있지 않는 인스턴스가 메모리에서 해제되지 않아 메모리 공간이 낭비**되는 현상입니다.

> ### 💁🏻‍♂️ 6-2 : 순환 참조에 대하여 설명해주세요

1. 두 가지 객체가 서로에 대한 강한 참조 상태를 가질 때 순환 참조라고 합니다.

2. 순환 참조가 발생하면 메모리 누수 현상이 발생하기 때문에 약한 참조를 통해 이를 해소시켜줘야 합니다.

> ### 💁🏻‍♂️ 6-3 : 강한 참조와 약한 참조에 대하여 설명해주세요

1. 강한 참조는 reference count 를 증가시키고, 약한 참조는 reference count 를 증가시키지 않습니다.

2. 일반적인 참조 방법이 강한 참조 방법이고, weak / unowned 등의 키워드를 사용해서 약한 참조를 할 수 있습니다.

> ### 💁🏻‍♂️ 6-4 : Retain Count 방식에 대해 설명해주세요

1. ARC 에서 사용하는 메모리 관리 방식을 Retain Count 방식이라고 합니다.

2. 메모리에서 reference count 를 증가시키는 것을 retain, 감소시키는 것을 release 라고 합니다.

> ### 💁🏻‍♂️ 6-5 : weak 과 unowned 를 비교 설명해주세요

1. unowned 는 **객체를 직접** 가리키지만, weak 는 **side table 을 거쳐서** 객체를 가리킵니다.

2. 그렇기 때문에 unowned 키워드를 사용할 경우, strong reference count == 0 이 되어버리면 **dangling pointer** 가 되어버리기 때문에 **인스턴스의 해제 시점을 예민하게 고려**해줘야 합니다.

3. weak 는 weak reference 를 증가시키고 unowned 는 unowned reference 를 증가시킵니다.

> **💁🏻‍♂️ 그러면 side table 이 무엇이고 reference count 종류에 대해 자세히 설명해보실래요**

1. **reference count 에는 3가지 종류**가 있습니다. strong / unowned / weak. HeapObject struct 내부에서는 이 3가지에 대한 rc 들을 모두 카운팅합니다.

2. strong rc == 0 이 되면 인스턴스가 **deinit** 상태가 됩니다. 이는 메모리가 dealloc 된 상태는 아닙니다.

3. unowned rc == 0 이 되면 **dealloc** 상태가 되어 완전히 메모리에서 해제됩니다.

4. dealloc 이 되었어도 weak rc != 0 이라면 side table 이 메모리에 남아있습니다. 이를 **freed** 상태라고 합니다. 

5. weak rc == 0 이 된다면 비로소 객체는 완전히 죽어 **DEAD** 상태가 됩니다.

6. **side table** 은 HeapObject 에서 **weak rc 를 관리하기 위해 별도로 생성된 자료구조** 입니다. weak 참조를 하게 되면 자동으로 side table 이 생성됩니다.

7. side table 은 Swift4 이전의 **좀비 오브젝트 문제**를 해결하기 위해 도입되었습니다.

> **💁🏻‍♂️ 좀비 오브젝트가 뭔데요?**

1. strong rc == 0 이지만 weak rc > 0 일 경우, 메모리에서 완전한 해제가 일어나지 않은 오브젝트를 말합니다.

2. 런타임에 weak reference 를 통해서 좀비 오브젝트가 해제 될 수 있지만 비효율적입니다.

- https://babbab2.tistory.com/27
- https://jeonyeohun.tistory.com/373
- https://linux-studying.tistory.com/34
- https://velog.io/@rnfxl92/Strong-Weak-Unowned
- https://sihyungyou.github.io/iOS-side-table/
- https://github.com/apple/swift/blob/d1c87f3c936c41418ee93320e42d523b3f
- https://velog.io/@wonhee010/Zeroing-Object-Life-Cycle

> ### 💁🏻‍♂️ 6-6 : 메모리를 힙과 스택으로 나누는 이유는 무엇일까요?

1. 스택은 이미 할당된 공간을 사용하는 것이기 때문에 빠르고, 힙은 동적 할당을 해서 pointer 로 접근을 합니다. 

2. 예를 들어 함수 내에서 객체의 값을 변경해줬다고 했을 때, **함수가 할 일을 마치고 스택에서 pop 되어도 변경된 객체의 값은 유지되어야 합니다.** 그래서 스택과 별도로 객체 데이터를 저장하기 위해서 힙이 필요합니다.

- https://velog.io/@heyksw/Kotlin-Object%EC%99%80-Class

***

## 7. ARC / MRC / GC

> ### 💁🏻‍♂️ 7-1 : ARC, MRC란 무엇인지 설명해주세요

1. ARC 와 MRC 는 모두 힙의 메모리를 관리하는 기법입니다.

2. ARC 는 Automatic. <u>**자동으로 HeapObject 안에 들어있는 Reference Count를 계산**</u>해서 메모리를 관리 해주는 방법입니다. 
	→ <u>**Java의 GC와는 다르게 컴파일 시점에 실행됩니다.**</u>

3. MRC 는 Manual. 수동으로 Reference Count를 계산합니다. Retain, release 메서드를 직접 작성해줘야합니다. Objective-C 에서 사용합니다.

```
🧐
- 동적 할당으로 인스턴스가 생성되면 해당 정보는 HeapObject 라는 struct로 관리됩니다.
- HeapObject 에는 동적 할당되는 객체를 구성하는 데이터 즉 reference count 와 type meta data 를 갖습니다.
```

> ### 💁🏻‍♂️ 7-2 : ARC는 compile time에 실행되는데 어떻게 동적으로 실행되는 것들의 reference count를 세고 메모리 관리를 할 수 있나요 ?

1. ARC 는 compile time 에 자동으로 retain 과 release 를 적절한 위치에 삽입해줍니다. MRC 에서 수동으로 작성하던 메서드를 자동으로 작성해줍니다.

2. Run time에 코드가 실행되다가 retain/release 에 의해 count 가 0이 되면 메모리를 해제합니다.

> ### 💁🏻‍♂️ 7-3 :  ARC와 GC는 어떤 차이점이 있나요? 

1. ARC는 컴파일 타임에 실행되고, GC는 런타임에 실행됩니다.

2. 이에 따라 ARC는 런타임 성능이 좋아진다는 장점을 갖지만, 앱 실행중 순환참조 발생시 매우 치명적이라는 단점을 갖습니다.

3. GC는 런타임에 참조를 계속 추적하는 리소스가 필요하다는 단점을 갖지만, ARC에 비해 치명적인 메모리 누수를 막을 확률이 높다는 장점을 갖습니다.

```
😀 ARC와 GC는 모두 힙메모리를 관리한다는 공통점을 갖습니다.
```

> ### 💁🏻‍♂️ 7-4 :  ARC가 메모리 해제를 할 수 없는 상황에 대해 설명해보세요. GC는 해결할 수 있을까요?

1. ARC의 경우 단순히 **컴파일 타임에 reference counting** 을 통해 메모리 관리를 하기 때문에 순환참조가 발생할 경우 메모리 해제를 하기 어렵습니다.

2. GC는 **런타임에 Mark-and-Sweep** 방식으로 모든 인스턴스를 체크합니다. 따라서 순환참조가 발생하더라도 메모리를 해제할 수 있습니다.

- https://sujinnaljin.medium.com/ios-arc-%EB%BF%8C%EC%8B%9C%EA%B8%B0-9b3e5dc23814
- https://babbab2.tistory.com/26

***

## 8. Delegate 패턴

> ### 💁🏻‍♂️ 8-1 :  Delegate 패턴에 대해 설명해주세요

1. Delegate 패턴은 어떤 객체가 해야 하는 일을 부분적으로 **확장해서 구체적으로 대신 처리**하게 합니다.

2. 프로토콜에 정의된 내용을 누군가 대신해주길 바라며 떠넘깁니다 (delegating). 그리고 일을 위임받은 자 (delegated) 는 delegate = self 해서 그 일을 구체적으로 대신해줍니다.

3. 델리게이트 패턴을 사용함으로써 **느슨한 결합**을 가질 수 있습니다. **프로토콜을 만족하기만 하면** 그 누구든 일을 대신해줘도 상관 없다는 뜻인데, 이는 요구 사항이 변경될 경우 작업을 매우 쉽게 수정할 수 있다는 뜻과도 같습니다. **다른 객체로 교체해도 잘 동작**하는 코드를 짤 수 있습니다.

4. 그리고 델리게이트 패턴은 **책임 분리를 촉진**합니다. 기능을 위임할 수 있는 객체가 있다는 것은 그만큼 **현재 객체에서 책임할 부분이 적어진다**는 뜻이기 때문입니다.

- 애초에 여기서 예를 들어버려서 설명해도 좋을 것 같다. ex) UITextViewDelegate

```swift
// 날 수 있는 새.
protocol Bird {
    func fly()
}
// 날아다니는 걸 보여주는 서커스.
class Circus {
    // 누구든 좋으니 날 수 있는 놈 아무나 날아봐. 해줘 ~
    // 누군가 날아주길 기대하며 떠넘긴다. (delegating)
    var delegate: Bird?
    
    func showFlying() {
        // 독수리인지 비둘기인지는 중요치 않아. 걍 아무나 날기만 해줘.
        delegate?.fly()
    }
}
// 독수리
class Eagle: Bird {
    init(circus: Circus) {
        // 내가 날아줄게 (delegated)
        circus.delegate = self
    }
    
    func fly() {
        print("쌔애앵")
    }
}

// 비둘기
class Pigeon: Bird {
    init(circus: Circus) {
        // 내가 날아줄게 (delegated)
        circus.delegate = self
    }
    
    func fly() {
        print("구구구")
    }
}

var someCircus = Circus()
var somePigeon = Pigeon(circus: someCircus)

// 만약 참새라는 클래스가 새로 생겨서 들어와도 좋아. 코드 수정이 필요가 없으니까.
someCircus.showFlying()


```

- https://velog.io/@heyksw/%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-24

***

> ### 💁🏻‍♂️ 8-2 : Delegate 패턴을 활용하는 경우를 예를 들어 설명해주세요.

1. **UITextViewDelegate** 의 textViewDidBeginEditing(). 텍스트 뷰의 편집을 감지하는 메서드

2. **UIScollViewDelegate** 의 viewDidScroll(). 스크롤 감지. offset 리턴

3. **UICollectionViewDelegate** 의 didSelectItemAt. 셀이 선택되었음을 감지.

```swift
// 예를 들어 이런식으로 사용된다.

class MyViewController: UIViewController, UITextViewDelegate {
    var myTextView = UITextView()
    let placeholder: String = "입력해주세요."
    
    override func viewDidLoad() {
        super.viewDidLoad()
        myTextView.delegate = self
    }
    
	// textview에 focus를 얻는 경우
    func textViewDidBeginEditing(_ textView: UITextView) {
        if myTextView.text == placeholder {
            myTextView.text = nil
            myTextView.textColor = .black
        }
    }
}

// 그리고 UITextView 클래스를 까보면 다음과 같이 delgate 변수가 있다.
@available(iOS 2.0, *)
open class UITextView : UIScrollView, UITextInput, UIContentSizeCategoryAdjusting {
...    
    weak open var delegate: UITextViewDelegate?
...

// 즉 myTextView 가 delegating 한 것이고(= Circus), 
// MyViewController 가 delegated 된 것이다(= Eagle)
// MyViewController 는 UITextViewDelegate 를 채택했기 때문에 위임받은 일을 할 수 있다.
// myTextView 에서 UITextViewDelegate 가 하는 일을 확장시키고 싶었고,
// 그 확장한 일을 MyViewController 에서 받아, 대신 처리한 것이다.
```

***

> ### 💁🏻‍♂️ 8-3 :  Delegate 패턴과 Escaping Closure 둘다 데이터를 전달할 수 있는데 각각 어떤 상황에 좋을까요?

1. Delegate 패턴은 프로토콜을 사용하기 때문에 좀더 엄격하게 행동을 규정할 수 있습니다.

2. Delegate 패턴은 중간에 쓰레드가 교체되지 않고 Escaping Closure 는 쓰레드가 교체됩니다. 따라서 쓰레드가 교체되지 않는 것을 바란다면 Delegate 패턴을 쓰는 것이 좋습니다. (컨설팅 때 들은 내용인데 구글링 해도 못찾겠음)

- https://shoveller.tistory.com/entry/Delegate-vs-Block-vs-Notification-vs-KVO

***

> ### 💁🏻‍♂️ 8-4 : Delegate Pattern과 Notification의 동작 방식의 차이에 대해 설명 해주세요.

1. **Delegate 패턴은 다른 객체의 인스턴스를 내부적으로 보유하여 그 인스턴스를 활용**하는 방식으로 동작하며, **Notification 은 객체를 Observing** 하는 방식으로 동작합니다.

2. **Notification 은 싱글턴**으로 되어있기 때문에 **화면간의 거리가 먼 경우에 유리**하고, **보다 다수의 객체들에게 이벤트를 전달하는 데에 유용**합니다.

3. 하지만 다른사람이 NotificationCenter 로 구현된 옵저버 패턴을 봤을 때, **정확히 어떤 객체들이 구독했는지 파악하기 까다롭습니다. **

4. Delegate 를 사용할 경우 다른 사람이 봤을 때 코드를 더 직관적으로 이해하기 좋습니다.

- https://neph3779.github.io/ios/DelegateVsNotification/

- https://velog.io/@hayeon/Delegates%EC%99%80-Notification-%EB%B0%A9%EC%8B%9D%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%97%90-%EB%8C%80%ED%95%B4-%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4#delegate-notification-%EB%B9%84%EA%B5%90

***

> ### 💁🏻‍♂️ 8-5 : NotificationCenter 동작 방식과 활용 방안에 대해 설명해주세요.

1. 이벤트가 발생했다는 것을 싱글턴 NotificationCenter 에 쏩니다.

2. NotiCenter 가 등록된 객체들에게 모두 이벤트를 보내줍니다.

***

> ### 💁🏻‍♂️ 8-6 : Singleton 패턴을 활용하는 경우를 예를 들어 설명해주세요.

1. 싱글턴은 앱 주기동안 오직 하나의 인스턴스를 생성하고 앱 전체 객체들이 알게하는 패턴입니다.

2. **뷰의 거리가 먼 곳 or 뎁스 차이가 많이 나는 곳에서 데이터 전달을 끌고 가야 하는 경우**, Delegate 패턴으로 계속해서 뷰 간에 전달하기는 까다롭기 때문에 싱글턴을 사용합니다.

2. **NotificationCenter** 에서 옵저버 패턴을 위해 싱글턴을 사용합니다.

***

## 9. KVC / KVO

> ### 💁🏻‍♂️ : KVC 와 KVO 에 대해 설명해주세요.

1. **KVC 는 Key Value Coding.** 객체의 값을 직접 사용하지 않고, Key Path를 이용해 간접적으로 사용하고 수정하는 방식입니다.

2. **KVO 는 Key Value Observing.** Key로 등록한 변수를 관찰하며 값이 변할때마다 어떠한 동작을 수행하는 것입니다. 프로퍼티 옵저버 willSet didSet 과 매우 유사하게 동작합니다.

> 💁🏻‍♂️ : 그럼 프로퍼티 옵저버와 KVO의 차이점은 무엇인가요?

- **프로퍼티 옵저버는 타입 내부에서 선언**하지만, **KVO 는 타입 정의 밖에서 observe 를 추가**합니다.

- willSet didSet 은 본인이 직접 타입을 만드는 경우에 구현해줄 수 있겠지만, **다른 사람 혹은 외부 라이브러리에서 정의한 타입이라면** 내부 소스를 마음대로 변경할 수 없습니다.

- 이럴때는 KVO 방식을 사용해서 옵저빙을 할 수 있습니다.


```swift
// KVC
struct A {
    var data = "Data"
}
 
var aInstance = A()
print(aInstance[keyPath: \.data]) // Data
print(aInstance.data) // Data
aInstance[keyPath: \.data] = "Data2"
print(aInstance[keyPath: \.data]) // Data2
 
let key = \A.data
print(aInstance[keyPath: key]) // Data2


// KVO
class Obj: NSObject {
    @objc dynamic var data = "Data"
}
 
let obj = Obj()
let observer = obj.observe(\.data, options: [.new, .old]) { _, changeInfo in
    print("\(changeInfo.oldValue) has been changed to \(changeInfo.newValue)")
}
 
obj.data = "Data2"
// Optional("Data") has been changed to Optional("Data2")
```

***

## 10. 스토리보드 vs 코드베이스

> ### 💁🏻‍♂️ : 스토리보드와 코드베이스의 차이를 설명해주세요

- 스토리보드를 사용하면 **눈에 보이는 직관적인 개발**을 할 수 있지만, 코드베이스로 UI를 개발했을 때보다 **코드 직관성이 떨어져서 협업에 불편함**을 느낄 수 있습니다.

***

## 11. 앱 상태

> ### 💁🏻‍♂️ 11-1 : 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가요?

1. iOS 앱은 Objective-C 기반으로 돌아가기 때문에 앱은 main 함수에서 시작됩니다.

2. main() 함수는 UIKit 내부에 숨겨져 있습니다. 그리고 main() 함수 내부에서 UIApplicationMain 함수를 실행합니다.

3. 그리고 **UIApplication** 이라는 객체를 생성합니다.

4. **UIApplication** 은 실행중인 앱을 제어하는 핵심 객체이고, 모든 앱은 단 하나의 UIApplication 인스턴스를 가집니다.

5. UIApplication.shared 로 접근할 수 있습니다.

6. UIApplication 인스턴스가 생성된 뒤에는 info.plist 파일을 읽어 파일에 기록된 정보를 참고해서 필요한 데이터를 로드합니다.

- https://hyun083.tistory.com/85
- https://blog.naver.com/PostView.naver?blogId=soojin_2604&logNo=222423840595&parentCategoryNo=&categoryNo=&viewDate=&isShowPopularPosts=false&from=postView

***
 
> ### 💁🏻‍♂️ 11-2 : @main에 대해서 설명하시오.

1. **@main 은 프로그램의 진입점**을 나타내고 주로 AppDelegate 에서 사용합니다.

2. UIKit 내부에는 프로그램 시작점인 main() 함수가 숨겨져 있는데, 이 @main 어노테이션을 사용함으로써 진입점을 나타냅니다.

3. Swift5.4 이전에서는 @main 대신에 **@UIApplicationMain** 키워드를 사용했는데, 이는 struct 에서는 사용하기 어려운 구조였기 때문에 class 와 struct 를 모두 커버 가능한 @main 을 사용하게 되었습니다.

- https://green1229.tistory.com/265

> ### 💁🏻‍♂️ 11-3 : App의 LifeCycle 에 대해 설명하시오.

- **Unattached** : 앱이 실행되지 않은 상태. 메모리에 올라오지 않은 상태.

- **Foreground** : 앱이 화면에 보여지는 상태로, CPU를 포함한 시스템 리소스를 우선적으로 사용합니다.

- 그리고 Foreground 상태는 InActive 상태와 Active 상태로 나눌 수 있습니다.

  - **Active** : 앱이 실행 중이고 이벤트를 받을 수 있는 상태
  
  - **InActive** : 앱이 실행 중이지만 전화나 알림에 의해 이벤트 입력을 멈추고 잠시 비활성화 된 상태

- **Background** : 앱이 화면을 점유하지 않지만 동작은 하고 있는 상태. 예를 들어 백그라운드에서 음악을 실행하는 상황

- **Suspend** : 백그라운드에서 활동을 멈춘 상태. 메모리에 올라는 가있습니다. 메모리가 부족한 상황이 생기면 Suspend 상태의 앱을 메모리에서 내리고 메모리 공간을 확보합니다.

- https://developer.apple.com/documentation/uikit/app_and_environment/managing_your_app_s_life_cycle

- https://co-dong.tistory.com/62

***

> ### 💁🏻‍♂️ 11-4 : 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?

- **Foreground** 상태는 앱이 실행되어 유저에게 보여지는 상태이고, 메모리 및 시스템 리소스를 효율적으로 사용해야 한다는 조건을 가집니다.

- **Background** 상태는 앱이 화면에 띄워져있지는 않지만 뒤에서 실행되고 있는 상태입니다. 제약사항으로는 사용자의 이벤트를 받을 수 없고, 메모리를 가능한 조금만 차지해야 한다는 특징이 있습니다.

***

> ### 💁🏻‍♂️ 11-5 : 상태 변화에 따라 다른 동작을 처리하기 위한 앱델리게이트 메서드들을 설명하시오.

- **didFinishLaunching** : 앱을 메모리에 올리고, 앱을 실행할 준비를 마쳤을 때 호출합니다.

- 그 외 라이프 사이클에 대한 메서드 들이 있습니다.

- **applicationDidBecomeActive** : Active 상태가 됐을 때 호출

- **applicationWillResignActive** : InActive 상태가 되려할 때 호출

- **applicationDidEnterBackground** : Background 상태가 됐을 때 호출

등등이 있습니다.

- https://developer.apple.com/documentation/uikit/uiapplicationdelegate

***

> ### 💁🏻‍♂️ 11-6 : UIWindow 에 대해 설명하시오.

1. **UIWindow 는 UIView 들을 담는 컨테이너**입니다. 

2. **UIWindow 의 코드를 열어보면, UIView 를 상속받고 있다**는 것을 알 수 있습니다. 

3. 이에 UIWindow 도 결국에는 일종의 뷰이지만, UIView 들을 액자처럼 담고 있는 컨테이너 뷰라고 생각 할 수 있습니다.

4. **iOS 12 까지는 window** 의 개념을 사용했지만 **iOS 13부터는 scene** 의 개념이 이를 대체하게 되었습니다.

- https://zeddios.tistory.com/283
- https://velog.io/@dev-lena/iOS-AppDelegate%EC%99%80-SceneDelegate

***

> ### 💁🏻‍♂️ 11-7 : AppDelegate와 SceneDelegate 에 대해 설명하시오.

- **iOS 12까지는 대부분 하나의 앱이 하나의 window** 를 가졌지만, **iOS 13 부터는 window의 개념이 scene으로 대체**됐고, **하나의 앱에서 여러 개의 scene**을 가질 수 있게 되었습니다.

1. 예전에는 **AppDelegate 가 Process LifeCycle 과 UI LifeCycle 을 모두 담당**했었지만, **SceneDelegate 가 등장하면서 UI LifeCycle 의 책임**을 옮겨 받게 되었습니다.

2. 그리고 AppDelegate 는 모든 scene 의 정보를 관리하는 **Scene Session** 을 관리하게 되었습니다. (scene configuration)

3. (여기 설명이 좋음) https://velog.io/@dev-lena/iOS-AppDelegate%EC%99%80-SceneDelegate
 
***

> ### 💁🏻‍♂️ 11-8 :  App Bundle의 구조와 역할에 대해 설명하시오.

1. **앱 번들은 앱의 성공적인 빌드를 위한 것들을 저장**합니다.

2. Info.plist , 실행파일 , 소스파일 등을 저장합니다.

3. Info plist 안에는 앱 버전, 번들 id, 권한 요청 메시지 등 여러가지 앱 메타 데이터를 저장합니다.
 
 ***
 
> ### 💁🏻‍♂️ 11-9 :  다크모드를 지원하는 방법에 대해 설명하시오.
 
1. **Xcode 의 Color Assets** 에서 Color Set 로 **다크 모드인 경우 색상과 라이트 모드인 경우 색상을 함께 등록**합니다.

2. 다크모드를 아예 사용하고 싶지 않다면 info plist 에서 설정해줄 수 있습니다.

- https://gyuios.tistory.com/120

***
## 12. Cocoa Touch

> ### 💁🏻‍♂️ : Cocoa Framework와 Cocoa Touch Framework의 차이를 설명해주세요.

- Cocoa Framework 는 macOS 의 애플리케이션 개발에 사용되고, Cocoa Touch Framework 는 iOS 의 앱개발에 사용됩니다.

- 아이폰, 아이패드는 터치 기반의 기기이기 때문에 코코아 터치입니다.

- Cocoa Touch 에는 Foundation 과 UIKit 프레임워크가 포함됩니다.

- https://velog.io/@heyksw/iOS-iOS-%EC%95%A0%ED%94%8C%EB%A6%AC%EC%BC%80%EC%9D%B4%EC%85%98-%EA%B3%84%EC%B8%B5%EA%B5%AC%EC%A1%B0


***
## 13. GCD

> ### 💁🏻‍♂️ 13-1: NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.

1. **iOS 에서 멀티스레딩을 할 수 있는 방법은 크게 Operation 과 GCD** 가 있습니다.

2. GCD (Grand Central Dispatch) 는 C 기반, Operation 은 Objective-C 기반으로, **GCD 가 상대적으로 더 가볍습니다.**

3. NSOperationQueue 에서는 KVO 가 사용가능하지만 GCD 에서는 사용불가 합니다.

- https://thoonk.tistory.com/30

***

> ### 💁🏻‍♂️ 13-2 : GCD API 동작 방식과 필요성에 대해 설명하시오.

1. **GCD 란 iOS 에서 멀티 스레딩**을 하기 위해 지원하는 API 입니다. 개발자는 해야할 작업들을 GCD 의 큐에 넘겨주기만 하면 OS 레벨에서 비동기 처리를 도와줍니다.

2. GCD 에서는 여러가지 형식의 큐를 지원합니다.

  - **Serial Queue** : 분산시킨 작업을 단 한개의 스레드에서 처리하는 큐. 주어진 시간에 하나의 작업만 순차적으로 수행.
  
  - **Concurrent Queue** : 분산시킨 작업을 여러개의 스레드에서 처리하는 큐. 주어진 시간에 여러 작업 수행 가능.
  
  - **Sync Queue** : 큐에 추가된 작업이 종료될때까지 기다리는 큐.
  
  - **Async Queue** : 큐에 추가된 작업을 종료될때까지 기다리지 않는 큐.

  - **Main Queue** : 메인 스레드 Serial 큐. UI 작업을 담당합니다.
  
  - **Global Queue** : 전체 시스템에 의해 공유되는 Concurrent Queue. UI가 아닌 작업을 수행합니다.
  
  - **Custom Queue** : 개발자가 직접 생성하는 큐. (Serial / Concurrent)
  
  - Sync vs Async : 작업을 보내는 시점에서 기다릴지 말지 기다리는 것
  
  - Serial vs Concurrent : 큐로 보내진 작업들을 한개의 스레드로 순차처리 할 것인지, 여러 스레드에 보내 순서상관없이 처리할 것인지.

- https://furang-note.tistory.com/37
- https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-4-a621eca0a1d2

  ***

> ### 💁🏻‍♂️ 13-3 : Dispatch Queue의 Serial Queue에 대해서 설명해보세요.

- Concurrent Queue 와 다르게, 동시에 여러개의 작업을 수행하지 않고, 순차적으로 하나의 작업만 수행하는 큐입니다. iOS 의 Main Queue 가 Serial Queue 입니다.

***
 
> ### 💁🏻‍♂️ 13-4 :  DispatchQueue.main.async 와 DispatchQueue.main.sync 의 차이

- 메인 큐는 시리얼 큐이고, UI 작업 흐름이 계속해서 수행되어야 하기 때문에 async 를 통해서 다른 작업을 추가하는 것이 좋습니다.

- **메인 큐에서 sync 를 사용하면 데드락이 발생**하는데 그 이유는 다음과 같습니다.

1. 메인 쓰레드에서 일을하다가 sync 블럭 "X"를 만나서 메인 시리얼 큐에 X 를 보냅니다. 그리고 sync 이기 때문에 이 "X" 가 끝나기만을 바라보는 상태로 들어갑니다.

2. 이제 디스패치 큐는 이 X를 처리해줄 스레드를 찾는 것이 책임인데, 어차피 메인 큐는 메인 쓰레드에게 일을 시켜야 합니다.

3. 메인 쓰레드는 아까 X 작업을 큐에 보내주면서 "나는 널 기다릴래" 라고 했는데 메인 큐가 다시 일을 시키려고 하는 상황입니다.

4. 그래서 서로가 원하는 작업을 수행하지 못해 데드락이 발생합니다.

- https://limjs-dev.tistory.com/106

***

> ### 💁🏻‍♂️ 13-5 : Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.

- **Qos 는 디스패치 큐의 우선순위를 부여하는 것**입니다. 

- userInteractive > userInitiated > default > utility > background 순으로 우선순위가 높습니다.

- (솔직히 5개의 우선순위가 있다는 것 정도만 알아두면 될듯)

- https://jcsoohwancho.github.io/2019-10-09-DispatchQueue%EC%9D%98Qos/

***
> ### 💁🏻‍♂️ 13-6 : DispatchGroup에 대해서 설명해보세요.

- **여러개로 분배된 작업들을 하나로 그룹지어서 한번에 파악**하고 싶을 때 Dispatch Group 을 사용합니다.

- 즉 그룹안에 있는 작업들 중 마지막으로 끝난 작업의 시점에 초점을 둡니다.

![](https://velog.velcdn.com/images/heyksw/post/4afc91b2-a786-498a-8f3f-c1dadd7cc2ba/image.png)



```swift
let group = DispatchGroup()
queue1.async(group: group) {
    for i in 0...5 {
        print(i)
    }
}
queue2.async(group: group) {
    for i in 100...105 {
        print(i)
    }
}
let queueForGroup = DispatchQueue(label: "q", attributes: .concurrent)
group.notify(queue: queueForGroup) {
    print("group end")
}
// 0~5, 100~105 모두 출력후 group end 출력
```

- https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-7-4d9dbe901835

- https://ios-development.tistory.com/263

***

> ### 💁🏻‍♂️ 13-7 : DispatchSemaphore를 사용하는 상황을 설명해주세요.

-  운영체제의 semaphore와 같습니다.

- 공유 자원에 동시 접근 가능한 작업 수를 명시하고, 임계 구역에 들어갈때는 semaphore의 wait()를, 작업을 수행하고 나올때는 signal() 을 호출합니다.

- signal 은 작업을 수행하고 세마포어 값을 반환하기 때문에 +1

- wait 은 열쇠를 획득하고 작업을 수행하러 가야하기 때문에 -1 입니다.

```swift
// semaphore 동시 작업 개수 1
let semaphore = DispatchSemaphore(value: 1)

DispatchQueue.global(qos: .background).async {
    print("task A start")
    print("task A end")
    semaphore.signal()
}

semaphore.wait()

DispatchQueue.global(qos: .background).async {
    print("task B start")
    print("task B end")
    semaphore.signal()
}

semaphore.wait()

DispatchQueue.global(qos: .background).async {
    print("task C start")
    print("task C end")
    semaphore.signal()
}

/* 
- 동시 접근 제한 1개인 경우
task A start
task A end
task B start
task B end
task C start
task C end

- 동시 접근 제한 2개 이상인 경우
task A start
task A end
task C start
task C end
task B start
task B end
순서가 보장되지 않고 동시 접근한다.

/*

```

- https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-10-cb37c3e0cf13

> ### 💁🏻‍♂️ 13-8 : GCD의 Barrier에 대해 설명해주세요.

- Concurrent 큐가 사용하고 있는 여러개의 쓰레드 중에서, 하나의 쓰레드만 제외하고 모든 쓰레드의 사용을 막는 것입니다.

- 해당 시점에서는 쓰레드를 오직 하나만 사용하기 때문에 serial 큐처럼 사용이 가능합니다.

- Concurrent 큐에 read 작업들은 일반적인 task 로, write 작업을 barrier task 로 넣는식으로 응용할 수 있습니다.

- read 는 여러개의 쓰레드가 동시접근해도 상관이 없지만, write 는 그렇지 않기 때문입니다.

![](https://velog.velcdn.com/images/heyksw/post/3f3a1dfc-f747-4136-96fa-d7d2e82946d6/image.png)


- https://sujinnaljin.medium.com/ios-%EC%B0%A8%EA%B7%BC%EC%B0%A8%EA%B7%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EB%8A%94-gcd-10-cb37c3e0cf13

***

## 14. 함수형 프로그래밍

 순수함수란 무엇인지 설명하시오.
 함수형 프로그래밍이 무엇인지 설명하시오.
 고차 함수가 무엇인지 설명하시오.
 1급 객체(혹은 1급 시민)에 대해서 설명해보세요. Swift에는 어떤 1급 객체들이 있나요?

***
## 15. 캐싱

 Kingfisher 설명, 장점
 라이브러리를 안쓰고 캐시 직접 구현 어떻게 할까요
- 같은 url 이지만 이미지가 바뀔 경우는요?
NSCache 동작 방법. 어디에 저장되나요?

***


## 16. 추가 개념

의존성 주입에 대하여 설명하시오.
모듈화란 무엇이고 왜 하나요
 MVC 구조에 대해 블록 그림을 그리고, 각 역할과 흐름을 설명하시오.
 init?()과 init()은 어떤 차이가 있나요?
 웹 서버와 HTTP 연결을 사용해서 데이터를 주거나 받으려면 사용해야 하는 클래스와 동작을 설명하시오.
 Core Data와 Sqlite 같은 데이터 베이스의 차이점을 설명하시오
 shallow copy와 deep copy의 차이점을 설명하시오.
 Synchronous 방식과 Asynchronous 방식으로 URL Connection을 처리할 경우의 장단점을 비교하시오.
 Run Loops에 대해 설명하시오.
 오토레이아웃을 코드로 작성하는 방법은 무엇인가? (3가지)
 멀티 쓰레드로 동작하는 앱을 작성하고 싶을 때 고려할 수 있는 방식들을 설명하시오.


