# 🍎 Swift 언어

## 0. 변수와 상수

> ### 💁🏻‍♂️ 0-1 : 변수와 상수에 대해서 설명해 주세요


1. 변수는 선언 이후에도 값을 계속해서 변경할 수 있지만, 상수는 선언 이후에 값을 변경할 수 없습니다
2. Swift에서 변수는 var 상수는 let으로 선언합니다
3. var는 참조하고 있는 주소값을 바꿀 수 있지만, let은 참조하는 주소값을 바꿀 수 없다는 의미를 가지기도 합니다

> **💁🏻‍♂️💁🏻‍♂️ 참조하는 주소값을 바꿀 수 없다는게 정확히 어떤 의미인가요?**

```swift
class Some {
    var num : Int
    
    init(num: Int) {
        self.num = num
    }
}

let a = Some(num: 10)
let b = Some(num: 20)
var c = Some(num: 30)
```

다음과 같은 인스턴스가 있을 때 상수는 값을 변경할 수 없기 때문에 아래와 같은 행위를 할 수 없습니다

```swift
a = b
a = c
b = a
b = c
```

class는 참조타입이기 때문에 각 변수와 상수에는 주소값(==포인터)이 들어있습니다.

즉, 상수로 선언했기 때문에 **참조 주소에 대한 포인터를 바꿀 수 없다는 의미**로 해석됩니다

아래와 같은 행위는 참조 주소값이 바뀌지 않기 때문에 가능합니다

```swift
a.num = b.num
b.num = c.num
```

그리고 아래와 같은 행위는 c가 참조하고 있는 a가 참조하는 주소값으로 변경합니다

```swift
c = a
```

> ### 💁🏻‍♂️ 0-2 : 타입추론에 대해서 설명해 주세요

1. 변수와 상수는 타입을 명시할 수도 있고 생략할 수도 있습니다.
2. 타입을 생략하는경우 컴파일러가 타입 추론 과정을 거쳐 타입을 지정합니다
3. 타입을 생략하는 경우 타입추론 과정을 거쳐야 하기 때문에 타입을 명시하는 것보다 컴파일에 더 오랜 시간이 걸릴 수 있습니다


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

> **💁🏻‍♂️💁🏻‍♂️ 편의 이니셜라이저 꼭 써야 하나요?**

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

옵셔널은 값이 있을 수도, 없을 수도 있음을 나타내는 스위프트의 특징 중 하나로 안전성을 강조합니다

```
📌 옵셔널을 사용하는 방법
- 데이터 타입 뒤에 **물음표**를 붙여서 사용할 수 있습니다 (var optinal : String?)
- **Optional**<Wrapped>을 통해 사용할 수 있습니다 (var optional : Optional<String>)
```


> **💁🏻‍♂️💁🏻‍♂️ 옵셔널을 사용하는 이유가 뭐죠?**

1. 스위프트에서 옵셔널이 아닌 변수나 상수에는 값이 없음을 의미하는 nil을 할당할 수 없습니다
2. 옵셔널을 사용함으로써 변수나 상수가 값이 없을 수도 있음을 직관적으로 받아들일 수 있습니다
3. 예로 함수에 전달되는 매개변수 중 굳이 넘기지 않아도 되는 경우에는 옵셔널로 정의하여 값이 없어도 괜찮음을 직관적으로 보여줄 수 있습니다

> **💁🏻‍♂️💁🏻‍♂️ 옵셔널은 어떻게 구현되어 있나요?**

1. 옵셔널은 **제네릭**이 적용된 **열거형**으로 구현되어 있습니다
2. @frozen 키워드는 type 변경을 제한 하기 때문에 옵셔널에 **case가 추가 되지 않음**을 보장합니다

```swift
@frozen public enum Optional<Wrapped> : ExpressibleByNilLiteral {
    case none
    case some(Wrapped)

		/// 중략
}
```

> **💁🏻‍♂️💁🏻‍♂️ 옵셔널을 일반 데이터 타입으로 사용하려면 어떻게 해야하나요?**

1. 스위프트에서 제공하는 옵셔널 바인딩을 사용하여 안전하게 옵셔널을 벗길 수 있습니다
2. if let 구문과 guard let 구문을 사용하여 옵셔널을 벗긴 값을 사용할 수 있습니다
3. 옵셔널 변수뒤에 **느낌표**를 붙여  강제 추출 할 수도 있습니다. 하지만, nil인 경우 런타임 에러가 나기 때문에 값이 있음을 확신할 수 있는 경우에만 사용해야 합니다

```
📌 옵셔널 바인딩 값의 사용
- if let은 if문 내부에서만 옵셔널 바인딩한 값을 사용할 수 있습니다
- guard let은 옵셔널 바인딩이 성공 하면 그 이후의 코드에서 변수를 자유롭게 사용할 수 있습니다

📌 ExpressibleByNilLiteral
- 이 프로토콜은 nil로 해당 타입을 초기화 할 수 있는 자격요건을 명시합니다
```

> ### 💁🏻‍♂️ 5-2 : enum에 대해서 설명해주세요

1. 열거형은 연관된 항목들을 묶어서 표현할 수 있는 데이터 타입 입니다
2. 열거형은 개발자가 정의한 항목 값 이외에는 추가하거나 수정이 불가능합니다
3. 열거형을 사용할 때에는 점(.)을 통해서 case에 접근할 수 있습니다
4. 열거형을 사용하면 오타를 줄여줄 수 있을 뿐만 아니라 코드의 가독성을 향상 시킬 수 있습니다
5. **원시값**이나 **연관값**을 사용하여 디테일한 값을 저장할 수 있습니다


> **💁🏻‍♂️💁🏻‍♂️ enum의 원시값에 대해서 자세히 설명해 주세요**

1. case에 원시값을 지정해 줄 수 있는데, enum을 선언할 때 이름 옆에 원시값의 type을 명시해주어야 합니다
2. 원시값의 type은 Number, Character, String 세가지만 가능합니다
3. 원시값은 type에 따라 자동으로 생성되는 경우도 있고, 모두 명시해주어야 하는 경우도 있습니다
4. 원시값은 **rawValue**라고 하는 속성을 통해 접근할 수 있습니다

```swift
enum Num : Int{
    case zero
    case one = 10
    case two
    case three
}

print(Num.two.rawValue) //11
```

