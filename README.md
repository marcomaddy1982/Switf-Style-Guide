# Switf-Style-Guide

## Table of Contents

- [General](#general)
- [Naming](#naming)
    - [General](#naming-general)
    - [File](#naming-file)
    - [Struct, Class, Protocol](#naming-struct-class-protocol)
    - [Variable, Constant](#naming-variable-constant)
    - [Method, Function](#naming-method-function)
    - [Source](#source)

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
    }
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

- Use camelCase (initial lowercase letter) for property, constant, variable
- When declaring variables you should be verbose, but not too verbose.
- Make the name descriptive. (Source Vokal Engineering)
- Should be written first with their most general part and then with their most specific part.

```swift
    // NOT PREFERRED
    let topTextBorder: CGFloat
    let bottomTextBorder: CGFloat

    // PREFERRED
    let textBorderTop: CGFloat
    let textMarginLeft: CGFloat
```
- Include an information about type in a name. Once should be enough. (Source Airbnb)

```swift
    // NOT PREFERRED
    let name: String
    var users = [User]()
    var conversations = [String: String]()

    // PREFERRED
    let nameText: String
    var userList = [User]()
    var conversationDictionary = [String: String]()
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

- Event-handling functions should be named like past-tense sentences.

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

## Source
