---
description: Guide for migrating Swift code to Swift 6 with concurrency and strict checking
globs: "**/*.swift"
alwaysApply: false
---

================================================
FILE: README.md
================================================
# The Swift Concurrency Migration Guide

This repository contains the source for [The Swift Concurrency Migration Guide][scmg],
which is built using [Swift-DocC][docc].

## Contributing

See [the contributing guide][contributing] for instructions on contributing
to the Swift Migration Guide.

## Building

Run `docc preview Guide.docc` in this repository's root directory.

After running DocC,
open the link that `docc` outputs to display a local preview in your browser.

> Note:
>
> If you installed DocC by downloading a toolchain from Swift.org,
> `docc` is located in `usr/bin/`,
> relative to the installation path of the toolchain.
> Make sure your shell's `PATH` environment variable
> includes that directory.
>
> If you installed DocC by downloading Xcode,
> use `xcrun docc` instead.

[contributing]: https://github.com/apple/swift-migration-guide/blob/main/CONTRIBUTING.md
[docc]: https://github.com/apple/swift-docc
[conduct]: https://www.swift.org/code-of-conduct
[scmg]: https://www.swift.org/migration/documentation/migrationguide


================================================
FILE: CODEOWNERS
================================================
# Lines starting with '#' are comments.
# Each line is a case-sensitive file pattern followed by one or more owners.
# Order is important. The last matching pattern has the most precedence.
# More information: https://docs.github.com/en/articles/about-code-owners
#
# Please mirror the repository's file hierarchy in case-sensitive lexicographic
# order.

* @hborla @mattmassicotte @shahmishal @ktoso



================================================
FILE: CONTRIBUTING.md
================================================
# Contributing to the Swift Migration Guide

## Welcome the Swift community!

