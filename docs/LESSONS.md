# Lessons

Short review queue of reusable failure classes from merged skills work. Each entry names a prevention action and the evidence behind it. Promote an entry into a regression test, an owner contract, or an existing engineering standard, then remove it here. The procedure is the `wrightkit-retro` skill in `wrightkit/.agents`.

### A durable claim must be checked against its authority, not written from assumption

- **Rule:** Capability claims in agent-facing guidance are verified by running the released tool — never inferred from a feature list. Where the tool refuses, the guide says so and gives the working path.
- **Why:** The Wright guide documented `inspect` as applying to OPY input; released Wright refuses it (`source-provider-unsupported`) and the support matrix documents the refusal — an agent following the guide would hit the refusal with no plan.
- **Prevention home:** verify-against-released-behavior at authoring time (the released-worktree verification pattern used in #14's review); workspace documentation-synchronization invariant. Cross-repo class escalation: wrightkit/.github#99; same class decided in wrightkit/opy-rs#556.
- **Incidents:** [#11](https://github.com/wrightkit/skills/pull/11) — observed 2026-09-30
