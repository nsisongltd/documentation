# LLM Efficiency Best Practices

LLM efficiency work should reduce memory, compute, latency, or cost without
hiding quality regressions. Measure carefully and keep model behavior visible.

## Core Principles

1. Define the target constraint: memory, latency, throughput, quality, or cost.
2. Measure baseline behavior before optimizing.
3. Track quality and efficiency together.
4. Prefer simple optimizations before complex ones.
5. Keep hardware behavior in mind.
6. Make quantization and training assumptions explicit.
7. Benchmark with realistic sequence lengths and batch sizes.

## Quantization

1. Choose quantization based on deployment hardware and quality tolerance.
2. Evaluate perplexity, task quality, and user-facing behavior after
   quantization.
3. Keep calibration data representative.
4. Treat outlier channels and activation ranges carefully.
5. Document weight, activation, and KV-cache precision.
6. Test long-context behavior after quantization.

## Memory

1. Track model weights, activations, optimizer state, gradients, and KV cache
   separately.
2. Use gradient checkpointing when memory is the bottleneck.
3. Use efficient optimizers when optimizer state dominates memory.
4. Keep batch size and sequence length tradeoffs explicit.
5. Monitor fragmentation and allocator behavior.
6. Avoid hidden copies in data loading and inference paths.

## Training And Fine-Tuning

1. Start with a strong baseline.
2. Keep data quality higher priority than training tricks.
3. Track tokens, effective batch size, learning rate, and schedule.
4. Evaluate during training, not only at the end.
5. Save enough metadata to reproduce runs.
6. Avoid overfitting small instruction datasets.

## Inference

1. Benchmark prefill and decode separately.
2. Track time to first token and tokens per second.
3. Measure latency under realistic concurrency.
4. Tune batch size and KV-cache policy for the service goal.
5. Use speculative decoding only when quality and system complexity justify it.
6. Keep fallback behavior for out-of-memory conditions.

## Evaluation

1. Use task-specific evaluations.
2. Include regression tests for important prompts.
3. Compare quality before and after efficiency changes.
4. Track hallucination-sensitive and reasoning-sensitive tasks separately.
5. Evaluate long context if the deployment promises it.
6. Keep benchmark scripts versioned.

## Review Checklist

1. What constraint is being optimized?
2. What is the baseline?
3. Did quality regress?
4. Is memory measured by category?
5. Are benchmarks realistic for deployment?
6. Are quantization assumptions documented?
7. Can the run be reproduced?
