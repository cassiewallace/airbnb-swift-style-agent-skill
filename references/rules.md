# Airbnb Swift Style Guide — Complete Rule Reference

Source: https://github.com/airbnb/swift

---

## Table of Contents

1. [Xcode Formatting](#1-xcode-formatting)
2. [Naming](#2-naming)
3. [Style — Functions](#3-style--functions)
4. [Style — Closures](#4-style--closures)
5. [Style — Operators](#5-style--operators)
6. [Patterns](#6-patterns)
7. [File Organization](#7-file-organization)
8. [SwiftUI](#8-swiftui)
9. [Testing](#9-testing)
10. [Tooling: SwiftFormat & SwiftLint](#10-tooling)

---

## 1. Xcode Formatting

### 1.1 Column Width
- **Max 100 characters per line** (preferred)
- **130 characters** is the strict linting limit — the formatter will break lines at this point
- Rationale: Readable on modern screens while still allowing side-by-side diffs

### 1.2 Indentation
- **Use 2 spaces** — never tabs
- Indentation must match what Xcode would produce automatically

### 1.3 Trailing Whitespace
- **Remove all trailing whitespace** on every line
- Configure Xcode: Preferences → Text Editing → Automatically trim trailing whitespace

---

## 2. Naming

### 2.1 Case Conventions

| Entity | Convention | Example |
|--------|-----------|---------|
| Types (class, struct, enum, actor) | PascalCase | `SpaceFleet` |
| Protocols | PascalCase | `SpaceThing` |
| Enum cases | lowerCamelCase | `.warpDrive` |
| Functions and methods | lowerCamelCase | `launchRocket()` |
| Variables and properties | lowerCamelCase | `fuelLevel` |
| Constants | lowerCamelCase | `maximumSpeed` |
| Generic type parameters | PascalCase | `T`, `Element` |

**✅ Correct:**
```swift
protocol SpaceThing { }
class SpaceFleet: SpaceThing { }
struct Spaceship { }
enum Planet {
  case mercury, venus, earth
}
let myFleet = SpaceFleet()
```

**❌ Incorrect:**
```swift
protocol spaceThing { }          // should be PascalCase
class spaceFleet { }             // should be PascalCase
let MyFleet = SpaceFleet()       // should be lowerCamelCase
enum planet {                    // should be PascalCase
  case Mercury, Venus, Earth     // enum cases should be lowerCamelCase
}
```

### 2.2 Boolean Names
- Use `is`, `has`, `does`, `can`, `should` prefixes to clearly signal a boolean

**✅ Correct:**
```swift
var isSpaceship: Bool
var hasSpacesuit: Bool
var doesSupportDocking: Bool
```

**❌ Incorrect:**
```swift
var spaceship: Bool    // ambiguous — could be a Spaceship object
var spacesuit: Bool    // not obviously a boolean
```

### 2.3 Acronym Capitalization
- **All-caps** in the middle or end of a name: `URL`, `ID`, `HTTP`
- **All-lowercase** at the *start* of a lowerCamelCase name: `url`, `id`, `http`

**✅ Correct:**
```swift
class URLValidator {
  func isValidURL(_ url: URL) -> Bool { }
  func isProfileURL(_ url: URL, for userID: String) -> Bool { }
}
let urlValidator = URLValidator()
let userID = "abc123"
```

**❌ Incorrect:**
```swift
class UrlValidator {                              // URL should be all-caps in type names
  func isValidUrl(_ URL: URL) -> Bool { }        // url should be lowercase at start of lowerCamelCase
  func isProfileUrl(_ url: URL, for userId: String) -> Bool { } // ID should be all-caps mid-name
}
let URLValidator = UrlValidator()                 // variable should be lowerCamelCase
```

### 2.4 Event Handler Naming
- Name event-handling functions as **past-tense sentences**
- Subject (the triggering view) may be omitted when context makes it clear
- Prefer `didTap` over `handleTap`, `tap`, `onTap`, or `tapped`

**✅ Correct:**
```swift
private func didTapBookButton() { }
private func didTapBook() { }
private func modelDidChange() { }
```

**❌ Incorrect:**
```swift
private func handleBookButtonTap() { }   // "handle" prefix
private func bookButtonTapped() { }      // present-perfect, not past-tense sentence
private func onTapBook() { }             // "on" prefix
private func tapBook() { }              // imperative
```

### 2.5 No Objective-C Class Prefixes
- Swift modules provide namespacing — two- or three-letter prefixes are unnecessary

**✅ Correct:**
```swift
class Account { }
class NetworkManager { }
```

**❌ Incorrect:**
```swift
class AIRAccount { }
class NYTNetworkManager { }
```

### 2.6 Private Backing Properties
- A single leading underscore `_` is allowed **only** when a private property backs an identically-named public property (type erasure, backing a `Published` value, etc.)
- Do not use underscores anywhere else

**✅ Correct (exception):**
```swift
public var isEnabled: Bool { _isEnabled }
private var _isEnabled = true
```

---

## 3. Style — Functions

### 3.1 Omit `Void` Return Type
```swift
// ✅
func launch() { }

// ❌
func launch() -> Void { }
```

### 3.2 Long Signatures — Break After Opening Paren
When a function signature exceeds the line limit, break after `(` and indent each parameter by 2 spaces:

```swift
// ✅
func move(
  from origin: Planet,
  to destination: Planet,
  using vehicle: Spaceship
) -> TravelPlan {

// ❌
func move(from origin: Planet, to destination: Planet, using vehicle: Spaceship) -> TravelPlan {
```

### 3.3 Default Parameters Last
Parameters with default values go at the **end** of the parameter list.

---

## 4. Style — Closures

### 4.1 Trailing Closure Syntax
Use trailing closure syntax when the closure is the last argument. Omit parentheses when it is the only argument.

```swift
// ✅
UIView.animate(withDuration: 0.3) {
  self.view.alpha = 0
}

planets.map { $0.name }

// ❌
UIView.animate(withDuration: 0.3, animations: {
  self.view.alpha = 0
})
```

### 4.2 Capture Lists
```swift
// ✅
button.onTap = { [weak self] in
  self?.launch()
}
```

### 4.3 Unused Parameters → `_`
```swift
// ✅
something.observe { _, newValue in
  update(newValue)
}

// ❌
something.observe { oldValue, newValue in   // oldValue unused
  update(newValue)
}
```

---

## 5. Style — Operators

### 5.1 Spacing
- Single space on **both sides** of binary operators
- No space between a unary operator and its operand

```swift
// ✅
let sum = a + b
let isNegative = -value < 0

// ❌
let sum = a+b
let sum = a +b
```

---

## 6. Patterns

### 6.1 Omit Explicit Types When Inferable

**✅ Correct:**
```swift
let sun = Star(mass: 1.989e30)
let earth = Planet.earth
let isEnabled = true
let count = 8
```

**❌ Incorrect:**
```swift
let sun: Star = Star(mass: 1.989e30)
let earth: Planet = Planet.earth
let isEnabled: Bool = true
let count: Int = 8
```

**When explicit types ARE required:**
```swift
let mass: Double          // no right-hand side
let moon: PlanetaryBody = earth.moon  // RHS is not a static member
let names: [String] = []  // empty collection literal
let value: Double? = nil  // nil needs a type
```

### 6.2 No `.init` + Explicit Type (prefer constructor call directly)

**✅ Correct:**
```swift
struct SolarSystemBuilder {
  let sun = Star(mass: 1.989e30)
  let earth = Planet.earth
}
```

**❌ Incorrect:**
```swift
struct SolarSystemBuilder {
  let sun: Star = .init(mass: 1.989e30)   // don't combine explicit type + .init
  let earth: Planet = .earth              // don't combine explicit type + leading dot
}
```

### 6.3 Omit Redundant `self`
Only use `self.` when the language **requires** it (disambiguating a parameter from a property, or capturing in a closure).

**✅ Correct:**
```swift
init(capacity: Int, allowsPets: Bool) {
  self.capacity = capacity          // required: param name matches property
  isFamilyFriendly = !allowsPets   // no self needed
}

func save() {
  repository.save(item)            // no self needed
}
```

**❌ Incorrect:**
```swift
init(capacity: Int, allowsPets: Bool) {
  self.capacity = capacity
  self.isFamilyFriendly = !allowsPets   // redundant self
}

func save() {
  self.repository.save(self.item)       // both redundant
}
```

### 6.4 Access Control
- Use the **most restrictive** access level that still works
- Explicitly mark `private` or `fileprivate` — don't rely on internal-by-default

### 6.5 `guard` for Early Exit

**✅ Correct:**
```swift
func launch(_ rocket: Rocket?) {
  guard let rocket else { return }
  rocket.ignite()
}
```

**❌ Incorrect:**
```swift
func launch(_ rocket: Rocket?) {
  if let rocket = rocket {
    rocket.ignite()
  }
}
```

### 6.6 Optional Handling — No Force-Unwrap
- Prefer `if let`, `guard let`, or optional chaining over `!`
- Force-unwrap is acceptable only in tests or when a documented invariant makes `nil` impossible

### 6.7 `Sendable` and Concurrency
- Prefer restructuring code so the compiler can statically verify `Sendable`
- Use `@preconcurrency` only for APIs you don't own
- Avoid `@unchecked Sendable`; document why if you must use it

---

## 7. File Organization

### 7.1 `// MARK:` Comments
```swift
// MARK: - Lifecycle
// MARK: - Public
// MARK: - Private
// MARK: - UITableViewDataSource
```

### 7.2 Protocol Conformances in Extensions
```swift
// MARK: - UITableViewDelegate
extension MyViewController: UITableViewDelegate {
  // conformance methods here
}
```

### 7.3 Import Order
Group and sort alphabetically: Swift stdlib → Apple frameworks → third-party → internal

```swift
import Foundation
import UIKit

import Alamofire
import SnapKit

import MyAppCore
```

---

## 8. SwiftUI

### 8.1 Keep `body` Simple
Extract sub-views or `@ViewBuilder` methods when `body` grows complex.

### 8.2 Previews
Include a `#Preview` for every view. Use realistic data.

### 8.3 Property Wrappers

| Wrapper | Use when |
|---------|---------|
| `@State` | Local, view-owned mutable state |
| `@Binding` | Two-way state passed from a parent |
| `@StateObject` | View owns the observable object's lifetime |
| `@ObservedObject` | View does not own the object's lifetime |
| `@EnvironmentObject` | Shared dependency injected via environment |

Do not use `@State` for data that outlives the view.

---

## 9. Testing

### 9.1 Test Naming
Name tests so the failure message is self-explanatory:
```swift
func test_launch_whenFuelIsEmpty_throwsError() { }
```

### 9.2 Arrange-Act-Assert Structure
```swift
func test_speed_afterBoost_isDoubled() {
  // Arrange
  let ship = Spaceship(speed: 10)

  // Act
  ship.boost()

  // Assert
  XCTAssertEqual(ship.speed, 20)
}
```

### 9.3 No Logic in Tests
No `if`, `for`, or `switch` inside test methods. Use separate test methods or a data-driven approach.

### 9.4 One Concept Per Test
Multiple `XCTAssert` calls are fine as long as they all belong to the same behavior.

---

## 10. Tooling

### SwiftFormat
Handles whitespace, indentation, trailing commas, redundant `self`, brace style.
```bash
brew install swiftformat
swiftformat .
swiftformat --lint .
```

### SwiftLint
Handles naming conventions, complexity, and style patterns.
```bash
brew install swiftlint
swiftlint
swiftlint --fix
```

### Airbnb SPM Plugin
```bash
swift package format
swift package format --lint
swift package format --exclude Tests
swift package format --targets MyTarget
swift package format --swift-version 6.2
```

A non-zero exit code means there are violations.

---

*Source: https://github.com/airbnb/swift*
