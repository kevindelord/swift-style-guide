## TODO:

* Swift 2.0: Error Handling
* Swift 2.0: Do/Try/Catch

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