```
🤔 원시값은 언제 자동 생성될까?
- 정수값이 들어 있는 경우 위에 선언된 값에서 +1 증가된 값을 생성합니다
- String은 원시값을 지정하지 않은 경우 case이름과 동일한 rawValue를 생성합니다
(detail : https://babbab2.tistory.com/116)
```


> **💁🏻‍♂️💁🏻‍♂️ 열거형의 연관값에 대해서 자세히 설명해 주세요**

1. 연관값은 case에 조금더 상세한 정보를 가질 수 있으며, 튜플 형태로 원하는 타입을 명시해서 사용할 수 있습니다
2. 연관값은 모든 case가 가져가 하는 것은 아니며 옵션입니다

```swift
enum Apple {
    case iPhone(model: String)
    case iPad
    case mackBook
}

var apple = Apple.iPhone(model: "14pro")

switch apple {
case .iPhone(model: "14pro"):
    print("최신형 iPhone")
case .iPhone(model: let model):
    print(model)
case .iPad:
    print("iPad")
case .mackBook:
    print("mackBook")
}

// 최신형 iPhone
```

> ### 💁🏻‍♂️ 5-3 : Swift의 컬렉션 타입에 대해서 설명해 주세요

1. 컬렉션 타입은 많은 양의 데이터를 묶어서 저장하고 관리할 수 있습니다. Swift의 컬렉션 타입에는 Array, Dictionary, Set이 있습니다
2. 배열은 같은 타입의 데이터를 **순서대로** 저장하는 컬렉션 타입입니다. 다른 위치에 같은 값이 존재할 수 있습니다. Array는 순서가 있기 때문에 원하는 위치에 데이터를 추가, 수정, 삭제 할 수 있습니다.
3. 딕셔너리는 순서 없이 데이터를 **key-value** 형태로 저장하는 컬렉션 타입입니다. 딕셔너리의 key값은 **Hashable** 프로토콜을 따라야하며 중복될 수 없습니다.
4. 세트는 같은 타입의 데이터를 **순서없이** 하나의 묶음으로 저장하는 컬렉션 타입입니다. 세트 내의 값은 모두 유일하며 **중복된 값을 가질 수 없습니다**. Set는 **Hashable** 프로토콜을 따르는 데이터 타입만 사용 가능합니다.

```
📌 Swift의 Array는 버퍼입니다.
필요에 따라 자동으로 버퍼의 크기를 조절해주므로 요소의 삽입 및 삭제가 자유롭습니다.

📌 Hashable 프로토콜
스위프트의 기본 데이터 타입은 모두 Hashable 프로토콜을 채택합니다.
```

> **💁🏻‍♂️💁🏻‍♂️ Array보다 Set을 사용하는게 더 좋을 때는 언제일까요?**

1. 순서가 중요하지 않고 데이터를 중복없이 고유하게 관리할 때는 Set을 사용하는 것이 더 좋습니다.
2. Set은 **삽입, 삭제, 조회를 모두 O(1)** 에 할 수 있기 때문에 순서가 중요하지 않으면서 삭제와 삽입이 빈번할 때도 Set이 더 좋을 수 있습니다.


> ### 💁🏻‍♂️ 5-4 : Any와 AnyObject에 대해서 설명해주세요

1. Any는 모든 데이터 타입을 사용할 수 있다는 의미입니다. 변수가 Any 타입으로 지정되어 있다면 모든 데이터 타입을 할당할 수 있습니다
2. AnyObject는 Any보다 조금 한정된 의미로 **클래스의 인스턴스**만 할당할 수 있습니다
3. Any로 선언된 변수의 값을 사용하기 위해서는 매번 타입 확인 및 변환을 해주어야 하기 때문에 번거롭습니다
4. Swift는 타입에 엄격한 언어라는 특성이 있어 Any와 AnyObject의 사용을 권장하지 않습니다



## 6. 클로저
> ### 💁🏻‍♂️ 6-1 : Closure에 대해 설명해 주세요

1. 클로저는 어떤 기능을 하는 코드들을 블록으로 모아놓은 객체입니다.스위프트에서 함수형 패러다임으로 구현할 때 매우 중요한 역할을 합니다.
2. 클로저는 class와 같은 **참조 타입**입니다.

> ### 💁🏻‍♂️ 6-2 : Closure 의 값 캡처에 대해 설명해 주세요

1. 클로저는 자신이 정의된 위치에서 외부에 있는 값들을 캡쳐할 수 있습니다. 클로저는 값의 타입과 관계 없이 값을 캡쳐할 때 ****참조**** 합니다.
2. 만약 **value 타입**을 참조하지 않고 복사하고 싶은 경우에는 Closure List를 사용해서 클로저를 선언할 당시의 값을 **상수**로 캡쳐할 수 있습니다. 상수로 캡쳐되기 때문에 클로저 내부에서 값을 바꿀 수 없습니다.

- capture list

```swift
var string = "hello"
let closure = { [string] in
	string = "hi" // ❌error
	print(string)
}
```
> **💁🏻‍♂️💁🏻‍♂️ 참조하면 메모리 이슈가 있을거 같은데요?**

클로저는 참조타입의 값을 캡쳐할 때 **strong**으로 캡쳐하기 때문에 참조하는 인스턴스의 RC값이 증가합니다.

클래스안에서 클로저를 사용하고, 그 클로저가 클래스의 값을 self로 참조하는 경우에는 해당 인스턴스를 nil처리 하더라도 클로저에 의해 메모리에서 해제되지 않는 **강한 순환 참조** 문제가 발생할 수 있습니다.

클로저의 강한 순환 참조는 **capture list를 사용하여 self를 weak으로 캡쳐**하여 RC값을 증가시키지 않도록 할 수 있습니다.

```
📌 강한 순환 참조
인스턴스가 서로를 강한 참조하는 경우를 강한 순환 참조라고 합니다.
참조 타입 인스턴스 A와 B가 있고, 각각의 인스턴스가 프로퍼티로 B와 A를 가지고 있는 경우를 예로 들겠습니다.
이 경우에는 A,B에 모두 nil을 선언 하더라도 메모리에서 해제되지 않습니다. 서로의 인스턴스를 참조하여 RC값이 남아있기 때문입니다.
하지만 더이상 A,B를 접근할 수 있는 방법이 없어 사용할 수 없습니다. A,B모두 메모리에 존재하지만 사용할 수 없는 상태가 되어 메모리 누수가 발생합니다.
이러한 문제를 강한 순환 참조 문제라고 합니다.
```
> **💁🏻‍♂️💁🏻‍♂️ self를 weak 말고 unowned로 캡쳐하면 안되나요?**

