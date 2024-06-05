# Assembly Best Practices

Assembly should be rare, small, and justified. Use it for low-level platform
work, boot code, interrupt handling, atomics, calling conventions, context
switching, or measured hot paths where higher-level code cannot express the
requirement correctly.

## Core Principles

1. Write assembly only when there is a clear reason.
2. Keep instruction sequences small.
3. Isolate architecture-specific code.
4. Document assumptions that the compiler cannot check.
5. Prefer maintainability over tiny unmeasured performance wins.

## File Organization

1. Put architecture-specific files under architecture-specific directories.
2. Keep assembly close to the platform code that depends on it.
3. Expose a small C or C++ interface when possible.
4. Keep public symbols minimal.
5. Use naming that makes the target architecture and purpose clear.

```c
/* arch_context_switch() is implemented in architecture-specific assembly. */
void arch_context_switch(struct task *from, struct task *to);
```

## Calling Conventions

1. Follow the platform ABI.
2. Preserve registers the ABI requires you to preserve.
3. Document input registers.
4. Document output registers.
5. Document clobbered registers.
6. Document stack alignment requirements.
7. Keep stack frame layout clear.

```asm
/* x0: source pointer
 * x1: destination pointer
 * x2: byte count
 * clobbers: x3, x4
 */
```

## Control Flow

1. Use readable labels.
2. Avoid unnecessary jumps.
3. Make fallthrough behavior obvious.
4. Keep error paths visible.
5. Avoid hidden side effects.
6. Keep loops simple enough to audit.

## Memory And Ordering

1. Document memory ordering assumptions.
2. Use barriers only when their necessity is understood.
3. Keep atomic sequences small and reviewed.
4. Avoid lock-free code unless there is a strong reason.
5. Make shared memory access rules visible to callers.

## Symbols And Sections

1. Be explicit about global symbols.
2. Keep local labels local.
3. Use the correct text, data, and special sections.
4. Specify alignment where required.
5. Mark non-executable data correctly.
6. Keep unwind and annotation requirements consistent with the platform.

## Comments

1. Explain why the instruction sequence exists.
2. Explain ABI, register, stack, and memory-order assumptions.
3. Do not comment every instruction with an obvious translation.
4. Keep comments updated when instructions change.

## Alternatives

1. Prefer compiler intrinsics when they generate correct code.
2. Prefer well-reviewed runtime primitives for atomics and barriers.
3. Prefer C for code that does not need exact instruction control.
4. Prefer generated assembly only when the generation process is checked into
   the project and reproducible.

## Testing

1. Test on the target architecture.
2. Test on the emulator used by the project when hardware is unavailable.
3. Add tests for boundary conditions.
4. Inspect generated binaries when symbol layout or section placement matters.
5. Run under sanitizers or instrumentation where the platform supports it.

## Review Checklist

1. Is assembly truly required?
2. Are ABI assumptions documented?
3. Are clobbers and preserved registers correct?
4. Is stack alignment correct?
5. Are memory ordering rules clear?
6. Are labels and jumps readable?
7. Has the code been tested on the target architecture?
