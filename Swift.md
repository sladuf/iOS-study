# 🍎 Swift 언어

## 1. Class / Struct
> ### 💁🏻‍♂️ 1-1 : Class 와 Struct의 차이를 설명해주세요

1. struct는 **value type**이고 class는 **reference type**입니다
2. 그래서 class는 ARC를 통해 메모리를 관리합니다
3. class는 상속이 가능하지만 struct는 상속할 수 없습니다

- 공통점
1. 프로퍼티와 메서드를 정의할 수 있습니다
2. 이니셜라이저를 정의하여 초기화될 때의 상태를 지정할 수 있습니다
3. extension을 통해 확장할 수 있습니다
4. 프로토콜을 준수할 수 있습니다

```
값타입과 참조타입의 가장 큰 차이는 '무엇이 전달되느냐' 입니다.
(💡 전달 : 함수의 파라미터나 새로운 변수에 할당하는 것)
값타입을 전달할 때에는 메모리에 전달인자를 위한 인스턴스가 새로 생성됩니다. 즉, 전달되는 값이 복사 됩니다.
참조타입을 전달할 때에는 인스턴스의 참조(주소)가 전달됩니다.
```

> **💁🏻‍♂️💁🏻‍♂️ 언제 class를 사용하고, 언제 struct를 사용하나요?**

Apple 가이드라인에서 다음 조건 중 하나 이상에 해당한다면 **구조체를 사용하는 것을 권장**합니다
- 연관된 간단한 값의 집합을 캡슐화하는 목적일 때
- 프로퍼티가 값 타입이고 참조보다 복사하는 것이 합당할 때
- 상속할 필요가 없을 때

즉, 상속 관계를 필요로 하거나 객체의 값이 계속 가공되는경우에는 class가 더 유리합니다

> ### 💁🏻‍♂️ 1-2 : Swift의 복사 방법에 대해서 설명해 주세요

1. swift는 **COW**방식을 사용해서 값타입을 복사합니다
2. COW는 원본과 복사본 중 수정되기 전까지는 복사를 하지 않고 **원본 리소스를 공유**하다가, 둘 중 하나에서 수정이 일어나는 경우 복사하는 작업을 합니다
3. 그래서 COW를 사용하는 타입에서 복사 후 첫번째 수정이 일어나는 경우에는 COW에 의해 약간의 **오버헤드**를 발생시킵니다
4. 모든 value type에서 사용되는 것이 아니라 **collection type**을 복사할 때 사용됩니다.

> **💁🏻‍♂️💁🏻‍♂️ 오버헤드가 발생하는데 COW를 왜 사용하는걸까요?**

swift의 COW는 시간과 공간 중에 공간의 효율을 선택했다고 볼 수 있습니다.

실제로 값의 복사만 일어나고 수정이 일어나지 않는 경우도 있기 때문에 불필요한 복사를 줄여 메모리를 절약할 수 있습니다.

