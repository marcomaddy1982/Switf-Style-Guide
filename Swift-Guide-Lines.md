# Swift-Style-Guide

## Table of Contents

- [General](#general)
- [Naming](#naming)
    - [General](#naming-general)
    - [File](#naming-file)
    - [Struct, Class, Protocol](#naming-struct-class-protocol)
    - [Variable, Constant](#naming-variable-constant)
    - [Method, Function](#naming-method-function)
- [Source](#source)
    - [Style](#style) 
    - [General](#style-general)
    - [Definitions](#style-definitions)
    - [Spacing](#style-spacing)
    - [Indentation](#style-indentation)
- [Optionals](#optionals)
    - [Force Unwrapping](#optionals-force-unwrapping)
    - [Pyramid of Doom](#optionals-pyramid-doom)
    - [Unwrapping Multiple Optionals](#optionals-unwrapping-multiple-optionals)
    - [Error Handling](#optionals-error-handling)
- [Access Modifiers](#access-modifiers)
- [Loops](#loops)
- [Delegates](#delegates)
- [Organization](#organization)
    - [Constants](#organization-constants)
    - [Init Method](#organization-init)
    - [Decoupling in methods](#organization-decoupling)
    - [Protocols](#organization-protocols)
    - [Class Organization](#organization-class-organization)
- [References](#references)

## General

- Use `Swift` types whenever possible (`Array`, `Dictionary`, `Set`, `String`, etc.) instead of types from `Objective-C`. Many Objective-C types can be automatically converted to Swift types and vice versa.

```swift
    // NOT PREFERRED
    let nameLabelText = NSString(format: "%@/%@", firstName, lastName)

    // PREFERRED
    let nameLabelText = "\(firstName)/\(lastName)"
    let alsoNameLabelText = firstName + "/" + lastName
```

- Swift Collection Types: Do not make `NSArray`, `NSDictionary`, and `NSSet` properties or variables. If you need to use a specific method only found on a `Foundation` collection, cast your Swift type in order to use that method.

```swift
    // NOT PREFERRED
    var objectArray: NSArray = NSArray()
    let names: AnyObject? = objectArray.value(forKeyPath: "key")

    // PREFERRED
    var objectArray = [[String: AnyObject]]()
    let values: AnyObject? = (objectArray as NSArray).value(forKeyPath: "key")
```

or 

```swift
    // PREFERRED
    var objectArray = [[String: AnyObject]]()
    let values: [String] = objectArray.compactMap { object in
        return object["key] as? String
    }
```

## Naming

### General <a name="naming-general"></a>

- Do not abbreviate or use shortened names.

```swift
    // NOT PREFERRED
    class Loading: UIView {
        let animDur: NSTimeInterval

        func startAnim() {
            let v = subviews.first
        }
    }

    // PREFERRED
    class LoadingView: UIView {
        let animationDuration: NSTimeInterval

        func startLoading() {
            let firstSubview = subviews.first
        }
    }
```

### File <a name="naming-file"></a>

- A file containing a single type `MyType` is named `MyType.swift`.
- A file containing a type `MyType` and some top-level helper functions is also named `MyType.swift`. 
- A file containing a single extension to a type `MyType` that adds conformance to a protocol MyProtocol is named `MyType+MyProtocol.swift`.
- A file containing multiple extensions to a type `MyType` that add conformances, nested types, or other functionality to a type can be named more generally, as long as it is prefixed with `MyType+`; for example, `MyType+Extentions.swift`.
- A file containing related declarations that are not otherwise scoped under a common type or namespace (such as a collection of global mathematical functions) can be named descriptively; for example, `Math.swift`.
- There is no need for `Objective-C` style prefixing in `Swift`.

### Struct, Class, Protocol <a name="naming-struct-class-protocol"></a>

- Use `PascalCase` for `struct`, `enum`, `class`, `typedef`, `associatedtype` names.

```swift
    // NOT PREFERRED
    protocol animal {
        // ...
    }

    class dog: animal {

        enum breed {
            // ...
        }
    }

    let mydog = dog()

    // PREFERRED
    protocol Animal {
        // ...
    }

    class Dog: Animal {

        enum Breed {
            // ...
        }
    }

    let myDog = Dog()
```

- Do not add a class prefix as in `Objective-C` style.

```swift
    // NOT PREFERRED
    class ZWConversation {
        // ...
    }

    // PREFERRED
    class Conversation {
        // ...
    }
```

### Variable, Constant <a name="naming-variable-constant"></a>

- Use `camelCase` (initial lowercase letter) for `constant` and `variable`
- When declaring variables you should be verbose, but not too verbose.
- Make the name descriptive.
- `constant` and `variable` should be written first with their most general part and then with their most specific part.

```swift
    // NOT PREFERRED
    let topTextBorder: CGFloat
    let bottomTextBorder: CGFloat

    // PREFERRED
    let textBorderTop: CGFloat
    let textMarginLeft: CGFloat
```
- Omit the type information in names when not absolutely necessary. For `Array` and `Set` use the `plural form` in naming for being more descriptive. Moreover it makes sense that the name should reflect the type in some way.

```swift
    // NOT PREFERRED
    let nameText: String
    var userList = [User]()
    var users = [String]() // This is an array of string -> No info about the type in the name.

    // PREFERRED
    let name: String
    var users = [User]()
    var userNames = [String]()
```

-  For `Dictionary` include the `key` in the name.

```swift
    // NOT PREFERRED
    var conversationDictionary = [String: String]()
    var conversations = [String: String]()

    // PREFERRED
    var conversationsByName = [String: String]()
```

- Name booleans like `isLoading`, `hasError`, `hasBeenShown`. 

### Method, Function <a name="naming-method-function"></a>

- Use `camelCase` (initial lowercase letter) for `function`, `method`, `argument names`,  etc.
- Acronyms in names (e.g. URL) should be all-caps except when it’s the start of a name that would otherwise be `lowerCamelCase`, in which case it should be uniformly lower-cased.

```swift
    // NOT PREFERRED
    class UrlValidator {

        func isValidUrl(_ URL: URL) -> Bool {
            // ...
        }
    }

    let URLValidator = UrlValidator()

    // PREFERRED
    class URLValidator {

        func isValidURL(_ url: URL) -> Bool {
            // ...
        }
    }

    let urlValidator = URLValidator()
```

- `Event-handling functions` should be named like `past-tense sentences`.

```swift
    // NOT PREFERRED
    class SignInViewController: LogInProtocol {

        private func handleLoginButtonTap() {
            // ...
        }

        private func userChanged() {
            // ...
        }
    }

    // PREFERRED
    class SignViewController: LogInProtocol {

        private func didTapLogInButton() {
            // ...
        }

        private func userDidChange() {
            // ...
        }
    }
```
## Style

### General <a name="style-general"></a>

- Prefer `let` to `var` whenever possible.

- Don't use types when they are inferred.

```swift
    // NOT PREFERRED
    let name: String = "Name"
    let user: User = User()

    // PREFERRED
    let name = "Name"
    let user = User()

    enum UserType {
        case normal
        case premioum
    }

    func someUser() -> UserType {
        // NOT PREFERRED
        return UserType.left

        // PREFERRED
        return .left
    }
```

- Don't use `self` when it's not necessary for disambiguation.

```swift
    final class Conversation {

    init(init(conversationID: Int, clientID: Int, isGroup: Bool) {
        // NOT PREFERRED
        self.conversationID = conversationID
        self.clientID = clientID
        self.hasMultipleUser = isGroup

        // PREFERRED
        self.conversationID = conversationID
        self.clientID = clientID
        hasMultipleUser = isGroup
    }

    private let conversationID: Int
    private let clientID: Int
    private var hasMultipleUser: Bool

    private func updateClientId(_ clientID: Int) {
            // PREFERRED
            self.clientID = clientID

            // NOT PREFERRED
            self.start()

            // PREFERRED
            start()
        }
    }
```

### Definitions <a name="style-definitions"></a>

- For `variable`, `constant` and `class` definition: Add the colon (`:`) immediately after an identifier, followed by a space.

```swift
    // NOT PREFERRED
    let value : Int?
    var value1 : String?
    var dict = [String:String]()
    var dict = [String : String]()

    // PREFERRED
    let value: Int?
    var value1: String?
    var dict = [String: String]()

    // NOT PREFERRED
    class ClassA : SuperClassA {
        // ...
    }

    // PREFERRED
    class ClassA: SuperClassA {
        // ...
    }
```

- For `method` and `closure`: Place a space before and after the return arrow.

    ```swift
    // NOT PREFERRED
    func someMethod()->String {
        // ...
    }

    // PREFERRED
    func someMethod() -> String {
        // ...
    }

    // NOT PREFERRED
    func someMethod(completion: ()->Void) {
        // ...
    }

    // PREFERRED
    func someMethod(completion: () -> Void) {
        // ...
    }

    // NOT PREFERRED
    func someMethod(completion: ()->Void)->String {
        // ...
    }

    // PREFERRED
    func someMethod(completion: () -> Void) -> String {
        // ...
    }
```

- For `method`: Omit `Void` return types.

```swift
    // NOT PREFERRED
    func someMethod() -> Void {
        // ...
    }

    // PREFERRED
    func someMethod() {
        // ...
    }
```

- For closure: in return types use `Void` instead `()`

    ```swift
    // NOT PREFERRED
    func someMethod(completion: () -> ())) {
        // ...
    }

    // PREFERRED
    func someMethod(completion: () -> Void) {
        // ...
    }
```

- For `closure`: Name unused closure parameters as underscores (`_`).

```swift
    // NOT PREFERRED
    someMethod() { argument1, argument2 in
        print(argument2)
    }

    // PREFERRED
    someMethod() { _, argument2 in
        print(argument2)
    }
```

- `Single-line closures` should have a space inside each brace.

```swift
    // NOT PREFERRED
    let result = array.filter {$0.isSomething == true}.map {$0.someMethod()}

    // PREFERRED
    let result = array.filter { $0.isSomething == true }.map { $0.someMethod() }
```

- Name members of  `tuples` and `enum` parameters for extra clarity. 

```swift
    // NOT PREFERRED
    func someMethod() -> (String, String) {
        return (4, 4)
    }

    // PREFERRED
    func someMethod() -> (firstName: String, lastName: String) {
        return (firstName: 4, lastName: 4)
    }

    // PREFERRED
    func someMethod() -> (firstName: String, lastName: String) {
        let firstName = "firstName"
        let lastName = "lastName"
        return (firstName, lastName)
    }

    // NOT PREFERRED
    enum WebError {
        case unreachable(URL)
        case unauthorized
        case serverError(Int)
    }

    // PREFERRED
    enum WebError {
        case unreachable(url: URL)
        case unauthorized
        case serverError(code: Int)
    }
```

### Spacing <a name="style-spacing"></a>

- Don't add useless `new lines` when it is not necessary.

```swift
    // NOT PREFERRED
    class ClassA 
    {

        init() 
        {
            // ...
        }

        private var property1: String?

        private var property2: String?

        // ...

        private func someMethod() {

            if property1.isEmpty 
            {
                // ...
            }    
            else 
            {
                // ...
            } 
        }
    }

    // PREFERRED
    class ClassA {

        init() {
            // ...
        }

        private var property1: String?
        private var property2: String?

        private func someMethod() {
            if property1.isEmpty {
                // ...
            } else {
                // ...
            } 
        }
    }
```

### Indentation <a name="style-indentation"></a>

- For `function` and  `method` definition that has many parameters use `Xcode` (CTRL-I) indentation.

```swift
    // NOT PREFERRED
    func someMethod(param1: String, param2: String, param3: Int, param4: Int, param5: Double, param6: Double) -> String {
        // ...
    }

    // PREFERRED 
    func someMethod(param1: String,
                    param2: String,
                    param3: Int,
                    param4: Int, 
                    param5: Double, 
                    param6: Double) -> String {
                    // ...
    }
```

- When calling a function that has many parameters, put each argument except the first on a separate line aligned with the first parameter.

```swift
    // NOT PREFERRED
    let result = someMethod(param1: "string", param2: "string", param3: 100, param4: 200, param5: 10.1, param6: 10.2) 

    // PREFERRED
    let result = someMethod(param1: "string", 
                            param2: "string",
                            param3: 100, 
                            param4: 200, 
                            param5: 10.1, 
                            param6: 10.2) 
```

- For `Array` and `Dictionary` defined with many elements, put each element on a separate line and add a trailing comma (`,`) also on the last element

```swift
    // NOT PREFERRED
    let array = [ element1, element2, element3, element4, element5]
    let dictionary = [ key1: value1, key2: value2, key3: value3, key4: value4, key5: value5]

    // PREFERRED 
    let array = [ 
        element1,
        element2, 
        element3, 
        element4,
        element5,
    ]

    let dictionary = [
        key1: value1, 
        key2: value2, 
        key3: value3, 
        key4: value4, 
        key5: value5,
    ]
```

- Use `one line` for `short ternary operator`, instead use `multiple line`  for `long ternary operator`.

```swift
    // NOT PREFERRED
    let value = isLoading 
                ? 100
                : 200
    let dictionary = isLoading ? someMethod(param1: "string1", param2: "string2") : otherMethod(param1: "string1", param2: "string2") 

    // PREFERRED 
    let value = isLoading ? 100 : 200
    let dictionary = isLoading 
                        ? someMethod(param1: "string1", param2: "string2") 
                        : otherMethod(param1: "string1", param2: "string2") 
```

## Optionals

### Force Unwrapping <a name="optionals-force-unwrapping"></a>

- Avoid `force unwrapping` optionals by using `!` or `as!`.
- Safely `unwrap` the `optional` first by using something like `guard let`, `if let`, `guard let as?`, `if let as?`, and `optional chaining`.

```swift
    // UNWRAP
    
    // NOT PREFERRED
    func someMethod() {
        let url = URL(string: "http://www.example.com/")! 
        UIApplication.shared.open(url)
    } 

    // PREFERRED
    func someMethod() {
        guard let url = URL(string: "http://www.example.com/") else {
            return
        }
    }
```
    
```swift
    // DOWNCAST:

    // NOT PREFERRED
    func someMethod() {
        let downcastedObj = object as! Subclass
        otherObject.property = downcastedObj
    }

    // PREFERRED
    func someMethod() {
        guard let downcastedObj = object as? Subclass else {
            return
            }

        otherObject.property = downcastedObj
    }
```

```swift
    // OPTIONAL CHAINING:

    // NOT PREFERRED
    delegate!.someMethod()

    // PREFERRED
    delegate?.someMethod()
```

### Pyramid of Doom <a name="optionals-pyramid-doom"></a>

- when using `if let` statement avoid `Pyramid of Doom`.

```swift
    // NOT PREFERRED
    if let value1 = dict[key1] as? String {
            let value2 = dict[key2] as? String {
                let value3 = dict[key3] as? String {
                    let value4 = dict[key4] as? String {

                        let object = MyClass(value1: value1, value2: value2, value3: value3, value4: value4)

                    }
                }
            }
        }


    // PREFERRED
    if 
       let value1 = dict[key1] as? String,
       let value2 = dict[key2] as? String,
       let value3 = dict[key3] as? String,
       let value4 = dict[key4] as? String {

            let object = MyClass(value1: value1, value2: value2, value3: value3, value4: value4)

    }
```

### Unwrapping Multiple Optionals <a name="optionals-unwrapping-multiple-optionals"></a>
- when `unwrap multiple optionals` put each variable in a new line, followed by a `,` except for the last line where should be placed the `else {` for `guard statement`, or `{` for `if` and `while` statements.

```swift
    // NOT PREFERRED
    guard let value1 = value1,
        let value2 = value2,
        let value3 = value3,
    else {
        return
    }
    
    if let value1 = value1,
        let value2 = value2,
        let value3 = value3,
        {
            // ...
        }
        
    // NOT PREFERRED
    guard let constantOne = valueOne, constantTwo = valueTwo, constantThree = valueThree else {
        return
    }
    
    if let constantOne = valueOne, let constantTwo = valueTwo, let constantThree = valueThree {
        // ...
    }

    // PREFERRED
    guard
        let value1 = value1,
        let value2 = value2,
        let value3 = value3 else {
            return
    }
    
    if
        let value1 = value1,
        let value2 = value2,
        let value3 = value3 {
            // ...
    }
```

### Error Handling <a name="optionals-error-handling"></a>
- Avoid using the `forced-try` expression, more safely `handle errors` using a `do` statement along with `try` and `catch`.

```swift
    // NOT PREFERRED
    func someMethod() {
        // ...
        let result = try! decoder.decode(Class.self, from: data)
        // ...
    }

    // PREFERRED
    func someMethod() {
        do {
            // ...
            let result = try! decoder.decode(Class.self, from: data)
            // ...
        } catch {
            print(error)
        }
    }
```

## Access Modifiers

- As first write the `access modifier` keyword when it is needed.

```swift
    // NOT PREFERRED
    static private let animationDuration: Float

    // PREFERRED
    private static let animationDuration: Float
```

- Prefer `public` to `open` and `private` to `fileprivate` unless you need that behavior.
- Do not write the `internal` access modifier keyword since it is the default.
- Don't repeat the access modifier when `overriding` a method.

## Loops

- Avoid the use of `forEach` except for simple `one line closures`. Prefer the of the `enumerated()` function to loop over a sequence.
- Use `map` when transforming`Arrays`, `compactMap` for `Arrays of Optionals` or `flatMap` for `Arrays of Arrays`.
- If you are not performing a transform, or if there are side effects do not use `map/flatmap`; use a `for in loop` instead. [see more](http://www.mokacoding.com/blog/when-to-use-map-flatmap-for/)

## Delegates

- When creating custom `delegate` methods, an `unnamed first parameter` should be the delegate source. 

```swift
    class ScreenViewController: UIViewController {

    }

    // NOT PREFERRED
    protocol ViewControllerDelegate: class {
        func didSelectButtonA(name: String)
        func shouldReload() -> Bool
    }

    // PREFERRED
    protocol ScreenViewControllerDelegate: class {
        func didSelectButtonA(_ screenViewController: ScreenViewController, name: String)
        func shouldReload(_ screenViewController: ScreenViewController) -> Bool
    }
```

## Organization:

### Constants <a name="organization-constants"></a>

- All `constants` that are instance-independent should be `static`. All such static constants should be placed in a `separate extension` of their `class`, `struct`, or `enum` place on top of the class definition

```swift
    class ScreenViewController: UIViewController {

    }

    // NOT PREFERRED
    class ClassA {
        // MARK: - Constants
        static let constant1 = "constant1"
        static let constant2 = 3

        // ...
    }

    // PREFERRED
    extension ClassA {
        // MARK: - Constants
        static let constant1 = "constant1"
        static let constant2 = 3
    }

    class ClassA {
        // ...
    }
```

### Init Method <a name="organization-init"></a>

- Prefer initializing properties at init time whenever possible, rather than using implicitly unwrapped optionals (exception UIViewController's view property)

/ WRONG
class MyClass {

init() {
super.init()
property1 = "property"
}

var property1: String!
}

// RIGHT
class MyClass {

init() {
property1 = "property"
super.init()
}

var property1: String
}

### Decoupling in methods<a name="organization-decoupling"></a>

- Avoid performing any meaningful or time-intensive work in init(). Extract that logic in one or more separate private methods.
- Extract complex property observers into separate private methods.

// WRONG
class UIButton {
var isLoading: Bool  {
didSet {
guard oldValue != isLoading else {
return
}

// Bunch of related statements.
}
}
}

// RIGHT
class UIButton {
var isLoading: Bool  {
didSet { isLoadingDidUpdate(from: oldValue) }
}

private func isLoadingDidUpdate(from oldValue: Bool) {
guard oldValue != isLoading else {
return
}

// Bunch of related statements.
}
}

- Extract complex callback blocks into separate private methods:

//WRONG
class ClassA {

func someMethod(completion: () -> Void) {
API.someAsyncMethod() { [weak self] param in
if let self = self {
// Processing and side effects
}
completion()
}
}
}

// RIGHT
class MyClass {

func request(completion: () -> Void) {
API.someAsyncMethod() { [weak self] param in
guard let self = self else { return }
self.separatPrivateMethod(param)
completion()
}
}

func separatPrivateMethod(param: SomeClass) {
// Processing and side effects
}
}

- ### Protocols<a name="organization-protocols"></a>

- When an object conform to a prorocol, use a separate extension for the implementation of protocol methods and add a // MARK: - ProtocolName comment to keep things well organized.

// WRONG
class MyViewcontroller: UIViewController, UITableViewDataSource, UITableViewDelegate {
// All methods
}

// RIGHT
class MyViewcontroller: UIViewController {
...
}

// MARK: - UITableViewDataSource
extension MyViewcontroller: UITableViewDataSource {
// Table view data source methods
}

// MARK: - UITableViewDelegate
extension MyViewcontroller: UITableViewDelegate {
// Scroll view delegate methods
}    

- ### Class Organization<a name="organization-class-organization"></a>

- Separate a class fallowing the examples

extention ClassA {
// MARK: - Constants
static let constant1 = "constant1"
static let constant2 = 3
}

class ClassA: SuperClass {
// MARK: - Init
init() {
...
}

init(param1: String, param2: String) {
...
}

// MARK: - Public property
var property1: String = ""
var property2: [String] = []
...

// MARK: - Private property
private var property3: String = ""
private var property4: [String] = []
...

// MARK: - Override property
override var overrideProperty: String {
...
}

override var overrideProperty1: String {
...
}

// In case of overriding life cycle methods 
// MARK: - Life Cycle
override func viewDidLoad() {
super.viewDidLoad()
...
}   

override func viewWillAppear(_ animated: Bool) {
super.viewWillAppear(animated)             
...
}

deinit {
...
}

// MARK: - Override Method 
override func overrideMethod() {
...
}

override func overrideMethod1() {
...
}

// MARK: - Public Method 
func someMethod() {
...
}

func someMethod1() {
...
}

func someMethod3() {
...
}

// MARK: - Private Method - Description
private func someMethod() {
...
}

private func someMethod1() {
...
}

private func someMethod2() {
...
}

// MARK: - Private Method - Description1
private func someMethod3() {
...
}

private func someMethod4() {
...
}

private func someMethod5() {
...
}

// MARK: - Private Method - Description1
...

}

extention ClassA: Protocol1 {
func someProtocolMethod() {
someHelperMethod()
}

private func someHelperMethod() {
...
}

func someProtocolMethod1() {
someHelperMethod1()
}

private func someHelperMethod1() {
...
}
}

extention ClassA: Protocol2 {
...
}

## References

- [Swift Official](https://swift.org/documentation/api-design-guidelines/)
- [Google](https://google.github.io/swift/#file-names)
- [AirBnb](https://github.com/airbnb/swift#use-implicit-types)
- [LinkedIn](https://github.com/linkedin/swift-style-guide)
- [Vokal Engineering](https://engineering.vokal.io/iOS/CodingStandards/Swift.md.html)
- [Raywenderlich](https://github.com/raywenderlich/swift-style-guide)
