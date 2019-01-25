# Snippets

#### Dismiss Keyboard
```swift
firstNameTextField.becomeFirstResponder()
```

#### Devie Type
```swift
switch UIDevice.current.userInterfaceIdiom {
    case .unspecified:
        print("unknown")
    case .phone:
        print("phone")
    case .pad:
        print("pad" )
    case .tv:
        print("tv" )
    case .carPlay:
        print("carPlay")
}
```

#### Closures
```swift
// Komplett
let c1 : (Int) -> Int = { (i : Int) ->  Int in return i - 1 }

// Ohne Typ-Angabe : Typinferenz
let c2 = { (i : Int) -> Int in return i-1 }

// Implizites Return bei nur einer Anweisung
let c3 = { (i : Int) -> Int in i-1 }

// Argumente automatisch $0, $1, etc zugewiesen
let c4 = { $0 -1 }

let d1 = { $0 > $1 }

let d2 = >
```

Alle c (c1-c4) und d - Closures sind Synonyme für einander.

**Trailing Closures**
Falls Closure als letztes Argument erwartet wird.
```swift
func myTrailingClosureFunc(i: Int, closure: () -> Void) {
    closure()
    print(i)
}

// Aufruf 1 
myTrailingClosure(i: 2, closure: {
    print("Normal Closure Impl")
})
// Aufruf 2 (Synonym)
myTrailingClosure(i: 2) {
    print("Trailing Closure Impl")
}
```

#### Array Persitenz
```swift
// Write
let myArray = ["Eins", "Zwei", "Drei"]
(myArray as NSArray).write(to: filePath, atomically: true)

// Read
if let readArray = NSArray(contentsOf: filePath) {
    print(readArray)
}
```