## TODO:

* none

## QUESTION:

* Should we force the weak references in blocks ?

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
