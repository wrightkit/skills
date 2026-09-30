# Events

Write the event with `@Event <name>`. `global` is the default. The compiler accepts these names (verified with `overpy@9.7.10`):

`global`, `eachPlayer`, `subroutine`, `playerJoined`, `playerLeft`, `playerDied`, `playerEarnedElimination`, `playerDealtFinalBlow`, `playerDealtDamage`, `playerTookDamage`, `playerDealtHealing`, `playerReceivedHealing`, `playerDealtKnockback`, `playerReceivedKnockback`.

## Which players an event provides

The compiler rejects a player variable an event does not provide:

| Event | `eventPlayer` | `attacker`, `victim` | `healer`, `healee` |
| --- | --- | --- | --- |
| `global` | no | no | no |
| `eachPlayer`, `playerJoined`, `playerLeft` | yes | no | no |
| `playerDied`, `playerEarnedElimination`, `playerDealtFinalBlow`, `playerDealtDamage`, `playerTookDamage` | yes | yes | no |
| `playerDealtHealing`, `playerReceivedHealing` | yes | no | yes |
| `playerDealtKnockback`, `playerReceivedKnockback` | yes | yes | yes |

The other event values (`eventDamage`, `eventHealing`, `eventAbility`, `eventDirection`, `eventWasCriticalHit`, `eventWasEnvironment`, `eventWasHealthPack`) compile in any player event, so the compiler does not tell you when one is meaningless. They are only meaningful for the event that produces them: `eventDamage` and `eventWasCriticalHit` in damage events, `eventHealing` in healing events, `eventAbility` for the ability that caused the event.

For the same reason, `attacker` can be `null` for an environmental death: check it before you read from it.

An event's frequency matters as much as its name: a rule on `playerDealtDamage` runs once per damage instance, and a rule on `global` or `eachPlayer` with a condition is re-evaluated continuously.
