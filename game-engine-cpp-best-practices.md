# Game Engine C++ Best Practices

Game engine C++ should make performance, memory, and frame behavior visible.
Use direct code, predictable data flow, and measurement before optimization.

## Core Principles

1. Measure before optimizing.
2. Keep frame cost visible.
3. Avoid hidden allocation in hot paths.
4. Prefer simple architecture that can be debugged quickly.
5. Keep platform-specific code isolated.
6. Make simulation deterministic where possible.
7. Keep the engine runnable at all times.

## Performance

1. Profile CPU time, GPU time, memory, and frame pacing separately.
2. Optimize the highest-impact bottleneck first.
3. Keep per-frame allocations out of hot systems.
4. Use cache-friendly data layouts for performance-critical code.
5. Prefer batch work over repeated tiny calls.
6. Keep performance budgets documented.

```text
60 FPS frame budget: 16.67 ms
120 FPS frame budget: 8.33 ms
```

## Architecture

1. Keep the main loop clear.
2. Separate simulation, rendering, input, audio, and asset loading.
3. Avoid hidden work during frame execution.
4. Make asset pipelines reproducible.
5. Keep debug views and counters close to engine systems.
6. Make platform code replaceable behind narrow interfaces.

## Memory

1. Make ownership explicit.
2. Prefer stable allocation patterns.
3. Use arenas, pools, or frame allocators when they simplify lifetime.
4. Avoid reference-counting in hot paths unless the cost is acceptable.
5. Keep large assets out of general-purpose memory churn.
6. Validate memory usage on target hardware.

## Debugging

1. Reproduce the bug before fixing it.
2. Add instrumentation when behavior is unclear.
3. Preserve crash context.
4. Use assertions for invariants in development builds.
5. Keep deterministic repro cases for simulation bugs.
6. Test on realistic hardware.

## Review Checklist

1. Was the performance claim measured?
2. Is the hot path free of avoidable allocation?
3. Is frame-time impact known?
4. Can the system be debugged with existing tools?
5. Are memory lifetimes clear?
6. Are platform assumptions isolated?
7. Was the change tested under realistic workload?
