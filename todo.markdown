## TODO:

* Find/Specify another prefix for unwrapped values inside `if let`.
* Swift 2.0 + Protocol extension: http://www.appcoda.com/swift-2-introduction/
* Swift 2.0 + Defer
* Swift 2.0 + Guard
* Swift 2.0 + Try/Catch

* didSet:
- When you change one value, just this value should be changed; to keep the context of your own interaction consistent. (#1)
- When updating one value, another one gets reloaded under the hood. (#2) ( current case: an array updates a table view)
- This developer might not be aware of this and do a second useless reload (#3).

## QUESTION:

* Should we force the protocol implementations inside extensions ?
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
