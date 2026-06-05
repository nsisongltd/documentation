# GPU Kernels And Attention Best Practices

GPU kernel and attention optimization should be IO-aware, benchmark-driven, and
validated for numerical correctness. Optimize memory movement as seriously as
math operations.

## Core Principles

1. Measure memory traffic, not only FLOPs.
2. Optimize for the actual target GPU.
3. Keep numerical behavior tested.
4. Benchmark realistic shapes.
5. Separate algorithmic wins from kernel implementation wins.
6. Make tiling, precision, and layout assumptions explicit.
7. Avoid complexity that cannot be maintained.

## Attention Algorithms

1. Track sequence length, head dimension, number of heads, and batch size.
2. Avoid materializing large attention matrices when possible.
3. Use tiling to reduce high-bandwidth memory traffic.
4. Keep softmax numerically stable.
5. Test causal, non-causal, padded, and variable-length cases.
6. Validate gradients for training kernels.

## Kernel Design

1. Choose tile sizes for the target architecture.
2. Coalesce memory accesses.
3. Use shared memory deliberately.
4. Avoid bank conflicts where possible.
5. Minimize synchronization.
6. Keep register pressure visible.
7. Measure occupancy without treating it as the only goal.

## Precision

1. Document input, accumulation, and output precision.
2. Test FP16, BF16, FP32, and lower-precision paths separately.
3. Compare outputs against a trusted reference implementation.
4. Define acceptable error tolerances.
5. Watch for overflow and underflow in softmax and reductions.
6. Keep deterministic modes available when needed.

## Benchmarking

1. Warm up kernels before timing.
2. Synchronize before reading timings.
3. Report hardware, driver, framework, and compiler versions.
4. Benchmark forward and backward passes separately.
5. Include small, medium, and large sequence lengths.
6. Report latency, throughput, memory use, and numerical error.

```python
torch.cuda.synchronize()
start = time.perf_counter()
output = attention(query, key, value)
torch.cuda.synchronize()
elapsed = time.perf_counter() - start
```

## Integration

1. Provide safe fallbacks for unsupported shapes or hardware.
2. Keep build flags documented.
3. Test across supported CUDA, ROCm, or accelerator versions.
4. Avoid silent precision changes.
5. Keep kernel launch overhead visible.
6. Make error messages useful when compilation or dispatch fails.

## Review Checklist

1. Is memory traffic reduced?
2. Are shapes representative?
3. Are numerical tolerances defined?
4. Are unsupported cases handled safely?
5. Is the benchmark reproducible?
6. Are target GPU assumptions documented?
7. Does the complexity justify the measured gain?
