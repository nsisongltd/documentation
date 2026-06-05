# Blockchain Protocol Best Practices

Blockchain protocol code should be conservative, auditable, and explicit about
trust, consensus, cryptography, and upgrade behavior. Treat every ambiguity as a
future outage or exploit.

## Core Principles

1. Define the trust model before designing the protocol.
2. Keep consensus-critical logic small and deterministic.
3. Prefer explicit state transitions over hidden side effects.
4. Make economic incentives and failure modes visible.
5. Treat cryptographic assumptions as API contracts.
6. Design upgrades carefully and test migration paths.
7. Assume hostile inputs, hostile networks, and partial failures.

## Consensus And State

1. Keep consensus rules versioned and documented.
2. Avoid nondeterministic behavior in consensus code.
3. Never depend on local time, random iteration order, floating point, or
   platform-specific behavior for state transitions.
4. Define block, transaction, and state validity precisely.
5. Keep serialization canonical.
6. Test replay behavior across client versions.
7. Use fixtures for known blocks, transactions, and state roots.

## Cryptography

1. Use reviewed cryptographic libraries.
2. Do not invent primitives.
3. Be explicit about hash functions, signature schemes, domains, and encodings.
4. Use domain separation for signatures and hashes.
5. Validate public keys, signatures, proofs, and serialized inputs.
6. Keep private keys out of application logs, telemetry, and crash reports.
7. Plan for key rotation and compromised-key response.

## Smart Contracts And Runtime Logic

1. Keep contract logic small.
2. Minimize privileged roles.
3. Use explicit access control.
4. Avoid unbounded loops over user-controlled state.
5. Treat external calls as hostile.
6. Prefer pull payments over push payments.
7. Test reentrancy, authorization, arithmetic, and upgrade behavior.

## Upgrades

1. Define who can upgrade and under what process.
2. Use timelocks or governance delay where appropriate.
3. Publish migration plans before activation.
4. Keep backward compatibility rules clear.
5. Test old-state-to-new-state migrations.
6. Provide rollback or pause strategies for severe failures.

## Observability

1. Emit events for meaningful state changes.
2. Keep node logs useful without leaking secrets.
3. Track consensus health, peer behavior, block production, and finality.
4. Alert on chain stalls, fork choice anomalies, and invalid block patterns.
5. Build tools for state inspection and replay.

## Testing And Verification

1. Unit test every state transition.
2. Use property-based tests for invariants.
3. Fuzz parsers, transaction validation, and networking boundaries.
4. Use differential testing between implementations where possible.
5. Run adversarial simulations.
6. Require external audits for high-value protocol changes.

## Review Checklist

1. Is the trust model documented?
2. Is consensus behavior deterministic?
3. Are serialization and hashing canonical?
4. Are cryptographic domains separated?
5. Are upgrade paths tested?
6. Are economic attacks considered?
7. Are invariants covered by tests or verification?
