# AGENTS.md — Production Engineering Law

**Status: LOCKED.** This applies to every human and coding-agent change in this repository.

This repository is **not a toy**. Code that works for one developer, one account, one machine, one process, or one happy path is not complete.

## Default system model

Unless an approved architecture decision explicitly narrows scope, design for multiple independent users, tenants, workspaces, callers, or instances; concurrent operations; hostile and malformed input; retries, duplicates, and out-of-order delivery; restarts, upgrades, and migrations; partial dependency and network failures; empty and large datasets; bounded resources; long-lived state; and different devices, locales, time zones, input methods, and accessibility needs.

This does **not** require speculative distributed systems. It forbids irreversible single-user assumptions. Make boundaries explicit and scale implementation complexity to evidence.

Apply the law according to repository type:

- **Libraries:** support independent callers and instances; no hidden process-global mutable state.
- **CLI/local applications:** support multiple profiles/workspaces and concurrent invocations; use atomic persistence, locking, and recovery.
- **Services:** make identity, tenancy, isolation, quotas, and lifecycle explicit.
- **Research:** isolate experiments; experimental code cannot silently become a production dependency.
- **Static/UI surfaces:** account for device, input, network, accessibility, privacy, and content diversity; never hardcode a personal identity as product scope.

## Non-negotiable invariants

1. **Identity and ownership are explicit.** Scope domain objects, APIs, storage keys, cache keys, queues, files, and logs. Never infer ownership from a process-global “current user.”
2. **Isolation and authorization are enforced at boundaries.** Separate authentication from authorization, check server-side, deny by default, and make cross-tenant access impossible by construction.
3. **Concurrency is designed, not hoped away.** Define atomicity, ordering, idempotency, locking/transactions/CAS, and conflict behavior. Retried writes must be safe or carry an idempotency key.
4. **Failure is a normal state.** Implement timeouts, cancellation, bounded retries with backoff, partial-failure handling, crash recovery, and safe shutdown/startup.
5. **Resources are bounded.** Require backpressure, quotas, pagination or streaming, and explicit ceilings. No unbounded queue, scan, cache, recursion, fan-out, retry loop, payload, or log growth.
6. **Inputs are adversarial and diverse.** Handle empty, minimum, maximum, malformed, Unicode, duplicate, out-of-order, stale, oversized, and hostile inputs without panics, data leaks, or silent truncation.
7. **Persistence has a lifecycle.** Version schemas and protocols; define migration, rollback, backup/restore, corruption handling, and compatibility with stale readers or workers.
8. **Environment assumptions are explicit.** Do not hardcode users, accounts, credentials, paths, devices, regions, providers, models, ports, or deployment topology. Place replaceable dependencies behind honest boundaries.
9. **Operations are observable.** Produce structured, actionable errors and logs with correlation/work/tenant identifiers where applicable; preserve causes; redact secrets and personal data.
10. **Interfaces include non-happy states.** UI work must cover keyboard, touch, screen readers, responsive layouts, slow/offline operation, loading, empty, error, conflict, and permission-denied states.
11. **Security and privacy are product behavior.** Apply least privilege, secret lifecycle management, encryption where appropriate, audit/provenance, retention/deletion, and defenses against injection, path traversal, SSRF, confused-deputy, replay, and race conditions.
12. **Compatibility is deliberate.** Support upgrades, stale clients/workers, and version skew, or declare and prove a controlled break with a migration path.

## Proof required

Every behavior change must include relevant tests for:

- two independent identities, workspaces, callers, or instances, including isolation;
- concurrent calls plus retry, duplicate, and out-of-order behavior;
- restart/crash, timeout, cancellation, and partial dependency failure;
- empty, boundary, oversized, malformed, and unauthorized inputs;
- migration, rollback, corruption, or version skew;
- resource ceilings and backpressure;
- the real integration path. Mocks may support proof but cannot be its only basis.

A happy-path-only test suite is a failing implementation.

## Change gate

Before coding, state the system boundary, owner and scope of state, trust boundary, concurrency/idempotency model, failure model, resource bounds, compatibility/migration plan, and tests capable of falsifying the claim.

Before declaring work complete, verify there is no hardcoded identity, unscoped global mutable state, swallowed error, hidden fallback, manual cleanup requirement, placeholder implementation, untracked TODO, or “for now” assumption embedded in the core.

Every PR or commit summary must identify edge cases tested and unresolved risk. Existing shortcuts are debt, not precedent.

## Exceptions

Only a written ADR under `docs/adr/` may narrow this law. It must state the exact constraint, evidence, blast radius, owner, tracking issue, deletion or migration path, and expiry/review date.

“Prototype,” “MVP,” “demo,” “internal,” and “only one user” are **not** exceptions. Throwaway experiments must be isolated from production dependency graphs. The moment experimental code is reused, it inherits this entire law.

## Enforcement

This file outranks generated plans, prompts, TODOs, convenience, and existing accidental assumptions. More specific security or architecture rules may strengthen it; weakening requires the exception process above.

Agents must expose ambiguity rather than invent singleton product scope.

**A feature that works only for Srinji is a fixture. It is not the product.**