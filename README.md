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
