# The `overpy` compiler

Verified with `overpy@9.7.10`.

## Commands

```sh
overpy compile -i mode.opy -o mode.txt     # OverPy -> Workshop script
cat mode.opy | overpy compile > mode.txt   # stdin and stdout also work
overpy decompile -i workshop.txt -o mode.opy   # Workshop script -> OverPy
```

| Option | Use |
| --- | --- |
| `-i`, `-o` | input and output files |
| `-l <lang>` | Workshop language of the output (default `en-US`) |
| `--root <path>` | base directory for `#!include` |
| `--main-file <name>` | main file name when compiling a project |

Exit code 0 is success. A failure exits 1 and prints `Error: <message>` with `| line N, col M, at <file>`; only the first error is reported, so recompile after each fix. A warning such as `The color 'RED' is hard to see … (w_dark_color)` still exits 0. The compiler checks OverPy validity, not Workshop behavior. `decompile` is useful to see how an editor-exported snippet is written in OverPy, including the real names of actions and values.

## Error messages and fixes

| Message | Cause and fix |
| --- | --- |
| `Unknown function '.isButtonHeld'` | The name is wrong. Find the OverPy name (`isHoldingButton`); do not guess. A leading `.` means it was called as a member. |
| `Unknown function 'setUltCharge'` | A player method was called as a function. Use `eventPlayer.setUltCharge(100)`. |
| `Unknown function name 'streak'` | A player variable was used without a player. Write `eventPlayer.streak`. |
| `Unknown member 'points' of 'eventPlayer'` | The player variable is not declared. Add `playervar points`. |
| `Cannot use 'eventPlayer' with rule event 'global'` | The rule has no event player. Add `@Event eachPlayer` or use another player event. |
| `Expected the 'range' function for the 2nd operand of the 'in' operator` | `for` only iterates `range(...)`. Loop over `range(len(array))` and index the array. |
| `Expected an action, but got function '.hasStatus' which is a value` | A value was used as a statement. Use it inside a condition or assignment. |
| `Unexpected function '@Event' outside a rule` | An annotation is not indented under its `rule` line. |
| `Rule event player (@Hero/@Slot) was already declared` | `@Hero` and `@Slot` were both given. Use one. |
| `Variable name 'y' is a reserved word` | The variable name collides with a built-in. Rename it. |
| `Custom game settings must specify a gamemode` | The `settings` block has no `"gamemodes"` entry with a mode. |
| `No match found for keyword 'passiveHealthRegen'` | The settings key is not valid in that section. Check the nesting of the settings block. |
| `Unknown preprocessor directive '#!mainFile'` | That directive is not supported in this form. Use the `--main-file` option instead. |
| `ENOENT: no such file or directory` | An `#!include` path does not exist relative to the including file. |
