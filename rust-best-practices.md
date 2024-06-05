# Rust Best Practices

Rust code should use ownership, lifetimes, and types to prevent bugs before
runtime. Keep unsafe code rare, small, documented, and wrapped behind safe APIs.

## Core Principles

1. Prefer safety by default.
2. Make ownership and lifetime rules clear.
3. Use types to express invariants.
4. Prefer explicit errors over panics for recoverable failures.
5. Avoid shared mutable state unless it is protected.
6. Keep unsafe code isolated.
7. Choose clarity before clever type tricks.

## Ownership

1. Borrow when the function does not need ownership.
2. Take ownership when the function must store or consume a value.
3. Keep borrowing scopes small.
4. Avoid unnecessary clones.
5. Clone deliberately when it simplifies ownership without meaningful cost.
6. Prefer immutable bindings by default.

```rust
fn normalize_email(email: &str) -> String {
    email.trim().to_lowercase()
}
```

## Types

1. Use domain types for important values.
2. Use enums for known states.
3. Avoid representing state with strings when an enum is possible.
4. Use `Option<T>` for values that may be absent.
5. Use `Result<T, E>` for recoverable failures.
6. Make invalid states impossible where practical.

```rust
enum PaymentState {
    Pending,
    Paid { receipt_id: String },
    Failed { reason: String },
}
```

## Error Handling

1. Do not panic for expected input or runtime failures.
2. Use `?` to propagate errors cleanly.
3. Preserve context when crossing module boundaries.
4. Use project-standard error crates when appropriate.
5. Keep library errors useful to callers.
6. Keep application errors useful to operators.

```rust
let profile = serde_json::from_str::<UserProfile>(payload)
    .map_err(|error| ProfileError::InvalidPayload { source: error })?;
```

## Unsafe Code

1. Avoid unsafe code unless there is no safe alternative.
2. Keep unsafe blocks tiny.
3. Document every safety requirement.
4. Wrap unsafe internals behind a safe public API.
5. Test boundary conditions.
6. Use Miri, sanitizers, or fuzzing where practical.

## Concurrency

1. Prefer ownership transfer over shared mutation.
2. Use channels when message passing clarifies the design.
3. Use `Mutex`, `RwLock`, or atomics deliberately.
4. Avoid holding locks across `.await`.
5. Keep task lifetimes clear.
6. Document thread-safety assumptions.

## API Design

1. Keep public APIs conservative.
2. Accept borrowed values when possible.
3. Return owned values when ownership transfer is expected.
4. Use traits when they express real behavior.
5. Avoid over-generic APIs.
6. Keep feature flags understandable.

## Tooling

1. Run `cargo fmt`.
2. Run `cargo clippy`.
3. Run `cargo test`.
4. Use `cargo audit` or equivalent dependency checks when configured.
5. Keep dependencies minimal.

```bash
cargo fmt
cargo clippy --all-targets --all-features
cargo test
```

## Review Checklist

1. Are ownership rules clear?
2. Are clones justified?
3. Are errors structured?
4. Is unsafe code isolated and documented?
5. Is shared mutable state protected?
6. Are public APIs conservative?
7. Did formatting, linting, and tests pass?
