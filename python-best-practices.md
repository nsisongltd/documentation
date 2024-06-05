# Python Best Practices

Python should be easy to read from top to bottom. Favor plain functions, clear
module boundaries, explicit errors, and standard library tools before adding
framework or dependency complexity.

## Core Principles

1. Prefer readability over cleverness.
2. Keep business logic separate from framework glue.
3. Make error behavior explicit.
4. Use the standard library when it fits.
5. Add types where they clarify contracts.
6. Write tests for behavior that matters.

## Formatting And Naming

1. Follow PEP 8 unless the project has a stricter standard.
2. Use `snake_case` for functions and variables.
3. Use `PascalCase` for classes.
4. Use `UPPER_SNAKE_CASE` for constants.
5. Keep imports at the top of the file.
6. Remove unused imports.
7. Avoid wildcard imports.

```python
from pathlib import Path


DEFAULT_RETRY_COUNT = 3
```

## Functions

1. Keep functions focused on one responsibility.
2. Prefer explicit arguments over hidden global state.
3. Return predictable types.
4. Avoid boolean parameters that change a function into two different
   functions.
5. Use small helper functions when they clarify intent.

```python
def normalize_email(email: str) -> str:
    return email.strip().lower()
```

## Types

1. Add type hints to public functions.
2. Add type hints around complex data structures.
3. Use `dataclasses` for simple structured data.
4. Use `Protocol` or interfaces only when they simplify testing or extension.
5. Avoid `Any` unless the boundary is genuinely dynamic.

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class UserProfile:
    user_id: int
    email: str
```

## Error Handling

1. Catch specific exceptions.
2. Do not use bare `except`.
3. Do not silently ignore errors.
4. Raise errors close to the source of invalid state.
5. Include context in exception messages.
6. Convert low-level errors into domain-level errors at module boundaries.

```python
try:
    payload = json.loads(raw_payload)
except json.JSONDecodeError as error:
    raise ValueError("Invalid user payload") from error
```

## Resources

1. Use context managers for files, locks, sockets, and temporary resources.
2. Specify text encodings.
3. Prefer `pathlib.Path` for filesystem paths.
4. Clean up temporary files predictably.

```python
from pathlib import Path


def read_config(path: Path) -> str:
    with path.open(encoding="utf-8") as handle:
        return handle.read()
```

## Data Structures

1. Use lists for ordered collections.
2. Use sets for membership checks.
3. Use dictionaries for lookups by key.
4. Prefer `tuple` or frozen dataclasses for immutable grouped values.
5. Avoid deeply nested dictionaries when a named structure would be clearer.

## Defaults And Mutability

1. Never use mutable default arguments.
2. Copy mutable inputs when the function should not mutate caller-owned data.
3. Avoid hidden mutation of global state.

```python
def add_item(item: str, items: list[str] | None = None) -> list[str]:
    if items is None:
        items = []

    items.append(item)
    return items
```

## Dependencies

1. Add dependencies only when they reduce real complexity.
2. Pin dependency versions in application projects.
3. Keep dependency boundaries isolated.
4. Prefer mature libraries for security-sensitive parsing, cryptography, HTTP,
   and serialization.

## Testing

1. Test public behavior, not private implementation details.
2. Use fixtures for repeated setup.
3. Keep unit tests fast.
4. Add integration tests around databases, HTTP, queues, and file behavior.
5. Test error paths.

```bash
pytest
```

## Tooling

1. Use the project formatter and linter.
2. Prefer `ruff` for fast linting when available.
3. Use `black` when the project standard allows it.
4. Use `mypy` or `pyright` when static type checks are part of the project.

```bash
ruff check .
pytest
```

## Review Checklist

1. Is the code readable without clever tricks?
2. Are errors handled explicitly?
3. Are public contracts typed?
4. Are resources closed predictably?
5. Are mutable defaults avoided?
6. Are dependencies justified?
7. Are tests included for important behavior?

---

### Made with ❤️ by Nsisong Labs
