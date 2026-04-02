# Airbnb Swift Style Guide — Complete Rule Reference

Source: https://github.com/airbnb/swift

---

## Table of Contents

1. [Xcode Formatting](#1-xcode-formatting)
2. [Naming](#2-naming)
3. [Style — General](#3-style--general)
4. [Style — Functions](#4-style--functions)
5. [Style — Closures](#5-style--closures)
6. [Style — Operators](#6-style--operators)
7. [Patterns](#7-patterns)
8. [File Organization](#8-file-organization)
9. [SwiftUI](#9-swiftui)
10. [Testing](#10-testing)
11. [Tooling: SwiftFormat & SwiftLint](#11-tooling)

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

## 3. Style — General

### 3.1 Trailing Commas
Add a trailing comma after the last element of multi-line, multi-element comma-separated lists. Omit trailing commas in single-element or single-line lists.

**✅ Correct:**
```swift
let planets = [
  mercury,
  venus,
  earth,
  mars,
]

let map = [
  "moon": 1,
  "sun": 1,
]
```

**❌ Incorrect:**
```swift
let planets = [
  mercury,
  venus,
  earth,
  mars   // missing trailing comma
]
```

*Enforced by:* SwiftFormat `trailingCommas` rule.

### 3.2 Collection Literal Spacing
No spaces inside the brackets of collection literals.

**✅ Correct:**
```swift
let objects = [mercury, venus, earth]
let map = ["moon": 1, "sun": 1]
```

**❌ Incorrect:**
```swift
let objects = [ mercury, venus, earth ]
let map = [ "moon": 1, "sun": 1 ]
```

*Enforced by:* SwiftFormat `spaceInsideBrackets` rule.

### 3.3 Named Tuple Members
Always name the members of a tuple for clarity. If a type has more than three fields, consider a struct instead.

**✅ Correct:**
```swift
func coordinates() -> (x: Int, y: Int) {
  return (x: 4, y: 4)
}
let point = coordinates()
print(point.x)
```

**❌ Incorrect:**
```swift
func coordinates() -> (Int, Int) {
  return (4, 4)
}
let point = coordinates()
print(point.0)   // positional access is fragile
```

### 3.4 Colon Spacing
Colons are always followed by a space and never preceded by a space.

**✅ Correct:**
```swift
let planet: CelestialObject = sun
class Planet: CelestialObject { }
let map: [String: Int] = [:]
```

**❌ Incorrect:**
```swift
let planet:CelestialObject = sun
let planet : CelestialObject = sun
class Planet : CelestialObject { }
```

*Enforced by:* SwiftFormat `spaceAroundOperators` rule.

### 3.5 Omit Unnecessary Parentheses
Remove parentheses from conditions, switch statements, and closure parameters when they are not required by the compiler.

**✅ Correct:**
```swift
if userCount > 0 { }
switch planet { }
let evens = numbers.filter { $0.isMultiple(of: 2) }
```

**❌ Incorrect:**
```swift
if (userCount > 0) { }
switch (planet) { }
let evens = numbers.filter { (number) in number.isMultiple(of: 2) }
```

*Enforced by:* SwiftFormat `redundantParens` rule.

### 3.6 Single-Line Comments Over Block Comments
Use `//` and `///` for all comments. Do not use `/* */` block comments.

**✅ Correct:**
```swift
/// A planet in the solar system.
class Planet {
  // Generate atmosphere before oceans
  func terraform() {
    generateAtmosphere()
    generateOceans()
  }
}
```

**❌ Incorrect:**
```swift
/**
 * A planet in the solar system.
 */
class Planet {
  /*
    Generate atmosphere before oceans
  */
  func terraform() { }
}
```

*Enforced by:* SwiftFormat `blockComments` rule.

### 3.7 Doc Comments (`///`)
Use `///` (not `//`) for documentation comments that describe declarations. Place them on the line immediately before the declaration.

**✅ Correct:**
```swift
/// A spacecraft capable of faster-than-light travel.
class Spaceship {
  /// The main propulsion system.
  var engine: Engine

  /// Engages the warp drive.
  func engage() { }
}
```

**❌ Incorrect:**
```swift
// A spacecraft capable of faster-than-light travel.  // plain comment, not doc comment
class Spaceship { }
```

*Enforced by:* SwiftFormat `docComments` rule.

### 3.8 Attribute Placement
Place attributes on the line **above** the declaration they annotate, with one exception: simple stored-property wrappers (like `@State`, `@Published`) that fit on the same line without affecting readability may stay inline.

**✅ Correct:**
```swift
@MainActor
class Spaceship { }

@discardableResult
public func executeRequest() -> URLSessionCancellable { }

// Simple stored property wrappers may stay on the same line
@State private var isEnabled = false
```

**❌ Incorrect:**
```swift
@MainActor class Spaceship { }          // attribute should be on its own line

@discardableResult public func executeRequest() -> URLSessionCancellable { }
```

*Enforced by:* SwiftFormat `wrapAttributes` rule.

### 3.9 Modifier Ordering
All modifiers for a declaration appear on the **same line** as the declaration. Access control comes first.

**✅ Correct:**
```swift
public final class Planet { }
nonisolated public func orbit() { }
private static let gravity = 9.8
```

**❌ Incorrect:**
```swift
public
final
class Planet { }        // each modifier on its own line

static private let gravity = 9.8   // access control not first
```

*Enforced by:* SwiftFormat `modifiersOnSameLine` rule.

### 3.10 Omit Explicit `internal`
The `internal` access level is the Swift default and should never be written explicitly.

**✅ Correct:**
```swift
class Spaceship { }
func launch() { }
```

**❌ Incorrect:**
```swift
internal class Spaceship { }
internal func launch() { }
```

*Enforced by:* SwiftFormat `redundantInternal` rule.

### 3.11 Conditional Assignment Expressions
When initializing a new property using a conditional (`if`/`switch`), prefer Swift's expression form over assigning inside separate branches.

**✅ Correct:**
```swift
let location =
  if let star = planet.star {
    "System: \(star.name)"
  } else {
    "Rogue planet"
  }

let fuelType = switch engine {
  case .ion: "xenon"
  case .chemical: "liquid oxygen"
}
```

**❌ Incorrect:**
```swift
var location: String
if let star = planet.star {
  location = "System: \(star.name)"
} else {
  location = "Rogue planet"
}
```

*Enforced by:* SwiftFormat `conditionalAssignment` rule.

### 3.12 Type Shorthand Syntax
Prefer Swift shorthand syntax for standard library types.

**✅ Correct:**
```swift
var names: [String]
var lookup: [String: Int]
var optional: String?
```

**❌ Incorrect:**
```swift
var names: Array<String>
var lookup: Dictionary<String, Int>
var optional: Optional<String>
```

*Enforced by:* SwiftFormat `typeSugar` rule.

---

## 4. Style — Functions

### 4.1 Omit `Void` Return Type
```swift
// ✅
func launch() { }

// ❌
func launch() -> Void { }
```

### 4.2 Long Signatures — Break After Opening Paren
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

### 4.3 Default Parameters Last
Parameters with default values go at the **end** of the parameter list.

### 4.4 Unused Parameters → `_`
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

## 5. Style — Closures

### 5.1 Trailing Closure Syntax
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

### 5.2 `Void` in Closure Return Types
Use `Void` (not `()`) for closure return types.

```swift
// ✅
var completion: () -> Void

// ❌
var completion: () -> ()
```

### 5.3 Capture Lists — Prefer `weak` Over `unowned`
Avoid `unowned` references in capture lists. Prefer `weak` to prevent crashes from dangling references.

```swift
// ✅
button.onTap = { [weak self] in
  self?.launch()
}

// ❌
button.onTap = { [unowned self] in
  self.launch()    // crashes if self is deallocated
}
```

### 5.4 Unused Closure Parameters → `_`
```swift
// ✅
something.observe { _, newValue in
  update(newValue)
}
```

---

## 6. Style — Operators

### 6.1 Infix Operator Spacing
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

### 6.2 Range Operators — No Spaces
Do not add spaces around range operators (`...`, `..<`).

```swift
// ✅
let range = 1...10
for i in 0..<count { }

// ❌
let range = 1 ... 10
for i in 0 ..< count { }
```

### 6.3 Ternary Operator — Wrap Before `?` and `:`
When a ternary expression must wrap, break before `?` and before `:`.

```swift
// ✅
let result = someCondition
  ? valueIfTrue
  : valueIfFalse

// ❌
let result = someCondition ?
  valueIfTrue :
  valueIfFalse
```

### 6.4 Use Commas Instead of `&&` in Conditions
In `if`, `guard`, and `while` statements, separate conditions with commas rather than `&&`.

```swift
// ✅
if let star = planet.star, star.isSun {
  ignite()
}

guard isEnabled, userCount > 0 else { return }

// ❌
if let star = planet.star && star.isSun {
  ignite()
}
```

---

## 7. Patterns

### 7.1 Omit Explicit Types When Inferable

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

### 7.2 No `.init` + Explicit Type (prefer constructor call directly)

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

### 7.3 Omit Redundant `self`
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

### 7.4 Access Control
- Use the **most restrictive** access level that still works
- Prefer `private` over `fileprivate`
- Prefer `public` over `open` (open allows subclassing across modules — only use when that is explicitly intended)
- Omit `internal` — it is the default and should never be written explicitly
- In extensions, specify access control **individually per declaration** rather than on the extension itself

**✅ Correct:**
```swift
public struct Spaceship {
  private let engine: Engine
}

extension Spaceship {
  public func blastOff() { }
  private func checkSystems() { }
}
```

**❌ Incorrect:**
```swift
internal class Spaceship { }   // internal is implicit — omit it

public extension Spaceship {
  func blastOff() { }          // access level on extension, not per-declaration
}
```

### 7.5 `guard` for Early Exit

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

### 7.6 Optional Handling — No Force-Unwrap
- Prefer `if let`, `guard let`, or optional chaining over `!`
- Force-unwrap is acceptable only in tests or when a documented invariant makes `nil` impossible

### 7.7 Caseless Enums for Namespacing
Use caseless `enum`s (not structs or classes) to group related `public` or `internal` constants and functions into namespaces. Enums cannot be instantiated, which prevents accidental misuse.

**✅ Correct:**
```swift
enum Environment {
  enum Earth {
    static let gravity = 9.8
  }
  enum Moon {
    static let gravity = 1.6
  }
}
```

**❌ Incorrect:**
```swift
struct Environment {
  static let earthGravity = 9.8   // struct can be instantiated accidentally
  static let moonGravity = 1.6
}
```

*Enforced by:* SwiftFormat `enumNamespaces` rule.

### 7.8 Prefer `for` Loops Over `forEach`
Use a `for` loop by default. `forEach` is acceptable only as the **last element in a functional chain**.

**✅ Correct:**
```swift
for planet in planets {
  planet.terraform()
}

// forEach at the end of a chain is fine
planets
  .filter { !$0.isGasGiant }
  .forEach { $0.terraform() }
```

**❌ Incorrect:**
```swift
planets.forEach { planet in   // not at end of a chain — use for loop
  planet.terraform()
}
```

*Enforced by:* SwiftFormat `preferForLoop` rule.

### 7.9 Avoid Global Functions
Prefer methods defined within a type over free functions. Use type nesting or extensions to keep related behavior together.

**✅ Correct:**
```swift
class Person {
  var age: Int { /* calculation */ }
  func jump() { }
}
```

**❌ Incorrect:**
```swift
func age(of person: Person) -> Int { }
func jump(person: Person) { }
```

### 7.10 `Sendable` and Concurrency
- Prefer restructuring code so the compiler can statically verify `Sendable`
- Use `@preconcurrency` only for APIs you don't own
- Avoid `@unchecked Sendable`; document why if you must use it

### 7.11 Prefer `final` Classes
Mark classes `final` by default unless inheritance is explicitly required. This improves performance and communicates intent.

```swift
// ✅
final class Spaceship { }

// Only omit final when subclassing is intentional
class Vehicle { }
final class Spaceship: Vehicle { }
```

*Enforced by:* SwiftFormat `preferFinalClasses` rule.

---

## 8. File Organization

### 8.1 `// MARK:` Comments
```swift
// MARK: - Lifecycle
// MARK: - Public
// MARK: - Private
// MARK: - UITableViewDataSource
```

### 8.2 Protocol Conformances in Extensions
```swift
// MARK: - UITableViewDelegate
extension MyViewController: UITableViewDelegate {
  // conformance methods here
}
```

### 8.3 Import Order
Group and sort alphabetically: Swift stdlib → Apple frameworks → third-party → internal

```swift
import Foundation
import UIKit

import Alamofire
import SnapKit

import MyAppCore
```

---

## 9. SwiftUI

### 9.1 Keep `body` Simple
Extract sub-views or `@ViewBuilder` methods when `body` grows complex.

### 9.2 Previews
Include a `#Preview` for every view. Use realistic data.

### 9.3 Omit Redundant `EmptyView` Branches
An `if` statement without an `else` branch implicitly produces no content when the condition is false, so an explicit `else { EmptyView() }` is redundant and should be removed.

**✅ Correct:**
```swift
var body: some View {
  if condition {
    Text("Launch")
  }
}
```

**❌ Incorrect:**
```swift
var body: some View {
  if condition {
    Text("Launch")
  } else {
    EmptyView()
  }
}
```

*Enforced by:* SwiftFormat `redundantEmptyView` rule.

### 9.4 Property Wrappers

| Wrapper | Use when |
|---------|---------|
| `@State` | Local, view-owned mutable state |
| `@Binding` | Two-way state passed from a parent |
| `@StateObject` | View owns the observable object's lifetime |
| `@ObservedObject` | View does not own the object's lifetime |
| `@EnvironmentObject` | Shared dependency injected via environment |

Do not use `@State` for data that outlives the view.

---

## 10. Testing

### 10.1 Test Naming
Name tests so the failure message is self-explanatory:
```swift
func test_launch_whenFuelIsEmpty_throwsError() { }
```

### 10.2 Arrange-Act-Assert Structure
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

### 10.3 No Logic in Tests
No `if`, `for`, or `switch` inside test methods. Use separate test methods or a data-driven approach.

### 10.4 One Concept Per Test
Multiple `XCTAssert` calls are fine as long as they all belong to the same behavior.

### 10.5 Swift Testing: Raw Identifiers for Test Function Names
When using Swift Testing, name test functions using **raw identifiers** (backtick-wrapped, space-separated words) instead of camelCase. Never put display names inside the `@Test` macro. Suite type names use regular UpperCamelCase.

**✅ Correct:**
```swift
struct SpaceshipTests {
  @Test
  func `warp drive enables FTL travel`() { ... }

  @Test
  func `launch fails when fuel is empty`() { ... }
}
```

**❌ Incorrect:**
```swift
// camelCase function name
@Test func testWarpDriveEnablesFTLTravel() { ... }

// Display name string in macro
@Test("Warp drive enables FTL travel")
func warpDriveEnablesFTLTravel() { ... }

// Raw identifier on suite type
struct `Spaceship tests` { ... }
```

*Requires:* Swift SE-0451 raw identifiers (available in Swift 6+).

### 10.6 Swift Testing: Omit Redundant `@Suite` Macro
Do not annotate a test suite with `@Suite` unless you are passing arguments (e.g., `.serialized`). The macro with no arguments is redundant — `@Test` discovery works the same without it.

**✅ Correct:**
```swift
struct SpaceshipTests {
  @Test
  func `warp drive enables FTL travel`() { ... }
}

// @Suite with arguments is fine
@Suite(.serialized)
struct SpaceshipIntegrationTests {
  @Test
  func `launch sequence completes`() { ... }
}
```

**❌ Incorrect:**
```swift
@Suite                        // no arguments — redundant
struct SpaceshipTests {
  @Test
  func `warp drive enables FTL travel`() { ... }
}
```

### 10.7 Swift Testing: Avoid Redundant `#expect` Messages
Do not pass a message string to `#expect` when it merely restates the condition. The macro already generates detailed failure output. Only add a message when it provides genuinely new context; a code comment is often better.

**✅ Correct:**
```swift
// No message — the macro output is sufficient
#expect(spaceship.isWarpDriveActive)
#expect(spaceship.speed > lightSpeed)

// Contextual message that adds new information
#expect(spaceship.speed > lightSpeed,
  "Spaceship must reach light speed before warp bubble forms")

// Or use a comment instead
// Spaceship must reach light speed before warp bubble forms
#expect(spaceship.speed > lightSpeed)
```

**❌ Incorrect:**
```swift
#expect(spaceship.isWarpDriveActive, "Warp drive should be active")
#expect(spaceship.speed > lightSpeed, "Speed should be greater than light speed")
```

---

## 11. Tooling

### SwiftFormat
Handles whitespace, indentation, trailing commas, redundant `self`, brace style, and many style rules.
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