Contributions to the Swift project are welcomed and encouraged!
Please see the [Contributing to Swift guide](https://www.swift.org/contributing/)
and check out the structure of the community.

To be a truly great community, Swift needs to welcome developers from all walks
of life, with different backgrounds, and with a wide range of experience. A
diverse and friendly community will have more great ideas, more unique
perspectives, and produce more great code. We will work diligently to make the
Swift community welcoming to everyone.

To give clarity of what is expected of our members, Swift has adopted the code
of conduct defined by the Contributor Covenant. This document is used across
many open source communities, and we think it articulates our values well. 
For more, see the [Code of Conduct](https://www.swift.org/code-of-conduct/).

## Contributing to swift-migration-guide
 
### How you can help

We would love your contributions in the form of:
- Filing issues to cover specific code patterns or additional sections of the
  guide
- Opening pull requests to improve existing content or add new content
- Reviewing others' pull requests for clarity and correctness of writing
  and code examples

### Submitting Issues and Pull Requests

#### Issues

File bugs about the content using the [issues page][bugs] on Github.

#### Opening pull requests

To create a pull request, fork this repository, push your change to
a branch, and open a pull request against the `main` branch.

#### Build and test

Run `docc preview Guide.docc` in this repository's root directory.

After running DocC, open the link that `docc` outputs to display a local
preview in your browser.

> Note:
>
> If you installed DocC by downloading a toolchain from Swift.org,
> `docc` is located in `usr/bin/`,
> relative to the installation path of the toolchain.
> Make sure your shell's `PATH` environment variable
> includes that directory.
>
> If you installed DocC by downloading Xcode,
> use `xcrun docc` instead.

#### Running CI

Pull requests must pass CI testing via `@swift-ci please test` before the change is merged.

### Getting your PR reviewed 

Reviewers will be tagged automatically when you open a pull request. You may
be asked to make changes during code review. When you are ready, use the
request re-review feature of github or mention the reviewers by name in a comment.

[bugs]: https://github.com/apple/swift-migration-guide/issues


================================================
FILE: LICENSE.txt
================================================
This project is licensed under the Apache License, Version 2.0.
See: https://www.apache.org/licenses/LICENSE-2.0



================================================
FILE: Package.swift
================================================
// swift-tools-version: 6.0

import PackageDescription

let package = Package(
    name: "MigrationGuide",
    platforms: [
        .macOS(.v10_15),
        .iOS(.v13),
        .watchOS(.v6),
        .tvOS(.v13),
        .macCatalyst(.v13),
        .visionOS(.v1),
    ],
    products: [
        .library(
            name: "Library",
            targets: ["Library"]
        ),
        .executable(name: "swift5_examples", targets: ["Swift5Examples"]),
        .executable(name: "swift6_examples", targets: ["Swift6Examples"]),
    ],
    targets: [
        .target(
            name: "Library"
        ),
        .testTarget(
            name: "LibraryXCTests",
            dependencies: ["ObjCLibrary", "Library"]
        ),
        .target(
            name: "ObjCLibrary",
            publicHeadersPath: "."
        ),
        .executableTarget(
            name: "Swift5Examples",
            dependencies:  ["Library", "ObjCLibrary"],
            swiftSettings: [
                .swiftLanguageMode(.v5),
                .enableUpcomingFeature("StrictConcurrency"),
            ]
        ),
        .executableTarget(
            name: "Swift6Examples",
            dependencies: ["Library", "ObjCLibrary"]
        )
    ]
)



================================================
FILE: .editorconfig
================================================
root = true

[*]
indent_style = space
indent_size = 4
end_of_line = lf
charset = utf-8
trim_trailing_whitespace = true
insert_final_newline = true



================================================
FILE: bin/local.sh
================================================
set -euxo pipefail

export DOCC_JSON_PRETTYPRINT="YES"

output="./migration-guide"

xcrun docc convert --experimental-enable-custom-templates --output-path ./migration-guide Guide.docc

pushd migration-guide

ruby -run -e httpd -- . -p 8000



================================================
FILE: bin/publish.sh
================================================
#! /bin/bash

set -euxo pipefail

export DOCC_JSON_PRETTYPRINT="YES"

output="./migration-guide"

docc convert \
    --experimental-enable-custom-templates \
    --hosting-base-path migration-guide \
    --output-path "$output" \
    MigrationGuide.docc


================================================
FILE: bin/redirects/index.html
================================================
<html>
<head>
    <title>The Swift Concurrency Migration Guide: Redirect</title>
    <meta http-equiv="refresh" content="10; url=/migration/documentation/migrationguide/">
<head>
<body>
<div>
This content has moved; redirecting to the
<a href="/migration/documentation/migrationguide/" id="redirect">new location</a>.
<noscript>
<aside>
This page requires JavaScript.
Please turn on JavaScript and refresh the page.
</aside>
</noscript>
</div>
<script>
const baseURL = "/migration/documentation/migrationguide/";

var newURL = baseURL;

document.getElementById("redirect").setAttribute("href", newURL);
window.location = newURL;
</script>
</body>
</html>



================================================
FILE: Guide.docc/CommonProblems.md
================================================
# Common Compiler Errors

Identify, understand, and address common problems you can encounter while
working with Swift concurrency.

The data isolation guarantees made by the compiler affect all Swift code.
This means complete concurrency checking can surface latent issues,
even in Swift 5 code that doesn't use any concurrency language features
directly.
With the Swift 6 language mode enabled, some of these potential issues
can also become errors.

After enabling complete checking, many projects can contain a large
number of warnings and errors.
_Don't_ get overwhelmed!
Most of these can be tracked down to a much smaller set of root causes.
And these causes, frequently, are a result of common patterns which aren't 
just easy to fix, but can also be very instructive while learning about
Swift's concurrency system.

## Unsafe Global and Static Variables

Global state, including static variables, are accessible from anywhere in a
program.
This visibility makes them particularly susceptible to concurrent access.
Before data-race safety, global variable patterns relied on programmers
carefully accessing global state in ways that avoided data-races
without any help from the compiler.

> Experiment: These code examples are available in package form.
Try them out yourself in [Globals.swift][Globals].

[Globals]: https://github.com/apple/swift-migration-guide/blob/main/Sources/Examples/Globals.swift

### Sendable Types

```swift
var supportedStyleCount = 42
```

Here, we have defined a global variable.
The global variable is both non-isolated _and_ mutable from any
isolation domain. Compiling the above code in Swift 6 mode
produces an error message:

```
1 | var supportedStyleCount = 42
  |              |- error: global variable 'supportedStyleCount' is not concurrency-safe because it is non-isolated global shared mutable state
  |              |- note: convert 'supportedStyleCount' to a 'let' constant to make the shared state immutable
  |              |- note: restrict 'supportedStyleCount' to the main actor if it will only be accessed from the main thread
  |              |- note: unsafely mark 'supportedStyleCount' as concurrency-safe if all accesses are protected by an external synchronization mechanism
2 |
```

Two functions with different isolation domains accessing this
variable risks a data race. In the following code, `printSupportedStyles()`
could be running on the main actor concurrently with a call to
`addNewStyle()` from another isolation domain:

```swift
@MainActor
func printSupportedStyles() {
    print("Supported styles: ", supportedStyleCount)
}

func addNewStyle() {
    let style = Style()

    supportedStyleCount += 1

    storeStyle(style)
}
```

One way to address the problem is by changing the variable's isolation.

```swift
@MainActor
var supportedStyleCount = 42
```

The variable remains mutable, but has been isolated to a global actor.
All accesses can now only happen in one isolation domain, and the synchronous
access within `addNewStyle` would be invalid at compile time.

If the variable is meant to be constant and is never mutated,
a straight-forward solution is to express this to the compiler.
By changing the `var` to a `let`, the compiler can statically
disallow mutation, guaranteeing safe read-only access.

```swift
let supportedStyleCount = 42
```

A global value can also be expressed with a computed property.
If such property consistently returns the same constant value,
this is semantically equivalent to a `let` constant as far as 
observable values/effects are concerned:

```swift
var supportedStyleCount: Int {
    42
}
```

If there is synchronization in place that protects this variable in a way that
is invisible to the compiler, you can disable all isolation checking for
`supportedStyleCount` using `nonisolated(unsafe)`.

```swift
/// This value is only ever accessed while holding `styleLock`.
nonisolated(unsafe) var supportedStyleCount = 42
```

Only use `nonisolated(unsafe)` when you are carefully guarding all access to
the variable with an external synchronization mechanism such as a lock or
dispatch queue.

### Non-Sendable Types

In the above examples, the variable is an `Int`,
a value type that is inherently `Sendable`.
Global _reference_ types present an additional challenge, because they
are typically not `Sendable`.

```swift
class WindowStyler {
    var background: ColorComponents

    static let defaultStyler = WindowStyler()
}
```

The problem with this `static let` declaration is not related to the
mutability of the variable.
The issue is `WindowStyler` is a non-`Sendable` type, making its internal state
unsafe to share across isolation domains.

```swift
func resetDefaultStyle() {
    WindowStyler.defaultStyler.background = ColorComponents(red: 1.0, green: 1.0, blue: 1.0)
}

@MainActor
class StyleStore {
    var stylers: [WindowStyler]

    func hasDefaultBackground() -> Bool {
        stylers.contains { $0.background == WindowStyler.defaultStyler.background }
    }
}
```

Here, we see two functions that could access the internal state of the
`WindowStyler.defaultStyler` concurrently.
The compiler only permits these kinds of cross-isolation accesses with
`Sendable` types.
One option is to isolate the variable to a single domain using a global actor.
Alternatively, it might make sense to add a conformance to `Sendable`
directly.

## Protocol Conformance Isolation Mismatch

A protocol defines requirements that a conforming type must satisfy,
including static isolation.
This can result in isolation mismatches between a protocol's declaration and
conforming types.

There are many possible solutions to this class of problem, but they often
involve trade-offs.
Choosing an appropriate approach first requires understanding _why_ there is a
mismatch in the first place.

> Experiment: These code examples are available in package form.
Try them out yourself in [ConformanceMismatches.swift][ConformanceMismatches].

[ConformanceMismatches]: https://github.com/apple/swift-migration-guide/blob/main/Sources/Examples/ConformanceMismatches.swift

### Under-Specified Protocol

The most commonly-encountered form of this problem happens when a protocol
has no explicit isolation.
In this case, as with all other declarations, this implies _non-isolated_.
Non-isolated protocol requirements can be called from generic code in any
isolation domain. If the requirement is synchronous, it is invalid for
a conforming type's implementation to access actor-isolated state:

```swift
protocol Styler {
    func applyStyle()
}

@MainActor
class WindowStyler: Styler {
    func applyStyle() {
        // access main-actor-isolated state
    }
}
```

The above code produces the following error in Swift 6 mode:

```
 5 | @MainActor
 6 | class WindowStyler: Styler {
 7 |     func applyStyle() {
   |          |- error: main actor-isolated instance method 'applyStyle()' cannot be used to satisfy nonisolated protocol requirement
   |          `- note: add 'nonisolated' to 'applyStyle()' to make this instance method not isolated to the actor
 8 |         // access main-actor-isolated state
 9 |     }
```

It is possible that the protocol actually _should_ be isolated, but
has not yet been updated for concurrency.
If conforming types are migrated to add correct isolation first, mismatches
will occur.

```swift
// This really only makes sense to use from MainActor types, but
// has not yet been updated to reflect that.
protocol Styler {
    func applyStyle()
}

// A conforming type, which is now correctly isolated, has exposed 
// a mismatch.
@MainActor
class WindowStyler: Styler {
}
```

#### Adding Isolation

If protocol requirements are always called from the main actor,
adding `@MainActor` is the best solution.

There are two ways to isolate a protocol requirement to the main actor:

```swift
// entire protocol
@MainActor
protocol Styler {
    func applyStyle()
}

// per-requirement
protocol Styler {
    @MainActor
    func applyStyle()
}
```

Marking a protocol with a global actor attribute will infer isolation
for the entire scope of the conformance.
This can apply to a conforming type as a whole if the protocol conformance is
not declared in an extension.

Per-requirement isolation has a narrower impact on actor isolation inference,
because it only applies to the implementation of that specific requirement.
It does not impact the inferred isolation of protocol extensions or other
methods on the conforming type.
This approach should be favored if it makes sense to have conforming types
that aren't necessarily also tied to the same global actor.

Either way, changing the isolation of a protocol can affect the isolation of
conforming types and it can impose restrictions on generic code using the
protocol.

You can stage in diagnostics caused by adding global actor isolation on a
protocol using [`@preconcurrency`][Preconcurrency].
This will preserve source compatibility with clients that have not yet
begun adopting concurrency.

[Preconcurrency]: <doc:LibraryEvolution#Preconcurrency-annotations>

```swift
@preconcurrency @MainActor
protocol Styler {
    func applyStyle()
}
```

#### Asynchronous Requirements

For methods that implement synchronous protocol requirements the isolation
of implementations must match exactly.
Making a requirement _asynchronous_ offers more flexibility for
conforming types.

```swift
protocol Styler {
    func applyStyle() async
}
```

It's possible to satisfy a non-isolated `async` protocol requirement with
an isolated method.

```swift
@MainActor
class WindowStyler: Styler {
    // matches, even though it is synchronous and actor-isolated
    func applyStyle() {
    }
}
```

The above code is safe, because generic code must always call `applyStyle()`
asynchronously, allowing isolated implementations to switch actors before
accessing actor-isolated state.

However, this flexibility comes at a cost.
Changing a method to be asynchronous can have a significant impact at
every call site.
In addition to an async context, both the parameters and return values may
need to cross isolation boundaries.
Together, these could require significant structural changes to address.
This may still be the right solution, but the side-effects should be carefully
considered first, even if only a small number of types are involved.

#### Preconcurrency Conformance

Swift has a number of mechanisms to help you adopt concurrency incrementally
and interoperate with code that has not yet begun using concurrency at all.
These tools can be helpful both for code you do not own, as well as code you
do own, but cannot easily change.

Annotating a protocol conformance with `@preconcurrency` makes it possible to
suppress errors about any isolation mismatches.

```swift
@MainActor
class WindowStyler: @preconcurrency Styler {
    func applyStyle() {
        // implementation body
    }
}
```

This inserts runtime checks to ensure that that static isolation
of the conforming class is always enforced.

> Note: To learn more about incremental adoption and dynamic isolation,
see [Dynamic Isolation][]

[Dynamic Isolation]: <doc:IncrementalAdoption#Dynamic-Isolation>

### Isolated Conforming Type

So far, the solutions presented assume that the causes of isolation
mismatches are ultimately rooted in protocol definitions.
But it could be that the protocol's static isolation is appropriate,
and the issue instead is only caused by the conforming type.

#### Non-Isolated

Even a completely non-isolated function could still be useful.

```swift
@MainActor
class WindowStyler: Styler {
    nonisolated func applyStyle() {
        // perhaps this implementation doesn't involve
        // other MainActor-isolated state
    }
}
```

The constraint on this implementation is isolated state and functions
become unavailable.
This can still be an appropriate solution, especially if the function is used
as a source of instance-independent configuration.

#### Conformance by Proxy

It's possible to use an intermediate type to help address static
isolation differences.
This can be particularly effective if the protocol requires inheritance by its
conforming types.

```swift
class UIStyler {
}

protocol Styler: UIStyler {
    func applyStyle()
}

// actors cannot have class-based inheritance
actor WindowStyler: Styler {
}
```

Introducing a new type to conform indirectly can make this situation work.
However, this solution will require some structural changes to `WindowStyler`
that could spill out to dependent code as well.

```swift
// class with necessary superclass
class CustomWindowStyle: UIStyler {
}

// now, the conformance is possible
extension CustomWindowStyle: Styler {
    func applyStyle() {
    }
}
```

Here, a new type has been created that can satisfy the needed inheritance.
Incorporating will be easiest if the conformance is only used internally by
`WindowStyler`.

## Crossing Isolation Boundaries

The compiler will only permit a value to move from one isolation domain to
another when it can prove it will not introduce data races.
Attempting to use values that do not satisfy this requirement in contexts that
can cross isolation boundaries is a very common problem.
And because libraries and frameworks may be updated to use Swift's
concurrency features, these issues can come up even when your code hasn't
changed.

> Experiment: These code examples are available in package form.
Try them out yourself in [Boundaries.swift][Boundaries].

[Boundaries]: https://github.com/apple/swift-migration-guide/blob/main/Sources/Examples/Boundaries.swift

### Implicitly-Sendable Types

Many value types consist entirely of `Sendable` properties.
The compiler will treat types like this as implicitly `Sendable`, but _only_
when they are non-public.

```swift
public struct ColorComponents {
    public let red: Float
    public let green: Float
    public let blue: Float
}

@MainActor
func applyBackground(_ color: ColorComponents) {
}

func updateStyle(backgroundColor: ColorComponents) async {
    await applyBackground(backgroundColor)
}
```

A `Sendable` conformance is part of a type's public API contract,
which is up to you to declare.
Because `ColorComponents` is marked `public`, it will not implicitly
conform to `Sendable`.
This will result in the following error:

```
 6 | 
 7 | func updateStyle(backgroundColor: ColorComponents) async {
 8 |     await applyBackground(backgroundColor)
   |           |- error: sending 'backgroundColor' risks causing data races
   |           `- note: sending task-isolated 'backgroundColor' to main actor-isolated global function 'applyBackground' risks causing data races between main actor-isolated and task-isolated uses
 9 | }
10 | 
```

A straightforward solution is to make the type's `Sendable`
conformance explicit:

```swift
public struct ColorComponents: Sendable {
    // ...
}
```

Even when trivial, adding `Sendable` conformance should always be
done with care.
Remember that `Sendable` is a guarantee of thread-safety and
removing the conformance is an API-breaking change.

### Preconcurrency Import

Even if the type in another module is actually `Sendable`, it is not always
possible to modify its definition.
In this case, you can use a `@preconcurrency import` to downgrade diagnostics
until the library is updated.

```swift
// ColorComponents defined here
@preconcurrency import UnmigratedModule

func updateStyle(backgroundColor: ColorComponents) async {
    // crossing an isolation domain here
    await applyBackground(backgroundColor)
}
```

With the addition of this `@preconcurrency import`,
`ColorComponents` remains non-`Sendable`.
However, the compiler's behavior will be altered.
When using the Swift 6 language mode,
the error produced here will be downgraded to a warning.
The Swift 5 language mode will produce no diagnostics at all.

### Latent Isolation

Sometimes the _apparent_ need for a `Sendable` type can actually be the
symptom of a more fundamental isolation problem.
The only reason a type needs to be `Sendable` is to cross isolation boundaries.
If you can avoid crossing boundaries altogether, the result can
often be both simpler and a better reflection of the true nature of your
system.

```swift
@MainActor
func applyBackground(_ color: ColorComponents) {
}

func updateStyle(backgroundColor: ColorComponents) async {
    await applyBackground(backgroundColor)
}
```

The `updateStyle(backgroundColor:)` function is non-isolated.
This means that its non-`Sendable` parameter is also non-isolated.
The implementation crosses immediately from this non-isolated domain to the
`MainActor` when `applyBackground(_:)` is called.

Since `updateStyle(backgroundColor:)` is working directly with
`MainActor`-isolated functions and non-`Sendable` types,
just applying `MainActor` isolation may be more appropriate.

```swift
@MainActor
func updateStyle(backgroundColor: ColorComponents) async {
    applyBackground(backgroundColor)
}
```

Now, there is no longer an isolation boundary for the non-`Sendable` type to
cross.
And in this case, not only does this resolve the problem, it also
removes the need for an asynchronous call.
Fixing latent isolation issues can also potentially make further API
simplification possible.

Lack of `MainActor` isolation like this is, by far, the most common form of
latent isolation.
It is also very common for developers to hesitate to use this as a solution.
It is completely normal for programs with a user interface to have a large
set of `MainActor`-isolated state.
Concerns around long-running _synchronous_ work can often be addressed with
just a handful of targeted `nonisolated` functions.

### Computed Value

Instead of trying to pass a non-`Sendable` type across a boundary, it may be
possible to use a `Sendable` function that creates the needed values.

```swift
func updateStyle(backgroundColorProvider: @Sendable () -> ColorComponents) async {
    await applyBackground(using: backgroundColorProvider)
}
```

Here, it does not matter than `ColorComponents` is not `Sendable`.
By using `@Sendable` function that can compute the value, the lack of
sendability is side-stepped entirely.

### Sending Argument

The compiler will permit non-`Sendable` values to cross an isolation boundary
if the compiler can prove it can be done safely.
Functions that explicitly state they require this can use the values
within their implementations with less restrictions.

```swift
func updateStyle(backgroundColor: sending ColorComponents) async {
    // this boundary crossing can now be proven safe in all cases
    await applyBackground(backgroundColor)
}
```

A `sending` argument does impose some restrictions at call sites.
But, this can still be easier or more appropriate than adding a
`Sendable` conformance.
This technique also works for types you do not control.

### Sendable Conformance

When encountering problems related to crossing isolation domains, a very
natural reaction is to just try to add a conformance to `Sendable`.
You can make a type `Sendable` in four ways.

#### Global Isolation

Adding global isolation to any type will make it implicitly `Sendable`.

```swift
@MainActor
public struct ColorComponents {
    // ...
}
```

By isolating this type to the `MainActor`, any accesses from other isolation domains
must be done asynchronously.
This makes it possible to safely pass instances around across domains.

#### Actors

Actors have an implicit `Sendable` conformance because their properties are
protected by actor isolation.

```swift
actor Style {
    private var background: ColorComponents
}
```

In addition to gaining a `Sendable` conformance, actors receive their own
isolation domain.
This allows them to work freely with other non-`Sendable` types internally.
This can be a major advantage, but does come with trade-offs.

Because an actor's isolated methods must all be asynchronous,
sites that access the type may require an async context.
This alone is a reason to make such a change with care.
But further, data that is passed into or out of the actor may itself
need to cross the isolation boundary.
This can result in the need for yet more `Sendable` types.

```swift
actor Style {
    private var background: ColorComponents

    func applyBackground(_ color: ColorComponents) {
        // make use of non-Sendable data here
    }
}
```

By moving both the non-Sendable data *and* operations on that data into the
actor, no isolation boundaries need to be crossed.
This provides a `Sendable` interface to those operations that can be freely
accessed from any asynchronous context.

#### Manual Synchronization

If you have a type that is already doing manual synchronization, you can
express this to the compiler by marking your `Sendable` conformance as
`unchecked`.

```swift
class Style: @unchecked Sendable {
    private var background: ColorComponents
    private let queue: DispatchQueue
}
```

You should not feel compelled to remove the use of queues, locks, or other
forms of manual synchronization to integrate with Swift's concurrency system.
However, most types are not inherently thread-safe.
As a general rule, if a type isn't already thread-safe, attempting to make
it `Sendable` should not be your first approach.
It is often easier to try other techniques first, falling back to
manual synchronization only when truly necessary.

#### Retroactive Sendable Conformance

Your dependencies may also expose types that are using manual synchronization.
This is usually visible only via documentation.
It is possible to add an `@unchecked Sendable` conformance in this case as well.

```swift
extension ColorComponents: @retroactive @unchecked Sendable {
}
```

Because `Sendable` is a marker protocol, a retroactive conformance
does not have direct binary compatibility issues.
However, it should still be used with extreme caution.
Types that use manual synchronization can come with conditions or
exceptions to their safety that may not completely match the semantics of
`Sendable`.
Further, you should be _particularly_ careful about using this technique
for types that are part of your system's public API.

> Note: To learn more about retroactive conformances,
see the associated [Swift evolution proposal][SE-0364].

[SE-0364]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0364-retroactive-conformance-warning.md

#### Sendable Reference Types

It is possible for reference types to be validated as `Sendable` without
the `unchecked` qualifier,
but this is only done under very specific circumstances.

To allow a checked `Sendable` conformance, a class:

- Must be `final`
- Cannot inherit from another class other than `NSObject`
- Cannot have any non-isolated mutable properties

```swift
public struct ColorComponents: Sendable {
    // ...
}

final class Style: Sendable {
    private let background: ColorComponents
}
```

A reference type that conforms to `Sendable` is sometimes a sign that a value
type would be preferable.
But there are circumstances where reference semantics need to be preserved,
or where compatibility with a mixed Swift/Objective-C code base is required.

#### Using Composition

You do not need to select one single technique for making a reference type
`Sendable.`
One type can use many techniques internally.

```swift
final class Style: Sendable {
    private nonisolated(unsafe) var background: ColorComponents
    private let queue: DispatchQueue

    @MainActor
    private var foreground: ColorComponents
}
```

The `background` property is protected by manual synchronization,
while the `foreground` property uses actor isolation.
Combining these two techniques results in a type that better describes its
internal semantics.
By doing this, the type continues to take advantage of the
compiler's automated isolation checking.

### Non-Isolated Initialization

Actor-isolated types can present a problem when they are initialized in
a non-isolated context.
This frequently occurs when the type is used in a default value expression or
as a property initializer.

> Note: These problems could also be a symptom of
[latent isolation](#Latent-Isolation) or an
[under-specified protocol](#Under-Specified-Protocol).

Here the non-isolated `Stylers` type is making a call to a
`MainActor`-isolated initializer.

```swift
@MainActor
class WindowStyler {
    init() {
    }
}

struct Stylers {
    static let window = WindowStyler()
}
```

This code results in the following error:

```
 7 | 
 8 | struct Stylers {
 9 |     static let window = WindowStyler()
   |                `- error: main actor-isolated default value in a nonisolated context
10 | }
11 | 
```

Globally-isolated types sometimes don't actually need to reference any global
actor state in their initializers.
By making the `init` method `nonisolated`, it is free to be called from any
isolation domain.
This remains safe as the compiler still guarantees that any state that *is*
isolated will only be accessible from the `MainActor`.

```swift
@MainActor
class WindowStyler {
    private var viewStyler = ViewStyler()
    private var primaryStyleName: String

    nonisolated init(name: String) {
        self.primaryStyleName = name
        // type is fully-initialized here
    }
}
```


All `Sendable` properties can still be safely accessed in this `init` method.
And while any non-`Sendable` properties cannot,
they can still be initialized by using default expressions.

### Non-Isolated Deinitialization

Even if a type has actor isolation, deinitializers are _always_ non-isolated.

```swift
actor BackgroundStyler {
    // another actor-isolated type
    private let store = StyleStore()

    deinit {
        // this is non-isolated
        store.stopNotifications()
    }
}
```

This code produces the error:

```
error: call to actor-isolated instance method 'stopNotifications()' in a synchronous nonisolated context
 5 |     deinit {
 6 |         // this is non-isolated
 7 |         store.stopNotifications()
   |               `- error: call to actor-isolated instance method 'stopNotifications()' in a synchronous nonisolated context
 8 |     }
 9 | }
```

While this might feel surprising, given that this type is an actor,
this is not a new constraint.
The thread that executes a deinitializer has never been guaranteed and
Swift's data isolation is now just surfacing that fact.

Often, the work being done within the `deinit` does not need to be synchronous.
A solution is to use an unstructured `Task` to first capture and
then operate on the isolated values.
When using this technique,
it is _critical_ to ensure you do not capture `self`, even implicitly.

```swift
actor BackgroundStyler {
    // another actor-isolated type
    private let store = StyleStore()

    deinit {
        // no actor isolation here, so none will be inherited by the task
        Task { [store] in
            await store.stopNotifications()
        }
    }
}
```

> Important: **Never** extend the life-time of `self` from within
`deinit`. Doing so will crash at runtime.



================================================
FILE: Guide.docc/CompleteChecking.md
================================================
# Enabling Complete Concurrency Checking

Incrementally address data-race safety issues by enabling diagnostics as warnings in your project.

Data-race safety in the Swift 6 language mode is designed for incremental
migration. You can address data-race safety issues in your projects
module-by-module, and you can enable the compiler's actor isolation and
`Sendable` checking as warnings in the Swift 5 language mode, allowing you to
assess your progress toward eliminating data races before turning on the
Swift 6 language mode.

Complete data-race safety checking can be enabled as warnings in the Swift 5
language mode using the `-strict-concurrency` compiler flag.

## Using the Swift compiler

To enable complete concurrency checking when running `swift` or `swiftc`
directly at the command line, pass `-strict-concurrency=complete`:

```
~ swift -strict-concurrency=complete main.swift
```

## Using SwiftPM

### Command-line invocation

`-strict-concurrency=complete` can be passed in a Swift package manager
command-line invocation using the `-Xswiftc` flag:

```
~ swift build -Xswiftc -strict-concurrency=complete
~ swift test -Xswiftc -strict-concurrency=complete
```

This can be useful to gauge the amount of concurrency warnings before adding
the flag permanently in the package manifest as described in the following
section.

### Package manifest

To enable complete concurrency checking for a target in a Swift package using
Swift 5.9 or Swift 5.10 tools, use [`SwiftSetting.enableExperimentalFeature`](https://developer.apple.com/documentation/packagedescription/swiftsetting/enableexperimentalfeature(_:_:))
in the Swift settings for the given target:

```swift
.target(
  name: "MyTarget",
  swiftSettings: [
    .enableExperimentalFeature("StrictConcurrency")
  ]
)
```

When using Swift 6.0 tools or later, use [`SwiftSetting.enableUpcomingFeature`](https://developer.apple.com/documentation/packagedescription/swiftsetting/enableupcomingfeature(_:_:))
in the Swift settings for a pre-Swift 6 language mode target:

```swift
.target(
  name: "MyTarget",
  swiftSettings: [
    .enableUpcomingFeature("StrictConcurrency")
  ]
)
```

Targets that adopt the Swift 6 language mode have complete checking
enabled unconditionally and do not require any settings changes.

## Using Xcode

### Build Settings

To enable complete concurrency checking in an Xcode project, set the
"Strict Concurrency Checking" setting to "Complete" in the Xcode build
settings.

### XCConfig

Alternatively, you can set `SWIFT_STRICT_CONCURRENCY` to `complete`
in an xcconfig file:

```
// In a Settings.xcconfig

SWIFT_STRICT_CONCURRENCY = complete;
```



================================================
FILE: Guide.docc/DataRaceSafety.md
================================================
# Data Race Safety

Learn about the fundamental concepts Swift uses to enable data-race-free
concurrent code.

Traditionally, mutable state had to be manually protected via careful runtime
synchronization.
Using tools such as locks and queues, the prevention of data races was
entirely up to the programmer. This is notoriously difficult
not just to do correctly, but also to keep correct over time.
Even determining the _need_ for synchronization may be challenging.
Worst of all, unsafe code does not guarantee failure at runtime.
This code can often seem to work, possibly because highly unusual conditions
are required to exhibit the incorrect and unpredictable behavior characteristic
of a data race.

More formally, a data race occurs when one thread accesses memory while the
same memory is being mutated by another thread.
The Swift 6 language mode eliminates these problems by preventing data races
at compile time.

> Important: You may have encountered constructs like `async`/`await`
and actors in other languages. Pay extra attention, as similarities to
these concepts in Swift may only be superficial.

## Data Isolation

Swift's concurrency system allows the compiler to understand and verify the
safety of all mutable state.
It does this with a mechanism called _data isolation_.
Data isolation guarantees mutually exclusive
access to mutable state. It is a form of synchronization,
conceptually similar to a lock.
But unlike a lock, the protection data isolation provides happens at
compile-time.

A Swift programmer interacts with data isolation in two ways:
statically and dynamically.

The term _static_ is used to describe program elements that are unaffected by
runtime state. These elements, such as a function definition,
are made up of keywords and annotations. Swift's concurrency system is 
an extension of its type system. When you declare functions and types,
you are doing so statically. Isolation can be a part of these static
declarations.

There are cases, however, where the type system alone cannot sufficiently
describe runtime behavior. An example could be an Objective-C type
that has been exposed to Swift. This declaration, made outside of Swift code,
may not provide enough information to the compiler to ensure safe usage. To
accommodate these situations, there are additional features that allow you
to express isolation requirements dynamically.

Data isolation, be it static or dynamic, allows the
compiler to guarantee Swift code you write is free of data races.

> Note: For more information about using dynamic isolation,
see <doc:IncrementalAdoption#Dynamic-Isolation>

### Isolation Domains

Data isolation is the _mechanism_ used to protect shared mutable state.
But it is often useful to talk about an independent unit of isolation.
This is known as an _isolation domain_.
How much state a particular domain is responsible for
protecting varies widely. An isolation domain might protect a single variable,
or an entire subsystem, such as a user interface.

The critical feature of an isolation domain is the safety it provides.
Mutable state can only be accessed from one isolation domain at a time.
You can pass mutable state from one isolation domain to another, but you can
never access that state concurrently from a different domain.
This guarantee is validated by the compiler.

Even if you have not explicitly defined it yourself,
_all_ function and variable declarations have a well-defined static
isolation domain.
These domains will always fall into one of three categories:

1. Non-isolated
2. Isolated to an actor value
3. Isolated to a global actor

### Non-isolated

Functions and variables do not have to be a part of an explicit isolation
domain.
In fact, a lack of isolation is the default, called _non-isolated_.
Because all the data isolation rules apply,
there is no way for non-isolated code to mutate state protected in another
domain.

<!--
  REFERENCE
  "Sea", in the context of concurrency, is a reference to the WWDC 2022 session
  "Eliminate data races using Swift Concurrency".

  Types like Island, Chicken, and Pineapple are also featured in that video.

  https://developer.apple.com/wwdc22/110351
-->

```swift
func sailTheSea() {
}
```

This top-level function has no static isolation, making it non-isolated.
It can safely call other non-isolated functions, and access non-isolated
variables, but it cannot access anything from another isolation domain.

```swift
class Chicken {
    let name: String
    var currentHunger: HungerLevel
}
```

This is an example of a non-isolated type.
Inheritance can play a role in static isolation.
But this simple class, with no superclass or protocol conformances,
also uses the default isolation.

Data isolation guarantees that non-isolated entities cannot access the mutable
state of other domains.
As a result of this, non-isolated functions and variables are always safe to
access from any other domain.

### Actors

Actors give the programmer a way to define an isolation domain,
along with methods that operate within that domain.
All stored properties of an actor are isolated to the enclosing actor instance.

```swift
actor Island {
    var flock: [Chicken]
    var food: [Pineapple]

    func addToFlock() {
        flock.append(Chicken())
    }
}
```

Here, every `Island` instance will define a new domain,
which will be used to protect access to its properties.
The method `Island.addToFlock` is said to be isolated to `self`.
The body of a method has access to all data that shares its isolation domain,
making the `flock` property synchronously accessible.

Actor isolation can be selectively disabled.
This can be useful any time you want to keep code organized within an
isolated type, but opt-out of the isolation requirements that go along with it.
Non-isolated methods cannot synchronously access any protected state.

```swift
actor Island {
    var flock: [Chicken]
    var food: [Pineapple]

    nonisolated func canGrow() -> PlantSpecies {
        // neither flock nor food are accessible here
    }
}
```

The isolation domain of an actor is not limited to its own methods.
Functions that accept an isolated parameter can also gain access to
actor-isolated state without the need for any other form of synchronization.

```swift
func addToFlock(of island: isolated Island) {
    island.flock.append(Chicken())
}
```

> Note: For an overview of actors, please see the [Actors][] section of
The Swift Programming Language.

[Actors]: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency#Actors

### Global Actors

Global actors share all of the properties of regular actors, but also provide
a means of statically assigning declarations to their isolation domain.
This is done with an annotation matching the actor name.
Global actors are particularly useful when groups of types all need to
interoperate as a single pool of shared mutable state.

```swift
@MainActor
class ChickenValley {
    var flock: [Chicken]
    var food: [Pineapple]
}
```

This class is statically-isolated to `MainActor`. This ensures that all access
to its mutable state is done from that isolation domain.

You can opt-out of this type of actor isolation as well,
using the `nonisolated` keyword.
And just as with actor types,
doing so will disallow access to any protected state.

```swift
@MainActor
class ChickenValley {
    var flock: [Chicken]
    var food: [Pineapple]

    nonisolated func canGrow() -> PlantSpecies {
        // neither flock, food, nor any other MainActor-isolated
        // state is accessible here
    }
}
```

### Tasks

A `task` is a unit of work that can run concurrently within your program.
You cannot run concurrent code in Swift outside of a task,
but that doesn't mean you must always manually start one.
Typically, asynchronous functions do not need to be aware of the
task running them.
In fact, tasks can often begin at a much higher level,
within an application framework, or even at the entry point of the program.

Tasks may run concurrently with one another,
but each individual task only executes one function at a time.
They run code in order, from beginning to end.

```swift
Task {
    flock.map(Chicken.produce)
}
```

A task always has an isolation domain. They can be isolated to an
actor instance, a global actor, or could be non-isolated.
This isolation can be established manually, but can also be inherited
automatically based on context.
Task isolation, just like all other Swift code, determines what mutable state
is accessible.

Tasks can run both synchronous and asynchronous code. Regardless of the
structure and how many tasks are involved, functions in the same isolation
domain cannot run concurrently with each other.
There will only ever be one task running synchronous code for any given
isolation domain.

> Note: For more information see the [Tasks][] section of
The Swift Programming Language.

[Tasks]: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency#Tasks-and-Task-Groups

### Isolation Inference and Inheritance

There are many ways to specify isolation explicitly.
But there are cases where the context of a declaration establishes isolation
implicitly, via _isolation inference_.

#### Classes

A subclass will always have the same isolation as its parent.

```swift
@MainActor
class Animal {
}

class Chicken: Animal {
}
```

Because `Chicken` inherits from `Animal`, the static isolation of the `Animal`
type also implicitly applies.
Not only that, it also cannot be changed by a subclass.
All `Animal` instances have been declared to be `MainActor`-isolated, which
means all `Chicken` instances must be as well.

The static isolation of a type will also be inferred for its properties and 
methods by default.

```swift
@MainActor
class Animal {
    // all declarations within this type are also
    // implicitly MainActor-isolated
    let name: String

    func eat(food: Pineapple) {
    }
}
```

> Note: For more information, see the [Inheritance][] section of
The Swift Programming Language.

[Inheritance]: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/inheritance

#### Protocols

A protocol conformance can implicitly affect isolation.
However, the protocol's effect on isolation depends on how the conformance
is applied.

```swift
@MainActor
protocol Feedable {
    func eat(food: Pineapple)
}

// inferred isolation applies to the entire type
class Chicken: Feedable {
}

// inferred isolation only applies within the extension
extension Pirate: Feedable {
}
```

A protocol's requirements themselves can also be isolated.
This allows more fine-grained control around how isolation is inferred
for conforming types.

```swift
protocol Feedable {
    @MainActor
    func eat(food: Pineapple)
}
```

Regardless of how a protocol is defined and conformance added, you cannot alter
other mechanisms of static isolation.
If a type is globally-isolated, either explicitly or via inference from a 
superclass, a protocol conformance cannot be used to change it.

> Note: For more information, see the [Protocols][] section of
The Swift Programming Language.

[Protocols]: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/protocols

#### Function Types

Isolation _inference_ allows a type to implicitly define the isolation of
its properties and methods.
But these are all examples of _declarations_.
It is also possible to achieve a similar effect with function values, through
isolation _inheritance_.

By default, closures are isolated to the same context they're formed in.
For example:

```swift
@MainActor
class Model { ... }

@MainActor
class C {
    var models: [Model] = []

    func mapModels<Value>(
      _ keyPath: KeyPath<Model, Value>
    ) -> some Collection<Value> {
        models.lazy.map { $0[keyPath: keyPath] }
    }
}
```

In the above code, the closure to `LazySequence.map` has type
`@escaping (Base.Element) -> U`. This closure must stay on the main
actor where it was originally formed. This allows the closure to capture
state or call isolated methods from the surrounding context.

Closures that can run concurrently with the original context are marked
explicitly through `@Sendable` and `sending` annotations described in later
sections.

For `async` closures that may be evaluated concurrently, the closure can still
capture the isolation of the original context. This mechanism is used by the
`Task` initializer so that the given operation is isolated to the original
context by default, while still allowing explicit isolation to be specified:

```swift
@MainActor
func eat(food: Pineapple) {
    // the static isolation of this function's declaration is
    // captured by the closure created here
    Task {
        // allowing the closure's body to inherit MainActor-isolation
        Chicken.prizedHen.eat(food: food)
    }

    Task { @MyGlobalActor in
        // this task is isolated to `MyGlobalActor`
    }
}
```

The closure's type here is defined by `Task.init`.
Despite that declaration not being isolated to any actor,
this newly-created task will _inherit_ the `MainActor` isolation of its
enclosing scope unless an explicit global actor is written.
Function types offer a number of mechanisms for controlling their
isolation behavior, but by default they behave identically to other types.

> Note: For more information, see the [Closures][] section of
The Swift Programming Language.

[Closures]: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/closures

## Isolation Boundaries

Isolation domains protect their mutable state, but useful programs need more
than just protection. They have to communicate and coordinate,
often by passing data back and forth.
Moving values into or out of an isolation domain is known as _crossing_ an
isolation boundary.
Values are only ever permitted to cross an isolation boundary where there
is no potential for concurrent access to shared mutable state.

Values can cross boundaries directly, via asynchronous function calls.
When you call an asynchronous function with a _different_ isolation domain,
the parameters and return value need to move into that domain.
Values can also cross boundaries indirectly when captured by closures.
Closures introduce many potential opportunities for concurrent accesses.
They can be created in one domain and then executed in another.
They can even be executed in multiple, different domains.

### Sendable Types

In some cases, all values of a particular type are safe to pass across
isolation boundaries because thread-safety is a property of the type itself.
This is represented by the `Sendable` protocol.
A conformance to `Sendable` means the given type is thread safe,
and values of the type can be shared across arbitrary isolation domains
without introducing a risk of data races.

Swift encourages using value types because they are naturally safe.
With value types, different parts of your program can't have
shared references to the same value.
When you pass an instance of a value type to a function,
the function has its own independent copy of that value.
Because value semantics guarantees the absence of shared mutable state, value
types in Swift are implicitly `Sendable` when all their stored properties
are also Sendable.
However, this implicit conformance is not visible outside of their
defining module.
Making a type `Sendable` is part of its public API contract
and must always be done explicitly.

```swift
enum Ripeness {
    case hard
    case perfect
    case mushy(daysPast: Int)
}

struct Pineapple {
    var weight: Double
    var ripeness: Ripeness
}
```

Here, both the `Ripeness` and `Pineapple` types are implicitly `Sendable`,
since they are composed entirely of `Sendable` value types.

> Note: For more information see the [Sendable Types][] section of
The Swift Programming Language.

[Sendable Types]: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency#Sendable-Types

#### Flow-Sensitive Isolation Analysis

The `Sendable` protocol is used to express thread-safety for a type as a
whole.
But there are situations when a particular _instance_ of a non-`Sendable`
type is being used in a safe way.
The compiler is often capable of inferring this safety through
flow-sensitive analysis known as [region-based isolation][RBI].

Region-based isolation allows the compiler to permit instances of
non-`Sendable` types to cross isolation domains when it can prove doing
so cannot introduce data races.

```swift
func populate(island: Island) async {
    let chicken = Chicken()

    await island.adopt(chicken)
}
```

Here, the compiler can correctly reason that even though `chicken` has a
non-`Sendable` type, allowing it to cross into the `island` isolation domain is
safe.
However, this exception to `Sendable` checking is inherently contigent on
the surrounding code.
The compiler will still produce an error should any unsafe accesses to the
`chicken` variable ever be introduced.

```swift
func populate(island: Island) async {
    let chicken = Chicken()

    await island.adopt(chicken)

    // this would result in an error
    chicken.eat(food: Pineapple())
}
```

[RBI]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0414-region-based-isolation.md

Region-based isolation works without any code changes.
But a function's parameters and return values can also explicitly state
that they support crossing domains using this mechanism.

```swift
func populate(island: Island, with chicken: sending Chicken) async {
    await island.adopt(chicken)
}
```

The compiler can now provide the guarantee that at all call sites, the
`chicken` parameter will never be subject to unsafe access.
This is a relaxing of an otherwise significant constraint.
Without `sending`, this function would only be possible to implement by
requiring that `Chicken` first conform to `Sendable`.

### Actor-Isolated Types

Actors are not value types, but because they protect all of their state
in their own isolation domain,
they are inherently safe to pass across boundaries.
This makes all actor types implicitly `Sendable`, even if their properties
are not `Sendable` themselves.

```swift
actor Island {
    var flock: [Chicken]  // non-Sendable
    var food: [Pineapple] // Sendable
}
```

Global-actor-isolated types are also implicitly `Sendable` for similar reasons.
They do not have a private, dedicated isolation domain, but their state is still
protected by an actor.

```swift
@MainActor
class ChickenValley {
    var flock: [Chicken]  // non-Sendable
    var food: [Pineapple] // Sendable
}
```

### Reference Types

Unlike value types, reference types cannot be implicitly `Sendable`.
And while they can be made `Sendable`,
doing so comes with a number of constraints.
To make a class `Sendable` it must contain no mutable state and all
immutable properties must also be `Sendable`.
Further, the compiler can only validate the implementation of final classes.

```swift
final class Chicken: Sendable {
    let name: String
}
```

It is possible to satisfy the thread-safety requirements of `Sendable`
using synchronization primitives that the compiler cannot reason about,
such as through OS-specific constructs or
when working with thread-safe types implemented in C/C++/Objective-C.
Such types may be marked as conforming to `@unchecked Sendable` to promise the
compiler that the type is thread-safe.
The compiler will not perform any checking on an `@unchecked Sendable` type,
so this opt-out must be used with caution.

### Suspension Points

A task can switch between isolation domains when a function in one
domain calls a function in another.
A call that crosses an isolation boundary must be made asynchronously,
because the destination isolation domain might be busy running other tasks.
In that case, the task will be suspended until the destination isolation
domain is available.
Critically, a suspension point does _not_ block.
The current isolation domain (and the thread it is running on)
are freed up to perform other work.
The Swift concurrency runtime expects code to never block on future work,
allowing the system to always make forward progress.
This eliminates a common source of deadlocks in concurrent code.

```swift
@MainActor
func stockUp() {
    // beginning execution on MainActor
    let food = Pineapple()

    // switching to the island actor's domain
    await island.store(food)
}
```

Potential suspension points are marked in source code with the `await` keyword.
Its presence indicates that the call might suspend at runtime, but `await` does not force a suspension. The function being called might
suspend only under certain dynamic conditions.
It's possible that a call marked with `await` will not actually suspend.

### Atomicity

While actors do guarantee safety from data races, they do not ensure
atomicity across suspension points.
Concurrent code often needs to execute a sequence of operations together as an
atomic unit, such that other threads can never see an intermediate state.
Units of code that require this property are known as _critical sections_.

Because the current isolation domain is freed up to perform other work,
actor-isolated state may change after an asynchronous call.
As a consequence, you can think of explicitly marking potential suspension
points as a way to indicate the end of a critical section.

```swift
func deposit(pineapples: [Pineapple], onto island: Island) async {
   var food = await island.food
   food += pineapples
   await island.store(food)
}
```

This code assumes, incorrectly, that the `island` actor's `food` value will not
change between asynchronous calls.
Critical sections should always be structured to run synchronously.

> Note: For more information, see the
[Defining and Calling Asynchronous Functions][] section of
The Swift Programming Language.

[Defining and Calling Asynchronous Functions]: https://docs.swift.org/swift-book/documentation/the-swift-programming-language/concurrency/#Defining-and-Calling-Asynchronous-Functions



================================================
FILE: Guide.docc/FeatureMigration.md
================================================
# Migrating to upcoming language features

Migrate your project to upcoming language features.

Upcoming language features can be enabled in the Swift compiler via a `-enable-upcoming-feature
<FeatureName>` flag. Some of these features also support a migration mode. This mode does not
actually enable the desired feature. Instead, it produces compiler warnings with the necessary
fix-its to make the existing code both source- and binary-compatible with the feature. The exact
semantics of such a migration is dependent on the feature, see their [corresponding
documentation](https://docs.swift.org/compiler/documentation/diagnostics/upcoming-language-features)
for more details.

## SwiftPM

> Note: This feature is in active development. Test with a [nightly
> snapshot](https://www.swift.org/install) for best results.

`swift package migrate` builds and applies migration fix-its to allow for semi-automated migration.
Make sure to start with a clean working tree (no current changes staged or otherwise) and a working
build - applying the fix-its requires there to be no build errors and will modify files in the
package *in place*.

To eg. migrate all targets in your package to `NonisolatedNonsendingByDefault`:
```sh
swift package migrate --to-feature NonisolatedNonsendingByDefault
```

Or a target at a time with `--targets`:
```sh
swift package migrate --targets TargetA --to-feature NonisolatedNonsendingByDefault
```

This will start a build, apply any migration fix-its, and then update the manifest:
```
> Starting the build.
... regular build output with migration diagnostics ...
> Applying fix-its.
> Updating manifest.
```

Check out the changes with your usual version control tooling, e.g., `git diff`:
```diff
diff --git a/Package.swift b/Package.swift
index a1e587c..11097be 100644
--- a/Package.swift
+++ b/Package.swift
@@ -14,10 +14,16 @@ let package = Package(
     targets: [
         .target(
             name: "TargetA",
+            swiftSettings: [
+                .enableUpcomingFeature("NonisolatedNonsendingByDefault"),
+            ]
         ),
     ]

diff --git a/Sources/packtest/packtest.swift b/Sources/packtest/packtest.swift
index 85253f5..8498bb5 100644
--- a/Sources/TargetA/TargetA.swift
+++ b/Sources/TargetA/TargetA.swift
@@ -1,5 +1,5 @@
 struct S: Sendable {
-  func alwaysSwitch() async {}
+  @concurrent func alwaysSwitch() async {}
 }
```

In some cases, the automated application of upcoming features to a target in the package manifest
can fail for more complicated packages, e.g., if settings have been factored out into a variable
that's then applied to multiple targets:
```
error: Could not update manifest for 'TargetA' (unable to find array literal for 'swiftSettings' argument). Please enable 'NonisolatedNonsendingByDefault' features manually.
```

If this happens, manually add a `.enableUpcomingFeature("SomeFeature")` Swift setting to complete
the migration:
```swift
// swift-tools-version: 6.2

let targetSettings: [SwiftSetting] = [
    // ...
    .enableUpcomingFeature("NonisolatedNonsendingByDefault")
]

let targetSettings:
let package = Package(
    name: "MyPackage",
    products: [
        // ...
    ],
    targets: [
        .target(
            name: "TargetA",
            swiftSettings: targetSettings
        ),
    ]
)
```



================================================
FILE: Guide.docc/IncrementalAdoption.md
================================================
# Incremental Adoption

Learn how you can introduce Swift concurrency features into your project
incrementally.

Migrating projects towards the Swift 6 language mode is usually done in stages.
In fact, many projects began the process before Swift 6 was even available.
You can continue to introduce concurrency features _gradually_,
addressing any problems that come up along the way.
This allows you to make incremental progress without disrupting the
entire project.

Swift includes a number of language features and standard library APIs to help
make incremental adoption easier.

## Wrapping Callback-Based Functions

APIs that accept and invoke a single function on completion are an extremely
common pattern in Swift.
It's possible to make a version of such a function that is usable directly from 
an asynchronous context.

```swift
func updateStyle(backgroundColor: ColorComponents, completionHandler: @escaping () -> Void) {
    // ...
}
```

This is an example of a function that informs a client its work is complete
using a callback.
There is no way for a caller to determine when or on what thread the callback
will be invoked without consulting documentation.

You can wrap this function up into an asynchronous version using
_continuations_.

```swift
func updateStyle(backgroundColor: ColorComponents) async {
    await withCheckedContinuation { continuation in
        updateStyle(backgroundColor: backgroundColor) {
            // ... do some work here ...

            continuation.resume()
        }
    }
}
```

> Note: You have to take care to _resume_ the continuation _exactly once_.
> If you miss invoking it, the calling task will remain suspended indefinitely.
> On the other hand, resuming a checked continuation more than once will cause an expected crash,
> protecting you from undefined behavior.

With an asynchronous version, there is no longer any ambiguity.
After the function has completed, execution will always resume in the same
context it was started in.

```swift
await updateStyle(backgroundColor: color)
// style has been updated
```

The `withCheckedContinuation` function is one of a [suite of standard library
APIs][continuation-apis] that exist to make interfacing non-async and async code possible.

> Note: Introducing asynchronous code into a project can surface data isolation
checking violations. To understand and address these, see [Crossing Isolation Boundaries][]

[Crossing Isolation Boundaries]: <doc:CommonProblems#Crossing-Isolation-Boundaries>
[continuation-apis]: https://developer.apple.com/documentation/swift/concurrency#continuations

## Dynamic Isolation

Expressing the isolation of your program statically, using annotations and
other language constructs, is both powerful and concise.
But it can be difficult to introduce static isolation without updating
all dependencies simultaneously.

Dynamic isolation provides runtime mechanisms you can use as a fallback for
describing data isolation.
It can be an essential tool for interfacing a Swift 6 component
with another that has not yet been updated,
even if these components are within the _same_ module.

### Internal-Only Isolation

Suppose you have determined that a reference type within your project can be
best described with `MainActor` static isolation.

```swift
@MainActor
class WindowStyler {
    private var backgroundColor: ColorComponents

    func applyStyle() {
        // ...
    }
}
```

This `MainActor` isolation may be _logically_ correct.
But if this type is used in other unmigrated locations,
adding static isolation here could require many additional changes.
An alternative is to use dynamic isolation to help control the scope.

```swift
class WindowStyler {
    @MainActor
    private var backgroundColor: ColorComponents

    func applyStyle() {
        MainActor.assumeIsolated {
            // use and interact with other `MainActor` state
        }
    }
}
```

Here, the isolation has been internalized into the class.
This keeps any changes localized to the type, allowing you make
changes without affecting any clients of the type.

However, a major disadvantage of this technique is the type's true isolation
requirements remain invisible.
There is no way for clients to determine if or how they should change based on
this public API.
You should use this approach only as a temporary solution, and only when you
have exhausted other options.

### Usage-Only Isolation

If it is impractical to contain isolation exclusively within a type, you can
instead expand the isolation to cover only its API usage.

To do this, first apply static isolation to the type,
and then use dynamic isolation at any usage locations:

```swift
@MainActor
class WindowStyler {
    // ...
}

class UIStyler {
    @MainActor
    private let windowStyler: WindowStyler
    
    func applyStyle() {
        MainActor.assumeIsolated {
            windowStyler.applyStyle()
        }
    }
}
```

Combining static and dynamic isolation can be a powerful tool to keep the
scope of changes gradual.

### Explicit MainActor Context

The `assumeIsolated` method is synchronous and exists to recover isolation
information from runtime back into the type-system by preventing execution
if the assumption was incorrect.
The `MainActor` type also has a method you can use to manually switch
isolation in an asynchronous context.

```swift
// type that should be MainActor, but has not been updated yet
class PersonalTransportation {
}

await MainActor.run {
    // isolated to the MainActor here
    let transport = PersonalTransportation()
    
    // ...
}
```

Remember that static isolation allows the compiler to both verify and automate
the process of switching isolation as needed.
Even when used in combination with static isolation, it can be difficult
to determine when `MainActor.run` is truly necessary.
While `MainActor.run` can be useful during migration,
it should not be used as a substitute for expressing the isolation
requirements of your system statically.
The ultimate goal should still be to apply `@MainActor`
to `PersonalTransportation`.

## Missing Annotations

Dynamic isolation gives you tools to express isolation at runtime.
But you may also find you need to describe other concurrency properties
that are missing from unmigrated modules.

### Unmarked Sendable Closures

The sendability of a closure affects how the compiler infers isolation for its
body.
A callback closure that actually does cross isolation boundaries but is
_missing_ a `Sendable` annotation violates a critical invariant of the
concurrency system.

```swift
// definition within a pre-Swift 6 module
extension JPKJetPack {
    // Note the lack of a @Sendable annotation
    static func jetPackConfiguration(_ callback: @escaping () -> Void) {
        // Can potentially cross isolation domains
    }
}

@MainActor
class PersonalTransportation {
    func configure() {
        JPKJetPack.jetPackConfiguration {
            // MainActor isolation will be inferred here
            self.applyConfiguration()
        }
    }

    func applyConfiguration() {
    }
}
```

If `jetPackConfiguration` can invoke its closure in another isolation domain,
it must be marked `@Sendable`.
When an un-migrated module hasn't yet done this, it will result in incorrect
actor inference.
This code will compile without issue but crash at runtime.

> Note: It is not possible for the compiler to detect or diagnose the
_lack_ of compiler-visible information.

To workaround this, you can manually annotate the closure with `@Sendable.`
This will prevent the compiler from inferring `MainActor` isolation.
Because the compiler now knows actor isolation could change,
it will require a task at the callsite and an await in the task.

```swift
@MainActor
class PersonalTransportation {
    func configure() {
        JPKJetPack.jetPackConfiguration { @Sendable in
            // Sendable closures do not infer actor isolation,
            // making this context non-isolated
            Task {
                await self.applyConfiguration()
            }
        }
    }

    func applyConfiguration() {
    }
}
```

Alternatively, it is also possible to disable runtime isolation assertions
for the module with the `-disable-dynamic-actor-isolation` compiler flag.
This will suppress all runtime enforcement of dynamic actor isolation.

> Warning: This flag should be used with caution.
Disabling these runtime checks will permit data isolation violations.

## Integrating DispatchSerialQueue with Actors

By default, the mechanism actors use to schedule and execute work
is system-defined.
However you can override this to provide a custom implementation.
The `DispatchSerialQueue` type includes built-in support for this facility.

```swift
actor LandingSite {
    private let queue = DispatchSerialQueue(label: "something")

    nonisolated var unownedExecutor: UnownedSerialExecutor {
        queue.asUnownedSerialExecutor()
    }

    func acceptTransport(_ transport: PersonalTransportation) {
        // this function will be running on queue
    }
}
```

This can be useful if you want to migrate a type towards the actor model
while maintaining compatibility with code that depends on `DispatchQueue`.

## Backwards Compatibility

It's important to keep in mind that static isolation, being part of the type
system, affects your public API.
But you can migrate your own modules in a way that improves their APIs for
Swift 6 *without* breaking any existing clients.

Suppose the `WindowStyler` is public API.
You have determined that it really should be `MainActor`-isolated, but want to
ensure backwards compatibility for clients.

```swift
@preconcurrency @MainActor
public class WindowStyler {
    // ...
}
```

Using `@preconcurrency` this way marks the isolation as conditional on the
client module also having complete checking enabled.
This preserves source compatibility with clients that have not yet begun
adopting Swift 6.

## Dependencies

Often, you aren't in control of the modules you need to import as dependencies.
If these modules have not yet adopted Swift 6, you may find yourself with
errors that are difficult or impossible to resolve.

There are a number of different kinds of problems that result from using
unmigrated code.
The `@preconcurrency` annotation can help with many of these situations:

- [Non-Sendable types][]
- Mismatches in [protocol-conformance isolation][]

[Non-Sendable types]: <doc:CommonProblems#Non-Sendable-Types>
[protocol-conformance isolation]: <doc:CommonProblems#Protocol-Conformance-Isolation-Mismatch>

## C/Objective-C

You can expose Swift concurrency support for your C and Objective-C APIs
using annotations.
This is made possible by Clang's
[concurrency-specific annotations][clang-annotations]:

[clang-annotations]: https://clang.llvm.org/docs/AttributeReference.html#customizing-swift-import

```
__attribute__((swift_attr(“@Sendable”)))
__attribute__((swift_attr(“@_nonSendable”)))
__attribute__((swift_attr("nonisolated")))
__attribute__((swift_attr("@UIActor")))
__attribute__((swift_attr("sending")))

__attribute__((swift_async(none)))
__attribute__((swift_async(not_swift_private, COMPLETION_BLOCK_INDEX))
__attribute__((swift_async(swift_private, COMPLETION_BLOCK_INDEX)))
__attribute__((__swift_async_name__(NAME)))
__attribute__((swift_async_error(none)))
__attribute__((__swift_attr__("@_unavailableFromAsync(message: \"" msg "\")")))
```

When working with a project that can import Foundation, the following
annotation macros are available in `NSObjCRuntime.h`:

```
NS_SWIFT_SENDABLE
NS_SWIFT_NONSENDABLE
NS_SWIFT_NONISOLATED
NS_SWIFT_UI_ACTOR
NS_SWIFT_SENDING

NS_SWIFT_DISABLE_ASYNC
NS_SWIFT_ASYNC(COMPLETION_BLOCK_INDEX)
NS_REFINED_FOR_SWIFT_ASYNC(COMPLETION_BLOCK_INDEX)
NS_SWIFT_ASYNC_NAME
NS_SWIFT_ASYNC_NOTHROW
NS_SWIFT_UNAVAILABLE_FROM_ASYNC(msg)
```

### Dealing with missing isolation annotations in Objective-C libraries

While the SDKs and other Objective-C libraries make progress in adopting Swift concurrency,
they will often go through the exercise of codifying contracts which were only explained in
documentation. For example, before Swift concurrency, APIs frequently had to document their
threading behavior with comments like "this will always be called on the main thread".

Swift concurrency enables us to turn these code comments, into compiler and runtime 
enforced isolation checks, that Swift will then verify when you adopt such APIs.

For example, the fictional `NSJetPack` protocol generally invokes all of its delegate methods
on the main thread, and therefore has now become MainActor-isolated. 

The library author can mark as MainActor isolated using the `NS_SWIFT_UI_ACTOR` attribute,
which is equivalent to annotating a type using `@MainActor` in Swift: 

```swift
NS_SWIFT_UI_ACTOR
@protocol NSJetPack // fictional protocol
  // ...
@end
```

Thanks to this, all member methods of this protocol inherit the `@MainActor` isolation, 
and for most methods this is correct. 

However, in this example, let us consider a method which was previously documented as follows:

```objc
NS_SWIFT_UI_ACTOR // SDK author annotated using MainActor in recent SDK audit
@protocol NSJetPack // fictional protocol
/* Return YES if this jetpack supports flying at really high altitude!
 
 JetPackKit invokes this method at a variety of times, and not always on the main thread. For example, ...
*/
@property(readonly) BOOL supportsHighAltitude;

@end
```

This method's isolation was accidentally inferred as `@MainActor`, because of the annotation on the enclosing type.
Although it has specifically documented a different threading strategy - it may or may not
be invoked on the main actor - annotating these semantics on the method was accidentally missed. 

This is an annotation problem in the fictional JetPackKit library. 
Specifically, it is missing a `nonisolated` annotation on the method,
which would inform Swift about the correct and expected execution semantics.

Swift code adopting this library may look like this:

```swift
@MainActor
final class MyJetPack: NSJetPack {
  override class var supportsHighAltitude: Bool { // runtime crash in Swift 6 mode
    true
  }
}
```

The above code will crash with a runtime check, which aims to ensure we are actually 
executing on the main actor as we're crossing from objective-c's non-swift-concurrency
land into Swift.

It is a Swift 6 feature to detect such issues automatically and crash at runtime 
when such expectations are violated. Leaving such issues un-diagnosed, could lead
to actual hard-to-detect data races, and undermine Swift 6's promise about data-race safety.

Such failure would include a similar backtrace to this:

```
* thread #5, queue = 'com.apple.root.default-qos', stop reason = EXC_BREAKPOINT (code=1, subcode=0x1004f8a5c)
  * frame #0: 0x00000001004..... libdispatch.dylib`_dispatch_assert_queue_fail + 120
    frame #1: 0x00000001004..... libdispatch.dylib`dispatch_assert_queue + 196
    frame #2: 0x0000000275b..... libswift_Concurrency.dylib`swift_task_isCurrentExecutorImpl(swift::SerialExecutorRef) + 280
    frame #3: 0x0000000275b..... libswift_Concurrency.dylib`Swift._checkExpectedExecutor(_filenameStart: Builtin.RawPointer, _filenameLength: Builtin.Word, _filenameIsASCII: Builtin.Int1, _line: Builtin.Word, _executor: Builtin.Executor) -> () + 60
    frame #4: 0x00000001089..... MyApp.debug.dylib`@objc static JetPack.supportsHighAltitude.getter at <compiler-generated>:0
    ...
    frame #10: 0x00000001005..... libdispatch.dylib`_dispatch_root_queue_drain + 404
    frame #11: 0x00000001005..... libdispatch.dylib`_dispatch_worker_thread2 + 188
    frame #12: 0x00000001005..... libsystem_pthread.dylib`_pthread_wqthread + 228
```

> Note: When encountering such an issue, and by investigating the documentation and API annotations you determine something
>  was incorrectly annotated, the best way to resolve the root cause of the problem is to report the issue back to the 
>  library maintainer.

As you can see, the runtime injected an executor check into the call, and the dispatch queue assertion (of it running on the MainActor), 
has failed. This prevents sneaky and hard to debug data-races.

The correct long-term solution to this issue is the library fixing the method's annotation, by marking it as `nonisolated`:

```objc
// Solution in the library providing the API:
@property(readonly) BOOL supportsHighAltitude NS_SWIFT_NONISOLATED;
````

Until the library fixes its annotation issue, you are able to witness the method using a correctly `nonisolated` method, like this:

```swift
// Solution in adopting client code, wishing to run in Swift 6 mode:
@MainActor
final class MyJetPack: NSJetPack {
  // Correct
  override nonisolated class var supportsHighAltitude: Bool {
    true
  }
}
```

This way Swift knows not to check for the not-correct assumption that the method requires main actor isolation.




================================================
FILE: Guide.docc/Info.plist
================================================
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>CFBundleDisplayName</key>
	<string>Swift 6 Concurrency Migration Guide</string>
	<key>CFBundleIdentifier</key>
	<string>org.swift.migration.6</string>
	<key>CDDefaultModuleKind</key>
	<string></string>
</dict>
</plist>


================================================
FILE: Guide.docc/LibraryEvolution.md
================================================
# Library Evolution

Annotate library APIs for concurrency while preserving source and ABI
compatibility.

Concurrency annotations such as `@MainActor` and `@Sendable` can impact source
and ABI compatibility. Library authors should be aware of these implications when
annotating existing APIs.

## Preconcurrency annotations

The `@preconcurrency` attribute can be used directly on library APIs to
stage in new concurrency requirements that are checked at compile time
without breaking source or ABI compatibility for clients:

```swift
@preconcurrency @MainActor
struct S { ... }

@preconcurrency
public func performConcurrently(
  completion: @escaping @Sendable () -> Void
) { ... }
```

Clients do not need to use a `@preconcurrency import` for the new errors
to be downgraded. If the clients build with minimal concurrency checking,
errors from `@preconcurrency` APIs will be suppressed. If the clients build
with complete concurrency checking or the Swift 6 language mode, the errors
will be downgraded to warnings.

For ABI compatibility, `@preconcurrency` will mangle symbol names without any
concurrency annotations. If an API was introduced with some concurrency
annotations, and is later updated to include additional concurrency
annotations, then applying `@preconcurrency` is not sufficient for preserving
mangling. `@_silgen_name` can be used in cases where you need more precise
control over mangling concurrency annotations.

Note that all APIs imported from C, C++, and Objective-C are automatically
considered `@preconcurrency`. Concurrency attributes can always be applied
to these APIs using `__attribute__((__swift_attr__("<attribute name>")))`
without breaking source or ABI compatibility.

## Sendable

### Conformances on concrete types

Adding a `Sendable` conformance to a concrete type, including conditional
conformances, is typically a source compatible change in practice.

**Source and ABI compatible:**

```diff
-public struct S
+public struct S: Sendable
```

Like any other conformance, adding a conformance to `Sendable` can change
overload resolution if the concrete type satisfies more specialized
requirements. However, it's unlikely that an API which overloads on a
`Sendable` conformance would change type inference in a way that breaks
source compatibility or program behavior.

Adding a `Sendable` conformance to a concrete type, and not one of its type
parameters, is always an ABI compatible change.

### Generic requirements

Adding a `Sendable` conformance requirement to a generic type or function is
a source incompatible change, because it places a restriction on generic
arguments passed by the client.

**Source and ABI incompatible:**

```diff
-public func generic<T>
+public func generic<T> where T: Sendable
```

**To resolve:** Apply `@preconcurrency` to the type or function declaration to
downgrade requirement failures to warnings and preserve ABI:

```swift
@preconcurrency
public func generic<T> where T: Sendable { ... }
```

### Function types

Like generic requirements, adding `@Sendable` to a function type is a 
source and ABI incompatible change:

**Source and ABI incompatible:**

```diff
-public func performConcurrently(completion: @escaping () -> Void)
+public func performConcurrently(completion: @escaping @Sendable () -> Void)
```

**To resolve:** Apply `@preconcurrency` to the enclosing function declaration
to downgrade requirement failures to warnings and preserve ABI:

```swift
@preconcurrency
public func performConcurrently(completion: @escaping @Sendable () -> Void)
```

## Main actor annotations

### Protocols and types

Adding `@MainActor` annotations to protocols or type declarations is a source
and ABI incompatible change.

**Source and ABI incompatible:**

```diff
-public protocol P
+@MainActor public protocol P

-public class C
+@MainActor public class C
```

Adding `@MainActor` to protocols and type declarations has a wider impact than
other concurrency annotations because the `@MainActor` annotation can be
inferred throughout client code, including protocol conformances, subclasses,
and extension methods.

Applying `@preconcurrency` to the protocol or type declaration will downgrade
actor isolation errors based on the concurrency checking level. However,
`@preconcurrency` is not sufficient for preserving ABI compatibility for
clients in cases where the `@preconcurrency @MainActor` annotation can be
inferred on other declarations in client code. For example, consider the
following API in a client library:

```swift
extension P {
  public func onChange(action: @escaping @Sendable () -> Void)
}
```

If `P` is retroactively annotated with `@preconcurrency @MainActor`, these
annotations will be inferred on the extension method. If an extension method is
also part of a library with ABI compatibility constraints, then
`@preconcurrency` will strip all concurrency related annotations from mangling.
This can be worked around in the client library either by applying the
appropriate isolation explicitly, such as:

```swift
extension P {
  nonisolated public func onChange(action: @escaping @Sendable () -> Void)
}
```

Language affordances for precise control over the ABI of a declaration are
[under development][ABIAttr].

[ABIAttr]: https://forums.swift.org/t/pitch-controlling-the-abi-of-a-declaration/75123

### Function declarations and types

Adding `@MainActor` to a function declaration or a function type is a
source and ABI incompatible change.

**Source and ABI incompatible:**

```diff
-public func runOnMain()
+@MainActor public func runOnMain()

-public func performConcurrently(completion: @escaping () -> Void)
+public func performConcurrently(completion: @escaping @MainActor () -> Void)
```

**To resolve:** Apply `@preconcurrency` to the enclosing function declaration
to downgrade requirement failures to warnings and preserve ABI:

```swift
@preconcurrency @MainActor
public func runOnMain() { ... }

@preconcurrency
public func performConcurrently(completion: @escaping @MainActor () -> Void) { ... }
```

## `sending` parameters and results

Adding `sending` to a result lifts restrictions in client code, and is
always a source and ABI compatible change:

**Source and ABI compatible:**

```diff
-public func getValue() -> NotSendable
+public func getValue() -> sending NotSendable
```

However, adding `sending` to a parameter is more restrictive at the caller.

**Source and ABI incompatible:**

```diff
-public func takeValue(_: NotSendable)
+public func takeValue(_: sending NotSendable)
```

There is currently no way to stage in a new `sending` annotation on a parameter
without breaking source compatibility.

### Replacing `@Sendable` with `sending`

Replacing an existing `@Sendable` annotation with `sending` on a closure
parameter is a source compatible, ABI incompatible change.

**Source compatible, ABI incompatible:**

```diff
-public func takeValue(_: @Sendable @escaping () -> Void)
+public func takeValue(_: sending @escaping () -> Void)
```

**To resolve:** Adding `sending` to a parameter changes name mangling, so any
adoption must preserve the mangling using `@_silgen_name`. Adopting `sending`
in parameter position must preserve the ownership convention of parameters. No
additional annotation is necessary if the parameter already has an explicit
ownership modifier. For all functions except initializers, use
`__shared sending` to preserve the ownership convention:

```swift
public func takeValue(_: __shared sending NotSendable)
```

For initializers, `sending` preserves the default ownership convention, so it's not
necessary to specify an ownership modifier when adopting `sending` on initializer
parameters:

```swift
public class C {
  public init(ns: sending NotSendable)
}
```



================================================
FILE: Guide.docc/MigrationGuide.md
================================================
# Migrating to Swift 6

@Metadata {
  @TechnologyRoot
}

@Options(scope: global) {
  @AutomaticSeeAlso(disabled)
  @AutomaticTitleHeading(disabled)
  @AutomaticArticleSubheading(disabled)
}

## Overview

Swift's concurrency system, introduced in [Swift 5.5](https://www.swift.org/blog/swift-5.5-released/),
makes asynchronous and parallel code easier to write and understand.
With the Swift 6 language mode, the compiler can now 
guarantee that concurrent programs are free of data races.
When enabled, compiler safety checks that were
previously optional become required.

Adopting the Swift 6 language mode is entirely under your control
on a per-target basis.
Targets that build with previous modes, as well as code in other
languages exposed to Swift, can all interoperate with
modules that have been migrated to the Swift 6 language mode.

It is possible you have been incrementally adopting concurrency features
as they were introduced.
Or, you may have been waiting for the Swift 6 release to begin using them.
Regardless of where your project is in this process, this guide provides
concepts and practical help to ease the migration.

You will find articles and code examples here that:

- Explain the concepts used by Swift's data-race safety model.
- Outline a possible way to get started with migration.
- Show how to enable complete concurrency checking for Swift 5 projects.
- Demonstrate how to enable the Swift 6 language mode.
- Present strategies to resolve common problems.
- Provide techniques for incremental adoption.

> Important: The Swift 6 language mode is _opt-in_.
Existing projects will not switch to this mode without configuration changes.
>
> There is a distinction between the _compiler version_ and _language mode_.
The Swift 6 compiler supports four distinct language modes: "6", "5", "4.2",
and "4".

### Contributing

This guide is under active development. You can view the source, see
full code examples, and learn about how to contribute in the [repository][].
We would love your contributions in the form of:

- Filing [issues][] to cover specific code patterns or additional sections of the guide
- Opening pull requests to improve existing content or add new content
- Reviewing others' [pull requests][] for clarity and correctness of writing and code examples

For more information, see the [contributing][] document.

[repository]: https://github.com/apple/swift-migration-guide
[issues]: https://github.com/apple/swift-migration-guide/issues
[pull requests]: https://github.com/apple/swift-migration-guide/pulls
[contributing]: https://github.com/apple/swift-migration-guide/blob/main/CONTRIBUTING.md

## Topics

- <doc:DataRaceSafety>
- <doc:MigrationStrategy>
- <doc:CompleteChecking>
- <doc:Swift6Mode>
- <doc:CommonProblems>
- <doc:IncrementalAdoption>
- <doc:SourceCompatibility>
- <doc:LibraryEvolution>

### Swift Concurrency in Depth

- <doc:RuntimeBehavior>



================================================
FILE: Guide.docc/MigrationStrategy.md
================================================
# Migration Strategy

Get started migrating your project to the Swift 6 language mode.

Enabling complete concurrency checking in a module can yield many data-race
safety issues reported by the compiler.
Hundreds, possibly even thousands of warnings are not uncommon.
When faced with a such a large number of problems,
especially if you are just beginning to learn about Swift's data isolation
model, this can feel insurmountable.

**Don't panic.**

Frequently, you'll find yourself making substantial progress with just a few
changes.
And as you do, your mental model of how the Swift concurrency system works
will develop just as rapidly.

> Important: This guidance should not be interpreted as a recommendation.
You should feel confident about using other approaches.

## Strategy

This document outlines a general strategy that could be a good starting point.
There is no one single approach that will work for all projects.

The approach has three key steps:

- Select a module
- Enable stricter checking with Swift 5
- Address warnings

This process will be inherently _iterative_.
Even a single change in one module can have a large impact on the state of the
project as a whole.

## Begin from the Outside

It can be easier to start with the outer-most root module in a project.
This, by definition, is not a dependency of any other module.
Changes here can only have local effects, making it possible to
keep work contained.

Your changes do not _need_ to be contained to the module, however.
Dependencies under your control that have [unsafe global state][Global] or
[trivially-`Sendable` types][Sendable] can be the root cause of many warnings
across your project.
These can often be the best things to focus on first.

[Global]: <doc:CommonProblems#Unsafe-Global-and-Static-Variables>
[Sendable]: <doc:CommonProblems#Implicitly-Sendable-Types>

## Use the Swift 5 Language Mode

You could find it quite challenging to move a project from Swift 5 with no
checking directly to the Swift 6 language mode.
It is possible, instead, to incrementally enable more of the Swift 6 checking
mechanisms while remaining in Swift 5 mode.
This will surface issues only as warnings,
keeping your build and tests functional as you progress.

To start, enable a single upcoming concurrency feature.
This allows you to focus on one _specific type_ of problem at a time.

Proposal    | Description | Feature Flag 
:-----------|-------------|-------------
[SE-0401][] | Remove Actor Isolation Inference caused by Property Wrappers | `DisableOutwardActorInference`
[SE-0412][] | Strict concurrency for global variables | `GlobalConcurrency`
[SE-0418][] | Inferring `Sendable` for methods and key path literals | `InferSendableFromCaptures`

[SE-0401]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0401-remove-property-wrapper-isolation.md
[SE-0412]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0412-strict-concurrency-for-global-variables.md
[SE-0418]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0418-inferring-sendable-for-methods.md

These can be enabled independently and in any order.

After you have addressed issues uncovered by upcoming feature flags,
the next step is to [enable complete checking][CompleteChecking] for the module.
This will turn on all of the compiler's remaining data isolation checks.

[CompleteChecking]: <doc:CompleteChecking>

## Address Warnings

There is one guiding principle you should use as you investigate
warnings: **express what is true now**.
Resist the urge to refactor your code to address issues.

You will find it beneficial to minimize the amount of change necessary to
get to a warning-free state with complete concurrency checking.
After that is done, use any unsafe opt-outs you applied as an indication of
follow-on refactoring opportunities to introduce a safer isolation mechanism.

> Note: To learn more about addressing common problems, see <doc:CommonProblems>.

## Iteration

At first, you'll likely be employing techniques to disable or workaround
data isolation problems.
Once you feel like you've reached the stopping point for a higher-level module,
target one of its dependencies that has required a workaround.

You don't have to eliminate all warnings to move on.
Remember that sometimes very minor changes can have a significant impact.
You can always return to a module once one of its dependencies has been
updated.



================================================
FILE: Guide.docc/RuntimeBehavior.md
================================================
# Runtime Behavior

Learn how Swift concurrency runtime semantics differ from other runtimes you may
be familiar with, and familiarize yourself with common patterns to achieve
similar end results in terms of execution semantics.

Swift's concurrency model with a strong focus on async/await, actors and tasks,
means that some patterns from other libraries or concurrency runtimes don't
translate directly into this new model. In this section, we'll explore common
patterns and differences in runtime behavior to be aware of, and how to address
them while you migrate your code to Swift concurrency.

## Limiting concurrency using Task Groups

Sometimes you may find yourself with a large list of work to be processed.

While it is possible to just enqueue "all" those work items to a task group like this:

```swift
// Potentially wasteful -- perhaps this creates thousands of tasks concurrently (?!)

let lotsOfWork: [Work] = ...
await withTaskGroup(of: Something.self) { group in
  for work in lotsOfWork {
    // If this is thousands of items, we may end up creating a lot of tasks here.
    group.addTask {
      await work.work()
    }
  }

  for await result in group {
    process(result) // process the result somehow, depends on your needs
  }
}
```

If you expect to deal with hundreds or thousands of items, it might be inefficient to enqueue them all immediately.
Creating a task (in `addTask`) allocates memory for the task in order to suspend and execute it.
While the amount of memory for each task isn't large, it can be significant when creating thousands of tasks that won't execute immediately.

When faced with such a situation, you can manually throttle the number of concurrently added tasks in the group, as follows:

```swift
let lotsOfWork: [Work] = ... 
let maxConcurrentWorkTasks = min(lotsOfWork.count, 10)
assert(maxConcurrentWorkTasks > 0)

await withTaskGroup(of: Something.self) { group in
    var submittedWork = 0
    for _ in 0..<maxConcurrentWorkTasks {
        group.addTask { // or 'addTaskUnlessCancelled'
            await lotsOfWork[submittedWork].work() 
        }
        submittedWork += 1
    }
    
    for await result in group {
        process(result) // process the result somehow, depends on your needs
    
        // Every time we get a result back, check if there's more work we should submit and do so
        if submittedWork < lotsOfWork.count, 
           let remainingWorkItem = lotsOfWork[submittedWork] {
            group.addTask { // or 'addTaskUnlessCancelled'
                await remainingWorkItem.work() 
            }  
            submittedWork += 1
        }
    }
}
```



================================================
FILE: Guide.docc/SourceCompatibility.md
================================================
# Source Compatibility

See an overview of potential source compatibility issues.

Swift 6 includes a number of evolution proposals that could potentially affect
source compatibility.
These are all opt-in for the Swift 5 language mode.

> Note: For the previous release’s Migration Guide, see [Migrating to Swift 5][swift5].

[swift5]: https://www.swift.org/migration-guide-swift5/

## Handling Future Enum Cases

[SE-0192][]: `NonfrozenEnumExhaustivity`

Lack of a required `@unknown default` has changed from a warning to an error.

[SE-0192]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0192-non-exhaustive-enums.md

## Concise magic file names

[SE-0274][]: `ConciseMagicFile`

The special expression `#file` has changed to a human-readable string
containing the filename and module name.

[SE-0274]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0274-magic-file.md

## Forward-scan matching for trailing closures

[SE-0286][]: `ForwardTrailingClosures`

Could affect code involving multiple, defaulted closure parameters.

[SE-0286]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0286-forward-scan-trailing-closures.md

## Incremental migration to concurrency checking

[SE-0337][]: `StrictConcurrency`

Will introduce errors for any code that risks data races.

[SE-0337]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0337-support-incremental-migration-to-concurrency-checking.md

> Note: This feature implicitly also enables [`IsolatedDefaultValues`](#Isolated-default-value-expressions),
[`GlobalConcurrency`](#Strict-concurrency-for-global-variables),
and [`RegionBasedIsolation`](#Region-based-Isolation).

## Implicitly Opened Existentials

[SE-0352][]: `ImplicitOpenExistentials`

Could affect overload resolution for functions that involve both
existentials and generic types.

[SE-0352]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0352-implicit-open-existentials.md

## Regex Literals

[SE-0354][]: `BareSlashRegexLiterals`

Could impact the parsing of code that was previously using a bare slash.

[SE-0354]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0354-regex-literals.md

## Deprecate @UIApplicationMain and @NSApplicationMain

[SE-0383][]: `DeprecateApplicationMain`

Will introduce an error for any code that has not migrated to using `@main`.

[SE-0383]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0383-deprecate-uiapplicationmain-and-nsapplicationmain.md

## Importing Forward Declared Objective-C Interfaces and Protocols

[SE-0384][]: `ImportObjcForwardDeclarations`

Will expose previously-invisible types that could conflict with existing
sources.

[SE-0384]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0384-importing-forward-declared-objc-interfaces-and-protocols.md

## Remove Actor Isolation Inference caused by Property Wrappers

[SE-0401][]: `DisableOutwardActorInference`

Could change the inferred isolation of a type and its members.

[SE-0401]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0401-remove-property-wrapper-isolation.md

## Isolated default value expressions

[SE-0411][]: `IsolatedDefaultValues`

Will introduce errors for code that risks data races.

[SE-0411]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0411-isolated-default-values.md

##  Strict concurrency for global variables

[SE-0412][]: `GlobalConcurrency`

Will introduce errors for code that risks data races.

[SE-0412]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0412-strict-concurrency-for-global-variables.md

## Region based Isolation

[SE-0414][]: `RegionBasedIsolation`

Increases the constraints of the `Actor.assumeIsolated` function.

[SE-0414]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0414-region-based-isolation.md

## Inferring `Sendable` for methods and key path literals

[SE-0418][]: `InferSendableFromCaptures`

Could affect overload resolution for functions that differ only by sendability.

[SE-0418]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0418-inferring-sendable-for-methods.md

## Dynamic actor isolation enforcement from non-strict-concurrency contexts

[SE-0423][]: `DynamicActorIsolation`

Introduces new assertions that could affect existing code if the runtime
isolation does not match expectations.

[SE-0423]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0423-dynamic-actor-isolation.md

## Usability of global-actor-isolated types

[SE-0434][]: `GlobalActorIsolatedTypesUsability`

Could affect type inference and overload resolution for functions that are
globally-isolated but not `@Sendable`. 

[SE-0434]: https://github.com/swiftlang/swift-evolution/blob/main/proposals/0434-global-actor-isolated-types-usability.md



================================================
FILE: Guide.docc/Swift6Mode.md
================================================
# Enabling The Swift 6 Language Mode

Guarantee your code is free of data races by enabling the Swift 6 language mode.

## Using the Swift compiler

To enable the Swift 6 language mode when running `swift` or `swiftc`
directly at the command line, pass `-swift-version 6`:

```
~ swift -swift-version 6 main.swift
```

## Using SwiftPM

### Command-line invocation

`-swift-version 6` can be passed in a Swift package manager command-line
invocation using the `-Xswiftc` flag:

```
~ swift build -Xswiftc -swift-version -Xswiftc 6
~ swift test -Xswiftc -swift-version -Xswiftc 6
```

### Package manifest

A `Package.swift` file that uses `swift-tools-version` of `6.0` will enable the Swift 6 language
mode for all targets. You can still set the language mode for the package as a whole using the
`swiftLanguageModes` property of `Package`. However, you can now also change the language mode as
needed on a per-target basis using the new `swiftLanguageMode` build setting:

```swift
// swift-tools-version: 6.0

let package = Package(
    name: "MyPackage",
    products: [
        // ...
    ],
    targets: [
        // Uses the default tools language mode (6)
        .target(
            name: "FullyMigrated",
        ),
        // Still requires 5
        .target(
            name: "NotQuiteReadyYet",
            swiftSettings: [
                .swiftLanguageMode(.v5)
            ]
        )
    ]
)
```

Note that if your package needs to continue supporting earlier Swift toolchain versions and you want
to use per-target `swiftLanguageMode`, you will need to create a version-specific manifest for pre-6
toolchains. For example, if you'd like to continue supporting 5.9 toolchains and up, you could have
one manifest `Package@swift-5.9.swift`:
```swift
// swift-tools-version: 5.9

let package = Package(
    name: "MyPackage",
    products: [
        // ...
    ],
    targets: [
        .target(
            name: "FullyMigrated",
        ),
        .target(
            name: "NotQuiteReadyYet",
        )
    ]
)
```

And another `Package.swift` for Swift toolchains 6.0+:
```swift
// swift-tools-version: 6.0

let package = Package(
    name: "MyPackage",
    products: [
        // ...
    ],
    targets: [
        // Uses the default tools language mode (6)
        .target(
            name: "FullyMigrated",
        ),
        // Still requires 5
        .target(
            name: "NotQuiteReadyYet",
            swiftSettings: [
                .swiftLanguageMode(.v5)
            ]
        )
    ]
)
```

If instead you would just like to use Swift 6 language mode when it's available (while still
continuing to support older modes) you can keep a single `Package.swift` and specify the version in
a compatible manner:
```swift
// swift-tools-version: 5.9

let package = Package(
    name: "MyPackage",
    products: [
        // ...
    ],
    targets: [
        .target(
            name: "FullyMigrated",
        ),
    ],
    // `swiftLanguageVersions` and `.version("6")` to support pre 6.0 swift-tools-version.
    swiftLanguageVersions: [.version("6"), .v5]
)
```


## Using Xcode

### Build Settings

You can control the language mode for an Xcode project or target by setting
the "Swift Language Version" build setting to "6".

### XCConfig

You can also set the `SWIFT_VERSION` setting to `6` in an xcconfig file:

```
// In a Settings.xcconfig

SWIFT_VERSION = 6;
```



================================================
FILE: Sources/Examples/Boundaries.swift
================================================
import Library

// MARK: Core Example Problem

/// A `MainActor`-isolated function that accepts non-`Sendable` parameters.
@MainActor
func applyBackground(_ color: ColorComponents) {
}

#if swift(<6.0)
/// A non-isolated function that accepts non-`Sendable` parameters.
func updateStyle(backgroundColor: ColorComponents) async {
    // the `backgroundColor` parameter is being moved from the
    // non-isolated domain to the `MainActor` here.
    //
    // Swift 5 Warning: passing argument of non-sendable type 'ColorComponents' into main actor-isolated context may introduce data races
    // Swift 6 Error: sending 'backgroundColor' risks causing data races
    await applyBackground(backgroundColor)
}
#endif

#if swift(>=6.0)
/// A non-isolated function that accepts non-`Sendable` parameters which must be safe to use at callsites.
func sending_updateStyle(backgroundColor: sending ColorComponents) async {
    await applyBackground(backgroundColor)
}
#endif

// MARK: Latent Isolation

/// MainActor-isolated function that accepts non-`Sendable` parameters.
@MainActor
func isolatedFunction_updateStyle(backgroundColor: ColorComponents) async {
    // This is safe because backgroundColor cannot change domains. It also
    // now no longer necessary to await the call to `applyBackground`.
    applyBackground(backgroundColor)
}

// MARK: Explicit Sendable

/// An overload used by `sendable_updateStyle` to match types.
@MainActor
func applyBackground(_ color: SendableColorComponents) {
}

/// The Sendable variant is safe to pass across isolation domains.
func sendable_updateStyle(backgroundColor: SendableColorComponents) async {
    await applyBackground(backgroundColor)
}

// MARK: Computed Value

/// A Sendable function is used to compute the value in a different isolation domain.
func computedValue_updateStyle(using backgroundColorProvider: @Sendable () -> ColorComponents) async {
    // The Swift 6 compiler can automatically determine this value is
    // being transferred in a safe way
    let components = backgroundColorProvider()
    await applyBackground(components)
}

#if swift(>=6.0)
/// A function that uses a sending parameter to leverage region-based isolation.
func sendingValue_updateStyle(backgroundColor: sending ColorComponents) async {
    await applyBackground(backgroundColor)
}
#endif

// MARK: Global Isolation
/// An overload used by `globalActorIsolated_updateStyle` to match types.
@MainActor
func applyBackground(_ color: GlobalActorIsolatedColorComponents) {
}

/// MainActor-isolated function that accepts non-`Sendable` parameters.
@MainActor
func globalActorIsolated_updateStyle(backgroundColor: GlobalActorIsolatedColorComponents) async {
    // This is safe because backgroundColor cannot change domains. It also
    // now no longer necessary to await the call to `applyBackground`.
    applyBackground(backgroundColor)
}

// MARK: actor isolation

/// An actor that assumes the responsibility of managing the non-Sendable data.
actor Style {
    private var background: ColorComponents

    init(background: ColorComponents) {
        self.background = background
    }

    func applyBackground() {
        // make use of background here
    }
}

// MARK: Manual Synchronization

extension RetroactiveColorComponents: @retroactive @unchecked Sendable {
}

/// An overload used by `retroactive_updateStyle` to match types.
@MainActor
func applyBackground(_ color: RetroactiveColorComponents	) {
}

/// A non-isolated function that accepts retroactively-`Sendable` parameters.
func retroactive_updateStyle(backgroundColor: RetroactiveColorComponents) async {
    await applyBackground(backgroundColor)
}

func exerciseBoundaryCrossingExamples() async {
    print("Isolation Boundary Crossing Examples")

#if swift(<6.0)
    print("  - updateStyle(backgroundColor:) passing its argument unsafely")
#endif

#if swift(>=6.0)
    print("  - using sending to allow safe usage of ColorComponents")
    let nonSendableComponents = ColorComponents()

    await sending_updateStyle(backgroundColor: nonSendableComponents)
#endif

    print("  - using ColorComponents only from the main actor")
    let t1 = Task { @MainActor in
        let components = ColorComponents()

        await isolatedFunction_updateStyle(backgroundColor: components)
    }

    await t1.value

    print("  - using preconcurrency_updateStyle to deal with non-Sendable argument")

    print("  - using a Sendable closure to defer creation")
    await computedValue_updateStyle(using: {
        ColorComponents()
    })

#if swift(>=6.0)
    print("  - enable region-based isolation with a sending argument")
    let capturableComponents = ColorComponents()

    await sendingValue_updateStyle(backgroundColor: capturableComponents)
#endif

    print("  - using a globally-isolated type")
    let components = await GlobalActorIsolatedColorComponents()

    await globalActorIsolated_updateStyle(backgroundColor: components)

    print("  - using an actor")
    let actorComponents = ColorComponents()

    let actor = Style(background: actorComponents)

    await actor.applyBackground()

    print("  - using a retroactive unchecked Sendable argument")
    let retroactiveComponents = RetroactiveColorComponents()

    await retroactive_updateStyle(backgroundColor: retroactiveComponents)
}



================================================
FILE: Sources/Examples/ConformanceMismatches.swift
================================================
import Library

// MARK: Under-Specified Protocol

#if swift(<6.0)
/// A conforming type that has now adopted global isolation.
@MainActor
class WindowStyler: Styler {
    // Swift 5 Warning: main actor-isolated instance method 'applyStyle()' cannot be used to satisfy nonisolated protocol requirement
    // Swift 6 Error: main actor-isolated instance method 'applyStyle()' cannot be used to satisfy nonisolated protocol requirement
    func applyStyle() {
    }
}
#endif

// MARK: Globally-Isolated Protocol

/// A type conforming to the global actor annotated `GloballyIsolatedStyler` protocol,
///  will infer the protocol's global actor isolation.
class GloballyIsolatedWindowStyler: GloballyIsolatedStyler {
    func applyStyle() {
    }
}

/// A type conforming to `PerRequirementIsolatedStyler` which has MainActor isolated protocol requirements,
/// will infer the protocol's requirements isolation for methods witnessing those protocol requirements *only*
/// for the satisfying methods.
class PerRequirementIsolatedWindowStyler: PerRequirementIsolatedStyler {
    func applyStyle() {
        // only this is MainActor-isolated
    }

    func checkStyle() {
        // this method is non-isolated; it is not witnessing any isolated protocol requirement
    }
}

// MARK: Asynchronous Requirements

/// A conforming type that can have arbitrary isolation and
/// still matches the async requirement.
class AsyncWindowStyler: AsyncStyler {
    func applyStyle() {
    }
}

// MARK: Using preconcurrency

/// A conforming type that will infer the protocol's global isolation *but*
/// with downgraded diagnostics in Swift 6 mode and Swift 5 + complete checking
class StagedGloballyIsolatedWindowStyler: StagedGloballyIsolatedStyler {
    func applyStyle() {
    }
}

// MARK: Using Dynamic Isolation

/// A conforming type that uses a nonisolated function to match
/// with dynamic isolation in the method body.
@MainActor
class DynamicallyIsolatedStyler: Styler {
    nonisolated func applyStyle() {
        MainActor.assumeIsolated {
            // MainActor state is available here
        }
    }
}

/// A conforming type that uses a preconcurency conformance, which
/// is a safer and more ergonomic version of DynamicallyIsolatedStyler.
@MainActor
class PreconcurrencyConformanceStyler: @preconcurrency Styler {
    func applyStyle() {
    }
}

// MARK: Non-Isolated

/// A conforming type that uses nonisolated and non-Sendable types but
/// still performs useful work.
@MainActor
class NonisolatedWindowStyler: StylerConfiguration {
    nonisolated var primaryColorComponents: ColorComponents {
        ColorComponents(red: 0.2, green: 0.3, blue: 0.4)
    }
}

// MARK: Conformance by Proxy

/// An intermediary type that conforms to the protocol so it can be
/// used by an actor
struct CustomWindowStyle: Styler {
    func applyStyle() {
    }
}

/// An actor that interacts with the Style protocol indirectly.
actor ActorWindowStyler {
    private let internalStyle = CustomWindowStyle()

    func applyStyle() {
        // forward the call through to the conforming type
        internalStyle.applyStyle()
    }
}

func exerciseConformanceMismatchExamples() async {
    print("Protocol Conformance Isolation Mismatch Examples")

    // Could also all be done with async calls, but this
    // makes the isolation, and the ability to invoke them
    // from a synchronous context explicit.
    await MainActor.run {
#if swift(<6.0)
        print("  - using a mismatched conformance")
        WindowStyler().applyStyle()
#endif

        print("  - using a MainActor-isolated type")
        GloballyIsolatedWindowStyler().applyStyle()

        print("  - using a per-requirement MainActor-isolated type")
        PerRequirementIsolatedWindowStyler().applyStyle()

        print("  - using an async conformance")
        AsyncWindowStyler().applyStyle()

        print("  - using staged isolation")
        StagedGloballyIsolatedWindowStyler().applyStyle()

        print("  - using dynamic isolation")
        DynamicallyIsolatedStyler().applyStyle()

        print("  - using a preconcurrency conformance")
        PreconcurrencyConformanceStyler().applyStyle()

        let value = NonisolatedWindowStyler().primaryColorComponents
        print("  - accessing a non-isolated conformance: ", value)
    }

    print("  - using an actor with a proxy conformance")
    await ActorWindowStyler().applyStyle()
}



================================================
FILE: Sources/Examples/DispatchQueue+PendingWork.swift
================================================
import Dispatch

extension DispatchQueue {
    /// Returns once any pending work has been completed.
    func pendingWorkComplete() async {
        // TODO: update to withCheckedContinuation https://github.com/apple/swift/issues/74206
        await withUnsafeContinuation { continuation in
            self.async(flags: .barrier) {
                continuation.resume()
            }
        }
    }
}



================================================
FILE: Sources/Examples/Globals.swift
================================================
import Dispatch

#if swift(<6.0)
/// An unsafe global variable.
///
/// See swift-6-concurrency-migration-guide/commonproblems/#Sendable-Types
var supportedStyleCount = 42
#endif

/// Version of `supportedStyleCount` that uses global-actor isolation.
@MainActor
var globallyIsolated_supportedStyleCount = 42

/// Version of `supportedStyleCount` that uses immutability.
let constant_supportedStyleCount = 42

/// Version of `supportedStyleCount` that uses a computed property.
var computed_supportedStyleCount: Int {
    42
}

/// Version of `supportedStyleCount` that uses manual synchronization via `sharedQueue`
nonisolated(unsafe) var queueProtected_supportedStyleCount = 42

/// A non-isolated async function used to exercise all of the global mutable state examples.
func exerciseGlobalExamples() async {
    print("Global Variable Examples")
#if swift(<6.0)
    // Here is how we access `supportedStyleCount` concurrently in an unsafe way
    for _ in 0..<10 {
        DispatchQueue.global().async {
            supportedStyleCount += 1
        }
    }

    print("  - accessing supportedStyleCount unsafely:", supportedStyleCount)

    await DispatchQueue.global().pendingWorkComplete()
#endif
    
    print("  - accessing globallyIsolated_supportedStyleCount")
    // establish a MainActor context to access the globally-isolated version
    await MainActor.run {
        globallyIsolated_supportedStyleCount += 1
    }

    // freely access the immutable version from any isolation domain
    print("  - accessing constant_supportedStyleCount when non-isolated: ", constant_supportedStyleCount)

    await MainActor.run {
        print("  - accessing constant_supportedStyleCount from MainActor: ", constant_supportedStyleCount)
    }

    // freely access the computed property from any isolation domain
    print("  - accessing computed_supportedStyleCount when non-isolated: ", computed_supportedStyleCount)

    // access the manually-synchronized version... carefully
    manualSerialQueue.async {
        queueProtected_supportedStyleCount += 1
    }

    manualSerialQueue.async {
        print("  - accessing queueProtected_supportedStyleCount: ", queueProtected_supportedStyleCount)
    }

    await manualSerialQueue.pendingWorkComplete()
}



================================================
FILE: Sources/Examples/IncrementalMigration.swift
================================================
import Dispatch
import ObjCLibrary

/// Example that backs an actor with a queue.
///
/// > Note: `DispatchSerialQueue`'s initializer was only made available in more recent OS versions.
@available(macOS 14.0, iOS 17.0, macCatalyst 17.0, tvOS 17.0, watchOS 10.0, *)
actor LandingSite {
    private let queue = DispatchSerialQueue(label: "SerialQueue")

	// this currently failed to build because of the @available usage, rdar://116684282
//    nonisolated var unownedExecutor: UnownedSerialExecutor {
//        queue.asUnownedSerialExecutor()
//    }

    func acceptTransport(_ transport: JPKJetPack) {
        // this function will be running on queue
    }
}

func exerciseIncrementalMigrationExamples() async {
    print("Incremental Migration Examples")

    if #available(macOS 14.0, iOS 17.0, macCatalyst 17.0, tvOS 17.0, watchOS 10.0, *) {
        print("  - using an actor with a DispatchSerialQueue executor")
        let site = LandingSite()

        let transport = JPKJetPack()

        await site.acceptTransport(transport)
    }
}



================================================
FILE: Sources/Examples/main.swift
================================================
import Dispatch

/// A Serial queue uses for manual synchronization
let manualSerialQueue = DispatchQueue(label: "com.apple.SwiftMigrationGuide")

// Note: top-level code provides an asynchronous MainActor-isolated context
await exerciseGlobalExamples()
await exerciseBoundaryCrossingExamples()
await exerciseConformanceMismatchExamples()
await exerciseIncrementalMigrationExamples()



================================================
FILE: Sources/Examples/PreconcurrencyImport.swift
================================================
@preconcurrency import Library

/// A non-isolated function  that accepts non-`Sendable` parameters.
func preconcurrency_updateStyle(backgroundColor: ColorComponents) async {
    // Swift 5: no diagnostics
    // Swift 6 Warning: sending 'backgroundColor' risks causing data races
    await applyBackground(backgroundColor)
}



================================================
FILE: Sources/Library/Library.swift
================================================
import Foundation

/// An example of a struct with only `Sendable` properties.
///
/// This type is **not** Sendable because it is public. If we want a public type to be `Sendable`, we must annotate it explicitly.
public struct ColorComponents {
    public let red: Float
    public let green: Float
    public let blue: Float

    public init(red: Float, green: Float, blue: Float) {
        self.red = red
        self.green = green
        self.blue = blue
    }

    public init() {
        self.red = 1.0
        self.green = 1.0
        self.blue = 1.0
    }
}

/// A variant of `ColorComponents` that could be marked as Sendable
public struct RetroactiveColorComponents {
    public let red: Float = 1.0
    public let green: Float = 1.0
    public let blue: Float = 1.0

    public init() {}
}

/// Explicitly-Sendable variant of `ColorComponents`.
public struct SendableColorComponents : Sendable {
    public let red: Float = 1.0
    public let green: Float = 1.0
    public let blue: Float = 1.0

    public init() {}
}

@MainActor
public struct GlobalActorIsolatedColorComponents : Sendable {
    public let red: Float = 1.0
    public let green: Float = 1.0
    public let blue: Float = 1.0

    public init() {}
}

public protocol Styler {
    func applyStyle()
}

@MainActor
public protocol GloballyIsolatedStyler {
    func applyStyle()
}

public protocol PerRequirementIsolatedStyler {
    @MainActor
    func applyStyle()
}

@preconcurrency @MainActor
public protocol StagedGloballyIsolatedStyler {
    func applyStyle()
}

public protocol AsyncStyler {
    func applyStyle() async
}

open class UIStyler {
}

public protocol InheritingStyler: UIStyler {
    func applyStyle()
}

public protocol StylerConfiguration {
    var primaryColorComponents: ColorComponents { get }
}



================================================
FILE: Sources/ObjCLibrary/JPKJetPack.h
================================================
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface JPKJetPack : NSObject

/// Disable async to show how completion handlers work explicitly.
+ (void)jetPackConfiguration:(void (NS_SWIFT_SENDABLE ^)(void))completionHandler NS_SWIFT_DISABLE_ASYNC;

@end

NS_ASSUME_NONNULL_END



================================================
FILE: Sources/ObjCLibrary/JPKJetPack.m
================================================
#import "JPKJetPack.h"

@implementation JPKJetPack

+ (void)jetPackConfiguration:(void (^)(void))completionHandler {
    dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
        completionHandler();
    });
}

@end



================================================
FILE: Sources/ObjCLibrary/ObjCLibrary.h
================================================
@import Foundation;

@interface OCMPattern : NSObject

@end

NS_SWIFT_UI_ACTOR
@interface PCMPatternStore : NSObject

@end

#import "JPKJetPack.h"



================================================
FILE: Sources/ObjCLibrary/ObjCLibrary.m
================================================
#import <Foundation/Foundation.h>

#import "ObjCLibrary.h"

@implementation OCMPattern

@end

@implementation PCMPatternStore

@end



================================================
SYMLINK: Sources/Swift5Examples -> Examples
================================================



================================================
SYMLINK: Sources/Swift6Examples -> Examples
================================================



================================================
FILE: Tests/Library/LibraryTests.swift
================================================
import Library
import ObjCLibrary
import Testing

struct LibraryTest {
    @Test func testNonIsolated() throws {
        let color = ColorComponents()

        #expect(color.red == 1.0)
    }

    @MainActor
    @Test func testIsolated() throws {
        let color = GlobalActorIsolatedColorComponents()

        #expect(color.red == 1.0)
    }

    @Test func testNonIsolatedWithGlobalActorIsolatedType() async throws {
        let color = await GlobalActorIsolatedColorComponents()

        await #expect(color.red == 1.0)
    }
}

extension LibraryTest {
    @Test func testCallbackOperation() async {
        await confirmation() { completion in
            // function explicitly opts out of an generated async version
            // so it requires a continuation here
            await withCheckedContinuation { continuation in
                JPKJetPack.jetPackConfiguration {
                    completion()
                    continuation.resume()
                }
            }
        }
    }
}



================================================
FILE: Tests/Library/LibraryXCTests.swift
================================================
import ObjCLibrary
import Library
import XCTest

final class LibraryXCTests: XCTestCase {
    func testNonIsolated() throws {
        let color = ColorComponents()

        XCTAssertEqual(color.red, 1.0)
    }

    @MainActor
    func testIsolated() throws {
        let color = GlobalActorIsolatedColorComponents()

        XCTAssertEqual(color.red, 1.0)
    }

    func testNonIsolatedWithGlobalActorIsolatedType() async throws {
        let color = await GlobalActorIsolatedColorComponents()
        let redComponent = await color.red

        XCTAssertEqual(redComponent, 1.0)
    }
}

extension LibraryXCTests {
    func testCallbackOperation() async {
        let exp = expectation(description: "config callback")

        JPKJetPack.jetPackConfiguration {
            exp.fulfill()
        }

        await fulfillment(of: [exp])
    }
}


