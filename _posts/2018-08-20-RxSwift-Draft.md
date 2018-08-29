---
layout: post
title: Getting Started with RxSwift
description: "RxSwift 시작하기"
tags: [ios, swift, RxSwift]
---

- RxSwift is a library for composing asynchronous and event-based code by using observable sequences and functional style operators, allowing for parameterized execution via schedulers.
- RxSwift, in its essence, simplifies developing asynchronous programs by allowing your code to react to new data and process it in a sequential, isolated manner. (more clear?)


#### Introduction to asynchronous programming

![Asynchronous programming]({{ site.baseUrl }}/assets/img/rxswift_asynchronous.png)


- All the different bits of your program don’t block each other’s execution.
- Writing code that truly runs in parallel, however, is rather complex, especially when different bits of code need to work with the same pieces of data.


#### Cocoa and UIKit Asynchronous APIs

- Apple has provided lots of APIs in the iOS SDK that help you write asynchronous code

> NotificationCenter,  **delegate** pattern,  Grand CEntral Dispatch,  Closure

- Since most of your typical classes would do something asynchronously, and all UI components are inherently asynchronous (대부분의 UI 코드는 사실상 모두 비동기)
- It’s impossible to make assumptions in what order the entirety of your app code will get executed (사실상 모든 코드는 비동기 이기 떄문에 전체 코드의 실행순서를 정확히 가정하는 것은 불가능[os상태, 네트워크 상태 등의 외부적인 요인도 영향을 끼침])

![Complex Asynchronous code]({{ site.baseUrl }}/assets/img/rxswift_complex_asynchronous.png)

#### Synchronous code

~~~ruby
var array = [1, 2, 3]
for number in array {
  print(number)
  array = [4, 5, 6]
}
print(array)
~~~

#### Asynchronous code

~~~ruby
var array = [1, 2, 3]
var currentIndex = 0
//this method is connected in IB to a button
@IBAction func printNext(_ sender: Any) {
  print(array[currentIndex])
  if currentIndex != array.count-1 {
    currentIndex += 1
  }
}
~~~

#### Asynchronous programming glossary

- Some of the language in RxSwift is so tightly bound to asynchronous, reactive, and/or functional programming that it will be easier if you first understand the following foundational terms

1. State, and sprcifically, shared mutable state (상태)

2. Imperative programming (명령형 프로그래밍)

3. Side effects (부작용의 뉘앙스는 아님)
State & Imperative programming

4. Decalarative code (변화의 폭을 개발자가 컨트롤 하는 코드?)
RxSwift combines some of the best aspects of imperative code and functional code.
Declarative code lets you define pieces of behavior, and RxSwift will run these behaviors any time there’s a relevant event and provide them an immutable, isolated data input to work with.

5. Reactive systems
- Responsive
- Resilient (회복성?)
: 오류에 대한 회복성?
- Elastic
: 다양한 일을 해야함?
- Message driven


#### Foundation of RxSwift

##### Observables

The Observable<T> class provides the foundation of Rx code: the ability to asynchronously produce a sequence of events that can “carry” an immutable snapshot of data T

An *Observable* can emit (and observers can receive) only three types of events
1. A *next* event
2. A *completed* event
3. An *error* event

###### Finite observerable sequences
> download files
~~~ruby
API.download(file: "http://www...")
  .subscribe(onNext: { data in
    ... append data to temporary file
  },
  onError: { error in
    ... display error to user
  },
  onCompleted: {
    ... use downloaded file
  })
~~~

###### Infinite observable sequences
 
 For example UIEvents, Notification Center, etc.
~~~ruby
UIDevice.rx.orientation
  .subscribe(onNext: { current in
    switch current {
      case .landscape:
        ... re-arrange UI for landscape
      case .portrait:
        ... re-arrange UI for portrait
    }
})
~~~


##### Operators

ObservableType and the implementation of Observable class include plenty of methods that abstract discrete pieces of asynchronous work, which can be composed together to implement more complex logic. (Highly decoupled and composable)

~~~ruby
UIDevice.rx.orientation
  .filter { value in
    return value != .landscape
  }
.map { _ in
    return "Portrait is the best!"
  }
  .subscribe(onNext: { string in
    showAlert(text: string)
  })
~~~

![Operator]({{ site.baseUrl }}/assets/img/rxswift_operators.png)

Decoupled이고 composable하기 떄문에 연결해서 사용하면 더 많은 걸 구현할 수 있음

##### Schedulers

Schedulers are the Rx Equivalent of dispatch queues.
Event의 발생을 serial한 queue 혹은 concurrent한 queue에서 관찰할 수 있도록 할 수 있음

![Schedulers]({{ site.baseUrl }}/assets/img/rxswift_scheduler.png)

##### App architecture

RxSwift doesn’t alter your app’s architecture in any way.
It’s important to note that you definitely do not have to start a project from scratch to make it a reactive app.

MVVM architecture

![MVVM]({{ site.baseUrl }}/assets/img/rxswift_mvvm.png)

##### RxCocoa

RxSwift is the implementation of the common Rx API.
RxCocoa is RxSwift’s companion library holding all classes that specifically aid development for UIKit and Cocoa.

### Observable

#### What is an observable

"observable", "observable sequence", "sequence", "stream"

#### Lifecycle of an observable

