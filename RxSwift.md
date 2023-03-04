# 🍎 RxSwift 관련

## 1. RxSwift 개념


> ### 💁🏻‍♂️ 1.1 Reactive Programming이 무엇인지 설명해주세요.

- **Reactive Programming 은 데이터의 흐름을 중심으로 실행되는 프로그래밍 패러다임**입니다.

- 데이터의 상태변화나, 이벤트에 따른 로직을 작성합니다.

- 이 때, 데이터의 흐름을 비동기적으로 처리할 수 있기 때문에 **비동기 프로그래밍**에 좋습니다.

- **함수형 프로그래밍을 결합**해 가독성 좋고 유지보수 좋은 코드를 짜는 데 유용합니다.

> ### 💁🏻‍♂️ 1.2 : RxSwift 를 왜 썼고 장단점이 뭐가 있나요?

1. **비동기 코드**를 쉽게 작성할 수 있습니다. 예를 들어 네트워크 통신에 의해 데이터를 가져오는 작업을 Observable 에 담아서 작성한다면, **Observable 이 뱉어내는 onNext, onCompleted 등의 이벤트를 기반**으로 비동기 처리를 쉽게 할 수 있습니다.

2. **함수형 프로그래밍을 지원**합니다. **map 이나 filter 등의 Operator 를 통해 데이터를 가공**하기 좋습니다.

3. **데이터 흐름을 다룰 때 유용한 클래스**들이 많습니다. **Observable, Subject 등의 클래스**를 사용해서 데이터 흐름을 효과적으로 다룰 수 있습니다.

> ### 💁🏻‍♂️ 1.3 : RxSwift 와 Combine 차이를 설명해주세요.

1. **RxSwift 는 애플이 만든 라이브러리가 아닙니다.** Rx는 Swift 뿐 아니라 Java, Python 에서도 사용되는 라이브러리입니다. 반면에 **Combine 은 애플이 만든 공식 라이브러리**입니다. 
이로 인해 파생되는 특징은 다음과 같습니다.

2. RxSwift 를 사용했을 시, Rx 를 사용하는 다른 개발자들과 소통할 수 있습니다. 로직을 통일 시킬 수 있습니다.

3. Combine 을 사용했을 시, 좀 더 안정적인 효과를 기대할 수 있습니다. 어느날 갑자기 Rx 라이브러리에 문제가 생긴다면 그건 애플의 책임이 아니기 때문입니다.

> ### 💁🏻‍♂️ 1.4 : RxCocoa 를 쓸 때 장점? 어떤 경험을 했나요

1. RxCocoa 를 사용하면 **rx.tap 을 통해 버튼 탭 이벤트**를 Observable 로 간주해서 버튼 이벤트를 처리할 수 있습니다. 그리고 throttle 이나 debounce 등의 기능을 쉽게 처리할 수 있습니다.

```swift
// 푸이의 유저 검색 버튼을 눌렀을 때 이벤트 처리
searchUserButton.rx.tap
	// throttle 처리
    .throttle(.seconds(3), latest: false, scheduler: MainScheduler.instance)
    .bind(onNext: { [weak self] in
        let searchUserViewController = SearchUserViewController()
        self?.navigationController?.pushViewController(searchUserViewController, animated: true)
```

2. **UITableView 의 delegate 메서드를 RxCocoa 를 통해 깔끔하게 작성**할 수 있습니다. 예를 들어 tableView 의 willDisplay 를 rxcocoa 를 통해 tableView.rx.willDisplayCell { cell, index .. } 로 깔끔하게 작성할 수 있습니다.

```swift
// 푸이의 피드 무한스크롤
feedTableView.rx.willDisplayCell
	.subscribe(on: MainScheduler.instance)
	.bind { [weak self] cell, index in
		guard let self = self else { return }
        
        // ...
        
        // 테이블 뷰의 마지막 cell 이 다가왔을 때 새로운 피드 패치
        if index.row == currentListLen - 1 {
        	self.viewModel.fetchFeedList()
       	}
}               
```

***

## 2. Observable

> ### 💁🏻‍♂️  2.0 : RxSwift 의 Observable 이란 무엇인지 설명해주세요.

- RxSwift 의 Observable 은 데이터 스트림이며, 이벤트를 방출하는 클래스입니다. 옵저버 패턴의 Publisher 와 같다고 생각할 수 있습니다.

- Observable 을 구독하게되면, 이벤트가 발생할때마다 그에 걸맞는 행동을 할 수 있게 됩니다.

- next : 일반적인 데이터 or 이벤트 방출

- error : 에러 방출, 스트림 종료

- complete : 성공적인 이벤트 방출 + 스트림 종료

- Observable 은 Disposable 타입을 반환합니다.

- disposable -> dispose() 를 정의해둔 프로토콜


```swift
let observable = Observable<Int>.create { observer in
    observer.onNext(1)
    observer.onNext(2)
    observer.onNext(3)
    observer.onCompleted()
    return Disposables.create()
}

observable.subscribe(onNext: { value in
    print(value)
}).dispose()

```