만약 클래스 내에서 클로저가 unowned로 self를 캡쳐하는 경우에는 시점 차이로 인해 해당 인스턴스가 nil이 할당 된 후에도 클로저의 작업이 실행되어야 하는 상황이 생길 수 있습니다.

unowned는 존재하지 않는 메모리 주소를 계속해서 참조하기 때문에 런타임 에러가 발생할 수 있습니다. 그래서 메모리에서 해제된 경우 nil을 반환하는 weak 사용을 권장합니다.

> **💁🏻‍♂️💁🏻‍♂️ Name Closure도 값을 캡쳐하나요?**

Name Closure인 경우에는 어디에 위치 하냐에 따라 다릅니다.

전역 함수는 어떠한 값도 캡쳐하지 않습니다.

중첩 함수는 자신을 포함하고 있는 함수의 값을 reference capture 합니다.

- https://babbab2.tistory.com/83

> ### 💁🏻‍♂️ 6-3 : Trailing Closure에 대해서 설명해 주세요

1. 함수 파라미터의 제일 마지막 요소가 클로저 일 때 소괄호 밖에서 클로저를 선언할 수 있는데 이를 후행 클로저라고 합니다.
2. 만약 파라미터에 클로저가 여러개 있는 경우, 다중 후행 클로저 문법을 사용할 수 있습니다. 이 경우에는 소괄호 밖에서 중괄호를 열고 닫으며 클로저를 표현합니다.
3. 클로저의 양이 길어지는 경우 후행 클로저를 사용하면 가독성을 높일 수 있습니다. Xcode에서도 후행 클로저의 사용을 권장하고 있습니다. (자동 변환됨)

> ### 💁🏻‍♂️ 6-4 : Escpaing Closure에 대해 설명해 주세요

1. 함수 파라미터로 전달한 클로저가 함수 종료 이후에 호출될 때 탈출 클로저를 사용해야 합니다.
2. 탈출 클로저를 사용해야 하는 경우는 함수 외부에서 클로저를 저장하거나 사용하는 경우 입니다.
3. 주로 비동기 작업을 할 때 탈출 클로저를 많이 사용합니다. 비동기 작업을 실행하는 함수는 비동기 작업으로 함수가 종료되고 호출해야 하는 클로저를 탈출 클로저로 받습니다.
4. 탈출 클로저는 클로저 앞에 **@escaping** 키워드를 사용하여 작성합니다.

> ### 💁🏻‍♂️ 6-5 : Function과 Closure의 차이점을 말해보세요

1. 클로저는 다양한 형태를 가질 수 있는데, 함수는 클로저의 형태 중 하나입니다. 즉, 함수는 클로저에 속합니다.
2. 하지만 주로 이름이 있는 클로저를 함수라고 표현하고, 익명 클로저를 클로저라고 표현합니다.

- https://babbab2.tistory.com/81

## 7. ARC

> ### 💁🏻‍♂️ 7-1 : ARC에 대해서 설명해 주세요

1. ARC는 swift에서 참조타입의 메모리를 자동으로 관리해주는 방식입니다.
2. ARC는 더이상 참조되지 않는 클래스의 인스턴스를 메모리에서 자동으로 해제합니다.
3. 메모리에서 인스턴스를 언제 해제할지에 대한 여부는 인스턴스의 RC값을 통해 결정합니다.

> **💁🏻‍♂️💁🏻‍♂️ 언제 RC값이 증가하고 언제 감소하나요?**

1. RC값은 인스턴스를 생성하거나, 복사하거나 참조할 때 증가합니다.
2. 함수내에서 인스턴스의 사용이 끝나거나 인스턴스를 참조하는 변수에 nil을 할당하게 되면 RC값이 감소합니다.

> ### 💁🏻‍♂️ 7-2 : 참조는 어떤게 있나요?

1. 참조의 종류는 strong, weak, unowned 세 가지 입니다.
2. strong은 강한 참조를 하여 RC값을 +1 증가 시키는 키워드 입니다.
3. weak과 unowned는 RC값을 증가시키지 않고 참조할 수 있는 키워드 입니다.

```
💡 strong은 강한참조, weak은 약한참조, unowned는 미소유참조 라고 합니다.
```

> **💁🏻‍♂️💁🏻‍♂️ weak과 unowned은 어떤 차이가 있나요?**

1. weak은 참조하고 있는 인스턴스가 메모리에서 해제되면 **nil**을 가지게 됩니다.
2. unowned는 참조하고 있는 인스턴스가 메모리에서 해제되더라도 그 인스턴스가 있던 메모리 주소를 **계속해서 참조**합니다. 즉 비어있는 공간을 참조하는 것입니다.
3. 객체를 참조하는 방식에서의 차이도 있습니다. unowned로 참조할 때는 객체 자체를 참조하지만, weak으로 참조할 때는 참조 카운트를 기록하는 **사이드 테이블을 참조**하고 이를 통해서 간접적으로 객체를 참조합니다.

> **💁🏻‍♂️💁🏻‍♂️ 위험한거 같은데 unowned을 사용하는 이유가 뭘까요?**

1. unowned는 객체를 참조 하지만, weak은 사이드 테이블을 참조합니다.
2. 사이드 테이블은 처음부터 존재하는 것이 아니라 weak reference로 참조될 때 생성됩니다.
3. 사이드 테이블을 생성하고 참조하는것은 그렇지 않을때 보다 오버헤드를 발생시킬 수 있습니다.
4. 그래서 참조하는 인스턴스가 메모리에 존재하는 것이 확실한 경우에는 unowned를 사용하는 것이 성능 향상에 도움이 될 수 있습니다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a90e959d-a91c-47dc-a274-ae60b2115ed3/Untitled.png)

> **💁🏻‍♂️💁🏻‍♂️ strong, weak, unowned reference는 각각 언제 사용할까요?**

