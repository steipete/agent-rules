---
description: Swift Observation framework documentation and usage patterns for reactive programming
globs: "**/*.swift"
alwaysApply: false
---

# Swift Observation Framework

The Observation framework provides a robust implementation of the observer design pattern in Swift, introduced in iOS 17, macOS 14, tvOS 17, and watchOS 10 (Swift 5.9+).

## Overview

The Observation framework centers around the `@Observable` macro, which automatically tracks changes to properties and notifies observers. This framework represents a significant improvement over the older `ObservableObject` protocol, offering better performance and a cleaner API.

### Key Benefits

- **Fine-grained observation**: SwiftUI views only update when observed properties actually change, not when any property on the object changes
- **Cleaner code**: No need for `@Published` property wrappers on every property
- **Better performance**: Views track dependencies at the property level, minimizing unnecessary view updates
- **Simpler syntax**: Less boilerplate compared to `ObservableObject`

## Using @Observable

To make a type observable, simply apply the `@Observable` macro to the class declaration:

```swift
import Observation

@Observable
class GameState {
    var score: Int = 0
    var level: Int = 1
    var playerName: String = ""

    // Private or computed properties work normally
    private var internalState: String = ""

    var displayName: String {
        "Player: \(playerName)"
    }
}
```

The `@Observable` macro automatically:
- Adds conformance to the `Observable` protocol
- Instruments property accessors to track reads and writes
- Manages observer registration and notification

## SwiftUI Integration

### Using Observable Objects in Views

With `@Observable`, you use `@State` instead of `@StateObject`:

```swift
import SwiftUI

struct GameView: View {
    @State private var game = GameState()

    var body: some View {
        VStack {
            Text("Score: \(game.score)")
            Text("Level: \(game.level)")

            Button("Increase Score") {
                game.score += 10
            }
        }
    }
}
```

**Important**: Only the specific properties read in the view's body will trigger updates. If `body` only reads `game.score`, changes to `game.level` won't cause a re-render.

### Passing Observable Objects

For child views that need to modify observable properties, use `@Bindable`:

```swift
struct PlayerEditor: View {
    @Bindable var game: GameState

    var body: some View {
        Form {
            // @Bindable allows creating bindings to properties
            TextField("Player Name", text: $game.playerName)

            Stepper("Level: \(game.level)", value: $game.level)
        }
    }
}

struct ParentView: View {
    @State private var game = GameState()

    var body: some View {
        PlayerEditor(game: game)
    }
}
```

### Environment Objects

Observable objects can be passed through the environment:

```swift
struct GameApp: View {
    @State private var game = GameState()

    var body: some View {
        ContentView()
            .environment(game)
    }
}

struct ContentView: View {
    @Environment(GameState.self) private var game

    var body: some View {
        Text("Score: \(game.score)")
    }
}
```

## Property-Level Control

### @ObservationTracked

Explicitly mark properties as tracked (usually automatic):

```swift
@Observable
class DataModel {
    @ObservationTracked var trackedValue: String = ""
}
```

Typically, you don't need to use `@ObservationTracked` explicitly, as `@Observable` instruments all stored properties automatically.

### @ObservationIgnored

Prevent specific properties from being observed:

```swift
@Observable
class CacheManager {
    var cachedData: [String: Data] = [:]

    // This property won't trigger observation updates
    @ObservationIgnored var lastAccessTime: Date = Date()

    // Internal implementation details
    @ObservationIgnored private var cacheHits: Int = 0
}
```

Use `@ObservationIgnored` for:
- Internal implementation details
- Properties that change frequently but don't affect UI
- Cached or derived data that shouldn't trigger updates
- Performance optimization when certain properties are read in loops

## Migrating from ObservableObject

### Old Pattern (ObservableObject)

```swift
import Combine

class OldGameState: ObservableObject {
    @Published var score: Int = 0
    @Published var level: Int = 1
    @Published var playerName: String = ""
}

struct OldGameView: View {
    @StateObject private var game = OldGameState()

    var body: some View {
        Text("Score: \(game.score)")
    }
}
```

### New Pattern (@Observable)

```swift
import Observation

@Observable
class GameState {
    var score: Int = 0
    var level: Int = 1
    var playerName: String = ""
}

struct GameView: View {
    @State private var game = GameState()

    var body: some View {
        Text("Score: \(game.score)")
    }
}
```

### Migration Checklist

1. **Replace `ObservableObject` with `@Observable`**
   - Remove `import Combine`
   - Add `import Observation`
   - Change `class Model: ObservableObject` to `@Observable class Model`

2. **Remove property wrappers**
   - Delete `@Published` from properties
   - Properties are observable by default