```
📌 리소스 공유
COW의 리소스 공유와 class의 참조는 조금 다른 경향을 가집니다
class 인스턴스를 참조할 때에는 서로 다른 메모리에 '같은 인스턴스의 주소값'을 참조하고 있습니다
COW의 리소스 공유는 동일한 메모리를 가르키고 있습니다
예를들어 b가 a를 복사한 경우에는 수정이 일어나기 전까지는 b는 메모리를 할당받지 않고 a와 같은 메모리를 사용합니다
다시 말하면,
class의 a와 b는 서로 다른 메모리에 할당 되지만 그 메모리에는 같은 '주소값'이 담겨 있습니다
collection type의 a와 b는 서로 같은 메모리를 사용하고 있고 그 메모리에 있는 값은 '데이터'입니다
그리고 데이터가 변경되면 b는 메모리를 할당 받고 같은 데이터를 복사한 후에 데이터가 변경됩니다
(참고 : https://nsios.tistory.com/56)

📌 collection type
Array, Set, Dictionary
```
- [https://babbab2.tistory.com/18](https://babbab2.tistory.com/18)

- [https://forestjae.tistory.com/30](https://forestjae.tistory.com/30)

> ### 💁🏻‍♂️ 1-3 : class의 성능을 향상 시킬 수 있는 방법을 설명해주세요

- **final**

1. 상속되지 않는 클래스에 **final**을 붙이면 메소드가 Static Dispatch를 사용하기 때문에 오버헤드가 줄어 성능이 향상 됩니다
2. 클래스는 상속이 가능하기 때문에 메소드가 오버라이딩 될 가능성이 있어 Dynamic Dispatch를 사용합니다
3. 하지만 final 키워드를 붙이면 클래스의 상속이 불가능하여 메소드의 오버라이딩도 불가능해져 Static Dispatch를 사용합니다
4. **Dynamic Dispatch**는 런타임시 실행할 메소드를 결정하고, **Static Dispatch**는 컴파일 타임에 결정하기 때문에 오버헤드를 줄일 수 있습니다


```
📌 Dynamic Dispatch
- class는 기본적으로 상속이 가능하기 때문에 상위 클래스와 하위 클래스 중 어디를 참조해야할지를 런타임 시점에 결정합니다
- 런타임 과정에서 vTable에서 함수를 찾아 메모리 주소를 '읽고' 그 주소로 '점프' 해야 하기 때문에 성능상 손해가 발생합니다
📌 vTable
- 정의 : 디스패치를 지원하기 위해 프로그래밍 언어에서 사용하는 메커니즘
- 클래스 마다 가지고 있는 가상테이블을 말하며 메서드들의 포인터 값을 가지고 있습니다
- 하위 클래스는 상위 클래스의 vTable 복사본을 가지고, 오버라이딩하는 경우에는 오버라이딩 한 메서드를 가리키는 함수 포인터로 저장됩니다
```


> **💁🏻‍♂️💁🏻‍♂️ Dispatch가 뭔데요?**

Dispatch는 어떤 메서드를 호출할 것인지를 결정하여, 그것을 실행하는 메커니즘입니다. Static, Dynamic 두 가지 방식이 있습니다

Static Dispatch는 호출할 함수를 컴파일 타임에 결정합니다. Dynamic Dispatch는 호출할 함수를 런타임에 결정합니다

기본적으로 값타입은 Static Dispatch를 사용하며, 참조타입에서 어떤 Dispatch를 선택하는지는 오버라이딩 가능성에 따라 결정됩니다

그래서 final 키워드를 붙이면 Static Dispatch를 사용하게 되는것 입니다



- **WMO**

1. Whole Module Optimization을 사용하면 성능향상을 기대할 수 있습니다
2. WMO는 모듈 전체를 하나의 덩어리로 컴파일하여 internal level에 대해서 오버라이딩 되지 않는 경우 내부적으로 final을 붙여서 컴파일 합니다
3. 이는 개발자가 발견하지 못한 상속되지 않는 클래스에 대해 Dynamic Dispatch의 사용을 줄여 성능 향상을 기대할 수 있습니다


```
🤔 WMO안하면 왜 자동으로 final을 못붙여주는걸까?
- 기본적으로 swift는 파일을 하나씩 컴파일 하기 때문에 서로 다른 파일에서 클래스를 상속했는지 컴파일 타임에 알 수 없어서 final을 붙일 수 없습니다
```

- [https://babbab2.tistory.com/145?category=828998](https://babbab2.tistory.com/145?category=828998)

> ### 💁🏻‍♂️ 1-4 : swift의 타입캐스팅에 대해서 설명해주세요

1. swift의 타입캐스팅은 타입을 바꾸는 것이 아닌 **사용 범위를 전환**하여 다른 타입처럼 행세할 수 있도록 합니다
2. 업캐스팅은 하위클래스가 상위클래스 타입으로 전환하는 것이고, 다운캐스팅은 상위클래스가 하위클래스 타입으로 전환하는 것입니다
3. 업캐스팅을 하는 경우에는 무조건 전환이 가능하므로 as를 사용합니다
4. 다운캐스팅을 하는 경우에는 실패 가능성이 있으므로 **as?** 또는 **as!** 를 사용합니다. 둘 다 런타임에 캐스팅합니다
5. as?는 옵셔널이 반환됩니다. as!는 옵셔널이 강제 추출되므로 다운캐스팅이 불가능한 경우에는 런타임 에러가 발생할 수 있습니다

> **💁🏻‍♂️💁🏻‍♂️ 캐스팅하면 타입이 바뀌지 않나요?**

1. 예를들어 class Person {}, class Student: Person {}를 살펴보겠습니다
2. let student = Student() as Persen을 하는 경우에는 업캐스팅되어 Student에서 새로 만든 프로퍼티와 메서드를 사용하는 것이 불가능합니다
3. 하지만 type(of: student)는 여전히 Student입니다. 즉, type이 바뀌는 것이 아니라 범위를 전환하는 것입니다




## 2. 접근제어자


> ### 💁🏻‍♂️ 2-1 : 접근 제어자의 종류엔 어떤게 있는지 설명해주세요

1. 접근제어는 접근수준 키워드를 통해 구현할 수 있습니다. 접근수준 키워드는 open, public, internal, fileprivate, private 다섯 가지가 있습니다.
2. 키워드 중에서 open은 클래스와 클래스의 멤버에서만 사용할 수 있습니다.
3. 접근 수준에는 규칙이 있는데 상위요소보다 하위요소가 더 높은 접근 수준을 가질 수 없습니다.


> ### 💁🏻‍♂️ 2-2 : open과 public 키워드의 차이를 설명해보세요

1. open과 public은 외부 모듈까지 접근을 허용한다는 공통점이 있습니다
2. open은 모듈 외부에서 상속하거나 오버라이딩 할 수 있습니다. 단, 클래스와 클래스 멤버에서만 사용할 수 있습니다
3. public은 모듈 외부에서 상속과 오버라이딩이 불가능합니다. 같은 모듈 내에서는 상속과 오버라이딩을 할 수 있습니다

> ### 💁🏻‍♂️ 2-3 : fileprivate과 private 키워드의 차이를 설명해보세요

fileprivate은 그 요소가 구현된 소스파일 내부에서 사용할 수 있지만, private은 기능을 정의하고 구현한 범위 내에서만 사용할 수 있습니다.

> **💁🏻‍♂️💁🏻‍♂️ class나 struct를 private으로 만들면 어떻게 될까요?**

아래의 코드는 정상 동작합니다.
```swift
private class A {
    func someFunction(){
        print("This is function")
    }
}

private let a = A()
a.someFunction()
```
추측하건데 구현체 자체에 private을 붙이는 경우에는 구현체의 상위인 '파일'을 구현체로 인식하는 것 같습니다.

그래서 동일한 파일 내에서는 A 클래스를 사용할 수 있습니다.

즉, fileprivate 처럼 동작합니다.

그 근거로 상위 접근 수준인 fileprivate 클래스내에 private 클래스의 인스턴스를 만들 수 있습니다.

```swift
fileprivate let a = A()
a.someFunction()
```

## 3. 이니셜라이저
> ### 💁🏻‍♂️ 3-1 : Convenience init에 대해 설명해주세요

1. 편의 이니셜라이저는 초기화를 좀 더 쉽게 할 수 있도록 도와주는 역할을 합니다
2. 편의 이니셜라이저는 꼭!!! 지정 이니셜라이저를 호출해야합니다 -> self.init()

> **💁🏻‍♂️💁🏻‍♂️ 정말 편의이니셜라이저는 꼭!!! 지정 이니셜라이저를 호출해야할까요?**


편의 이니셜라이저는 여러개 존재할 수 있습니다. 편의 init이 다른 편의 init을 호출할 수도 있습니다

하지만 최종적으로 호출되는 편의 이니셜라이저는 꼭! 지정 이니셜라이저를 호출해야 합니다

그렇기 때문에 결론적으로 편의 이니셜라이저를 호출하면 마지막에 지정 이니셜라이저를 호출하게 됩니다

> **💁🏻‍♂️💁🏻‍♂️ 편의 이니셜라이저 꼭 써야 하나요? **

아니요! 편의 이니셜라이저는 옵션입니다

차이점이 있다면 지정 이니셜라이저는 self.init을 호출할 수 없다는 것입니다

어떤 클래스에 다음과 같은 저장 프로퍼티와 지정 이니셜라이저가 있을때

```swift
var a : Int
var b : Int
var c : Int

init(a: Int, b: Int, c: Int) {
    self.a = a
    self.b = b
    self.c = c
}
```

아래의 두 이니셜라이저는 똑같이 동작합니다. 그렇기 때문에 아래의 두 이니셜라이저는 동시에 선언할 수 없습니다

```swift
init(a: Int) {
    self.a = a
    self.b = 0
    self.c = 0
}

convenience init (a: Int){
    self.init(a: a, b: 0, c: 0)
}
```

- https://zeddios.tistory.com/141

> ### 💁🏻‍♂️ 3-2 : required init에 대해서 설명해 주세요

1. required init은 상속받은 모든 하위 클래스들이 재정의 해야하는 이니셜라이저 입니다
3. 하위 클래스가 required init을 구현해야하는 강제성이 있으므로 override 키워드를 붙이지 않습니다
4. 하위 클래스에서 아무런 이니셜라이저도 구현하지 않는 경우에는 상위 클래스의 이니셜라이저가 자동상속 되므로 required init을 구현하지 않아도 됩니다.
5. 단, 하위클래스에서 어떠한 종류의 이니셜라이저라도 구현하는 경우에는 자동상속 되지 않으므로 required init을 함께 구현해야 합니다


```
⭐️ convenience init, required init은 **class 이니셜라이저** 입니다!
```

> ### 💁🏻‍♂️ 3-3 : class의 초기화 진행 과정에 대해서 설명해 주세요

swift의 클래스 초기화는 2단계를 거칩니다

- **1단계 : 클래스에 정의한 저장 프로퍼티에 초깃값이 할당됩니다**

- **2단계 : 모든 저장 프로퍼티의 초기 상태가 결정되면 사용자 정의할 기회를 가집니다**

초기화 단계는 프로퍼티를 초기화하기전에 프로퍼티 값에 접근하는 것을 막아 초기화를 안전하게 할 수 있도록 합니다

컴파일러는 2단계 초기화를 오류 없이 처리하기 위해 **네 가지 safety-checks**를 합니다

1. 자식클래스의 init이 부모클래스의 init을 호출하기 전에 자신의 프로퍼티를 모두 초기화 했는지 확인합니다

```swift
class Perents {
    var home : String
    var firstName : String
    
    init(home: String, firstName: String) {
        self.home = home
        self.firstName = firstName
    }

}

class Child : Perents {
    var lastName : String
    var age : Int
    
❌
    init(lastName: String, age: Int) {
        super.init(home: "한강뷰", firstName: "김")
        self.lastName = lastName
        self.age = age
    }

⭕️
    init(lastName: String, age: Int) {
        self.lastName = lastName
        self.age = age
        super.init(home: "한강뷰", firstName: "김")
    }
}
```

2. 자식클래스의 init은 상속받은 프로퍼티에 값을 할당하기 전에 반드시 부모클래스의 init을 호출해야합니다

```swift
init(lastName: String, age: Int) {
        self.lastName = lastName
        self.age = age

        self.home = "강남" ❌
        super.init(home: "한강뷰", firstName: "김")
        self.home = "강남" ⭕️
    }
```

3. 편의 이니셜라이저는 그 어떤 프로퍼티라도 값을 할당하기 전에 다른 init을 호출해야 합니다

```swift
convenience init(age: Int){
    self.age = 20 ❌
    self.home = "한강뷰" ❌
    self.init(lastName: "홍길동", age: age)
    self.age = 20 ⭕️
    self.home = "한강뷰" ⭕️
}
```

4. 초기화 1단계를 마치기 전까지 이니셜라이저는 메서드를 호출하거나 프로퍼티 값을 읽을 수 없습니다.
    
    ```swift
    init(lastName: String, age: Int) {
        sayHello() ❌ //프로퍼티에 값 할당 전이므로 1단계 전임
        self.lastName = lastName
        self.age = age
        super.init(home: "한강뷰", firstName: "김")
        sayHello() ⭕️ //나와 상위에 있는 모든 프로퍼티에 값을 할당 했으므로 1단계 마침
    }
    
    func sayHello(){
        print("hello")
    }
    ```


> ### 💁🏻‍♂️ 3-4 : deinit은 언제 사용할까요?
1. 클래스 인스턴스가 메모리에서 해제될 때 클래스 인스턴스와 관련하여 정리하는 작업이 필요한 경우에 사용합니다
2. deinit 키워드를 사용하여 구현하면 메모리에서 해제되는 시점에 자동으로 호출됩니다
3. 인스턴스가 해제될 때 가지고 있던 데이터를 어딘가에 보내주거나 저장해야 하는 경우, 인스턴스의 메모리 해제를 확인하고 싶은 경우에 사용할 수 있습니다

## 4. Extension
> ### 💁🏻‍♂️ 4-1 : Extension에 대해 설명하시오.
1. extension은 클래스, 구조체, 열거형, 프로토콜 타입에 새로운 기능을 추가할 수 있습니다
2. 기존에 있는 프로퍼티와 메서드를 **재정의 할 수 없습니다**
3. 프로퍼티는 저장 프로퍼티를 제외한 **연산 프로퍼티만** 정의할 수 있습니다. 메서드는 자유롭게 정의할 수 있습니다
4. deinit은 정의할 수 없고 init은 class의 경우에는 편의 이니셜라이저만 추가할 수 있습니다. **값타입은 경우에는 따라 지정 이니셜라이저를 추가할 수 있습니다.**

```
📌 값타입에서 extension을 통해 지정 이니셜라이저를 추가할 수 있는 경우
- 모든 저장 프로퍼티에 기본값이 있어야 합니다
- 타입내에 기본 이니셜라이저와 멤버와이즈 이니셜라이저 외에 사용자 정의 이니셜라이저가 없어야 합니다
```

## 5, 데이터타입
> ### 💁🏻‍♂️ 5-1 : Optional에 대해서 설명해주세요


> **💁🏻‍♂️💁🏻‍♂️ Optional은 내부적으로 어떻게 구현되어 있나요? **

> ###  💁🏻‍♂️ 5-2 : enum에 대해서 설명해주세요


> ### 💁🏻‍♂️ 5-3 : let과 var의 차이를 설명해주세요
- https://github.com/jeonyeohun/Getting-Ready-For-Interview/blob/main/iOS-Swift/swift.md

> ### 💁🏻‍♂️ 5-4 : Swift에서 참조 타입은 어떤게 있나요?


> ### Any와 AnyObject에 대해서 설명해주세요
> ### Any와 AnyObject의 차이






## 프로토콜
> ### 프로토콜이란 무엇인지 설명하시오.
> ### Codable에 대하여 설명하시오.
> ### Hashable 프로토콜에 대해서 설명해보세요. 
> ### Hashable 프로토콜을 채택하는 커스텀 타입이 Equtable도 채택해야하는 이유가 무엇인가요?
> ### Hashable이 무엇이고, Equatable을 왜 상속해야 하는지 설명하시오.
> ### 열거형도 Hashable을 채택했을 때 자동으로 Hashable하게 만들 수 있나요?
> ### Codable과 NSCoding의 차이는?
> ### associatedType이 무엇인지 설명해주세요.

## 클로저
> ### Closure에 대하여 설명하시오.
> ### Closure 의 값 캡처에 대해 설명하시오.
> ### Closure와 함수와의 관계에 대해 설명하시오.
> ### Function과 Closure의 차이
> ### Trailing Closure에 대해서 설명해보세요. 
> ### Closure 와 Escpaing Closure 에 대한 설명해주세요



## 컬렉션타입
> ### Array보다 Set을 사용하는게 더 좋을 때는 언제일까요?
> ### Array, Set, Dictionary의 차이점이 뭘까요? 





## 키워드
> ### mutating 키워드에 대해 설명하시오.
> ### lazy 에 대해 설명하시오.
> ### some 키워드에 대해 설명하시오.
> ### typealias 가 무엇인지 말해주세요.
> ### required 키워드에 대해서 설명해보세요. 




## 메모리관리
> ### weak과 unowned 의 차이를 설명하고 예를 들어주세요.
> ### unowned는 언제 사용하는 것이 안전할까요?
> ### weak만 사용하지 않고 unowned도 사용하는 이유가 무엇을까요?
> ### COW를 직접 구현한다면 어떻게 구현할 것인지?
> ### strong, weak, unowned reference는 각각 언제 사용할까요? 

## enum
> ### Enum에서 raw value와 associated value에 대해 설명해보세요.

## 패러다임
> ### Swift Standard Library의 map, filter, reduce, compactMap, flatMap에 대하여 설명하시오.
> ### 고차함수 중 flatMap과 compactMap의 차이를 설명해보세요.
> ### Protocol Oriented Programming과 Object Oriented Programming의 차이점을 설명하시오.

## function / method
> ### instance 메서드와 class 메서드의 차이점을 설명하시오.
> ### class 메서드와 static 메서드의 차이점을 설명하시오.
> ### function과 method의 차이를 말해보세요. 
> ### inout은 언제 사용하면 좋을까요?
> ### defer란 무엇인지 설명하시오.
> ### defer가 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우를 설명하시오.

## 프로퍼티
> ### 연산 프로퍼티와 클로저를 가지는 저장 프로퍼티의 차이를 설명해보세요. 
> ### property wrapper에 대해서 설명하시오.
> ### Swift Property 종류에 대해 설명해주세요

***

### Subscripts에 대해 설명하시오.
### String은 왜 subscript로 접근이 안되는지 설명하시오.



### as? 와 as! 차이를 설명해보세요. 


### Generic이 무엇이고 어떻게 동작하는지 설명해주세요.



### Generic에 대해 설명하시오.

### Result타입에 대해 설명하시오.



### 스위프트에서 추상 클래스를 만들려면 어떻게 해야할까요? 



### Subscription에 대해서 설명해주세요.




### Self와 self의 차이가 뭘까요?

### @objc는 언제 사용하나요?



### autoclosure attribute에 대해서 설명해보세요. 

### Never 반환 타입에 대해 설명해보세요.





# 예시
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