> ### 💁🏻‍♂️ 2.1 : RxSwift에서 Hot Observable과 Cold Observable의 차이를 설명하시오.

- Hot Observable 은 이벤트가 구독여부와 상관없이 발생하고 있다가, 구독을 한 시점부터 이벤트를 받아볼 수 있는 Observable 입니다.

- Cold Observable 은 구독을 시작했을 때 비로소 이벤트를 방출하는 Observable 입니다. **RxSwift의 기본 Observable 은 Cold Observable** 입니다. 

- Hot Observable 은 TV 지상파 방송, Cold Observable 은 넷플릭스에 비유할 수 있습니다.

> ### 💁🏻‍♂️ 2.2 : Single, Completable, Maybe의 차이점에 대해 설명하고, 언제 적용하면 좋을지 설명하시오.

- 먼저 Single, Completable, Maybe 는 모두 Observable 입니다.

- Single 은 Success / Error 둘 중 하나만을 오직 한번만 방출합니다. 정확히 한 가지 요소를 한 번만 방출할 때 사용합니다. 예를 들어, 네트워크 요청을 보내고 요청에 대한 응답 한번 받아야할 경우 Single 을 사용할 수 있습니다.

```swift
func fetchData() -> Single<Data> {
    return Single.create { single in
        // ... 네트워크 요청 처리 ...
        if let data = responseData {
            single(.success(data))
        } else {
            single(.error(NetworkError.failed))
        }
        return Disposables.create()
    }
}
```

- Completable 은 데이터를 발행하지 않으며, 작업이 완료 되었음을 나타낼 때 사용합니다. 예를 들어, 서버에 데이터를 보내는 요청을 하고, "전송 완료" or "전송 실패"를 출력하고 싶을 경우 사용할 수 있습니다.

```swift
func sendData() -> Completable {
    return Completable.create { completable in
        // ... 데이터 전송 처리 ...
        if success {
            completable(.completed)
        } else {
            completable(.error(NetworkError.failed))
        }
        return Disposables.create()
    }
}

```

- Maybe 는 데이터를 발행할수도 있고 하지 않을 수도 있습니다. 주로 "데이터가 있는지 여부"를 반환하는 작업에 사용합니다. 예를 들어 로컬 스토리지에서 데이터를 읽는 작업을 수행하고 데이터가 있으면 그 값을 반환, 그렇지 않으면 completed 를 사용합니다.

```swift
func readData() -> Maybe<Data> {
    return Maybe.create { maybe in
        // ... 데이터 읽기 처리 ...
        if let data = readData {
            maybe(.success(data))
        } else {
            maybe(.completed)
        }
        return Disposables.create()
    }
}

```

***

## 3.Disposable

> ### 💁🏻‍♂️ 3.1 : Disposable 의 개념을 설명해주세요

- RxSwift 의 Disposable 은 **Observable 의 구독을 해제**하는 데 사용되는 개념입니다. 

- Observable 은 subscribe() 메서드를 통해서 구독을 시작하고, 이 **subscribe 메서드는 Disposable 객체를 반환**합니다.

- Disposable.dispose() 를 하면 구독이 해제됩니다.

```swift
let disposable = observable.subscribe(onNext: { value in
    print("Received value: \(value)")
})

disposable.dispose() // 구독 취소

```
- 예를 들어 Observable 이 UI 이벤트를 발행하는데, 유저가 그 화면에서 나간다면 더 이상 그 Observable 은 필요가 없어질 것이기 때문에 dispose() 해줘야 합니다.

> ### 💁🏻‍♂️ 3.2 : DisposeBag 에 대해서 설명해주세요. 언제 메모리를 해제하나요?

- DisposeBag 을 사용하면, **구독을 해제할 대상들을 담아 한 번에 dispose 처리**할 수 있습니다.

- **DisposeBag 이 Dispose() 메서드를 호출하거나, DisposeBag 이 메모리에서 해제될 때, DisposeBag 에 포함된 Disposable 객체들이 함께 해제**됩니다.

- 일반적으로 **ViewController 나 ViewModel 에서 DisposeBag 을 사용**하게 되는데, 해당 객체가 메모리에서 해제될 때 DisposeBag 도 함께 메모리에서 해제되므로 **메모리 누수를 방지**합니다.

***

## 4. Subject, Driver, Relay

> ### 💁🏻‍♂️ 4.1 : Observable 과 Subject 의 차이를 설명해주세요

- **Observable 은 Cold Observable** 이고, **Subject 는 Hot Observable** 입니다.

- **Subject 는 Observer 의 역할과 Observable 의 역할을 모두** 할 수 있습니다.

- 즉, **Observable 이기 때문에 이벤트를 발행**할 수 있고,

- O**bserver 이기 때문에 이벤트를 수신 받고, 그 이벤트에 맞는 처리를 할 수 있습니다.**

