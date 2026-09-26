---
name: wright
description: Use when working on an Overwatch Workshop project with Wright available, to discover supported capabilities and use its semantic diagnostics, queries, validated edits, and compilation.
license: AGPL-3.0
---

# Work with Wright

Wright is the source of executable Workshop tooling for this workflow. This guide is optional; use the project's instructions and Wright's current capabilities as authority for the task at hand.

1. Discover the installed Wright version, available commands or service operations, and capabilities for the project and source language before assuming an operation is supported. Use Wright's help and structured capability response when available. Distinguish an unsupported operation from a failed operation.
2. Use supported Wright diagnostics and semantic queries to understand the project. Use lint, analysis, inspection, and compilation when Wright declares them available for the input. Prefer structured CLI output or session API results over parsing terminal prose. Text search can locate source, but cannot establish semantic identity or correctness.
3. Make changes through Wright's validated edit operations when the relevant operation is supported. After any change, re-run the applicable checks and queries, validate the edit, and compile when compilation is supported and needed for the task. Inspect the resulting diagnostics and source before claiming success.
4. Respect ownership: Workshop semantics belong to `workshop-rs`, OverPy semantics to `opy-rs`, and DEL/OSTW semantics to `deltin-rs`; Wright exposes their supported capabilities. Do not recreate missing language or Workshop semantics in prompts, scripts, or textual heuristics.
5. When a needed capability is unsupported, say exactly what is unavailable and which claim remains unverified. Do not turn unsupported results into an unverified fallback or claim a textual approximation is equivalent to Wright's semantic result.

Retrieve domain facts through Wright when it exposes them. Do not treat this guide as a Workshop API reference or a fixed inventory of Wright commands.
