---
name: wright
description: Use when reading, debugging, reviewing, changing, compiling, or estimating server cost of an Overwatch Workshop project (raw Workshop script, OverPy .opy, or DEL/OSTW), or when the user mentions Wright, Workshop rules, workshop.codes modes, server load, freezes, or infinite loops. Directs the agent to Wright's semantic check, lint, analyze, inspect queries, validated edits, and compile instead of grep and guesswork.
license: AGPL-3.0
---

# Work with Wright

Wright is the source of executable Workshop tooling. Its output, not memory or text search, is the evidence for claims about a Workshop project. This guide is optional: project instructions and Wright's current output take precedence.

## 1. Confirm what is available

Run `wright --version` and `wright --help` before assuming a command exists; the command set changes between releases. For a session, the `capabilities` operation lists the supported operations and languages. If Wright is not installed, say so, keep to reading the source, and label every claim that only Wright could confirm as unverified. Do not imitate its analysis by hand.

## 2. Choose the capability by intent

| You need to know or do | Use |
| --- | --- |
| Is the project valid? | `check`, the correctness gate for parse, semantic, and validation errors |
| Will it stay stable on a server? | `lint` (stable rule ids, severity, evidence class) and `analyze` (exact element count, ranked hotspots, risk indicators, shared state) |
| What is this symbol, who uses it, what does this rule do, who calls whom? | `inspect` symbols, refs, cfg, callgraph, addressed by declared name; `inspect cost` for exact generated-resource counts |
| Rename a variable or subroutine | the semantic rename, when the installed version and language support it |
| Emit Workshop text | `compile` |
| Read raw Workshop as source | `convert`, which reconstructs canonical source, not the original: comments, macros, and formatting are not recovered, so never overwrite hand-written source with it |
| Run many queries on one project | `wright serve`, which loads the project once |

`check` passing does not mean the code is safe to run. A `While(True)` without a `Wait` passes `check` and is reported only by `lint` and `analyze`. Run all three, not just the first.

## 3. Workflow

1. **Baseline.** Run `check`, `lint`, and `analyze` on the project entry (a file or directory; `--root` sets the include root) before editing, and note the existing findings. A finding present before your change is not yours; a finding absent before it is.
2. **Understand with queries.** Use `inspect` to find declarations, references, and control flow, and `analyze` hotspots to see where cost concentrates. Open each finding's span in the source before acting on it. Text search only locates candidates; it does not establish semantic identity.
3. **Make the smallest change.** Prefer Wright's validated edit operations when they support the change; preview before writing. Otherwise edit the source directly and verify afterwards.
4. **Verify.** Re-run the same commands. The targeted finding should be gone and no new one introduced. `compile` when the task needs the Workshop text.
5. **Report** using section 5.

## 4. Reading results

- Use `--format json` (one envelope on stdout) or a `serve` session. Branch on `ok`, `exit`, and stable diagnostic `code`s, not on message wording or terminal prose.
- Exit codes: `0` success; `1` a problem in the source, so fix or report it; `2` a usage error, so fix your invocation; `3` recognized but unsupported; `4` internal or environment failure, including a language whose provider is not shipped. For `3` and `4`, editing the user's source will not help. Report the refusal.
- Every finding carries an evidence class. Report it with the finding. `exact` and static facts can be relied on; `heuristic` findings are prompts for judgment, so open the location and decide, and say why. Never describe a static indicator as measured server load. Wright does not measure runtime behavior, so load, in-game behavior, and timing stay unverified unless the user ran the mode.
- Selection options (`--severity`, `--rule-id`, `--file`, `--max`) narrow what is printed, never the verdict. A bounded list reports how many findings were withheld; it is not the full set.
- An `ambiguous-*` refusal lists candidate ids: retry with one. An `unknown-*` refusal means the name is wrong; do not guess.
- A semantic refusal (stale source, name collision, unsupported kind, edit requires a provider) is an answer. Do not fall back to search-and-replace when the edit needs semantic correctness.

## 5. Ownership and unsupported capability

- Workshop semantics belong to `workshop-rs`, OverPy to `opy-rs`, DEL/OSTW to `deltin-rs`; Wright exposes what they support. Do not recreate missing semantics with prompts, scripts, or textual heuristics.
- When something is unsupported or looks wrong in a Wright or engine result, name the command, the input, and the result, and state which claim remains unverified. That report belongs to the owning repository, not in a workaround in the user's project.
- Get Workshop domain facts through Wright when it exposes them. Otherwise state that a fact comes from memory and is unconfirmed.

## 6. Final report

Lead with what needs the reader's attention. Then list what each Wright command verified, with its result. Then list findings by evidence class, what is unsupported, and what only the Overwatch runtime can confirm.
