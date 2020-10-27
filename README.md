# RxSwift-Introduction

 ## Introducation
 Wouldn’t it be better if you could set things up so the code updates reflect changes automatically? That’s the idea of reactive programming: Your application can react to changes in the underlying data without you telling it to do so. This makes it easier to focus on the logic at hand rather than maintaining a particular state.
You can achieve this in Swift through Key-Value Observation and using didSet, but it can be cumbersome to set up. Alternatively, We can use RxSwift that facilitate reactive programming.

 ## Observables and Observers
* An Observable emits notifications of change.
* An Observer subscribes to an Observable and gets notified when that Observable has changed.

You can have multiple Observers listening to an Observable. When the Observable changes, it will notify all its Observers.

## DisposeBag
* The DisposeBag is an additional tool RxSwift provides to help deal with ARC and memory management.
* When deinit() is called on the object that holds the DisposeBag, each disposable Observer is automatically unsubscribed from what it was observing. This allows ARC to take back memory as it normally would.
* Without a DisposeBag, you’d get one of two results. Either the Observer would create a retain cycle, hanging on to what it’s observing indefinitely, or it could be deallocated, causing a crash.

## Subjects
* Subjects act as both an observable and an observer. They can receive events and also be subscribed to.
* There are 4 subject types in RxSwift:
  - **PublishSubject:** Starts empty and only emits new elements to subscribers.
  - **BehaviorSubject:** Starts with an initial value and replays it or the latest element to new subscribers.
  - **ReplaySubject:** Initialized with a buffer size and will maintain a buffer of elements up to that size and replay it to new subscribers.
  - **BehaviorRelay (Variable):** Wraps a BehaviorSubject, preserves its current value as state, and replays only the latest/initial value to new subscribers.
  
 ## Example
 * PublishSubject
   ```swift
   let disposeBag = DisposeBag()
   let pubSubject = PublishSubject<String>()
   pubSubject.onNext("Ignored...")
  
   pubSubject.subscribe(onNext: { text in
       print(text)
    }).disposed(by: disposeBag)
    
   pubSubject.onNext("Printed!")
   ```
   In the result just show Printed!
   
 * BehaviorSubjects
   ```swift
   let disposeBag = DisposeBag()
   let subject = BehaviorSubject<String>(value: "Initial value")
  
   subject.onNext("Not printed!")
   subject.onNext("Not printed!")
   subject.onNext("Printed!")
  
   subject.subscribe(onNext: { text in
       print(text)
    }).disposed(by: disposeBag)
  
   subject.onNext("Printed one!")
   subject.onNext("Printed two!")
   ```
   In the result show <br />
   Printed! <br />
   Printed one! <br />
   Printed two!
   
 * ReplaySubjects
   ```swift
   let disposeBag = DisposeBag()
   let subject = ReplaySubject<String>.create(bufferSize: 3)
   subject.onNext("Not printed!")
   subject.onNext("Printed!")
   subject.onNext("Printed!")
   subject.onNext("Printed!")
  
   subject.subscribe(onNext: { text in
       print(text)
    }).disposed(by: disposeBag)
  
   subject.onNext("Printed end!")
   ```  
   In the result show <br />
   Printed! <br />
   Printed! <br />
   Printed! <br />
   Printed end! <br />
   
 * BehaviorRelay (Variable)
   ```swift
   import RxCocoa
   
   let disposeBag = DisposeBag()
   let behRelay = BehaviorRelay(value: "Current String")
        
    /// Getting the value
    print(behRelay.value)
        
    /// Setting the value
    behRelay.accept("Second String")
        
    /// Observing the value
    behRelay.asObservable().subscribe(onNext: { text in
            print(text)
      }).disposed(by: disposeBag)
        
    behRelay.accept("Third String")
   ```  
   In the result show <br />

   Current String <br />
   Second String <br />
   Third String <br />
   
   
 ## Example subjects with handel response
 
