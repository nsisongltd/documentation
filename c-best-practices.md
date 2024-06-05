# C Best Practices

C should be direct, readable, and honest about memory, ownership, and failure.
These practices are inspired by systems programming standards commonly used in
kernel-level work: small functions, clear control flow, explicit errors, and no
cleverness that makes maintenance harder.

## Core Principles

1. Prefer simple code that can be reviewed quickly.
2. Make ownership, lifetime, and cleanup rules obvious.
3. Treat warnings as real problems.
4. Avoid undefined behavior.
5. Optimize from evidence, not instinct.
6. Keep abstractions thin and useful.
7. Write code for maintainers, not just for the compiler.

## Naming

1. Use descriptive lowercase names with underscores.
2. Name functions after the action they perform.
3. Name structs after the thing they represent.
4. Avoid abbreviations unless they are standard in the codebase.
5. Use consistent prefixes for related functions.

```c
int user_cache_insert(struct user_cache *cache, struct user *user);
int user_cache_remove(struct user_cache *cache, uint64_t user_id);
```

## Functions

1. Keep functions short enough to understand without scrolling too much.
2. Give each function one clear responsibility.
3. Avoid deeply nested control flow.
4. Return early for invalid input and failure cases.
5. Keep helper functions close to the code that uses them.
6. Prefer `static` for file-local functions.

```c
static int parse_header(const char *line, struct header *header)
{
    if (line == NULL || header == NULL) {
        return -1;
    }

    /* Parse header here. */
    return 0;
}
```

## Memory And Ownership

1. Every allocation must have a clear owner.
2. Every ownership transfer must be visible in the API contract.
3. Check every allocation.
4. Free memory in the reverse order of acquisition where practical.
5. Set pointers to `NULL` after freeing only when it prevents real reuse bugs.
6. Avoid hidden allocation inside functions whose names do not imply it.
7. Prefer stack allocation for small, bounded, short-lived objects.

```c
struct user *user_create(const char *name)
{
    struct user *user = calloc(1, sizeof(*user));
    if (user == NULL) {
        return NULL;
    }

    user->name = strdup(name);
    if (user->name == NULL) {
        free(user);
        return NULL;
    }

    return user;
}
```

## Error Handling

1. Check return values from allocations, system calls, file operations, and
   parsing functions.
2. Prefer explicit error returns for recoverable errors.
3. Use a consistent error convention across each module.
4. Do not swallow errors.
5. Keep cleanup paths simple.
6. Use `goto` for cleanup when it makes resource handling clearer.

```c
int write_report(const char *path, const char *body)
{
    FILE *file = fopen(path, "w");
    if (file == NULL) {
        return -1;
    }

    if (fputs(body, file) == EOF) {
        fclose(file);
        return -1;
    }

    return fclose(file);
}
```

## Headers And APIs

1. Keep headers minimal.
2. Expose only what other translation units need.
3. Use include guards or `#pragma once`, following the project standard.
4. Do not put private helpers in public headers.
5. Keep structs opaque when callers should not depend on their layout.
6. Include the headers you use directly.

```c
/* user_cache.h */
struct user_cache;

struct user_cache *user_cache_create(void);
void user_cache_destroy(struct user_cache *cache);
```

## Types

1. Use `size_t` for sizes.
2. Use fixed-width integer types when size matters.
3. Use signed types only when negative values are meaningful.
4. Avoid unnecessary casts.
5. Avoid mixing signed and unsigned arithmetic casually.
6. Use `bool` for boolean state.
7. Use `const` for data that should not be modified.

## Macros

1. Avoid macros for logic.
2. Prefer functions or `static inline` functions.
3. Parenthesize macro arguments and macro bodies.
4. Do not write macros with surprising side effects.
5. Keep macros reserved for constants, compile-time configuration, and small
   patterns that cannot be expressed cleanly otherwise.

```c
#define ARRAY_LEN(items) (sizeof(items) / sizeof((items)[0]))
```

## Concurrency

1. Document which lock protects which data.
2. Keep critical sections small.
3. Avoid calling unknown or blocking code while holding a lock.
4. Define ownership and lifetime rules for shared objects.
5. Use atomics only when the memory ordering is understood.
6. Prefer simple locking over clever lock-free code unless performance data
   justifies the complexity.

## Undefined Behavior To Avoid

1. Reading uninitialized memory.
2. Using freed memory.
3. Writing past array bounds.
4. Signed integer overflow.
5. Invalid pointer arithmetic.
6. Violating alignment requirements.
7. Breaking strict aliasing rules.
8. Shifting by a negative amount or by the width of the type.

## Tooling

1. Compile with strong warnings:

```bash
cc -Wall -Wextra -Werror
```

2. Run sanitizers in test builds when possible:

```bash
cc -fsanitize=address,undefined
```

3. Use static analysis for critical code.
4. Use the project formatter or `clang-format` when configured.
5. Keep build flags consistent between local and CI environments.

## Review Checklist

1. Are ownership and cleanup rules obvious?
2. Are all error paths handled?
3. Can the function be understood quickly?
4. Are there unnecessary macros or abstractions?
5. Are integer sizes and signedness correct?
6. Could any pointer use become invalid?
7. Are concurrency assumptions documented?
8. Was the code built with warnings enabled?

---

### Made with ❤️ by Nsisong Labs