- 다시말하면, **Observer 는 어떤 데이터를 보낼 지 미리 정해진 형태의 스트림**이지만, **Subject 는 Subject 외부에서 새로 데이터를 넣어줄 수도 있고, 구독도 할 수 있는 유연성**을 가지고 있습니다.

```swift
// 예시 코드 1 : Observable 과 Subject 의 동작 차이

// Observable
let observable = Observable<Int>.create { observer in
    observer.onNext(1)
    observer.onNext(2)
    observer.onNext(3)
    observer.onCompleted()
    return Disposables.create()
}

observable.subscribe(onNext: { value in
    print(value)
})

// Subject
let subject = PublishSubject<Int>()

subject.onNext(1)
subject.onNext(2)
subject.onNext(3)

subject.subscribe(onNext: { value in
    print(value)
})

```

```swift
// 예시 코드 2 : 왜 Subject 가 Observer 와 Observable 의 역할을 모두 한다고 하는가.
import RxSwift

// Observable 로서 "initial value" 방출
let subject = BehaviorSubject(value: "initial value")

// 새로운 값을 수동으로 삽입 -> Observer 로서 "new value" 라는 값을 수신했음
subject.onNext("new value")

subject.subscribe(onNext: { value in
    print("Received value: \(value)")
})

// 새로운 값을 수동으로 삽입
subject.onNext("another value")

/* 출력 결과
Received value: new value
Received value: another value
*/

```

- https://jellysong.tistory.com/110

> ### 💁🏻‍♂️ 4.2 : Subject의 종류와 차이점에 대해 설명하시오.

- **PublishSubject** 와 **BehaviorSubject** 에 대해 설명해보겠습니다.

- 가장 큰 차이는 **초기값이 있느냐 없느냐** 인데요, PublishSubject 의 경우, 새로운 구독자가 생겼을 때 , 최근에 발생했던 이벤트의 값을 전달하지 않습니다. 반면에 BehaviorSubject 는 새로운 구독자가 생겼을 때, 최근에 발생했던 이벤트의 값을 전달합니다.

- https://velog.io/@heyksw/RxSwift-BehaviorRelay-BehaviorSubject

> ### 💁🏻‍♂️ 4.3 : Subject와 Driver의 차이를 설명하시오.

- **Subject 와 Driver 는 모두 RxCocoa 의 Observable 이지만, 몇가지 차이점**이 있습니다.

- Subject 는 Observer 와 Observable 의 역할을 모두 수행할 수 있는 클래스입니다. 

- Observable 처럼 이벤트를 방출할 수 있지만, 언제든지 수동으로 이벤트를 새로 수신 받아 생성할 수 있기 때문에 유연합니다.

- **Driver 는 UI 이벤트를 처리하기 위해 설계된 Observable** 입니다. UI 이벤트는 메인 스레드에서 작동해야 하므로, **Driver 는 자동으로 MainScheduler 에서 작동**됩니다. 

- 또한 **Driver 는 error 이벤트를 방출하지 않으며, 구독이 해제되기 전까지 항상 메모리에 존재**합니다.

> ### 💁🏻‍♂️ 4.4 : Subject 와 Relay 의 차이를 설명해주세요.

- **공통점으로는 둘 다 RxCocoa 에 포함되었고, Observer 의 역할과 Observable 의 역할을 수행**할 수 있다는 것입니다.

- 즉, 이벤트의 발행도 할 수 있고, 이를 구독해 이벤트에 걸맞는 행동 처리를 할 수 있습니다.

- **차이점 첫번째로는, Subject 는 RxSwift 의 클래스이고, Relay 는 RxCocoa 의 클래스라는 것**입니다.

- 두번째로는, **Subject 는 completed 나 error 를 통해서 스트림을 종료시키지만, Relay 는 Dispose 가 되기 전까지 계속 동작합니다.**

- 세번째로는, **Subject 의 데이터 스트림 값 출력은 구독을 통해 가능하고, Relay 의 데이터 스트림 값 출력은 .value 를 통해서 가능**합니다.

- https://velog.io/@heyksw/RxSwift-BehaviorRelay-BehaviorSubject

***

> ### 💁🏻‍♂️ 4.5 : Driver 와 Relay 의 차이를 설명해주세요.

- **Driver 는 MainScheduler 에서 행동하는 것을 보장하고, Relay 는 그렇지 않습니다.** 

- 따라서 UI 이벤트를 처리할때 Driver 를 사용하는 것이 좀 더 적절하고, Relay 는 비 UI 이벤트에서도 간단하게 처리할 때 사용하기 좋습니다.

***
# ref
https://github.com/JeaSungLEE/iOSInterviewquestions
https://caution-dev.github.io/swift/2019/03/16/iOS-Q&A.html
https://github.com/jeonyeohun/Getting-Ready-For-Interview/blob/main/iOS-Swift/swift.md
