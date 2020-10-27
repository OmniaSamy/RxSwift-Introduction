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
