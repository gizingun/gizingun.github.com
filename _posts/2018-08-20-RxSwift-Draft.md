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