3. **Update view property wrappers**
   - Replace `@StateObject` with `@State`
   - Replace `@ObservedObject` with `var` (plain property)
   - Replace `@EnvironmentObject` with `@Environment`

4. **Use `@Bindable` for two-way binding**
   - When you need `$` bindings in child views
   - Replaces patterns where you passed `@ObservedObject`

5. **Add `@ObservationIgnored` where needed**
   - For properties that were not `@Published` before
   - For internal state that shouldn't trigger updates

### Key Differences

| Feature | ObservableObject | @Observable |
|---------|-----------------|-------------|
| Import | `import Combine` | `import Observation` |
| Protocol conformance | Explicit | Automatic via macro |
| Published properties | Explicit `@Published` | Automatic for all properties |
| View ownership | `@StateObject` | `@State` |
| View observation | `@ObservedObject` | Plain property |
| Environment | `@EnvironmentObject` | `@Environment(Type.self)` |
| Bindings | Automatic | Use `@Bindable` |
| Update granularity | Object-level | Property-level |

## Advanced Patterns

### Computed Properties and Observation

Computed properties work naturally with `@Observable`:

```swift
@Observable
class ShoppingCart {
    var items: [Item] = []

    // This computed property is automatically observed
    // Views reading it will update when items changes
    var total: Double {
        items.reduce(0) { $0 + $1.price }
    }

    var itemCount: Int {
        items.count
    }
}
```

### Nested Observable Objects

```swift
@Observable
class UserProfile {
    var name: String = ""
    var settings: UserSettings = UserSettings()
}

@Observable
class UserSettings {
    var notifications: Bool = true
    var theme: String = "light"
}

struct ProfileView: View {
    @State private var profile = UserProfile()

    var body: some View {
        VStack {
            // Observes both profile.name and profile.settings.notifications
            Text("\(profile.name)")
            Toggle("Notifications", isOn: $profile.settings.notifications)
        }
    }
}
```

### Conditional Observation

You can control observation based on runtime conditions:

```swift
@Observable
class ConditionalModel {
    var isEnabled: Bool = true

    @ObservationIgnored private var _internalValue: String = ""

    var value: String {
        get {
            isEnabled ? _internalValue : ""
        }
        set {
            if isEnabled {
                _internalValue = newValue
            }
        }
    }
}
```

### Async Operations

Observable objects work seamlessly with Swift concurrency:

```swift
@Observable
class DataLoader {
    var items: [Item] = []
    var isLoading: Bool = false
    var error: Error?

    func loadData() async {
        isLoading = true
        defer { isLoading = false }

        do {
            items = try await fetchItems()
            error = nil
        } catch {
            self.error = error
        }
    }
}

struct DataView: View {
    @State private var loader = DataLoader()

    var body: some View {
        Group {
            if loader.isLoading {
                ProgressView()
            } else if let error = loader.error {
                Text("Error: \(error.localizedDescription)")
            } else {
                List(loader.items) { item in
                    Text(item.name)
                }
            }
        }
        .task {
            await loader.loadData()
        }
    }
}
```

## Best Practices

### 1. Use @Observable for Model Objects

```swift
// ✅ Good: Observable model
@Observable
class UserSession {
    var user: User?
    var isAuthenticated: Bool = false
}

// ❌ Avoid: Struct can't be @Observable (use classes)
@Observable
struct UserSession { // Compiler error
    var user: User?
}
```

### 2. Minimize Property Dependencies

```swift
// ✅ Good: Specific property access
struct ScoreView: View {
    var game: GameState

    var body: some View {
        Text("Score: \(game.score)") // Only observes score
    }
}

// ⚠️ Less efficient: Whole object in interpolation
struct GameView: View {
    var game: GameState

    var body: some View {
        Text("\(game)") // May observe all properties
    }
}
```

### 3. Use @ObservationIgnored for Internal State

```swift
@Observable
class ImageLoader {
    var image: UIImage?
    var isLoading: Bool = false

    // Don't trigger updates for internal cache
    @ObservationIgnored private var cache: [URL: UIImage] = [:]
    @ObservationIgnored private var activeTask: Task<Void, Never>?
}
```

### 4. Prefer @State for Ownership

```swift
// ✅ Good: View owns the model
struct GameView: View {
    @State private var game = GameState()

    var body: some View {
        GameBoard(game: game)
    }
}

// ✅ Good: Child receives reference
struct GameBoard: View {
    var game: GameState // Plain property, no wrapper needed

    var body: some View {
        Text("Score: \(game.score)")
    }
}
```

### 5. Use @Bindable for Two-Way Binding

