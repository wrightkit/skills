---
name: overpy
description: Use when writing, editing, fixing, or compiling OverPy (.opy) source for an Overwatch Workshop mode with the upstream `overpy` compiler, including rule and variable syntax, events and event variables, macros and includes, settings blocks, and reading compiler errors. Makes the agent compile after every change and look names up instead of guessing them.
license: AGPL-3.0
---

# Write OverPy with the upstream compiler

OverPy is a Python-like language that compiles to Workshop script. The upstream compiler decides whether code is valid, so run it after every change and read its errors.

## Workflow

1. Write or edit the `.opy` file.
2. Compile: `overpy compile -i mode.opy -o mode.txt` (install with `npm install -g overpy`). Exit 0 means it compiled. An error prints `Error: <message>` and `| line N, col M, at <file>` and exits 1. Fix the first error first; later ones can be consequences.
3. Read warnings too. A warning is not an error but often points at a real problem.
4. Do not guess a name. OverPy names are camelCase and often differ from the Workshop editor's (`Is Button Held` is `isHoldingButton`, `Create HUD Text` is `hudText`). When a name is rejected, find the real one: search the installed package (`grep -o "isHoldingButton" "$(npm root -g)/overpy/overpy.js"`), or decompile a Workshop snippet that uses it (`overpy decompile`), or use a Workshop reference if one is installed.

## Rules that cause most errors

- A rule is `rule "name":` followed by annotations (`@Event`, `@Condition`, `@Hero`, `@Team`) and then an indented body.
- The default event is `global`, which has no `eventPlayer`. A rule that uses `eventPlayer` needs `@Event eachPlayer` or another player event.
- Declare every variable before use: `globalvar name`, `playervar name`. A loop variable must be declared too. Player variables are accessed through a player (`eventPlayer.points`, never a bare `points`), and reading one that was not declared with `playervar` is an error.
- `for` only iterates `range(...)`: use `for i in range(len(arr)):` and index the array.
- Actions and values are different: a value such as `eventPlayer.hasStatus(...)` cannot stand alone as a statement.
- Player methods use member syntax: `eventPlayer.setUltCharge(100)`, not `setUltCharge(eventPlayer, 100)`.
- A `while` loop needs a `wait(...)` or it will freeze the server; `wait(x, Wait.IGNORE_CONDITION)` keeps running if the rule's condition turns false.
- The `settings {…}` block must name at least one game mode under `"gamemodes"`.
- Put `#!include` after the declarations it depends on.

## Where to look

- [references/language.md](references/language.md): syntax with examples that compile: rules, variables, subroutines, macros, control flow, strings, arrays, settings.
- [references/compiler.md](references/compiler.md): CLI options, and common error messages with their fixes.
- [references/events.md](references/events.md): events and which event variables each one provides.

This guide covers OverPy and its compiler only. It does not replace checking Workshop behavior: the compiler proves the code is valid OverPy, not that it does what the mode should do.
