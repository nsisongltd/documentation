# Swift Best Practices

Swift code should be safe by default, expressive without being magical, and
clear about ownership, errors, concurrency, and API design.

## Core Principles

1. Prefer safety by default.
2. Use strong types to express intent.
3. Keep APIs clear at the call site.
4. Prefer value types when they model the domain well.
5. Make errors explicit.
6. Use protocol-oriented design when it simplifies behavior.
7. Avoid clever syntax that hides cost or control flow.

## Naming

1. Follow Swift API Design Guidelines.
2. Make call sites read naturally.
3. Use nouns for types and properties.
4. Use verbs for mutating operations.
5. Avoid abbreviations unless they are standard.
6. Use argument labels to clarify meaning.

```swift
func sendEmail(to recipient: EmailAddress, subject: String) throws {
    // Send email.
}
```

## Types

1. Prefer `struct` for value semantics.
2. Use `class` when identity, inheritance, or shared mutable state is needed.
3. Use `enum` for finite state.
4. Avoid optional values when a non-optional value can be guaranteed.
5. Use `Result` when storing success or failure as a value.
6. Keep force unwraps out of production code unless the invariant is guaranteed.

```swift
enum PaymentState {
    case pending
    case paid(receiptID: String)
    case failed(reason: String)
}
```

## Error Handling

1. Use `throws` for recoverable errors.
2. Use typed domain errors where helpful.
3. Preserve context when translating errors.
4. Avoid `try!` outside tests or guaranteed startup invariants.
5. Use `defer` for cleanup that must happen on every path.

```swift
do {
    let profile = try decoder.decode(UserProfile.self, from: data)
    return profile
} catch {
    throw ProfileError.invalidPayload(error)
}
```

## Optionals

1. Use `guard let` to exit early when required values are missing.
2. Use optional chaining for simple access.
3. Avoid nested optional handling.
4. Convert optionals to domain errors at boundaries.

```swift
guard let userID = request.userID else {
    throw RequestError.missingUserID
}
```

## Concurrency

1. Prefer structured concurrency with `async` and `await`.
2. Keep task lifetimes clear.
3. Use actors to protect shared mutable state.
4. Avoid blocking work on the main actor.
5. Keep UI updates on the main actor.
6. Handle cancellation deliberately.

## API Design

1. Keep public APIs small.
2. Make invalid states hard to represent.
3. Prefer immutable public properties.
4. Keep protocol requirements minimal.
5. Avoid exposing implementation details.
6. Document behavior that affects callers.

## Tooling

1. Use `swift-format` or the project formatter.
2. Run unit tests before committing.
3. Use compiler warnings as design feedback.
4. Keep package dependencies minimal.
5. Test on supported OS and Swift versions.

## Review Checklist

1. Is the API clear at the call site?
2. Are optionals handled safely?
3. Are force unwraps justified?
4. Are errors explicit and useful?
5. Is shared mutable state protected?
6. Are value and reference types chosen deliberately?
7. Are tests covering important behavior?

---

### Made with ❤️ by Nsisong Labs
