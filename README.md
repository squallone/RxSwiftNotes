These a compilation of notes about RxSwift

## Why RxSwift
* Declarative
* Functional and reactive
* Consistent patterns and operators
* Handle mutable state
* Compositional
* Decoupled
* Multi-platform

## Why Not RxSwift?
* Steep learning curve
* Dependencies
* Adoption
* More responsibilities
* Not a panacea

## RxSwift patterns
- Observer
- Iterator

### Observable <Type>
- Emits an event containing an element
- Subscribers can react to each event emitted
  
 Here is a diagram of an observable of integers but these could also be whatever type like taps or gestures
  
![Image](https://www.mediafire.com/convkey/b05a/kjve28n42xkicqyzg.jpg)

The vertical bar represent the end of the line for that observable, it is termiated at that point, it cannot emmit anymore elements 
  
### Life cycle of an Observable

- Next
 — Elements, e.g, integers or taps
- Element
 — Observable is terminated
- Completed
 — Observable is terminated

An Observable can emit an next event containing an element and when the observable is done it emits a completed event

![Life cycle](https://www.mediafire.com/convkey/2697/xls147p946xvpf7zg.jpg)

This is consider a normal termination but sometimes when can get a failure termination, when that happens an observable will emit an error event, this also terminates the observable.

![Life cycle error](https://www.mediafire.com/convkey/461e/83dg2bdj03f9mh9zg.jpg)

An observable can only emit one error event and it's done and can no longer emit anymore events

#### Observable events
```
public enum Event<Element> {
  // Next element is produced
  case next(Element)
  // Secuence terminated with an error
  case error(Swift.Error)
  // Secuence completed successfully
  case completed
}
```

## Subjects

Following the definition from [ReactiveX](http://reactivex.io/)


> A Subject is a sort of bridge or proxy that is available in some implementations of ReactiveX that acts both as an observer and as an Observable. Because it is an observer, it can subscribe to one or more Observables, and because it is an Observable, it can pass through the items it observes by reemitting them, and it can also emit new items.


What this means is that subjects can accept subscriptions and emit events (like a typical Observable), as well as add new elements into the sequence.

### Subject Types

There are four subject types:
- `PublishSubject`
- `BehaviorSubject`
- `Variable`
- `ReplaySubject`

### Publish Subject

PublishSubject emits only new items to its subscriber; every item added to the subject before the subscription will be not emitted.

Example

```
enum MyError: Error {
    case error1
}

let disposeBag = DisposeBag()
            
let pubSubj = PublishSubject<String>()
            
pubSubj.on(.next("(next 1")) //event emitted to no subscribers
            
pubSubj.subscribe({ //subscriber added, but no replay of "next 1"
                print("line: \(#line),", "event: \($0)")
            })
.disposed(by: disposeBag)
            
pubSubj.on(.next("(next 2")) //event emitted and received by subscriber
pubSubj.onError(MyError.error1) //emits error and terminates sequence
            
pubSubj.on(.next("next 3")) //pubSubj cannot emit this event
/* prints: 
line: 8, event: next((next 2)
line: 8, event: error(error1) 
```

## Dispose

To cancel production of sequence elements and free resources immediately, call dispose on the returned subscription.

If a sequence terminates in finite time, not calling dispose or not using `disposed(by: disposeBag)` won't cause any permanent resource leaks. However, those resources will be used until the sequence completes, either by finishing production of elements or returning an error.

If a sequence does not terminate on its own, such as with a series of button taps, resources will be allocated permanently unless dispose is called manually, automatically inside of a disposeBag, with the `takeUntil` operator, or in some other way.

Using dispose bags or takeUntil operator is a robust way of making sure resources are cleaned up. We recommend using them in production even if the sequences will terminate in finite time.
