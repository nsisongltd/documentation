# Go Best Practices

Go code should be simple, readable, composable, and easy to maintain. Favor
small interfaces, explicit errors, clear package boundaries, and boring code
that works well in production.

## Core Principles

1. Keep code simple enough to understand quickly.
2. Prefer composition over inheritance-style designs.
3. Make dependencies explicit.
4. Handle errors where they occur.
5. Keep packages focused.
6. Avoid clever abstractions.
7. Optimize after measuring.

## Packages

1. Name packages after what they provide, not where they are used.
2. Keep package APIs small.
3. Avoid package names like `common`, `utils`, or `helpers` when a domain name
   would be clearer.
4. Keep internal details unexported.
5. Use `internal/` for code that should not be imported by other modules.
6. Avoid circular dependencies by moving shared contracts into smaller packages.

## Naming

1. Use short names for small local scopes.
2. Use descriptive names for exported identifiers.
3. Do not repeat the package name in exported symbols.
4. Use initialisms consistently, such as `HTTP`, `ID`, and `URL`.
5. Name interfaces after behavior when they contain one or two methods.

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

## Error Handling

1. Return errors explicitly.
2. Do not ignore errors.
3. Add context when returning errors across boundaries.
4. Use `errors.Is` and `errors.As` for error inspection.
5. Keep panic for programmer errors or unrecoverable startup failures.
6. Do not log and return the same error unless there is a clear reason.

```go
user, err := store.FindUser(ctx, userID)
if err != nil {
    return nil, fmt.Errorf("find user %q: %w", userID, err)
}
```

## Interfaces

1. Accept interfaces, return concrete types.
2. Keep interfaces small.
3. Define interfaces close to the consumer.
4. Avoid interface pollution before there is more than one implementation.
5. Use compile-time checks for important interface implementations.

```go
var _ http.Handler = (*Server)(nil)
```

## Concurrency

1. Use goroutines deliberately.
2. Always define goroutine lifetime.
3. Pass `context.Context` through request-scoped work.
4. Avoid leaking goroutines.
5. Prefer channels for ownership transfer and synchronization when they clarify
   the design.
6. Prefer mutexes for protecting shared state when a mutex is simpler.
7. Run tests with the race detector.

```bash
go test -race ./...
```

## Testing

1. Prefer table-driven tests for repeated cases.
2. Test behavior through public APIs.
3. Keep tests deterministic.
4. Use subtests for named scenarios.
5. Avoid over-mocking. Use small interfaces when tests need substitution.

```go
func TestNormalizeEmail(t *testing.T) {
    tests := []struct {
        name string
        in   string
        want string
    }{
        {name: "trim and lowercase", in: " User@Example.COM ", want: "user@example.com"},
    }

    for _, tt := range tests {
        t.Run(tt.name, func(t *testing.T) {
            got := NormalizeEmail(tt.in)
            if got != tt.want {
                t.Fatalf("got %q, want %q", got, tt.want)
            }
        })
    }
}
```

## Tooling

1. Run `gofmt` on every file.
2. Use `go test ./...` before committing.
3. Use `go vet ./...` for static checks.
4. Keep module dependencies minimal.
5. Use `golangci-lint` when the project standard includes it.

## Review Checklist

1. Is the package boundary clear?
2. Are errors handled explicitly?
3. Are interfaces small and consumer-owned?
4. Are goroutine lifetimes controlled?
5. Are dependencies justified?
6. Are tests table-driven where useful?
7. Did `gofmt`, `go vet`, and tests pass?

---

### Made with ❤️ by Nsisong Labs
