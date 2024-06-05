# C++ Best Practices

C++ should be used with restraint. Use language features when they make
ownership, lifetime, type safety, and intent clearer. Avoid abstraction-heavy
designs that hide simple behavior.

## Core Principles

1. Prefer simple code over clever code.
2. Use RAII for resource management.
3. Prefer values and standard containers.
4. Keep ownership explicit.
5. Avoid surprising implicit behavior.
6. Keep templates and inheritance conservative.
7. Let tooling catch mistakes early.

## Naming

1. Follow the naming convention already used by the project.
2. Use descriptive names for classes, functions, and variables.
3. Avoid abbreviations unless they are standard in the domain.
4. Name functions after the action they perform.
5. Name types after the concept they represent.

## Ownership

1. Prefer values for ordinary objects.
2. Prefer references for non-owning required parameters.
3. Prefer pointers for optional non-owning parameters.
4. Use `std::unique_ptr` for exclusive ownership.
5. Use `std::shared_ptr` only when shared ownership is real.
6. Avoid owning raw pointers.
7. Avoid manual `new` and `delete` in application code.

```cpp
class FileHandle {
public:
    explicit FileHandle(FILE* file) : file_(file) {}

    ~FileHandle()
    {
        if (file_ != nullptr) {
            fclose(file_);
        }
    }

private:
    FILE* file_;
};
```

## Classes

1. Keep classes focused on one responsibility.
2. Prefer composition over inheritance.
3. Keep inheritance shallow.
4. Make constructors establish valid objects.
5. Avoid heavy work in constructors when it can fail in complex ways.
6. Mark methods `const` when they do not mutate object state.
7. Use `explicit` for single-argument constructors.

```cpp
class UserId {
public:
    explicit UserId(int value) : value_(value) {}

    int value() const
    {
        return value_;
    }

private:
    int value_;
};
```

## Types

1. Prefer `enum class` over unscoped enums.
2. Prefer `nullptr` over `NULL` or `0`.
3. Use `std::optional` for optional values.
4. Use `std::variant` when a value can be one of several known types.
5. Use `std::string_view` carefully for non-owning string views.
6. Avoid implicit conversions that make behavior surprising.

## Error Handling

1. Follow the project standard for exceptions.
2. If exceptions are used, define clear module boundaries and guarantees.
3. If exceptions are not used, return explicit status or result types.
4. Do not mix error-handling styles casually in the same module.
5. Preserve context when translating errors.

## Templates

1. Use templates when they improve type safety or remove real duplication.
2. Keep template interfaces small.
3. Prefer concepts or clear static assertions for constraints.
4. Avoid template metaprogramming that hides simple logic.
5. Keep compile-time complexity in mind.

## Performance

1. Measure before optimizing.
2. Avoid unnecessary copies of large objects.
3. Use move semantics deliberately.
4. Pass small cheap values by value.
5. Pass large read-only values by `const&`.
6. Prefer standard algorithms before hand-written loops when they improve
   clarity.

## Concurrency

1. Prefer simple synchronization primitives.
2. Keep lock ownership clear.
3. Avoid holding locks while calling unknown code.
4. Document thread-safety guarantees on shared classes.
5. Use atomics only with understood memory ordering.

## Tooling

1. Compile with strong warnings.
2. Use sanitizers in test builds.
3. Use static analysis for critical code.
4. Use the project formatter.
5. Run tests under the same standard version used in production.

```bash
c++ -Wall -Wextra -Werror
c++ -fsanitize=address,undefined
```

## Review Checklist

1. Is ownership clear?
2. Could RAII simplify cleanup?
3. Are raw owning pointers avoided?
4. Are constructors and conversions unsurprising?
5. Is inheritance necessary?
6. Are templates justified?
7. Does error handling follow one clear style?
8. Was the code built with warnings and tests?

---

### Made with ❤️ by Nsisong Labs
