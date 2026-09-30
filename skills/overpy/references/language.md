# OverPy language notes

Every `opy` example here compiles with `overpy@9.7.10` when placed after a settings block (see the first example). Names below are OverPy names.

## A file

```opy
settings {
    "main": {"description": "Streak race"},
    "gamemodes": {"ffa": {"enabledMaps": ["workshopIsland"]}}
}

playervar score

rule "score on elimination":
    @Event playerEarnedElimination
    @Condition attacker != victim
    attacker.score += 1
```

- `settings {…}` comes first and must contain `"gamemodes"`. Map and mode names are camelCase (`workshopIsland`).
- Blocks are indented. `@` annotations sit directly under the `rule` line. Several `@Condition` lines are all required.
- Comments start with `#`.

## Rule annotations

```opy
rule "ana only":
    @Event eachPlayer
    @Hero ana
    @Team 1
    wait(1)
    eventPlayer.setUltCharge(100)
```

`@Event` defaults to `global`. `@Hero` and `@Slot` both restrict the event player, so they cannot be combined. `@Team` takes `1`, `2`, or `all`.

## Variables and subroutines

```opy
globalvar counter
playervar streak

def resetAll():
    @Name "resetAll"
    counter = 0
    eventPlayer.streak = 0

rule "call it":
    @Event eachPlayer
    resetAll()
```

Declare variables at the top level before any rule that uses them. A player variable is always read and written through a player (`eventPlayer.streak`, `attacker.score`); a bare `streak` is an error. `playervar y` is rejected as a reserved word, so avoid single-letter names. A subroutine is `def name():` with an `@Name` and is called like a function.

## Macros and includes

```opy
#!define LIMIT 5
#!define clamp(x, lo, hi) min(max(x, lo), hi)

globalvar v

rule "m":
    v = clamp(LIMIT * 3, 0, 10)
```

`#!define` is text substitution, so the substituted value appears in the compiled output. `#!include "lib.opy"` pulls in another file, resolved relative to the including file (use `--root` for another base), and must come after the variables the included code uses.

## Control flow

```opy
globalvar i
globalvar arr

rule "flow":
    arr = [1, 2, 3]
    for i in range(3):
        if arr[i] == 2:
            continue
        elif arr[i] > 2:
            break
        else:
            wait(0.25)
    while i < 10:
        i += 1
        wait(0.1)
```

`for` accepts only `range(...)`. To visit every player: `for i in range(len(players)): players[i].…`. There is no local variable: every loop variable is a declared variable, shared across the rule.

Repeating a rule while its condition holds:

```opy
globalvar x

rule "loop if":
    @Condition x < 3
    x += 1
    wait(1, Wait.IGNORE_CONDITION)
    if RULE_CONDITION:
        goto RULE_START
```

## Strings, arrays, vectors

```opy
playervar score
globalvar list
globalvar far

rule "data":
    @Event eachPlayer
    hudHeader(eventPlayer, "Score: {0}".format(eventPlayer.score), HudPosition.LEFT, 0, Color.WHITE, HudReeval.VISIBILITY_AND_STRING)
    list = [p for p in getAllPlayers() if p.isAlive()]
    list = sorted(list, lambda p: p.getHealth())
    list.append(3)
    far = distance(eventPlayer.getPosition(), vect(0, 0, 0))
    eventPlayer.teleport(Vector.UP * 3)
```

Strings use `"{0}".format(value)`. Array comprehensions, `sorted(array, lambda x: …)`, `len()`, and `.append()` work. A vector is `vect(x, y, z)`; `Vector.UP` and friends are constants. Booleans are `true` and `false`, and logic uses `and`, `or`, `not`.

## Workshop settings

```opy
#!define killsToWin createWorkshopSetting(int[1:200], "Match", "Kills to win", 50, 0)

rule "s":
    @Event eachPlayer
    @Condition eventPlayer.getScore() >= killsToWin
    declarePlayerVictory(eventPlayer)
```

`createWorkshopSetting(type, category, name, default, sortOrder)` defines a lobby setting; wrap it in a `#!define` and use the name like a value.

## Effects and other entities

```opy
playervar fx

rule "effect":
    @Event eachPlayer
    createEffect(getAllPlayers(), Effect.SPHERE, Color.RED, eventPlayer.getPosition(), 1, EffectReeval.VISIBILITY_POSITION_AND_RADIUS)
    eventPlayer.fx = getLastCreatedEntity()
    wait(2)
    destroyEffect(eventPlayer.fx)
```

Store the entity right after creating it (`getLastCreatedEntity()`) if you need to destroy it later.