```swift
// ✅ Good: Using @Bindable for bindings
struct SettingsEditor: View {
    @Bindable var settings: AppSettings

    var body: some View {
        Form {
            TextField("Username", text: $settings.username)
            Toggle("Notifications", isOn: $settings.notificationsEnabled)
        }
    }
}

// ❌ Avoid: Can't create bindings without @Bindable
struct BadSettingsEditor: View {
    var settings: AppSettings

    var body: some View {
        TextField("Username", text: $settings.username) // Compiler error
    }
}
```

### 6. Thread Safety Considerations

The Observation framework doesn't provide thread safety by default:

```swift
@Observable
class ThreadSafeModel {
    private let queue = DispatchQueue(label: "model.queue")

    private var _value: Int = 0

    var value: Int {
        get { queue.sync { _value } }
        set { queue.sync { _value = newValue } }
    }
}

// Or use actor for concurrent access
actor ActorModel {
    var value: Int = 0

    func updateValue(_ newValue: Int) {
        value = newValue
    }
}
```

Note: Actors cannot use `@Observable` macro. Use `@Observable` classes with manual synchronization, or use actors without observation.

## Performance Optimization

### Selective Property Reading

```swift
@Observable
class LargeDataSet {
    var items: [Item] = []
    var metadata: Metadata = Metadata()
    var statistics: Statistics = Statistics()
}

// ✅ Efficient: Only observes items
struct ItemListView: View {
    var data: LargeDataSet

    var body: some View {
        List(data.items) { item in
            ItemRow(item: item)
        }
    }
}

// ✅ Efficient: Only observes statistics
struct StatsView: View {
    var data: LargeDataSet

    var body: some View {
        Text("Total: \(data.statistics.total)")
    }
}
```

### Computed Properties for Derived State

```swift
@Observable
class ViewModel {
    var firstName: String = ""
    var lastName: String = ""

    // Automatically observed when accessed
    var fullName: String {
        "\(firstName) \(lastName)"
    }
}
```

### Avoiding Observation Overhead

```swift
@Observable
class Metrics {
    var displayedCount: Int = 0

    // High-frequency counter that doesn't need observation
    @ObservationIgnored private var totalIterations: Int = 0

    func processItems() {
        for _ in items {
            totalIterations += 1 // Won't trigger updates
        }
        displayedCount += 1 // Will trigger updates
    }
}
```

## Testing Observable Objects

```swift
import Testing
import Observation

@Test
func observablePropertyChanges() {
    let model = GameState()

    #expect(model.score == 0)

    model.score = 100
    #expect(model.score == 100)
}

@Test
func computedPropertyUpdates() {
    let cart = ShoppingCart()

    #expect(cart.total == 0)

    cart.items.append(Item(price: 10.0))
    cart.items.append(Item(price: 20.0))

    #expect(cart.total == 30.0)
}
```

## Common Patterns

### Repository Pattern

```swift
@Observable
class UserRepository {
    var users: [User] = []
    var isLoading: Bool = false
    var error: Error?

    func fetchUsers() async {
        isLoading = true
        defer { isLoading = false }

        do {
            users = try await api.fetchUsers()
            error = nil
        } catch {
            self.error = error
        }
    }
}
```

### State Machine Pattern

```swift
@Observable
class StateMachine {
    enum State {
        case idle
        case loading
        case loaded(Data)
        case error(Error)
    }

    var state: State = .idle

    func load() async {
        state = .loading
        do {
            let data = try await fetchData()
            state = .loaded(data)
        } catch {
            state = .error(error)
        }
    }
}
```

### Coordinator Pattern

```swift
@Observable
class NavigationCoordinator {
    var path: [Route] = []
    var sheet: Sheet?
    var alert: Alert?

    func navigate(to route: Route) {
        path.append(route)
    }

    func present(_ sheet: Sheet) {
        self.sheet = sheet
    }
}
```

## Platform Availability

The Observation framework requires:
- iOS 17.0+
- macOS 14.0+
- tvOS 17.0+
- watchOS 10.0+
- Swift 5.9+
- Xcode 15.0+

For earlier platforms, continue using `ObservableObject` and Combine.

## Additional Resources

- [WWDC23: Discover Observation in SwiftUI](https://developer.apple.com/videos/play/wwdc2023/10149/)
- [Apple Documentation: Observation Framework](https://developer.apple.com/documentation/observation)
- [Apple Documentation: Migrating from ObservableObject](https://developer.apple.com/documentation/swiftui/migrating-from-the-observable-object-protocol-to-the-observable-macro)
- [Swift Evolution Proposal SE-0395: Observability](https://github.com/apple/swift-evolution/blob/main/proposals/0395-observability.md)
