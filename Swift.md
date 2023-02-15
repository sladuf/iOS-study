# 🍎 Swift 언어

## 1. Class / Struct
> ### 💁🏻‍♂️ 1-1 : Class 와 Struct의 차이를 설명해주세요

1. struct는 **value type**이고 class는 **reference type**입니다
2. 그래서 reference counting은 class의 인스턴스에만 적용됩니다
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

> **💁🏻‍♂️ → 언제 class를 사용하고, 언제 struct를 사용하나요?**

Apple 가이드라인에서 다음 조건 중 하나 이상에 해당한다면 구조체를 사용하는 것을 권장합니다
- 연관된 간단한 값의 집합을 캡슐화하는 목적일 때
- 프로퍼티가 값 타입이고 참조보다 복사하는 것이 합당할 때
- 상속할 필요가 없을 때

즉, 상속 관계를 필요로 하거나 객체의 값이 계속 가공되는경우에는 class가 더 유리합니다

> ### 💁🏻‍♂️ 1-2 : Swift의 복사 방법에 대해서 설명해 주세요

1. swift는 COW방식을 사용해서 값타입을 복사합니다
2. COW는 원본과 복사본 중 수정되기 전까지는 복사를 하지 않고 **원본 리소스를 공유**하다가, 둘 중 하나에서 수정이 일어나는 경우 복사하는 작업을 합니다
3. 그래서 COW를 사용하는 타입에서 복사 후 첫번째 수정이 일어나는 경우에는 COW에 의해 약간의 오버헤드를 발생시킵니다
4. 모든 value type에서 사용되는 것이 아니라 **collection type**을 복사할 때 사용됩니다

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

# 여기부터 


> ### 💁🏻‍♂️ 1-3 : class의 성능을 향상 시킬 수 있는 방법을 설명해주세요
- final, WMO
- [https://babbab2.tistory.com/145?category=828998](https://babbab2.tistory.com/145?category=828998)


> ### 💁🏻‍♂️ 1-4 : upcasting과 downcasting의 차이


> ### enum


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
> > ### Closure 와 Escpaing Closure 에 대한 설명해주세요

## 데이터타입
> ### Any와 AnyObject의 차이
> ### AnyObject에 대해 설명하시오.
> ### Optional 이란 무엇인지 설명하시오.
> ### Optional은 내부적으로 어떻게 구현되어 있나요?
> ### let과 var의 차이는 무엇인가요?
- https://github.com/jeonyeohun/Getting-Ready-For-Interview/blob/main/iOS-Swift/swift.md
> ### Swift에서 참조 타입을 말해보세요.


## 컬렉션타입
> ### Array보다 Set을 사용하는게 더 좋을 때는 언제일까요?
> ### Array, Set, Dictionary의 차이점이 뭘까요? 

## 이니셜라이저
> ### Convenience init에 대해 설명하시오.
> ### Convenience init에 대해 설명해주세요
> ### deinit은 언제 사용할까요? 


## Extension
> ### Extension에 대해 설명하시오.
> ### Extension 내부에서 함수를 override할 수 있는지 설명하시오.
> ### Extension은 메서드를 Override 할 수 있을까요?


## 키워드
> ### mutating 키워드에 대해 설명하시오.
> ### lazy 에 대해 설명하시오.
> ### some 키워드에 대해 설명하시오.
> ### typealias 가 무엇인지 말해주세요.
> ### required 키워드에 대해서 설명해보세요. 


## 접근제어자
> ### 접근 제어자의 종류엔 어떤게 있는지 설명하시오.
> ### open과 public 키워드의 차이를 설명해보세요.
- https://github.com/jeonyeohun/Getting-Ready-For-Interview/blob/main/iOS-Swift/swift.md
> ### fileprivate을 설명하고 언제 사용하면 좋을지 이야기해보세요.
> ### fileprivate과 private의 차이를 설명해보세요. 

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