* next
* completed (Once an observable is terminated, it can no longer emit events)
* error

#### Creating observables

* just
* of
* from

~~~ruby
    let observable: Observable<Int> = Observable<Int>.just(one)
    let observable2 = Observable.of(one, two, three)
    let observable3 = Observable.of([one, two, three])
    let observable4 = Observable.from([one, two, three])
~~~

#### Subscribing to observables

Observable doesn’t do anything until it receives a subscription

~~~ruby
    let one = 1
    let two = 2
    let three = 3
    let observable = Observable.of(one, two, three)
    observable.subscribe { event in
        print(event)
    }
~~~


~~~ruby
    observable.subscribe { event in
      if let element = event.element {
        print(element)
      }
    }
~~~

~~~ruby
    observable.subscribe(onNext: { element in
      print(element)
    })
~~~


observable sequence with zero elements

~~~ruby
    elementsxample(of: "empty") {
      let observable = Observable<Void>.empty()
    }
~~~

#### Disposing and terminating

dispose - Observable의 해제

~~~ruby
    let observable = Observable.of("A", "B", "C")
    // 2
    let subscription = observable.subscribe { event in
        // 3
        print(event)
    }
    subscription.dispose()
~~~

disposebag

If you forget to add a subscription to a dispose bag, or manually call dispose on it when you’re done with the subscription, or in some other way cause the observable to terminate at some point, you will probably leak memory

create

~~~ruby
    enum MyError: Error {
        case anError
    }

    let disposeBag = DisposeBag()
    
    Observable<String>.create { observer in
        // 1
        observer.onNext("1")
//        observer.onError(MyError.anError)
        
        // 2
//        observer.onCompleted()
        
        // 3
        observer.onNext("?")
        
        // 4
        return Disposables.create()
        }
        .subscribe(
            onNext: { print($0) },
            onError: { print($0) },
            onCompleted: { print("Completed") },
            onDisposed: { print("Disposed") }
        )
        .disposed(by: disposeBag)
~~~

Memory leak

~~~ruby
    enum MyError: Error {
        case anError
    }
    let disposeBag = DisposeBag()
    Observable<String>.create { observer in
        // 1
        observer.onNext("1")
        //    observer.onError(MyError.anError)
        // 2
        //    observer.onCompleted()
        // 3
        observer.onNext("?")
        // 4
        return Disposables.create()
        }
        .subscribe(
            onNext: { print($0) },
            onError: { print($0) },
            onCompleted: { print("Completed") },
            onDisposed: { print("Disposed") }
    ).disposed(by: disposeBag)
~~~

#### Creating ovservable factories

~~~ruby
    let disposeBag = DisposeBag()
    
    // 1
    var flip = false
    
    // 2
    let factory: Observable<Int> = Observable.deferred {
        
        // 3
        flip = !flip
        
        // 4
        if flip {
            return Observable.of(1, 2, 3)
        } else {
            return Observable.of(4, 5, 6)
        }
    }
    
    for _ in 0...3 {
        factory.subscribe(onNext: {
            print($0, terminator: "")
        })
            .disposed(by: disposeBag)
        
        print()
    }
~~~

Externally, an observable factory is indistinguishable from a regular observable

#### Using Traits

Traits are observables with a narrower set of behaviors than regular observables.

"Single, Maybe, Completable"

Single - .next / .completed

Completable - .next / .completed

Maybe -  Single + Completable

~~~ruby
  let disposeBag = DisposeBag()
    
    // 1
    var flip = false
    
    // 2
    let factory: Observable<Int> = Observable.deferred {
        
        // 3
        flip = !flip
        
        // 4
        if flip {
            return Observable.of(1, 2, 3)
        } else {
            return Observable.of(4, 5, 6)
        }
    }
    
    for _ in 0...3 {
        factory.subscribe(onNext: {
            print($0, terminator: "")
        })
            .disposed(by: disposeBag)
        
        print()
    }
~~~

##### Subject

PublishSubject only emits to current subscribers

~~~ruby
let subject = PublishSubject<String>()
  subject.onNext("Is anyone listening?")
  
  let subscriptionOne = subject
    .subscribe(onNext: { string in
      print(string)
    })
  
  subject.on(.next("1"))
~~~

Subjects act as both an observable and an observer

* PublishSubject - start empty and only emits new elements to subscribers
* BehaviorSubject - Start with an initial value and replays it or the latest element to  new subscribers
* ReplaySubject - Initialized with a buffer size and will maintain a buffer of elements up to that size and replay it to new subscribers
* Variable - Wraps a BehaviorSubject, preserves its current value as state, and replays only the latest/initial value to new subscribers 

![Publish Subject]({{ site.baseUrl }}/assets/img/rxswift_publishSubject.png)

![Behavior Subject]({{ site.baseUrl }}/assets/img/rxswift_behaviorSubject.png)

Behavior subjects are useful when you want to pre-populate a view with the most recent data

rxswitf_replaySubject

![Replay Subject]({{ site.baseUrl }}/assets/img/rxswitf_replaySubject.png)

Keep in mind when using a replay subject that this buffer is held in memory

##### Variable

Variable wraps a BehaviorSubject and stores its current value as state

You can access that current value via its value property

In order to access a variable’s underlying behavior subject, you call asObservable() on it