1. 기본적으로 **단방향으로 참조**가 이뤄지는 경우에는 strong도 안전하기 때문에 인스턴스가 반드시 메모리에 존재해야 하는 경우에는 strong을 사용해야 합니다.
2. weak은 강한 순환 참조 문제가 생길 수 있을 때 사용할 수 있습니다.
3. unowned는 해당 변수가 참조하는 인스턴스보다 먼저 해제되는 것이 확실한 상황에서만 사용해야 합니다.
- [https://sihyungyou.github.io/iOS-side-table/](https://sihyungyou.github.io/iOS-side-table/)

> ### 💁🏻‍♂️ 7-3 : class가 메모리에서 해제되는 과정을 설명해보세요

1. class 인스턴스는 strong, weak, unowned 세가지 방식을 통해 참조 될 수 있으며, 각각은 각자의 reference counting을 합니다.
2. 만약 weak count가 증가하는 경우에는 사이드 테이블을 생성하여 weak reference는 사이드 테이블을 참조합니다.
3. 인스턴스의 RC값인 strong reference count 값이 0이 되면 **deinited** 됩니다.
4. deinited되면 unowned count값이 감소하고 unowned count값이 0이 되면 **메모리에서 해제**됩니다.
5. 만약 weak count값이 0이 아니라면 사이드 테이블이 남아있는 **freed** 상태가 되며 이때 weak count값이 감소하고 weak count값이 0이 되면, 해당 오브젝트의 side table이 해제되며 객체가 완전히 소멸됩니다.

- https://jeonyeohun.tistory.com/373

> ### 💁🏻‍♂️ 7-3 : COW에 대해서 설명해 주세요
> **💁🏻‍♂️💁🏻‍♂️ COW를 지원하지 않는 value type에서 COW를 사용하고 싶으면 어떻게 구현해야 할까요?**


## 8. 프로토콜

> ### 💁🏻‍♂️ 8-1 : 프로토콜이란 무엇인지 설명해 주세요

1. 프로토콜이란 객체가 어떤 역할을 하기위한 인터페이스를 정의하는 것입니다.
2. 프로토콜은 정의를 하고 제시만 할 뿐, 스스로 기능을 구현하지 않습니다.
3. 프로토콜을 통해 **다형성**을 구현할 수 있으며, 프로토콜을 이용하여 객체 간에 유연한 커뮤니케이션을 할 수 있습니다.
4. **클래스, 구조체, 열거형**이 프로토콜을 채택할 수 있으며, 채택한 타입은 프로토콜이 제시하는 기능을 모두 구현해야 합니다.

```
📌 다형성
하나의 객체가 다양한 타입을 가질 수 있는 것을 의미합니다.
📌 채택의 제한
프로토콜의 상속 리스트에 class 키워드를 추가하면, class만 채택가능한 프로토콜이 됩니다.
📌 프로퍼티
- 프로퍼티는 항상 var 키워드를 사용해야 합니다.
- 해당 프로토콜을 채택한 후에는 var, let 중 어떤것으로 선언해도 무관합니다.
- 프로퍼티를 { get set }으로 요구한 경우에는 채택한 후에도 let을 사용해서는 안됩니다. set을 할 수 없기 때문입니다.
- 하지만 { get }으로 요구했더라도, 채택한 후에 필요하다면 set을 적용 해도 무관합니다.
```
> **💁🏻‍♂️💁🏻‍♂️ 프로토콜은 기능 구현을 하지 못하나요?**

1. 아니요, 프로토콜을 이용해 추상클래스로써 역할을 할 수 있습니다.
2. 프로토콜은 기본적으로 기능 구현을 할 수 없고, 클래스는 추상 표현이 불가능 합니다.
3. 하지만 프로퍼티와 메서드를 프로토콜로 정의하고 **extension**을 통해 프로토콜 메서드의 기본 구현체를 만들면 추상클래스와 동일한 개념을 가집니다.

```
💡
- 프로토콜은 extension을 통해서만 구현체 작성이 가능합니다.
- 프로토콜간에 상속이 가능합니다.
```

> **💁🏻‍♂️💁🏻‍♂️ 프로토콜에서 정의된 기능은 무조건 구현해야 할까요?*

1. 아니요, 프로토콜은 **선택적 요구사항을 지정**할 수 있습니다.
2. 단, 선택적 요구사항을 정의하고 싶은 프로토콜은 **objc** 속성이 부여된 프로토콜이어야 한다는 규칙이 있습니다.
3. 그리고 선택적 요구사항이 적용된 프로토콜은 objc 속성이 부여된 클래스에서만 사용 가능합니다.
4. 선택적 요구사항은 `@objc optional` 키워드를 요구사항 앞에 붙여줘야 합니다.

```swift
@objc protocol SomeProtocol {
	func essential() //프로토콜 채택시 반드시 구현해야함!!
	@objc optional func option() //선택적으로 구현해도 됨
}
```

> ### 💁🏻‍♂️ 8-2 : associatedType이 무엇인지 설명해주세요

associatedType은 프로토콜 내에서 타입을 지정하지 않고, 프로토콜을 채택하여 구현할 때 타입을 지정할 수 있도록 도와줍니다. 프로토콜에서 일종의 제네릭 역할을 합니다.

- [https://zeddios.tistory.com/382](https://zeddios.tistory.com/382)

> ### 💁🏻‍♂️ 8-3 : Codable에 대하여 설명해 주세요

1. Encodable과 Decodable 프로토콜의 합성 프로토콜입니다.
2. Codable을 준수하는 타입은 다른 표현 방식으로 **상호 변환**할 수 있습니다.
3. 대표적으로 JSONEncoder, JSONDecoder과 결합하여 인스턴스를 JSON으로 변환하고, JSON을 인스턴스로 변환할 때 사용할 수 있습니다.

```
📌 Encodable과 Decodable?
- 사용자 정의 데이터 타입을 다른 형식으로 인코딩(with Encodable)하거나 디코딩(with Decodable)할 수 있는 프로토콜입니다.
- Foundation 프레임워크에 있는 여러 클래스와 호환하면 다양한 기능을 할 수 있습니다.
```

> **💁🏻‍♂️💁🏻‍♂️ Codable과 NSCoding의 차이는?*

1. Codable은 클래스, 열거형, 구조체에 모두 적용할 수 있지만 NSCoding은 클래스 타입에만 적용이 가능합니다.
2. NSCoding은 NSObject를 상속받은 클래스에서만 채택할 수 있습니다. 또 NSCoding을 채택하면 반드시 NSKeyedArchiver와 NSKeyedUnarchiver를 사용해 **Data 타입으로 저장하고 디코딩**해야합니다.
3. ❗️**정리중**
- [https://codesquad-yoda.medium.com/codable-vs-nscoding-차이점-4b47e240c0b8](https://codesquad-yoda.medium.com/codable-vs-nscoding-%EC%B0%A8%EC%9D%B4%EC%A0%90-4b47e240c0b8)

> ### 💁🏻‍♂️ 8-4 : Equatable 프로토콜에 대해서 설명해 주세요

1. 타입끼리 비교(==)연산을 하기 위해 필수적으로 구현해야 하는 프로토콜 입니다.
2. 기본 타입은 Equatable을 따르고 있어 비교 연산이 가능하지만, 커스텀 타입의 경우 직접 채택해 주어야 합니다.
3. 구조체는 프로퍼티가 **모두 기본 타입**인 경우 Equatable을 채택하면 비교 연산이 가능합니다.
4. 클래스의 경우 직접 메서드를 구현해주어야 합니다.
5. 열거형의 경우 연관값이 없으면 자동으로 Equatable을 채택합니다. 연관값이 있는 경우  Equatable을 직접 채택해줘야 하며, 연관값이 모두 Equatable을 따르고 있어야 합니다.

```swift
//직접 메서드를 구현해줘야 하는 경우
static func == (lhs: Class, rhs: Class) -> Bool {
    return lhs.first == rhs.first && lhs.second == rhs.second
}
```

- [https://babbab2.tistory.com/148](https://babbab2.tistory.com/148)

> ### 💁🏻‍♂️ 8-5 : Hashable 프로토콜에 대해서 설명해 주세요

1. Hashable을 채택하는 타입은 모두 값을 정수 해시값으로 표현할 수 있습니다.
2. Hashable은 Equatable 프로토콜을 준수하고 있습니다.
3. 기본 데이터 타입은 Hashable을 따르고 있지만, 커스텀 타입의 경우 직접 채택해야 합니다.
4. 구조체는 프로퍼티가 모두 기본 타입이면 Hashable을 채택하기만 하면 됩니다.
5. 클래스는 직접 hash함수를 구현해주어야 합니다. 추가로 Equatable을 채택하고 있기 때문에 == 함수도 함께 구현해주어야 합니다.
6. 열거형의 경우 연관값이 없으면 자동으로 Hashable을 채택합니다. 연관값이 있는 경우 Hashable을 채택해줘야 하며, 연관값이 모두 Hashable을 채택해야 합니다.

> **💁🏻‍♂️💁🏻‍♂️ Equatable을 왜 채택해야 하나요?*

해시값은 고유값이어야 합니다. 그래서 고유값을 식별해 줄 수 있는 == 함수가 필요합니다.

그래서 Equatable을 채택하여 ==함수를 구현해야 합니다.

- [https://babbab2.tistory.com/149](https://babbab2.tistory.com/149)
- [https://velog.io/@hayeon/Hashable이-무엇이고-Equatable을-왜-상속해야-하는지-설명하시오#왜-hashable은-equatable을-상속해야할까](https://velog.io/@hayeon/Hashable%EC%9D%B4-%EB%AC%B4%EC%97%87%EC%9D%B4%EA%B3%A0-Equatable%EC%9D%84-%EC%99%9C-%EC%83%81%EC%86%8D%ED%95%B4%EC%95%BC-%ED%95%98%EB%8A%94%EC%A7%80-%EC%84%A4%EB%AA%85%ED%95%98%EC%8B%9C%EC%98%A4#%EC%99%9C-hashable%EC%9D%80-equatable%EC%9D%84-%EC%83%81%EC%86%8D%ED%95%B4%EC%95%BC%ED%95%A0%EA%B9%8C)

## 9. 키워드
> ### 💁🏻‍♂️ 9-1 : mutating 키워드에 대해 설명해 주세요

1. 기본적으로 Swift에서는 **구조체나 열거형** 내부에서 변수를 변경할 수 없습니다.
2. 하지만, mutating 키워드를 함수 앞에 붙이면 함수 내부에서 해당 구조체나 열거형의 값을 변경할 수 있습니다.

> ### 💁🏻‍♂️ 9-2 : lazy 에 대해 설명해 주세요

1. lazy는 지연 저장 프로퍼티로, 호출이 있어야만 값을 초기화 합니다.
2. lazy는 호출될 때 값을 초기화 하므로 init시점 이후에 값을 초기화 할 가능성이 있습니다. 그러므로 상수가 아닌변수로 정의해야 합니다.
3. lazy 프로퍼티를 적절히 사용하면 성능저하나 공간 낭비를 줄일 수 있습니다.

> **💁🏻‍♂️💁🏻‍♂️ lazy는 어떤 문제가 있을 수 있나요?**

다중 스레드 환경에서 lazy 키워드로 생성한 변수를 여러 스레드가 동시에 접근하는 경우 여러번 초기화되는 문제가 생길 수 있습니다.

> ### 💁🏻‍♂️ 9-3 : typealias 가 무엇인지 말해주세요

1. 기존 타입에 새로운 이름(별명)을 지정할 때 사용하는 키워드입니다.
2. typealias를 잘 사용하면 코드를 더욱 가독성 있게 만들 수 있습니다.

```swift
typealias Name = String
let name : Name = "김제니"
```

> ### 💁🏻‍♂️ 9-4 : required 키워드에 대해서 설명해 주세요

1. 클래스나 프로토콜을 상속받는 클래스에서 해당 메서드가 반드시 구현되어야 할 때 사용합니다.
2. required 키워드가 붙은 메서드는 하위 클래스에서 구현되지 않으면 컴파일 오류가 발생합니다.
3. 만약, override 키워드와 함께 사용되는 경우, override 키워드가 반드시 함께 사용되어야 합니다.

## 10. 패러다임

> ### 💁🏻‍♂️ 10-1 : Swift의 map, filter, reduce에 대하여 설명해 주세요

1. 스위프트는 함수를 일급 객체로 취급합니다. 그래서 함수를 매개변수로 사용할 수 있습니다.
2. 매개변수로 함수를 갖는 함수를 **고차 함수**라고 부르는데, map, filter, reduce는 모두 고차함수에 속합니다.
3. map은 초기값과 클로저를 전달 받아 전달받은 클로저를 실행하여 그 결과를 다시 반환해주는 함수입니다. 기존 데이터를 변형하지 않고 새로운 값을 반환합니다. 예를들어, 초기 값의 타입을 Int → String으로 변환할 때 사용할 수 있습니다.
4. filter는 컨테이너 내부의 값을 걸러서 추출하는 함수입니다. 조건에 맞는 값을 모아 map과 마찬가지로 새로운 컨테이너를 반환합니다. 예를들어, 초기 값이 Int인 경우 10이상인 값만 추출 할 때 사용할 수 있습니다.
5. reduce는 초기값을 하나의 값으로 합쳐주는 함수입니다. reduce를 사용하여 배열의 모든 값을 합치는 경우 그 결과는 배열이 아닌 하나의 값으로 나오게 됩니다.

> **💁🏻‍♂️💁🏻‍♂️ map의 종류인 flatMap과 compactMap에 대해서 설명해 주세요**

1. 두 함수는 요소가 일차원 배열일 때 동일하게 동작합니다.
2. 일차원 배열에서 클로저의 실행 결과가 옵셔널일 때, nil인 경우 nil을 제거하고 옵셔널 바인딩 한 결과를 배열로 만들어 반환합니다. 하지만, 2차원 배열인 경우 nil을 제거하지않습니다.
3.  flatMap은 결과를 **1차원 배열로(flatten)** 만들어내고, compactMap은 2차원 배열 그대로 반환한다는 차이점이 있습니다.
- [https://jinshine.github.io/2018/12/14/Swift/22.고차함수(2) - map, flatMap, compactMap/](https://jinshine.github.io/2018/12/14/Swift/22.%EA%B3%A0%EC%B0%A8%ED%95%A8%EC%88%98(2)%20-%20map,%20flatMap,%20compactMap/)

```
💡 swift에서 같은 일차원 옵셔널 배열을 사용할 때, compactMap을 권장합니다.

'flatMap' is deprecated: Please use compactMap(_:) for the case where closure returns an optional value

```

> ### 💁🏻‍♂️ 10-2 : Protocol Oriented Programming과 Object Oriented Programming의 차이점을 설명하시오.

## 11. function / method

> ### 💁🏻‍♂️ 11-1 : function과 method의 차이를 말해보세요.

1. funtion은 특정 객체에 속해있지 않은 코드 블록입니다.
2. method는 클래스, 구조체, 열거형 등 특정 객체 안에서 정의 되는 함수입니다.
3. function은 외부 객체의 상태를 변경할 수 있지만, method는 자기가 속해 있는 객체의 프로퍼티나 메서드만 접근할 수 있습니다.

> ### 💁🏻‍♂️ 11-2 : instance 메서드와 class 메서드의 차이점을 설명해주세요.

1. 인스턴스 메서드는 인스턴스에 속한 함수를 뜻합니다. 인스턴스 내부의 값을 변경하는 등 인스턴스와 관련된 일을 합니다. (우리가 일반적으로 객체 안에 선언해서 사용하는 함수가 인스턴스 메서드임!!)
2. 클래스 메서드는 **클래스의 타입 메서드** 중 하나로, 타입 자체에 호출이 가능한 메서드 입니다. 인스턴스를 생성하지 않아도 타입을 통해 메서드를 호출 할 수 있습니다.

> ### 💁🏻‍♂️ 11-3 : class 메서드와 static 메서드의 차이점을 설명해주세요.

1. 두 메서드는 클래스의 타입 메서드 입니다.
2. 타입 메서드는 타입 자체에 호출이 가능한 메서드로, 인스턴스를 생성하지 않아도 호출할 수 있습니다.
3. **static** 메서드는 상속 후 메서드 **재정의가 불가능** 하고, **class** 메서드는 상속 후 **재정의 할 수 있습니다.**

> ### 💁🏻‍♂️ 11-4 : 함수에서 외부에 있는 값타입 데이터를 바꾸고 싶으면 어떻게 해야 될까요?

1. 일반적으로 함수의 인자는 상수 값으로 전달되어 데이터를 변경할 수 없습니다.
2. 함수 내에서 데이터를 변경하고자 하는 경우에는 **inout** 키워드를 사용할
3. inout 키워드는 인자의 타입 앞에 선언합니다. 이는 해당 인자의 **메모리 주소를 전달** 받겠다는 의미입니다.
4. 함수를 호출할 때에는 인자 앞에 `&`를 붙여 **메모리 주소를 전달**한다고 표시 해주어야 합니다.
5. inout은 함수 호출시 메모리 주소를 전달해야 하기 때문에 **오버헤드**가 발생할 수 있습니다.

```swift
func functionName(_ value : inout String) {
    value = "change"
}

var someValue: String = "hello"
functionName(&someValue)
print(someValue) // "change"
```

> ### 💁🏻‍♂️ 11-5 :  defer란 무엇인지 설명해주세요.

1. defer는 코드 블록을 지연하여 함수 실행시 가장 마지막에 실행되도록 하는 키워드 입니다.
2. defer안에 있는 내용은 함수가 반환하기 직전에 실행됩니다.
3. 함수내에서 일어나는 일 중 가장 마지막에 정리해야 하는 일을 처리할 수 있습니다. (리소스 해제, 임시 데이터 삭제, 파일 닫기 etc)

> **💁🏻‍♂️💁🏻‍♂️ defer가 여러개일 경우 호출되는 순서는 어떻게 되고, defer가 호출되지 않는 경우는 없을까요?**

1. defer가 여러개일 경우 함수 내에서 선언한 순서의 역순으로 실행됩니다. 함수 코드 블록의 가장 아래에 있는 defer부터 순차적으로 실행됩니다.
2. 만약 함수를 실행중인 스레드가 비정상적으로 종료되거나, defer를 읽기 전에 함수가 return 되는 경우에는 defer를 실행하지 않습니다.
3. guard문을 사용하게 되면 옵셔널 바인딩에 실패하는 경우, guard문 아래에 있는 defer가 실행되지 않을 수 있습니다.

```swift
func say(){
    defer {
        print("나는 마지막이다!")
    }
    defer {
        print("나는 세번째")
    }
    print("내가 제일 처음 호출되겠지")
    defer {
        print("나는 두번째로 호출되겠지")
    }
}
say() // print에 적힌 순서대로 출력
```

> **💁🏻‍♂️💁🏻‍♂️ defer를 언제 사용할까요?**

1. 함수가 종료되기 직전에 실행되어야 하는 코드를 작성할 수 있습니다.
2. 예를들어 함수에서 lock을 걸어 동기화문제를 해결 하는 경우, 함수가 종료되기 직전에 unlock을 해주어야 데드락에 걸리지 않습니다.
3. 함수가 종료될 때 마다 해야 하는 행위를 함수 내에 있는 여러개의 return 앞에 써주기 보다는, defer를 함수 상단에 선언해주면 함수가 알아서 종료될 때 해야하는 행위를 수행할 수 있습니다.

## 12. 프로퍼티

> ### 💁🏻‍♂️ 12-1 : 저장 프로퍼티와 연산 프로퍼티의 차이점을 설명해 주세요.

1. 저장 프로퍼티는 객체 내에서 값을 저장할 때 사용하는 기본적인 프로퍼티 입니다.
2. 변수나 상수로 선언할 수 있으며, 초기 값을 설정할 수 있습니다.
3. 연산 프로퍼티는 값을 저장하지 않고 인스턴스의 내부 또는 외부 값을 연산을 하는 프로퍼티 입니다.
4. 연산 프로퍼티를 통해 인스턴스 내부에 있는 은닉화 된 값을 간접적으로 수정 또는 접근 할 수 있습니다.

> **💁🏻‍♂️💁🏻‍♂️ 메서드를 사용하면 되는데 굳이 연산 프로퍼티를 쓰는 이유가 뭘까요?**

1. 인스턴스의 외부에서 내부 값에 접근하기 위해서는 getter, setter 두 개의 메서드를 구현해야 합니다.
2. 이는 두 메서드가 분산 구현되어 코드의 가독성이 나빠질 수 있습니다.
3. 연산 프로퍼티는 그 역할을 동시에 하여 간편하고 직관적인 코드를 작성할 수 있습니다.
4. 하지만, 연산 프로퍼티는 **쓰기 전용으로 구현할 수 없으므로** 쓰기 전용을 구현하기 위해서는 메서드를 사용해야 합니다.

> ### 💁🏻‍♂️ 12-2 : 지연 저장 프로퍼티에 대해서 설명해 주세요.

1. 지연 저장 프로퍼티는 해당 프로퍼티가 호출 될 때 값을 초기화 합니다.
2. **lazy** 키워드를 프로퍼티 앞에 붙여 사용할 수 있습니다.
3. 지연 저장 프로퍼티를 사용하면 성능저하나 공간 낭비를 줄일 수 있습니다.

> ###  💁🏻‍♂️ 12-3 : property warrper에 대해서 설명해 주세요.

1. 프로퍼티 래퍼는 여러 프로퍼티에 대해 반복되는 코드를 재사용할 수 있도록 해주는 기능입니다.
2. 프로퍼티 래퍼를 사용하여 **보일러 플레이트를 코드**를 제거할 수 있습니다.
3. @propertyWrapper키워드를 사용해 구조체를 정의하고 내부에 프로퍼티에 대한 동작을 정의해두면, 프로퍼티를 선언할 때 프로퍼티 래퍼를 키워드로 붙여 미리 정의한 동작을 재사용할 수 있습니다.
4. 프로퍼티를 가질 수 있는 클래스, 구조체, 열거형에서만 사용할 수 있습니다.

```
🤔 보일러 플레이트 코드
특정 작업을 수행하기 위해 반복적으로 작성되는 코드를 의미합니다.

```

- 기본 틀은 다음과 같습니다.
```swift
//property wrapper 임을 알려주는 어노테이션 입니다.
@propertyWrapper
struct Wrapper {
    var value : String
    
    //꼭!! 작성해야 하는 연산 프로퍼티 입니다. 중복되는 코드를 작성합니다.
    var wrappedValue: String {
        get { "Value : \(value)"}
        set { value = newValue }
    }
}
```

- 네트워크 에러 상태 코드를 정의하는 상황을 예시로 들어 보겠습니다.

```swift
struct NotFound {
    private var _alert : String = "404 not found"
    
    **var alert : String {
        get{ "Alert!! \(_alert)" }
        set{ _alert = newValue }
    }**
    
    init(_ value : String){
        self._alert = value
    }
}

struct Forbidden {
    private var _alert : String = "403 forbidden"
    
    **var alert : String {
        get{ "Alert!! \(_alert)" }
        set{ _alert = newValue }
    }**
    
    init(_ value : String){
        self._alert = value
    }
}
```
- 위 코드를 프로퍼티 래퍼를 적용하여 다음과 같이 중복을 제거할 수 있습니다.

```swift
@propertyWrapper
struct Alert {
    private var _alert : String
    
    var wrappedValue : String {
        get{ "Alert!! \(_alert)" }
        set{ _alert = newValue }
    }
    
    init(wrappedValue alert: String) {
        self._alert = alert
    }
}

struct NotFound {
    @Alert var alert : String = "404 not found"
}

struct Forbidden {
    @Alert var alert : String = "403 forbidden"
}
```

- Userdefault에 프로퍼티 래퍼를 적용하면 쉽게 접근하고 수정할 수 있습니다.

```swift
@propertyWrapper
struct UserDefault<T> {
    let key : String
    let value : T
    
    var wrappedValue : T {
        get { UserDefaults.standard.object(forKey: key) as? T ?? self.value }
        set { UserDefaults.standard.set(newValue, forKey: key)}
    }
}

class UserManager {
    
    @UserDefault(key: "userName", value: nil) //기본값 설정
    static var userName : String?
    @UserDefault(key: "autoLogin", value: false) //기본값 설정
    static var autoLogin : Bool

}
```

***

## 13. 제네릭

1. 제네릭은 타입을 일반화하여 재사용 가능한 함수나 객체를 만들 수 있는 기능입니다.
2. 제네릭을 통해 코드의 재사용성과 유지보수성을 향상 시킬 수 있습니다.

> **💁🏻‍♂️💁🏻‍♂️ 제네릭의 타입 제약**

1. 제네릭은 타입을 일반화하기 때문에 어떤 타입도 수용할 수 있지만, 제약을 걸어 타입을 제한할 수 있습니다.
2. 제네릭 타입을 명시할 때 특정 프로토콜을 따르는 타입만 사용할 수 있도록 제약할 수 있습니다.
3. 타입 제약은 클래스나 프로토콜만 가능합니다.

## 14. Subscript

1. subscript는 클래스, 구조체, 열거형 등에서 **컬렉션 요소**에 접근하는 방법을 제공하는 기능입니다.
2. 컬렉션 요소는 배열, 딕셔너리, 집합 등이 있습니다.
3. subscript는 index를 파라미터로 받는 함수를 만들어 함수 내에서 get과 set을 작성해야 합니다. get만 작성하여 읽기 전용으로 구현할 수도 있습니다.
4. subscript를 작성한 후에는 인스턴스의 이름 뒤에 대괄호([])를 붙여서 호출할 수 있습니다.
5. subscript는 구현부 또는 익스텐션 구현부에 위치해야 합니다.

```swift
class MyArray {
    var array = [Int]()

    subscript(index: Int) -> Int {
        get {
            return array[index]
        }
        set(newValue) {
            array[index] = newValue
        }
    }
}

var myArray = MyArray()
myArray.array = [1, 2, 3, 4, 5]

let value = myArray[2] // 3
```

> **💁🏻‍♂️💁🏻‍♂️ String도 서브스크립트로 접근할 수 있나요?**

네, String은 Character 컬렉션 타입 입니다. 따라서 특정 문자에 접근하기 위해 인덱스를 사용할 수 있습니다.

```swift
class MyClass {
    var myString = "Hello, world!"
    
    subscript(index: Int) -> Character {
        get {
            return myString[myString.index(myString.startIndex, offsetBy: index)]
        }
        set {
            let startIndex = myString.index(myString.startIndex, offsetBy: index)
            myString.replaceSubrange(startIndex...startIndex, with: String(newValue))
        }
    }
}

var myClass = MyClass()
print(myClass[1]) // "e"

myClass[0] = "J"
print(myClass.myString) // "Jello, world!"
```

***

## 15. 추가 개념

> ### 💁🏻‍♂️ 15-1 : Result타입에 대해 설명해 주세요.

성공 혹은 실패에 대한 정보를 담는 타입입니다. 제네릭 열거형으로 구현되어 있습니다.

> **💁🏻‍♂️💁🏻‍♂️ 옵셔널과 차이점?**

옵셔널은 값이 없을 때 무조건 nil을 가지고 있지만, Result 타입은 실패했을 때 오류에 대한 정보를 담은 연관 값을 가지고 있습니다.


> ### 💁🏻‍♂️ 15-2 : 스위프트에서 추상 클래스를 만들려면 어떻게 해야할까요? 

1. 스위프트는 추상 클래스 문법을 지원하지 않지만 **프로토콜**을 통해 동일한 동작을 하도록 할 수 있습니다.
2. 프로퍼티와 메서드의 원형을 프로토콜에 선언해두고, 프로토콜을 **extension**하여 프로토콜 메서드의 기본 구현체를 만들어주면 추상클래스와 동일한 개념의 구현을 할 수 있습니다.

> ### 💁🏻‍♂️ 15-3 : Self와 self의 차이가 뭘까요?

1. 대문자 Self는 타입 자체를 나타내는 **키워드** 입니다.
2. class, struct의 자기 자신의 타입을 나타내거나, 프로토콜에서 해당 프로토콜을 채택하는 타입을 나타낼 때 사용됩니다.
3. 소문자 self는 **예약어**로 현재 인스턴스를 나타냅니다. 즉, 해당 인스턴스에 대한 **참조**를 나타냅니다.

> ### 💁🏻‍♂️ 15-4 : @objc는 언제 사용하나요?

1. Objective-C 런타임에 swift 메서드를 호출하기 위해서 사용할 수 있습니다.
2. swift에서 프로토콜의 메서드를 호출할 때 static 디스패치를 사용하는데, @objc를 사용하면 메서드를 동적 디스패치로 동작할 수 있습니다. 이를 통해 유연성을 제공합니다.

> ### 💁🏻‍♂️ 15-5 : autoclosure attribute에 대해서 설명해보세요. 

1. **함수 인자에 autoclosure 속성**을 붙이면, 전달되는 **인자 코드를 감싸서 자동으로 클로저로** 만들어줍니다.

2. 다시 말해 **일반 표현의 코드를 클로저 표현의 코드로 만들어주는 역할**을 합니다. 이때 클로저에는 **인자가 없고 리턴값만 존재해야 합니다.**

3. 클로저 호출에 필요한 **중괄호를 없앰으로써 코드의 간결성**을 높일 수 있습니다.

4. Swift에서 @autoclosure를 사용하는 대표적인 예로 **assert()**함수가 있습니다.

```swift
// @autoclosure 를 사용하지 않은 경우
func normalPrint(_ closure: () -> Void) {
    closure()
}

// 이 함수를 호출할 때 클로저 부분은 다음과 같이 대괄호 {...}로 묶어서 인자를 넣어야 합니다.
normalPrint({ print("I'm Normal Expression") })

// @autoclosure 를 사용한 경우
func autoClosurePrint(_ closure: @autoclosure () -> Void) {
    closure()
}

// @autoclosure를 이용하면 함수에 클로저를 인자로 사용할때 일반 표현을 사용할 수 있습니다.
autoClosurePrint(print("I'm AutoClosure Expression"))

```

- https://jusung.github.io/AutoClosure/


> ### 💁🏻‍♂️ 15-6 : Never 반환 타입에 대해 설명해보세요.

- **에러 등의 이유로 정상적으로 리턴 되지않는 함수의 리턴 타입**입니다. **실제로 어떤 값을 리턴하지는 않습니다.** (The return type of functions that do not return normally, that is, a type with no values.)

```swift
func crashAndBurn() -> Never {
    fatalError("Something very, very bad happened")
}
```

- https://developer.apple.com/documentation/swift/never


> ### 💁🏻‍♂️ 15-7 : 모나드에 대해서 설명해주세요.

- **모나드는 값이 선택적으로 존재하는 함수 객체(functor)입니다.**

- **함수 객체(functor) = 값을 담고 있는 컨테이너 타입**입니다. 고차 함수 **map 을 적용시킬 수 있다**는 특징이 있습니다. Swift 에서는 Array, Dictionary, Optional 등이 컨테이너 타입입니다.

- **Swift 에서 대표적으로 모나드는 Optional** 이 있습니다.

- Optional 은 값이 **선택적으로 존재**하고, **map 을 적용**시킬 수도 있기 때문입니다.

- 모나드는 함수 객체 안에 포함되는 개념이지만, 함수 객체와의 차이점이 있습니다. 바로 **모나드에는 flatMap (옵셔널 해제 기능 추가) 를 사용할 수** 있고, 모나드가 아닌 함수 객체에는 flatMap 을 사용할 수 없다는 것입니다.

- https://zeddios.tistory.com/449
