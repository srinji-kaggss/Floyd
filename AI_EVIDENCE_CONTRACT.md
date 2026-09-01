# AI Evidence Contract

AI/model output is **untrusted first-party evidence** until independently verified.

## Required claim states
Every material capability claim must distinguish:
1. **Planned** — intended only.
2. **Present in code** — identifiable implementation at an exact commit.
3. **Exercised** — exact claimed path actually run on that commit.
4. **Independently evidenced** — evidence not authored solely by the same model/agent making the claim.
5. **Safe to rely on / merge** — relevant failure modes, scope, compatibility, and negative tests reviewed.

Never collapse these states into “done.”

## Evidence rules
- Self-authored tests, generated fixtures, PR descriptions, docs, and model summaries are first-party evidence only.
- Green CI proves only the checks that actually ran; it does not prove unstated semantics.
- A content hash does not prove a persistent Merkle DAG.
- A single-host test does not prove distributed orchestration.
- A standards-inspired test does not prove certification or standards compliance.
- Architectural intent, type signatures, mocks, and compile success do not prove runtime behavior.
- Capability statements must name the exact exercised path, commit, environment, and observed result.

## Falsification first
Before declaring success, search for the cheapest counterexample that would make the claim false. Prefer adversarial, external, differential, integration, and end-to-end evidence over self-confirming tests.

## Scope discipline
Set a hard scope budget before editing. If a supposedly small fix expands into broad architecture churn, stop and re-evaluate. Large unrelated rewrites are scope failure, not diligence.

## Merge gate
AI-authored work is not merge-safe merely because it is mergeable, reviewed by the same agent, or green in CI. Merge safety requires evidence proportionate to the claim and risk.

## Reporting rule
When evidence is missing, say **unknown**, **not exercised**, or **not independently verified**. Never substitute a plausible story.

This contract outranks convenience, velocity, and model confidence.