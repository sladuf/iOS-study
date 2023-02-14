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


https://velog.io/@eddy_song/ios-mvc


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

https://ykss.netlify.app/web/design_pattern/

***

## 5. 디자인 패턴 정의

> ### 💁🏻‍♂️ : 디자인 패턴이란 무엇이고, 왜 사용하나요?

- 디자인 패턴이란 일반적인 개발 과정에서 자주 발생하는 문제를 해결하기 위한 개발자들의 교과서입니다.

- 사용 이유

1. 이미 증명된 솔루션이기 때문에 개인적으로 생각해낸 해결방안보다 사용할 근거가 충분합니다.

2. 약속되어있기 때문에 아예 다른 사람이 처음 코드를 보게 되더라도, 디자인 패턴에 대한 이해만 있다면 수월하게 코드를 이해할 수 있습니다.

https://velog.io/@k7120792/Model-View-ViewModel-Pattern#%EC%9E%A5%EC%A0%90

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

1. weak 과 unowned 는 모두 reference count 를 증가시키지 않습니다.

2. ...

> ### 💁🏻‍♂️ 6-6 : 메모리를 힙과 스택으로 나누는 이유는 무엇일까요?

https://babbab2.tistory.com/27

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

https://sujinnaljin.medium.com/ios-arc-%EB%BF%8C%EC%8B%9C%EA%B8%B0-9b3e5dc23814
https://babbab2.tistory.com/26

***

## 8. Delegate 패턴

 Delegate 패턴 설명해주세요
 Delegate 패턴과 Escaping Closure 둘다 데이터를 전달할 수 있는데 각각 어떤 상황에 좋을까요?
 Delegate Pattern과 Notification의 동작 방식의 차이에 대해 설명하세요.
 Delegate 패턴을 활용하는 경우를 예를 들어 설명하시오.
 Delegate란 무엇인지 설명하고, retain 되는지 안되는지 그 이유를 함께 설명하시오.
 NotificationCenter 동작 방식과 활용 방안에 대해 설명하시오.
 Singleton 패턴을 활용하는 경우를 예를 들어 설명하시오.

***

## 9. 스토리보드 vs 코드베이스

> ### 스토리보드와 코드베이스의 차이를 설명해주세요
***

## 10. 앱 상태

 앱이 시작할 때 main.c 에 있는 UIApplicationMain 함수에 의해서 생성되는 객체는 무엇인가?
 @Main에 대해서 설명하시오.
 앱이 foreground에 있을 때와 background에 있을 때 어떤 제약사항이 있나요?
 상태 변화에 따라 다른 동작을 처리하기 위한 앱델리게이트 메서드들을 설명하시오.
 App delegate에 대해 설명하시오.
 scene delegate에 대해 설명하시오.
 App의 Not running, Inactive, Active, Background, Suspended에 대해 설명하시오.
 App Bundle의 구조와 역할에 대해 설명하시오.
 다크모드를 지원하는 방법에 대해 설명하시오.

***
## 11. Cocoa Touch

 Cocoa Framework와 Cocoa Touch Framework의 차이

***
## 12. GCD
 NSOperationQueue 와 GCD Queue 의 차이점을 설명하시오.
 GCD API 동작 방식과 필요성에 대해 설명하시오.
 Global DispatchQueue 의 Qos 에는 어떤 종류가 있는지, 각각 어떤 의미인지 설명하시오.
 GCD의 QoS에 대해서 설명해보세요.
 GCD의 Barrier에 대해 설명해주세요.

***
## 13. Dispatch Queue

 Dispatch Queue의 Serial Queue에 대해서 설명해보세요.
 DispatchQueue.main.async 와 DispatchQueue.main.sync 의 차이
 DispatchGroup에 대해서 설명해보세요. 
 DispatchWorkItem의 장점이 무엇인가요?
 OperationQueue에 대해서 설명해보세요. DispatchQueue와는 어떤 것이 다른가요?
 DispatchSemaphore를 사용하는 상황을 설명해주세요.

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
## 16. KVC / KVO

 KVC에 대해서 설명해보세요.
 KVO에 대해서 설명해보세요.

***

## 17. 추가 개념

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



### ref
https://github.com/JeaSungLEE/iOSInterviewquestions
https://caution-dev.github.io/swift/2019/03/16/iOS-Q&A.html
https://github.com/jeonyeohun/Getting-Ready-For-Interview/blob/main/iOS-Swift/swift.md
