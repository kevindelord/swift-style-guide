# Swift Style Guide

This guide is a fork from the [Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide).
It has then be modified and improved to match different styles and add some missing parts.

This style guide is different from othersn nonetheless the goals are the same: define a style, syntax and structure while coding in Swift.
Of course, efficacity, readability, and simplicity are the most important points.

## Table of Contents

* [Naming](#naming)
  * [Structures](#structures)
  * [Enumerations](#enumerations)
    * [Functions](#functions)
    * [Switch Cases](#switch-cases)
  * [Class Prefixes](#class-prefixes)
* [Spacing](#spacing)
* [Magic Numbers](#magic-numbers)
* [Rounded Brackets](#rounded-brackets)
* [Ternary operator](#ternary-operator)
* [Comments](#comments)
* [Code alignement and structure](#code-alignement-and-structure)
* [Classes and Structures](#classes-and-structures)
  * [Use of Self](#use-of-self)
  * [Protocol Conformance](#protocol-conformance)
  * [Computed Properties](#computed-properties)
* [Function Declarations](#function-declarations)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
  * [Constants](#constants)
  * [Optionals](#optionals)
  * [Struct Initializers](#struct-initializers)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Control Flow](#control-flow)
* [Semicolons](#semicolons)
* [Language](#language)
* [Credits](#credits)


## Naming

Use descriptive names with camel case for classes, methods, variables, etc. Class names should be capitalized, while method names and variables should start with a lower case letter.

**Preferred:**

```swift
class WidgetContainer {
    var widgetButton: UIButton
    let widgetHeightPercentage = 0.85
}
```

**Not Preferred:**

```swift
class app_widgetContainer {
    var wBut: UIButton
    let WHeightPCT = 0.85
}
```

For functions and init methods, prefer named parameters for all arguments unless the context is very clear. Include external parameter names if it makes function calls more readable.

```swift
func dateFromString(dateString: String) -> NSDate
func convertPointAt(#column: Int, #row: Int) -> CGPoint
func timedAction(#delay: NSTimeInterval, perform action: SKAction) -> SKAction!

// would be called like this:
dateFromString("2014-03-14")
convertPointAt(column: 42, row: 13)
timedAction(delay: 1.0, perform: someOtherAction)
```

For methods, follow the standard Apple convention of referring to the first parameter in the method name:

```swift
class Guideline {
    func combineWithString(incoming: String, options: Dictionary?) { ... }
    func upvoteBy(amount: Int) { ... }
}
```
### Structures

Use UpperCamelCase for static values within structures:

```swift
struct Duration {
    static let FadeOut      = 0.3
    static let FadeIn       = 0.8

    var someVariable        : AnyObject?
}
```

### Enumerations

Use UpperCamelCase for enumeration values:

```swift
enum Shape {
    case Rectangle
    case Square
    case Triangle
    case Circle
}
```

Use a type to your enum only and only if you need the `rawValue` in your code.
Without this, prefer simple enumeration without type.

```swift
enum Device : Int {
    case iPhone = 0
    case iPad
    case iPod
}
```

#### Functions

Use an enum instead of a struct when you need to iterate through all elements or if you need a variable type.

Enumerations and structures became amazing in Swift as they can now have (static) functions.

This is extremely usefull when you need data or logic depending on an enum value, for example a title of a segue.

```swift
enum Device : Int {
    case iPhone = 0
    case iPad
    case iPod

    func title() -> String {
        switch self {
        case .iPhone: return L("device.iPhone")
        case .iPad: return L("device.iPad")
        case .iPod: return L("device.iPod")
        }
    }

    static func titleForId(id: Int) -> String? {
        return Device(rawValue: id)?.title()
    }
}
```

#### Switch Cases

One other very handy thing Xcode does is to warn the developer when a case is missing inside a `switch` when iterating through an enum.
It does warn you only if you do not set any `default` case.

It can be annoying to write multiple times the same case. But, next time a developer adds a case to the enum, Xcode will tell him where he might misses something.

**Preferred:**
```swift
static func canCall(device: Device) -> Bool {
    switch device {
    case .iPhone:       return true
    case .iPod, .iPad:  return false
    }
}
```

**Not Preferred:**
```swift
static func canCall(device: Device) -> Bool {
    switch device {
    case .iPhone: return true
    default: return false
    }
}
```

### Class Prefixes

Swift types are automatically namespaced by the module that contains them and you should not add a class prefix. If two names from different modules collide you can disambiguate by prefixing the type name with the module name.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```


## Spacing

* Indent using tabs equivalent to 4 spaces, and indent by inserting tab characters. Be sure to set xcode like this:

  ![Xcode indent settings](screens/indentation.png)

* Also think about automatically trim the whitespaces:

  ![Xcode trim whitespaces](screens/trim-whitespaces.png)

* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on the same line as the statement but close on a new line.
* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.

**Preferred:**
```swift
if (user.isHappy == true) {
    // Do something
} else {
    // Do something else
}
```

**Not Preferred:**
```swift
if user.isHappy
{
  // Do something
}
else {
  // Do something else
}
```

* There should be exactly one blank line between methods to aid in visual clarity and organization. Whitespace within methods should separate functionality, but having too many sections in a method often means you should **refactor into several methods**.

**Preferred:**
```swift
func firstMethod() {
    // Explanation
    if (a == b) {
        // Do something
    }
}

func secondMethod() {
    // Do something
}
```

**Not Preferred:**
```swift
func firstMethod() {
  if (a == b) { /* Do something */ }
}
func secondMethod() {
  // Do something
}
```

## Magic Numbers

No magic numbers in any case, by any chance. It has to be or dynamic from an outlet, or calculated.

If, somehow, it is absolutely NOT possible, then create a constant in the constants file inside a structure.

The following example show how to create such structures. 
The more your constants are *structured* the better.

PS: Note how everything is aligned.

**Preferred:**
```swift
// Constants.swift file

// MARK: - Games

struct GameConstants {

    // Informative comment about the type of constant
    static let ScrambleAnimationDelay   = 0.6

    // Animation Duration for pop up in [...]
    struct AnimationDuration {
        static let FadeOut              = 0.3
        static let FadeIn               = 0.5
    }

    // Top margin for view XY. Can't be dynamic because of [...]
    static let TopViewLeftMargin        = 20
}

// ViewController.swift file

UIView.animateWithDuration(GameConstants.AnimationDuration.FadeIn) {
    self.myView.alpha = 0
}
```

**Not Preferred:**
```swift
// ViewController.swift file

UIView.animateWithDuration(0.5, animations: {
    self.myView.alpha = 0
})
```

## Rounded Brackets

Here we go with another restrictive rule: the rounded brackets. For historical reason and for a better readability a developer should use rounded brackets mostly everywhere.
It keeps the code more understandable and prevent easy mistakes.

They should be used when comparing values, ternary operators, if else, while, ?? (swift), where (swift) and returning more than one single value.
Here is a small example in Swift showing all cases:

**Preferred:**
```swift
Int i = 0
var message : String? = nil
BOOL check = (true == false)  // comparison
 
while (check == true) {
    if (i >= 10) {  // if
        check = false
    } else if (i == 2) {
        i++
    }
    message = (check == true ? "valid" : "invalid") // ternary
    if let msg = message as? String where (i > 7) { // where
        println(message)
    }
    i++
}
return (message ?? "message does not exist") // ??
```

## Ternary operator

The ternary operator is amazing handful and pretty, but it can also be badly used and gives headaches to any developers.

A developer should only use it with just one level of operation and with rounded brackets.
Without those rules, a developer will code this:

**Not Preferred:**
```swift
value = a == b ? b != c ? 4 : d == e ? 6 : 1 : 0
```

This is horrible to read, debug and understand.

## Comments

A beautiful code is also a documented code. You should always try to explain and document the logic your are implementing.

Even if for you it looks simple, and it surely does "now", but it won't in 5 months for another developer.

Comments above the functions are, of course, well appreciated to explain what they are doing, the purpose and general informations about them.

But inline comments are also very used to describe step-by-step what is going on inside the function.

```swift
/**
 * Function to calculate top margin for the current view depending on [...]
 */
func calculateTopMargin() -> CGFloat {

    // Get the top x origin
    let separatorFrame = self.separator?.frame.x

    // Calculate position depending on [...]
    let finalPosition = (separatorFrame * 2) + self.defaultGap()

    // Apply frame
    self.popUpView?.frame = CGRectMake(...)
}
```

For more information about the documenting your code on Swift, please read this [blog post](http://nshipster.com/swift-documentation/) on NSHipster.

## Code alignement and structure

* The pragma mark should be use as much as possible to actually separate and structure your classes and code within different files.

The pragma mark should be indented, capitalised and using a separator. It should describe what the next methods are about.

**Preferred:**
```swift
class MyViewController : UIViewController {

    // MARK: - Actions

    func superMethod() {
    }
}
```

**Not Preferred:**
```swift
class MyViewController : UIViewController {
// MARK actions
    func superMethod() {
    }
}
```

* Make sure the code is aligned to itself. This is just about structure and better looking code: for example the `=` character, the start of the lines, the function names, etc. The alignment is done with "tabulations".
It is also extremely appreciated to use comments to separate the class attributes and pragma mark between functions.

**Preferred:**
```swift
class MyViewController                      : UIViewController {

    // MARK: - Outlets

    @IBOutlet var lettersGameButton         : UIButton?
    @IBOutlet var headBodyLegsGameButton    : UIButton?
    @IBOutlet var colorMatcherGameButton    : UIButton?

    // MARK: - Instance Variables

    var destinationType                     : FFGameType?
    let transitionManager                   = FFTransitionManager()

    // MARK: - View Lifecycle

    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)
        // Your code here.
    }

    // MARK: - Actions

    @IBAction func buttonPressed(sender: UIButton) {
        // Your code here.
    }
}
```

**Not Preferred:**
```swift
class MyViewController : UIViewController {

// MARK Outlets

    @IBOutlet var lettersGameButton : UIButton?
    @IBOutlet var headBodyLegsGameButton : UIButton?
    @IBOutlet var colorMatcherGameButton : UIButton?
    var destinationType : FFGameType?
    let transitionManager = FFTransitionManager()

// MARK View Lifecycle

    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)
        // Your code here.
    }
    @IBAction func buttonPressed(sender: UIButton) {
        // Your code here.
    }
}
```


## Classes and Structures

### Which one to use?

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Sometimes, things should be structs but need to conform to `AnyObject` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

### Example definition

Here's an example of a well-styled class definition:

```swift
class circle      : Shape {

    var x         : Int
    var y         : Int
    var radius    : Double

    var diameter  : Double {
        get {
            return (radius * 2)
        }
        set {
            radius = (newValue * 0.5)
        }
    }

    init(x: Int, y: Int, radius: Double) {
        self.x = x
        self.y = y
        self.radius = radius
    }

    convenience init(x: Int, y: Int, diameter: Double) {
        self.init(x: x, y: y, radius: diameter / 2)
    }

    func describe() -> String {
        return "I am a circle at \(centerString()) with an area of \(computeArea())"
    }

    override func computeArea() -> Double {
        return (M_PI * radius * radius)
    }

    private func centerString() -> String {
        return "(\(x),\(y))"
    }
}
```

The example above demonstrates the following style guidelines:

 + Specify types for properties, variables, constants, argument declarations and other statements with a space after the colon but not before, e.g. `x: Int`, and `Circle: Shape`.
 + Align the multiple variables and structures declaration.
 + Indent getter and setter definitions and property observers.
 + Separate variable declarations with getter and setter.
 + Note the rounded brackets.


### Use of Self

For conciseness, avoid using `self` since Swift does not require it to access an object's properties or invoke its methods.

Use `self` when required to differentiate between property names and arguments in initializers, and when referencing properties in closure expressions (as required by the compiler):

```swift
class BoardLocation {
    let row: Int, column: Int

    init(row: Int, column: Int) {
        self.row = row
        self.column = column

        let closure = {
            println(self.row)
        }
    }
}
```

### Protocol Conformance


In Swift, protocols can be used, but it's better appreciated to use blocks.
In the end they are the very same thing: a pointer to a function owned by an object (a specific object or self).


The blocks are better as they are easier to read, easier to implement and to understand.
Moreover they are better integrated in the language as they are in Obj-C.
It is always possible to remove/replace a logic done with **custom** delegates.


When adding native protocol conformance to a class, prefer adding a separate class extension for the protocol methods.
This keeps the related methods grouped together with the protocol and can simplify instructions to add a protocol to a class with its associated methods.

Also, do not forget the `// MARK: -` comment to keep things well-organized!

**Preferred:**
```swift
class MyViewcontroller: UIViewController {
    // class stuff here
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
    // table view data source methods
}

// MARK: - UIScrollViewDelegate
extension MyViewcontroller: UIScrollViewDelegate {
    // scroll view delegate methods
}
```

**Not Preferred:**
```swift
class MyViewcontroller: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
    // all methods
}
```

### Computed Properties

For conciseness, if a computed property is read-only, omit the get clause. The get clause is required only when a set clause is provided.
Please note the rounded brackets.

**Preferred:**
```swift
var diameter: Double {
    return (radius * 2)
}
```

**Not Preferred:**
```swift
var diameter: Double {
    get {
      return radius * 2
    }
}
```

## Function Declarations

Be extremely careful on the function names: they have to be very explicit on what they do.
The other way around, the code should __match the function name__ (the name should tell what the function does).
It is super easy to keep coding and changing your code, and in the end have a function doing something completely different than what its name says.


Other point, a function should __not exceed 40 lines__. This drastic limit forces the developer to think twice about the code and its structure.
A better encapsulation will help you with this rule.


Keep short function declarations on one line including the opening brace:

```swift
func reticulateSplines(spline: [Double]) -> Bool {
    // reticulate code goes here
}
```

For functions with long signatures, add line breaks at appropriate points and add an extra indent on subsequent lines:

```swift
func reticulateSplines(spline: [Double], adjustmentFactor: Double,
        index: Int, comment: String) -> Bool {
    // reticulate code goes here
}
```

## Closure Expressions

Use trailing closure syntax only if there's a single closure expression parameter at the end of the argument list. Give the closure parameters descriptive names.

**Preferred:**
```swift
UIView.animateWithDuration(1.0) {
  self.myView.alpha = 0
}

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  },
  completion: { finished in
    self.myView.removeFromSuperview()
  }
)
```

**Not Preferred:**
```swift
UIView.animateWithDuration(1.0, animations: {
  self.myView.alpha = 0
})

UIView.animateWithDuration(1.0,
  animations: {
    self.myView.alpha = 0
  }) { f in
    self.myView.removeFromSuperview()
}
```

For single-expression closures where the context is clear, do not use implicit returns.
The code as to be as self explained as possible.
Please also note the rounded brackets.

**Preferred:**
```swift
attendeeList.sort { (a, b) in
    return (a > b)
}
```

**Not Preferred:**
```swift
attendeeList.sort { a, b in
    a > b
}
```

## Types

Always use Swift's native types when available. Swift offers bridging to Objective-C so you can still use the full set of methods as needed.

**Preferred:**
```swift
let width = 120.0                                    // Double
let widthString = (width as NSNumber).stringValue    // String
```

**Not Preferred:**
```swift
let width: NSNumber = 120.0                          // NSNumber
let widthString: NSString = width.stringValue        // NSString
```

In Sprite Kit code, use `CGFloat` if it makes the code more succinct by avoiding too many conversions.

### Constants

Constants are defined using the `let` keyword, and variables with the `var` keyword. Always use `let` instead of `var` if the value of the variable will not change.

**Tip:** A good technique is to define everything using `let` and only change it to `var` if the compiler complains!

#### Global Constants

* The constants should be in one single constants files called Constants.swift or Constants.h. Of course this file should also be prefix depending on your project.
* As said earlier, the more your file is structured the better: Create structures, add comments and pragma marks.
* Try to avoid single and anarchic constants without logic or explanation around. In such case they are just magic numbers hidden behind a constant.
* The term `constant` refers to all strings/numbers that are fixed and can't be dynamically changed. 
They are used for database keys, API endpoints and response codes, user default keys, segue identifiers, etc. Actually all values that should not change. But if they do, they are all created in one single file and the change will take mostly 5 seconds.

**Preferred:**
```swift
//
// User Default
//
enum HUUserDefault                          : String {

    // Keys set in PList files
    case AppId                              =       "AppId"
    case HockeyId                           =       "HockeyAppId"
    case APIBaseURL                         =       "ApiBaseURL"
    case APIUserCredential                  =       "ApiUserCredential"
    case APIPasswordCredential              =       "ApiPasswordCredential"
 
    static let allValues                    =       [AppId, HockeyId, APIBaseURL, APIUserCredential, APIPasswordCredential]
}

//
// Segues
//
enum HUSegueIdentifier                      : String {
    case FormulaDetail                      =       "showDetailFormula"
    case SearchViewController               =       "showSearchView"
}

//
// Cells
//
enum HUCellReuseIdentifier                  : String {
    case FormulaCell                        =       "HUFormulaCell_id"
    case SearchCell                         =       "HUSearchCell_id"
    case OptionCell                         =       "HUOptionCell_id"
}

//
// Database
//
struct DB {

    static let DatabaseName                 =       "Huethig.sqlite"

    struct Key {
        static let Id                       =       "id"
        static let UpdatedAt                =       "lastUpdate"
        static let Key                      =       "key"
        static let Value                    =       "value"
    }
}

//
// API
//
struct API {

    // Endpoints
    enum Endpoint                           : String {
        case Formula                        =       "formula"
        case PDF                            =       "pdf"
        case ProductId                      =       "productid"
    }
}
```

**Not Preferred:**
```swift
// User Default
let K_APP_ID = "AppId"
let HOCKEY_ID = "HockeyAppId"

let K_FORMULADETAIL = "showDetailFormula"
let K_SEARCHVIEWCONTROLLER = "showSearchView"
let K_FORMULACELL = "HUFormulaCell_id"
let K_SEARCHCELL = "HUSearchCell_id"
let K_OPTIONCELL = "HUOptionCell_id"

// Database
let DB_ID = "id"
let DB_UPDATEDAT = "lastUpdate"
let DB_KEY = "key"
let DB_VALUE = "value"

let API_FORMULA = "formula"
let API_PDF = "pdf"
let API_PRODUCTID = "productid"
```

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

Use implicitly unwrapped types declared with `!` only for instance variables that you know will be initialized later before use, such as subviews that will be set up in `viewDidLoad`.

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let textContainer = self.textContainer {
    // do many things with textContainer
}
```

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, shadow the original name when appropriate rather than using names like `unwrappedView` or `actualLabel`.

**Preferred:**
```swift
var subview : UIView?
var volume  : Double?

// later on...
if let subview = subview, volume = volume {
    // do something with unwrapped subview and volume
}
```

**Not Preferred:**
```swift
var optionalSubview: UIView?
var volume: Double?

if let unwrappedSubview = optionalSubview {
    if let realVolume = volume {
        // do something with unwrappedSubview and realVolume
    }
}
```

### Struct Initializers

Use the native Swift struct initializers rather than the legacy CGGeometry constructors.
The `CGPoint` is a new native swift function, it should be used rather than the old `CGPointMake` as it is a legacy C method.
Even though they might do the same now, the swift one has much future.

Also it’s more readable in a swift environment and feels better integrated.

**Preferred:**
```swift
let bounds      = CGRect(x: 40, y: 20, width: 120, height: 80)
let centerPoint = CGPoint(x: 96, y: 42)
```

**Not Preferred:**
```swift
let bounds = CGRectMake(40, 20, 120, 80)
let centerPoint = CGPointMake(96, 42)
```

Prefer the struct-scope constants `CGRect.infiniteRect`, `CGRect.nullRect`, etc. over global constants `CGRectInfinite`, `CGRectNull`, etc. For existing variables, you can use the shorter `.zeroRect`.

### Type Inference

Prefer compact code and let the compiler infer the type for a constant or variable, unless you need a specific type other than the default such as `CGFloat` or `Int16`.

**Preferred:**
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = [String]()
let maximumWidth: CGFloat = 106.5
```

**Not Preferred:**
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
var names: [String] = []
```

**NOTE**: Following this guideline means picking descriptive names is even more important than before.


### Syntactic Sugar

Prefer the shortcut versions of type declarations over the full generics syntax.

**Preferred:**
```swift
var deviceModels  : [String]
var employees     : [Int: String]
var faxNumber     : Int?
```

**Not Preferred:**
```swift
var deviceModels: Array<String>
var employees: Dictionary<Int, String>
var faxNumber: Optional<Int>
```


## Control Flow

Prefer the `for-in` style of `for` loop over the `for-condition-increment` style.

**Preferred:**
```swift
for _ in 0..<3 {
    println("Hello three times")
}

for (index, person) in enumerate(attendeeList) {
    println("\(person) is at position #\(index)")
}
```

**Not Preferred:**
```swift
for var i = 0; i < 3; i++ {
    println("Hello three times")
}

for var i = 0; i < attendeeList.count; i++ {
    let person = attendeeList[i]
    println("\(person) is at position #\(i)")
}
```


## Semicolons

Swift does not require a semicolon after each statement in your code. They are only required if you wish to combine multiple statements on a single line, which of course should be avoided. 

Just do not write multiple statements on a single line separated with semicolons.

The only exception to this rule is the `for-conditional-increment` construct, which requires semicolons. However, alternative `for-in` constructs should be used where possible.

**Preferred:**
```swift
let swift = "not a scripting language"
```

**Not Preferred:**
```swift
let swift = "not a scripting language";
```

**NOTE**: Swift is very different to JavaScript, where omitting semicolons is [generally considered unsafe](http://stackoverflow.com/questions/444080/do-you-recommend-using-semicolons-after-every-statement-in-javascript)

## Language

Use US English spelling to match Apple's API.

**Preferred:**
```swift
let color = "red"
```

**Not Preferred:**
```swift
let colour = "red"
```

## Credits

This style guide is a fork from the collaborative effort from the most stylish raywenderlich.com team members.
Please see the full list on the [orginal page](https://github.com/raywenderlich/swift-style-guide#credits).

## See Also

* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
