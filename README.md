# Switf-Style-Guide

## Table of Contents

- [General](#general)
- [Naming](#naming)
    - [General](#naming-general)
    - [File](#naming-file)
    - [Struct, Class, Protocol](#naming-struct-class-protocol)
    - [Variable, Constant](#naming-variable-constant)
    - [Method, Function](#naming-method-function)

## General

- Use Swift types whenever possible (Array, Dictionary, Set, String, etc.) instead of types from Objective-C. Many Objective-C types can be automatically converted to Swift types and vice versa.

```swift
    // NOT PREFERRED
    let nameLabelText = NSString(format: "%@/%@", firstName, lastName)

    // PREFERRED
    let nameLabelText = "\(firstName)/\(lastName)"
    let alsoNameLabelText = firstName + "/" + lastName
```

- Swift Collection Types: Do not make NSArray, NSDictionary, and NSSet properties or variables. If you need to use a specific method only found on a Foundation collection, cast your Swift type in order to use that method.

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

- A file containing a single type MyType is named MyType.swift.
- A file containing a type MyType and some top-level helper functions is also named MyType.swift. 
- A file containing a single extension to a type MyType that adds conformance to a protocol MyProtocol is named MyType+MyProtocol.swift.
- A file containing multiple extensions to a type MyType that add conformances, nested types, or other functionality to a type can be named more generally, as long as it is prefixed with MyType+; for example, MyType+Extentions.swift.
- A file containing related declarations that are not otherwise scoped under a common type or namespace (such as a collection of global mathematical functions) can be named descriptively; for example, Math.swift.
- There is no need for Objective-C style prefixing in Swift.

### Struct, Class, Protocol <a name="naming-struct-class-protocol"></a>

- Use PascalCase for struct, enum, class, typedef, associatedtype names.

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

- Do not add a class prefix as in Objective-C-style. (Source Airbnb)

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

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

Curabitur pretium tincidunt lacus. Nulla gravida orci a odio. Nullam varius, turpis et commodo pharetra, est eros bibendum elit, nec luctus magna felis sollicitudin mauris. Integer in mauris eu nibh euismod gravida. Duis ac tellus et risus vulputate vehicula. Donec lobortis risus a elit. Etiam tempor. Ut ullamcorper, ligula eu tempor congue, eros est euismod turpis, id tincidunt sapien risus a quam. Maecenas fermentum consequat mi. Donec fermentum. Pellentesque malesuada nulla a mi. Duis sapien sem, aliquet nec, commodo eget, consequat quis, neque. Aliquam faucibus, elit ut dictum aliquet, felis nisl adipiscing sapien, sed malesuada diam lacus eget erat. Cras mollis scelerisque nunc. Nullam arcu. Aliquam consequat. Curabitur augue lorem, dapibus quis, laoreet et, pretium ac, nisi. Aenean magna nisl, mollis quis, molestie eu, feugiat in, orci. In hac habitasse platea dictumst.

### Method, Function <a name="naming-method-function"></a>

Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

Curabitur pretium tincidunt lacus. Nulla gravida orci a odio. Nullam varius, turpis et commodo pharetra, est eros bibendum elit, nec luctus magna felis sollicitudin mauris. Integer in mauris eu nibh euismod gravida. Duis ac tellus et risus vulputate vehicula. Donec lobortis risus a elit. Etiam tempor. Ut ullamcorper, ligula eu tempor congue, eros est euismod turpis, id tincidunt sapien risus a quam. Maecenas fermentum consequat mi. Donec fermentum. Pellentesque malesuada nulla a mi. Duis sapien sem, aliquet nec, commodo eget, consequat quis, neque. Aliquam faucibus, elit ut dictum aliquet, felis nisl adipiscing sapien, sed malesuada diam lacus eget erat. Cras mollis scelerisque nunc. Nullam arcu. Aliquam consequat. Curabitur augue lorem, dapibus quis, laoreet et, pretium ac, nisi. Aenean magna nisl, mollis quis, molestie eu, feugiat in, orci. In hac habitasse platea dictumst.
