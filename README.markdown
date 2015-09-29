# Swift Style Guide

This guide is a fork from the [Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide).
It has then be modified and improved to match different styles and add some missing parts.

This style guide is different from others, nonetheless the goals are the same: define a style, syntax and structure while coding in Swift.
Of course, efficacity, readability, and simplicity are the most important points.

## Table of Contents

* [Spacing](#spacing)
  * [Empty lines](#empty-lines)
  * [Whitespaces](#whitespaces)
* [Comparison](#comparison)
  * [Explicit Type](#explicit-type)
  * [Brackets Position](#brackets-position)
* [Naming](#naming)
  * [Structures](#structures)
  * [Enumerations](#enumerations)
    * [Functions](#functions)
    * [Switch Cases](#switch-cases)
* [Rounded Brackets](#rounded-brackets)
* [Ternary operator](#ternary-operator)
* [Comments](#comments)
* [Code Alignement And Structure](#code-alignement-and-structure)
  * [Mark](#mark)
  * [Alignment](#alignment)
  * [Nested Closures](#nested-closures)
* [Classes](#classes)
  * [Class or Structure](#class-or-structure)
  * [Class Attributes](#class-attributes)
  * [Use of Self](#use-of-self)
  * [Class Definition](#class-definition)
  * [Protocol Conformance](#protocol-conformance)
  * [Computed Properties](#computed-properties)
* [Function Declarations](#function-declarations)
  * [Visibility](#visibility)
* [Closures](#closures)
* [Types](#types)
  * [Constants](#constants)
    * [Global Constants](#global-constants)
    * [Magic Numbers](#magic-numbers)
    * [Hard Coded Values](#hard-coded-values)
  * [Optionals](#optionals)
    * [Optional Chaining](#optional-chaining)
    * [Single Optional Binding](#single-optional-binding)
    * [Multiple Optional Bindings](#multiple-optional-bindings)
    * [More Condition Check](#more-condition-check)
    * [?? Operator](#??-operator)
    * [Naming Convention](#naming-convention)
  * [Struct Initializers](#struct-initializers)
    * [Accessing CGRect](#accessing-cgrect)
  * [Type Inference](#type-inference)
  * [Syntactic Sugar](#syntactic-sugar)
* [Control Flow](#control-flow)
* [Semicolons](#semicolons)
* [Language](#language)
* [Credits](#credits)
* [See Also](#see-also)

## Spacing

* Indent using tabs equivalent to 4 spaces, and indent by inserting tab characters. Be sure to set xcode like this:

  ![Xcode indent settings](screens/indentation.png)

* Also think about automatically trim the whitespaces:

  ![Xcode trim whitespaces](screens/trim-whitespaces.png)

* Tip: You can re-indent by selecting some code (or ⌘A to select all) and then Control-I (or Editor\Structure\Re-Indent in the menu). Some of the Xcode template code will have 4-space tabs hard coded, so this is a good way to fix that.

### Empty lines

* There should be exactly **one blank line between methods** to aid in visual clarity and organization. 
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

### Whitespaces

Whitespace within methods should separate functionality, but having too many sections in a method often means you should **refactor into several methods**.

A single whitespace must be use between elements:
- after/before `if` and `else`.
- when calculating and comparing values.
- when opening and closing brackets.
- when declaring a function.

**Preferred:**
```swift
func dummyFunction(number: Int) -> Int {
    var result = 0

    if (number > 0) {
        result = (number * number)
    }
    return result
}
```

**Not Preferred:**
```swift
func dummyFunction(number: Int)->Int {
    var result = 0
    if(number>0){
        result = (number*number)
    }
    return result
}
```

## Comparison

### Explicit Type

* Always specify the type to compared a value: add `== type` within your if statements.

In Swift the type, that a value can be verify against, is uncertain.
An object is sometimes an optional or not, it might be able to be checked against nil or not, maybe a against a boolean, etc.

For this very specific reason, one should always specify what's after the comparison operator: `==`, `!=`, etc.

**Preferred:**
```swift
if (finished == true) {
    // do something
}
```

**Not Preferred:**
```swift
if (finished) {
    // do something
}
```

**NOTE:** For an optional, prefer to use the operator `??` to compare the object against `nil`.

### Brackets Position

* Braces for `if`/`else`/`switch`/`while` etc. always open and close on the same line as the statement.

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

A developer should only use it with just one level of operation and with [rounded brackets](#rounded-brackets).
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

## Code Alignement And Structure

### Mark

The mark should be use as much as possible to actually separate and structure your classes and code within different files.

The mark should be indented, capitalised and using a separator. It should describe what the next methods are about.

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
// MARK: - Actions
    func superMethod() {
    }
}
```

### Alignment

Make sure the code is aligned to itself. This is just about structure and better looking code: for example the `=` character, the start of the lines, the function names, etc. The alignment is done with "tabulations".
It is also extremely appreciated to use comments to separate the [Class Attributes](#class-attributes) and mark between functions.

**Preferred:**
```swift
class MyViewController                           : UIViewController {

    // MARK: - Outlets

    @IBOutlet weak var lettersGameButton         : UIButton?
    @IBOutlet weak var headBodyLegsGameButton    : UIButton?
    @IBOutlet weak var colorMatcherGameButton    : UIButton?

    // MARK: - Instance Variables

    var destinationType                          : FFGameType?
    let transitionManager                        = FFTransitionManager()

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

Please note the differences between the `// MARK: - ` and the missing `weak` references for the IBOutlets.

**Not Preferred:**
```swift
class MyViewController : UIViewController {

//MARK Outlets

    @IBOutlet var lettersGameButton      : UIButton?
    @IBOutlet var headBodyLegsGameButton : UIButton?
    @IBOutlet var colorMatcherGameButton : UIButton?
    var destinationType : FFGameType?
    let transitionManager = FFTransitionManager()

//MARK View Lifecycle

    override func viewWillAppear(animated: Bool) {
        super.viewWillAppear(animated)
        // Your code here.
    }
    @IBAction func buttonPressed(sender: UIButton) {
        // Your code here.
    }
}
```

### Nested Closures

As said earlier, a line should not be too long and obvisouly as readable as possible.

Where it gets complicated it's when you can/want to nest closures or functions.
Meaning calling a function and giving the result directly to another without creating a variable in between.

This is absolutely not a bad thing, if it is well done and well structured. 

**Preferred:**
```swift
let action = UIAlertAction(title:L("ALERTVIEW_BUTTON_CANCEL"), style:.Cancel, handler:nil)
let subCrazy = UIMoreCrazyThing(string: "Crazy", action)
UICrazyThing(subCrazieness: subCrazy)
```

**Not Preferred:**
```swift
UICrazyThing(subCrazieness: UIMoreCrazyThing(string: "Crazy", UIAlertAction(title:L("ALERTVIEW_BUTTON_CANCEL"), style:.Cancel, handler:nil)))
```

The first version has shorter lines and is more readable. The second fit in one line but is more complicated to read.

Of course, for very small and simple cases you can nest your functions, but be mindful of other developers and their ability to read and understand your code.

## Classes

### Class or Structure

Remember, structs have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structs for things that do not have an identity. An array that contains [a, b, c] is really the same as another array that contains [a, b, c] and they are completely interchangeable. It doesn't matter whether you use the first array or the second, because they represent the exact same thing. That's why arrays are structs.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that do have an identity or a specific life cycle. You would model a person as a class because two person objects are two different things. Just because two people have the same name and birthdate, doesn't mean they are the same person. But the person's birthdate would be a struct because a date of 3 March 1950 is the same as any other date object for 3 March 1950. The date itself doesn't have an identity.

Sometimes, things should be structs but need to conform to `AnyObject` or are historically modeled as classes already (`NSDate`, `NSSet`). Try to follow these guidelines as closely as possible.

### Class Attributes

When declaring a class, you should never use explicit types unless you directly specify a default value.
The best thing to do is to use optionals whenever you can.

In general, you can not be sure that all attributes will be correctly created, linked, instanciated, initialised, etc.

**Preferred:**
```swift
class Plane {
    var pilot                : Pilot?
    var passengerCount       : Int = 0
    var passengers           = [Passenger]()

    @IBOutlet weak var text  : UILabel?
}
```

**Not Preferred:**
```swift
class Plane {
    var pilot                : Pilot!
    var passengerCount       : Int!
    var passengers           : [Passenger]!

    @IBOutlet weak var text  : UILabel!
}
```

### Use of Self

Even though Swift does not require the usage of `self`, a developer should force himself to use it everywhere he can.
It is then easier for any other developer to understand where does a value come from or where is the current execution process going.

**Preferred:**
```swift
class MyViewController              : UIViewController {

    @IBOutlet weak var titleLabel   : Int?

    func setTitle(text: String) {
        self.titleLabel?.text = text
        self.updateUI()
    }

    func updateUI() {
        self.titleLabel?.backgroundColor = UIColor.clearColor()
    }
}
```

**Not Preferred:**
```swift
class MyViewController              : UIViewController {

    @IBOutlet weak var titleLabel   : Int?

    func setTitle(text: String) {
        titleLabel?.text = text
        updateUI()
    }

    func updateUI() {
        titleLabel?.backgroundColor = UIColor.clearColor()
    }
}
```

### Class Definition

Here's an example of a well-styled class definition:

```swift
class Circle      : Shape {

    var x         : Int = 0
    var y         : Int = 0
    var radius    : Double?

    var diameter  : Double {
        get {
            return ((self.radius ?? 0) * 2)
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
        self.init(x: x, y: y, radius: diameter * 0.5)
    }

    func describe() -> String {
        return "I am a circle at \(self.centerString()) with an area of \(self.computeArea())"
    }

    override func computeArea() -> Double {
        let radius = (self.radius ?? 0)
        return (M_PI * radius * radius)
    }

    // MARK: - Private

    private func centerString() -> String {
        return "(\(self.x),\(self.y))"
    }
}
```

When executing:
```swift
let circle = Circle(x: 2, y: 2, radius: 10)
println(circle.describe())
// I am a circle at (2,2) with an area of 314.159265358979
```

The example above demonstrates the following **important** style guidelines:

* Specify types for properties, variables, constants, argument declarations and other statements with a spaces around the colon, `x : Int`, and `Circle : Shape`. The colons have to be aligned with each others.
* Align the multiple variable and structure declarations.
* Indent getter and setter definitions and property observers.
* Separate variable declarations with getter and setter.
* Note the [rounded brackets](#rounded-brackets).
* Note the separator `// MARK: -` and its indentation.
* `self.` is always used.
* [Class Attributes](#class-attributes) as optionals or with default values.
* Show an example of `convenience init`.


### Protocol Conformance


In Swift, protocols can be used, but it's better appreciated to use closures.
In the end they are the very same thing: a pointer to a function owned by an object (a specific object or self).


The closures are better as they are easier to read, easier to implement and to understand.
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

Please note the [rounded brackets](#rounded-brackets).

**Preferred:**
```swift
var diameter: Double {
    return (self.radius * 2)
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


Antoher good thing is to keep the lines of code maximum __between 100 and 120 characters__.


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

### Visibility

By default, declare your functions and properties as `private`. Variables should also be declared as constants using `let`. See [Constants](#constants) for more information.

Later on whenever you need to change them, let the compiler complain and then change it to `public` or `var`. This way you can be sure that you and other developers only have access to what is safe to access.

**Preferred:**
```swift
class MyViewcontroller: UIViewController {
    private var progress: Int?

    func animateProgress() {
       self.calculateProgressBasedOnMoonPhase()
       // ...
    }

    private func calculateProgressBasedOnMoonPhase() {
       // ...
    }
}
```

**Not Preferred:**
```swift
class MyViewcontroller: UIViewController {
    var progress: Int?

    func animateProgress() {
       self.calculateProgressBasedOnMoonPhase()
       // ...
    }

    func calculateProgressBasedOnMoonPhase() {
       // ...
    }
}
```

## Closures

Closures (like blocks in Objective-C) are one of the most complicated subject when dealing with guidelines in Swift.
The code needs to be clear, very easy to read and understandable in seconds.

Here are some very specific rules:

* Never write the closure name if there is just one closure in a function.
* Never write the parameters of the closure if there is none.

**Preferred:**
```swift
UIView.animateWithDuration(1.0) {
    self.myView.alpha = 0
}
```

**Not Preferred:**
```swift
UIView.animateWithDuration(1.0, animations: { () -> Void in
    self.myView.alpha = 0
})
```

* Always declare the parameters of the closure inside **[rounded brackets](#rounded-brackets)**.
* Always **write all closure names** if there are more than one closure.
* Always **specify the type** of each parameters of the closure if it is coming from a function. You can **not** be certain of the type.
* Always write the full names of the parameters of a closure.

**Preferred:**
```swift
UIView.animateWithDuration(1.0, animations: {
        self.myView.alpha = 0
    }, completion: { (finished: Bool) in
        self.myView.removeFromSuperview()
    }
)
```

**Not Preferred:**
```swift
UIView.animateWithDuration(1.0, animations: {
    self.myView.alpha = 0
  }) { f in () -> Void in
    self.myView.removeFromSuperview()
}
```

* For very specific context, when you can be sure of the type, you can shorten the syntax and remove the parameter types.
* When returning a value, always use **explicit returns**.

The code as to be as self explained as possible.
Please also note the [rounded brackets](#rounded-brackets).

**Preferred:**
```swift
var list = [1, 4, 2, 3]
list.sort { (a, b) -> Bool in
    return (a > b)
}
```

**Not Preferred:**
```swift
var list = [1, 4, 2, 3]
list.sort { a, b in
    a > b
}
```

* When the context is very very easy clear you can also use the shortcuts `$0` and `$1`

Please note the [rounded brackets](#rounded-brackets).

```swift
var list = [1, 4, 2, 3]
list.sort { ($0 > $1) }
```

###Closures as parameter

* When declaring a function that takes a closure as parameter, **always declare the closure as optional**.

**Preferred:**
```swift
func myFunction(closure: (() -> Void)?) {
    if (something == true) {
        closure?()
    }
}
```

**Not Preferred:**
```swift
func myFunction(closure: () -> Void) {
    if (something == true) {
        closure()
    }
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

* The constants should be in one single constants files.
* The more this file is structured the better: Create structures, add comments and marks.
* Try to avoid single and anarchic constants without logic or explanation around. In such case they are just magic numbers hidden behind a constant.
* The term `constant` refers to all strings/numbers that are fixed and can't be dynamically changed. 


Constants are used for database keys, API endpoints and response codes, user default keys, segue identifiers, etc.
Actually all values that should not change. But if they do, they are all created in one single file and the change will take mostly 5 seconds.

**Preferred:**
```swift
//
// User Default
//
enum HUUserDefault                          : String {

    // Keys set in PList files
    case AppId                              = "AppId"
    case HockeyId                           = "HockeyAppId"
    case APIBaseURL                         = "ApiBaseURL"
    case APIUserCredential                  = "ApiUserCredential"
    case APIPasswordCredential              = "ApiPasswordCredential"
 
    static let allValues                    = [AppId, HockeyId, APIBaseURL, APIUserCredential, APIPasswordCredential]
}

//
// Segues
//
enum HUSegueIdentifier                      : String {
    case FormulaDetail                      = "showDetailFormula"
    case SearchViewController               = "showSearchView"
}

//
// Cells
//
enum HUCellReuseIdentifier                  : String {
    case FormulaCell                        = "HUFormulaCell_id"
    case SearchCell                         = "HUSearchCell_id"
    case OptionCell                         = "HUOptionCell_id"
}

//
// Database
//
struct DB {

    static let DatabaseName                 = "Huethig.sqlite"

    struct Key {
        static let Id                       = "id"
        static let UpdatedAt                = "lastUpdate"
        static let Key                      = "key"
        static let Value                    = "value"
    }
}

//
// API
//
struct API {

    // Endpoints
    enum Endpoint                           : String {
        case Formula                        = "formula"
        case PDF                            = "pdf"
        case ProductId                      = "productid"
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

#### Magic Numbers

No magic numbers in any case, by any chance. It has to be dynamic, or from an IBOutlet, or calculated.

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

#### Hard Coded Values

Hard-coded values are an easy thing that every quick-and-dirty developer does. But it is the worst in terms of code quality and maintability.

For example what about hard-coded keys to read from the database? or to parse a json?
One might think that they will never change and everything will be *fine*.
Big mistake! What about a typo in one key that cause a crash or random behavior ?

How many hours of debug will be needed to find and correct the error?

**Preferred:**
```swift
func predicateFromJSON(json: [String : AnyObject]) -> NSPredicate? {
    if let value = json[JSON.Key.Id] as? Int {
        return NSPredicate(format: "\(DB.Key.Id) ==[c] \(value)")
    }
    return nil
}
```

**Not Preferred:**
```swift
func predicateFromJSON(json: [String : AnyObject]) -> NSPredicate? {
    if let id = json["id"] as? Int {
        return NSPredicate(format: "id ==[c] \(id)")
    }
    return nil
}
```

### Optionals

Declare variables and function return types as optional with `?` where a nil value is acceptable.

You should just simply **never unwrap an optional** value using `!` or `as!`.
There is always a better way to keep the code safe and avoid crashes.

If you keep focusing on doing a nice code right in the begining you won't have any trouble to avoid unwrapping crashes.

#### Optional Chaining

When accessing an optional value, use optional chaining if the value is only accessed once or if there are many optionals in the chain. This way no crash will occur if one of the objects is `nil`.

**Preferred:**
```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

**Not Preferred:**
```swift
self.textContainer!.textLabel!.setNeedsDisplay()
```

#### Single Optional Binding

Use optional binding when it's more convenient to unwrap once and perform multiple operations:

```swift
if let _textContainer = self.textContainer {
    // do many things with _textContainer
}

if let _superText = self.generateSuperText() as? String where (_superText.count > 0) {
    // do many things with _superText
}
```

Unlike the [Multiple Optional Bindings](#multiple-optional-bindings), when binding one single optional you can:
- inline the `if let` and the variable
- inline the `where` with the variable

#### Multiple Optional Bindings

Starting [Swift 1.2](http://nshipster.com/swift-1.2/) you can create multiple bindings:

> Swift 1.2 allows multiple simultaneous optional bindings, providing an escape from the trap of needing deeply nested if let statements to unwrap multiple values. Multiple optional bindings are separated by commas and can be paired with a where clause that acts like the expression in a traditional if statement. 

About syntax, please the following points:
- New line after the first `let`.
- All variables are aligned.
- The opening brackets `{` at the end of the last line.
- The two levels of indentation.
- The [Naming Convention](#naming-convention) between the _binded_ and optional variables.

```swift
let a = "10".toInt()
let b = "5".toInt()
let c = "3".toInt()
```

**Preferred:**
```swift
if let
    _a = a,
    _b = b,
    _c = c
    where (_c != 0) {
        println("\((_a + _b) / _c)")
        // 5
}
```

**Not Preferred:**
```swift
if let a = a, b = b, c = c where c != 0 {
    println("\((a + b) / c)")
    // 5
}
```

#### More Condition Check

In some case you need a `if` statement before binding optionals.
With swift you could even integrate this one in the same `if let`.

Please note the indentation differences.

**Preferred:**
```swift
if (array.count == 0),
    let
        _a = a,
        _b = b
        where (b > a) {
            println(a)
}
```

**Not Preferred:**
```swift
if (array.count == 0) {
    if let
        _a = a,
        _b = b
        where (b > a) {
            println(a)
    }
}
```

#### ?? Operator

Swift has very neat operator used to safely unwrapped optionals: `??`

Close to the [Ternary operator](#ternary-operator), this one returns the optional value or a default value if `nil` is found.

**Preferred:**
```swift
let text = (self.generateSuperText() as? String ?? "default value")
```

**Not Preferred:**
```swift
var text : String?
if let _superText = self.generateSuperText() as? String {
    text = _superText
} else {
    text = "default value"
}
```

Please note the [rounded brackets](#rounded-brackets).

#### Naming Convention

When naming optional variables and properties, avoid naming them like `optionalString` or `maybeView` since their optional-ness is already in the type declaration.

For optional binding, add a small prefix to the original name rather than using names like `unwrappedView` or `actualLabel`. Usually `_` is quick and easy to understand.

**Preferred:**
```swift
var subview : UIView?
var volume  : Double?

// later on...
if let
    _subview = subview,
    _volume = volume {
        // do something with unwrapped subview and volume
}
```

**Not Preferred:**
```swift
var optionalSubview : UIView?
var volume          : Double?

if let
    unwrappedSubview = optionalSubview,
    realVolume = volume {
        // do something with unwrappedSubview and realVolume
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

#### Accessing CGRect

A CGRect's width and height can be negative, a developer should always the standard `CGRectGet` functions to access a value.
Those actually normalize the result before returning it.

> CGGeometry Reference defines structures for geometric primitives and functions that operate on them. The data structure CGPoint represents a point in a two-dimensional coordinate system. The data structure CGRect represents the location and dimensions of a rectangle. The data structure CGSize represents the dimensions of width and height.
>
> The height and width stored in a CGRect data structure can be negative. For example, a rectangle with an origin of [0.0, 0.0] and a size of [10.0,10.0] is exactly equivalent to a rectangle with an origin of [10.0, 10.0] and a size of [-10.0,-10.0]. Your application can standardize a rectangle—that is, ensure that the height and width are stored as positive values—by calling the CGRectStandardize function. All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. **For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.**

**Preferred:**
```swift
let bounds = CGRect(x: 40, y: 20, width: 120, height: 80)

let x = CGRectGetMinX(bounds)
let y = CGRectGetMinY(bounds)
let w = CGRectGetWidth(bounds)
let h = CGRectGetHeight(bounds)

```

**Not Preferred:**
```swift
let bounds = CGRectMake(40, 20, 120, 80)

let x = bounds.origin.x
let y = bounds.origin.y
let w = bounds.size.width
let h = bounds.size.heigth
```

### Type Inference

Prefer compact code and let the compiler infer the type for a constant or variable, unless you need a specific type other than the default such as `CGFloat` or `Int16`.

**Preferred:**
```swift
let message       = "Click the button"
let currentBounds = computeViewBounds()
var names         = [String]()
let maximumWidth  : CGFloat = 106.5
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
let color     = "red"
let favorites = [1, 2, 3]
```

**Not Preferred:**
```swift
let colour     = "red"
let favourites = [1, 2, 3]
```

## Credits

This style guide is a fork from the collaborative effort from the most stylish raywenderlich.com team members.
Please see the full list on the [orginal page](https://github.com/raywenderlich/swift-style-guide#credits).

The current fork has been mainly improved and maintained by:
* [Kevin Delord](https://github.com/kevindelord)

## See Also

* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
