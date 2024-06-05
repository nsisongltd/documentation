# Java Best Practices

Java code should favor clear object models, stable APIs, portability, and
predictable runtime behavior. Use types to express contracts and keep classes
focused enough to maintain over many years.

## Core Principles

1. Optimize for long-term maintainability.
2. Keep public APIs small and stable.
3. Prefer clear domain types over loosely structured maps.
4. Use composition before inheritance.
5. Make invalid input fail early.
6. Avoid exposing mutable internals.
7. Measure performance before optimizing.

## Class Design

1. Give each class one responsibility.
2. Keep inheritance shallow.
3. Prefer interfaces for capabilities.
4. Prefer immutable value objects.
5. Make fields private.
6. Keep constructors simple and valid.
7. Use `final` where it clarifies intent.

```java
public final class EmailAddress {
    private final String value;

    public EmailAddress(String value) {
        if (value == null || value.isBlank()) {
            throw new IllegalArgumentException("Email address is required");
        }

        this.value = value;
    }

    public String value() {
        return value;
    }
}
```

## API Design

1. Use descriptive method names.
2. Avoid surprising side effects.
3. Return empty collections instead of `null`.
4. Use `Optional` for optional return values, not fields or parameters by
   default.
5. Document behavior callers depend on.
6. Avoid breaking public method signatures casually.
7. Version APIs deliberately.

## Error Handling

1. Throw exceptions for exceptional failures.
2. Use meaningful exception types.
3. Preserve the cause when wrapping exceptions.
4. Do not catch exceptions just to hide them.
5. Validate arguments at boundaries.
6. Keep recoverable and unrecoverable failures distinguishable.

```java
try {
    return objectMapper.readValue(payload, UserProfile.class);
} catch (JsonProcessingException error) {
    throw new InvalidUserPayloadException("Invalid user payload", error);
}
```

## Collections And Data

1. Program to interfaces such as `List`, `Map`, and `Set`.
2. Choose collection implementations deliberately.
3. Avoid mutating collections passed by callers unless the API says so.
4. Use immutable collections for public return values when appropriate.
5. Avoid raw types.
6. Use generics to express constraints.

## Concurrency

1. Prefer immutable shared data.
2. Use executor services instead of unmanaged threads.
3. Keep lock ownership clear.
4. Avoid holding locks while calling unknown code.
5. Use concurrent collections for shared mutable state when appropriate.
6. Document thread-safety guarantees.

## Portability

1. Be explicit about character encodings.
2. Avoid timezone and locale assumptions.
3. Keep platform-specific code behind small interfaces.
4. Test on supported Java versions.
5. Use standard library APIs before platform-specific calls.

## Tooling

1. Use the project formatter.
2. Run unit tests before committing.
3. Use static analysis where configured.
4. Keep dependency versions managed.
5. Check production builds, not only IDE builds.

## Review Checklist

1. Is the class responsibility clear?
2. Are public APIs stable and descriptive?
3. Are mutable internals protected?
4. Are exceptions meaningful?
5. Is concurrency behavior documented?
6. Is the code portable across supported runtimes?
7. Are tests included for important behavior?
