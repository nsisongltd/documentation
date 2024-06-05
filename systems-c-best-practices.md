# Systems C Best Practices

Systems C should be compact, portable, explicit about bytes and ownership, and
careful with malformed input. This guide is useful for codecs, emulators,
compilers, runtimes, operating-system code, and low-level tools.

## Core Principles

1. Keep implementations compact.
2. Avoid unnecessary dependencies.
3. Be explicit about integer sizes and byte order.
4. Validate boundaries before reading or writing.
5. Treat malformed input as normal.
6. Keep platform-specific behavior isolated.
7. Use fuzzing and sanitizers for parser-heavy code.

## Portability

1. Use fixed-width integer types where layout matters.
2. Avoid compiler-specific behavior unless guarded.
3. Keep byte order conversion explicit.
4. Avoid assuming alignment.
5. Test on multiple architectures when portability matters.
6. Keep cross-compilation practical.

```c
#include <stdint.h>

uint32_t read_u32_le(const unsigned char *data)
{
    return ((uint32_t)data[0])
        | ((uint32_t)data[1] << 8)
        | ((uint32_t)data[2] << 16)
        | ((uint32_t)data[3] << 24);
}
```

## Parsers And Binary Formats

1. Check every length before reading.
2. Avoid integer overflow in offset calculations.
3. Keep parsing state explicit.
4. Return structured errors when possible.
5. Add test vectors for edge cases.
6. Fuzz untrusted input paths.

## Performance And Size

1. Measure hot paths.
2. Track binary size when footprint matters.
3. Keep allocation patterns predictable.
4. Prefer table-driven logic when it reduces complexity.
5. Avoid generality the project does not need.
6. Keep code generation reproducible if generated code is used.

## Review Checklist

1. Are integer sizes explicit?
2. Are byte-order assumptions visible?
3. Are malformed inputs handled safely?
4. Are platform-specific paths isolated?
5. Are dependencies justified?
6. Are parser paths fuzzable?
7. Did sanitizers or equivalent checks run?
