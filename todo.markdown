## TODO:

* Swift 2.0 + Protocol extension: http://www.appcoda.com/swift-2-introduction/
* Swift 2.0 + Defer
* Swift 2.0 + Try/Catch

* Should we force the protocol implementations inside extensions ?

## QUESTION:

* Should we force the weak references in blocks ?
* Find/Specify another prefix for unwrapped values inside `if let`.

https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/WorkingwithBlocks/WorkingwithBlocks.html

```swift
self.performBlockAfterDelay(3.0, block: { [weak self] in 
	self?.runHintAnimation()
})
```

## Sample markdown:

**Preferred:**
```swift
```

**Not Preferred:**
```swift
```
